# Столбцы

Создает новый столбец типа `{type}`:

```php
Column::{type}('{field name}')
```

## Поддерживаемые типы

 - [string](columns#string)
 - [lists](columns#lists)
 - [count](columns#count)
 - [image](columns#image)
 - [datetime](columns#datetime)
 - [order](columns#order)
 - [action](columns#action)
 - [checkbox](columns#checkbox)
 - [custom](columns#custom)

## Заголовок столбца

Вы можете задать заголовок столбца следующим кодом:

```php
…->label('My Column Header')
```
 
## Создание собственного столбца

Вы можете зарегистрировать свой класс в качестве столбца:

```php
Column::register('yesNo', \Acme\YesNoColumn::class)
```

Ваш класс столбца должен расширять `SleepingOwl\Admin\Columns\Column\BaseColumn` (если вам не нужно получать значение из объекта по имени) или `SleepingOwl\Admin\Columns\Column\NamedColumn` (в противном случае).

```php
class YesNoColumn extends SleepingOwl\Admin\Columns\Column\NamedColumn
{

	public function render()
	{
		$params = [
			'value'  => $this->getValue($this->instance, $this->name()),
		];
		return view('my-view', $params);
	}
	
}
```

Использование в конфигурации модели:

```php
$display->columns([
	Column::yesNo(),
]);
```


## Запрет на сортировку по столбцу

```php
Column::string('my_field')->orderable(false)
```

## Дополнения к столбцам

### Дополнение Filter

```php
Column::filter('{filter_alias}')->model('App\MyModel')->value('{field to grab filter value from}')
```

Это дополнение добавляет ссылку на фильтр в каждую ячейку. Вы можете опустить указание модели (`model()`) если фильтр принадлежит текущей модели.

*Пример:*

```php
Column::string('category.title')->label('Category')->append(
	Column::filter('category_id')
)
```

### Дополнение Url

```php
Column::url('{field to grab url from}')
```

Это дополнение добавляет ссылку в каждую ячейку. Ссылка будет извлечена из указанного поля модели.

*Пример:*

```php
Column::string('url', 'Url')->append(
	Column::url('full_url)
)
```


-----
<a name="string"></a>
## String

Содержимое ячейки будет простым значением поля из вашей модели или одной из связанных моделей.

```php
Column::string('{field name}')
```

### Название поля

Название поля может быть одним из следующих:
 
 - Название поля из вашей модели (из базы данных или используемых при помощи мутаторов).
 
 ```php
 Column::string('title')
 Column::string('url')
 ```
 
 - Название поля из связанных моделей
 
 ```php
 Column::string('category.title') // category() creates belongs-to relation
 Column::string('city.state.title') // you can use nested relations
 ```
 
-----
<a name="lists"></a>
## Lists

Содержимое ячейки будет список связанных моделей. Используется для связей `many-to-many`.

```php
Column::lists('categories.title')
```

Отображает список всех заголовков категорий, связанных с текущей записью.

`categories()` должен определять связь `belongs-to-many` в данном случае.
 
-----
<a name="count"></a>
## Count

Содержимое ячейки будет количество связанных записей. Используется со связями `has-many`.

```php
Column::count('images')
```

```php
Column::count('images')->append(
	Column::filter('school_id')->model('App\SchoolImage')
)
```

`images()` должен определять связь `has-many` в данном случае.
 
-----
<a name="image"></a>
## Image

Содержимым ячейки будет миниатюра изображения.

```php
Column::image('photo')
```

Поле `photo` должно содержать полный урл изображения либо путь относительно public директории.

Столбцы с изображениями не могут быть сортируемыми.
 
-----
<a name="datetime"></a>
## Datetime

Содержимым ячейки будет дата или время.

```php
Column::datetime('{field}')
```

### Формат даты и времени

Формат по умолчанию определен в конфиге параметром `datetimeFormat`. Вы можете переопределить его при помощи метода `format($format)`.

```php
Column::datetime('created_at')->format('d.m.Y H:i:s')
Column::datetime('created_at')->format('m/d/Y g:i')
```

-----
<a name="order"></a>
## Order

Содержимым ячейки будут стрелки вверх и вниз для сортировки моделей.

```php
Column::order()
```

Ваша модель должна подключать трэйт `use SleepingOwl\Admin\Traits\OrderableModel;` для использования данного столбца. Ваша модель должна содержать целочисленное поле, представляющее собой порядок записей. Создайте метод `getOrderField()` в вашей модели, который возвращает название этого поля:

```php
public function getOrderField()
{
	return 'order';
}
```
 
### Пример модели

```php
namespace App;

use Illuminate\Database\Eloquent\Model;

class Category extends Model
{
	use SleepingOwl\Admin\Traits\OrderableModel;
	
	public function getOrderField()
	{
		return 'order';
	}
}
```
 
-----
<a name="action"></a>
## Action

Содержимым ячейки будет кнопка с произвольным действием.

```php
Column::action('{name}')
```

### Стиль кнопки

Вы можете указать класс иконки для использования (из FontAwesome):

```php
Column::action('show')->icon('fa-globe')
```

Доступно 2 стиля: `short` и `long`

```php
 # Это создаст кнопку без надписи, только иконка. Заголовок будет всплывать при наведении.
Column::action('show')->label('Label')->icon('fa-globe')->style('short')

 # Это создаст кнопку с иконкой и заголовком.
Column::action('show')->label('Label')->icon('fa-globe')->style('long')
```

**Важно:** стиль по умолчанию &mdash; `long` без иконки.

### Target кнопки

Вы можете указать target для кнопки:

```php
Column::action('show')->label('Label')->url('http://test.com/:id')->target('_blank')
```

### URL кнопки

Вы можете указать url для кнопки, `:id` будет заменен на id строки, в которой расположена кнопка:

```php
Column::action('show')->label('Label')->url('http://test.com/:id')
```

или вы можете указать функцию для генерация url`а:

```php
Column::action('show')->label('Label')->url(function ($instance)
{
	return URL::route('my-route', [$instance->id]);
})
```

### Произвольное действие

Используйте метод `->callback()` для задания произвольного действия:

```php
Column::action('show')->label('Label')->callback(function ($instance)
{
	# Any code you want
})
```

-----
<a name="checkbox"></a>
## Checkbox

Столбец checkbox используется для осуществления массовых действий.

```php
Column::checkbox()
```

Подробнее смотрите раздел [массовые операции](displays#table).
 
-----
<a name="custom"></a>
## Custom

Этот столбец использует ваше замыкание для поления значения ячейки.

```php
Column::custom()->label('Published')->callback(function ($instance)
{
	return $instance->published ? '&check;' : '-';
}),
```