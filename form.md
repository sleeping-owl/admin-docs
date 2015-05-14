# Формы

Форма может быть любым из [поддерживаемых способов вывода списка](displays) или самой формой. Вы можете комбинировать их по желанию.

```php
..->createAndEdit(function ()
{
	$form = AdminForm::form();
	$form->items([
		// create form items here
	]);
	return $form;
})
```

## Элементы формы

Вы можете создавать элементы формы следующим кодом:

```php
FormItem::{type}('{field name}', '{label}')
```

### Поддерживаемые элементы формы

 - [columns](form#columns)
 - [text](form#text)
 - [textAddon](form#textaddon)
 - [hidden](form#hidden)
 - [checkbox](form#checkbox)
 - [date](form#date)
 - [time](form#time)
 - [timestamp](form#timestamp)
 - [file](form#file)
 - [image](form#image)
 - [images](form#images)
 - [select](form#select)
 - [multiselect](form#multiselect)
 - [radio](form#radio)
 - [textarea](form#textarea)
 - [ckeditor](form#ckeditor)
 - [custom](form#custom)
 - [view](form#view)
 
### Валидация

```php
FormItem::text('title')->required()->unique()->validationRule('my-custom-rule')
```

Подробнее смотрите раздел [валидация](validation).

### Значение по умолчанию

Вы можете указать значение по умолчанию для элемента формы.

```php
FormItem::text('title')->defaultValue('My new item title')
```

### Регистрация собственного элемента формы

Вы можете регистрировать собственные элементы формы в файле `bootstrap.php` в директории `bootstrapDirectory` (*по умолчанию `app/admin/bootstrap.php`*).

```php
FormItem::register('{type}', 'App\MyFormItem')
```

Ваш класс должен расширять `SleepingOwl\Admin\FormItems\BaseFormItem` (если вам не нужно получание значения модели по названия) или `SleepingOwl\Admin\FormItems\NamedFormItem` (в противном случае).

### Добавление скриптов и стилей

Вы можете добавить свои скрипты и стили на страницу, которая использует ваш элемент формы.

```php
public function initialize()
{
	parent::initialize();

	AssetManager::addScript(asset('my.js'));
	AssetManager::addStyle(asset('my.css'));
}
```

### Пример

bootstrap.php

```php
FormItem::register('myItem', 'Acme\MyItem')
```

Acme/MyItem.php

```php
use SleepingOwl\Admin\FormItems\NamedFormItem;

class MyItem extends NamedFormItem
{

	public function render()
	{
		$params = $this->getParams();
		// $params will contain 'name', 'label', 'value' and 'instance'
		return view('my-form-item-view, $params);
	}

}
```

Использование в конфигурации модели

```php
FormItem::myItem('field')->label('My Label');
```

<a name="columns"></a>
## Columns

Позволяет разбивать форму на несколько столбцов. Используйте метод `columns()` для указания столбцов.

```php
FormItem::columns()->columns([
	[
		FormItem::...
	],
	[
		...
	],
])
```

<a name="text"></a>
## Text

Создает текстовое поле.

```php
FormItem::text('title', 'Title')
```

<a name="textaddon"></a>
## TextAddon

Создает текстовое поле с вставкой вначале или вконце.

По умолчанию расположение вставки - `before`.

```php
FormItem::textAddon('url', 'Url')->addon('http://my-site.com/')->placement('before')
FormItem::textAddon('price', 'Price')->addon('$')->placement('after')
```

<a name="hidden"></a>
## Hidden

Создает скрытое поле. Полезно в связке с параметрами вида ([подробнее](displays#table)).

```php
FormItem::hidden('field')
```

<a name="checkbox"></a>
## Checkbox

Создает checkbox с заголовком.

```php
FormItem::checkbox('active', 'Active')
```

<a name="date"></a>
## Date

Создает поле выбора даты.

```php
FormItem::date('date', 'Date')
```

Формат даты по умолчанию описан в [конфигурации](configuration)  в поле `dateFormat`. Вы можете переопределить его используя метод `format($format)`.

```php
FormItem::date('date', 'Date')->format('d.m.Y')
```

<a name="time"></a>
## Time

Создает поле выбора времени.

```php
FormItem::time('time', 'Time')
```

Формат даты по умолчанию описан в [конфигурации](configuration)  в поле `timeFormat`. Вы можете переопределить его используя метод `format($format)`.

```php
FormItem::time('time', 'Time')->format('H:i')
```

### Отображение секунд

```php
FormItem::time('time', 'Time')->format('H:i:s')->seconds(true)
```

<a name="timestamp"></a>
## Timestamp

Создает поле выбора даты и времени.

```php
FormItem::timestamp('timestamp', 'Timestamp')
```

Формат даты по умолчанию описан в [конфигурации](configuration)  в поле `datetimeFormat`. Вы можете переопределить его используя метод `format($format)`.

```php
FormItem::timestamp('timestamp', 'Timestamp')->format('d.m.Y H:i')
```

### Отображение секунд

```php
FormItem::timestamp('timestamp', 'Timestamp')->format('d.m.Y H:i:s')->seconds(true)
```

<a name="file"></a>
## File

Создает поле загрузки файла. Файл будет загружен в директорию `imagesUploadDirectory` из [конфигурации](configuration). Если вам необходимо другое расположение файла - перемещайте его вручную в вашей модели.

```php
FormItem::file('file', 'File')
```

<a name="image"></a>
## Image

Создает поле загрузки изображения с предпросмотром. Изображение будет загружено в директорию `imagesUploadDirectory` из [конфигурации](configuration). Если вам необходимо другое расположение файла - перемещайте его вручную в вашей модели.

```php
FormItem::image('photo', 'Photo')
```

<a name="images"></a>
## Images

Создает поле загрузки нескольких изображений с предпросмотром. Изображения будут загружены в директорию `imagesUploadDirectory` из [конфигурации](configuration). Если вам необходимо другое расположение файла - перемещайте его вручную в вашей модели.

Ваша модель должна реализовывать [field accessors](http://laravel.com/docs/5.0/eloquent#accessors-and-mutators) для данного поля и возвращать массив урлов изображений и сохранять массив.

```php
FormItem::images('photos', 'Photos')
```

Самый простой способ загружать и сохранять изображения - поле типа `text` с аксессором:

```php
public function getPhotosAttribute($value)
{
	return preg_split('/,/', $value, -1, PREG_SPLIT_NO_EMPTY);
}

public function setPhotosAttribute($photos)
{
	$this->attributes['photos'] = implode(',', $photos);
}
```

<a name="select"></a>
## Select

Создает поле выбора.

```php
FormItem::select('category_id', 'Category')
```

### Предоставление данных

Массивом:

```php
->options([1 => 'First', 2 => 'Second', 3 => 'Third])
```

При помощи enum (значения массива используются в качестве ключей):

```php
->enum(['First', 'Second', 'Third])
```

При помощи модели:

```php
->model('App\MyModel')->display('title')
```

### Обнуляемое поле

Вы можете пометить элемент как обнуляемый:

```php
…->nullable()
```

<a name="multiselect"></a>
## Multiselect

Создает поле выбора нескольких записей.

```php
FormItem::multiselect('categories', 'Categories')
```

### Предоставление данных

Массивом:

```php
->options([1 => 'First', 2 => 'Second', 3 => 'Third])
```

При помощи enum (значения массива используются в качестве ключей):

```php
->enum(['First', 'Second', 'Third])
```

При помощи модели:

```php
->model('App\MyModel')->display('title')
```

### Сохранение данных

Создайте новый метод мутатора в вашей модели. Пример:

```php
public function setCategoriesAttribute($categories)
{
	$this->categories()->detach();
	if ( ! $categories) return;
	if ( ! $this->exists) $this->save();
	
	$this->categories()->attach($categories);
}
```

Метод `categories()` определяет связь `belongs-to-many` в данном случае.

<a name="radio"></a>
## Radio

Создает поле radio.

```php
FormItem::radio('category_id', 'Category')
```

### Предоставление данных

Массивом:

```php
->options([1 => 'First', 2 => 'Second', 3 => 'Third])
```

При помощи enum (значения массива используются в качестве ключей):

```php
->enum(['First', 'Second', 'Third])
```

При помощи модели:

```php
->model('App\MyModel')->display('title')
```

### Обнуляемое поле

Вы можете пометить элемент как обнуляемый:

```php
…->nullable()
```

<a name="textarea"></a>
## Textarea

Создает поле textarea.

```php
FormItem::textarea('text', 'Text')
```

<a name="ckeditor"></a>
## CKEditor

Создает поле ckeditor.

```php
FormItem::ckeditor('text', 'Text')
```

<a name="custom"></a>
## Custom

Создает произвольный элемент формы. Вы должны указать метод отображения (`display()`) и метод сохранения (`callback()`).

```php
FormItem::custom()->display(function ($instance)
{
	return view('my-form-item', ['instance' => $instance]);
})->callback(function ($instance)
{
	$instance->myField = 12;
})
```

<a name="view"></a>
## View

Вставляет ваш произвольный view. Вы можете выводить там что вам угодно и вставлять скрипты. В ваш view будет передана переменная `$instance`.

```php
FormItem::view('admin.article.view')
```