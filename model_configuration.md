# Конфигурация модели

Конфигурация моделей SleepingOwl Admin должны быть расположены в директории `bootstrapDirectory` (*по умолчанию: `app/admin`*).

Вы можете хранить конфигурацию моделей в одном файле или разделить на несколько по желанию.

Ниже приведен пример того, как может выглядеть конфигурация модели:

```php
Admin::model('App\Contact2')->title('Contact')->alias('contacts2')->display(function ()
{
	$display = AdminDisplay::table();
	$display->with('country', 'companies');
	$display->filters([
		Filter::related('country_id')->model('App\Country'),
	]);
	$display->columns([
		Column::image('photo')->label('Photo'),
		Column::string('fullName')->label('Name'),
		Column::datetime('birthday')->label('Birthday')->format('d.m.Y'),
		Column::string('country.title')->label('Country')->append(Column::filter('country_id')),
		Column::lists('companies.title')->label('Companies'),
	]);
	return $display;
})->createAndEdit(function ()
{
	$form = AdminForm::form();
	$form->items([
		FormItem::columns()->columns([
			[
				FormItem::text('firstName', 'First Name')->required(),
				FormItem::text('lastName', 'Last Name')->required(),
				FormItem::text('phone', 'Phone'),
				FormItem::text('address', 'Address'),
			],
			[
				FormItem::image('photo', 'Photo'),
				FormItem::date('birthday', 'Birthday')->format('d.m.Y'),
			],
			[
				FormItem::select('country_id', 'Country')->model('App\Country')->display('title'),
				FormItem::textarea('comment', 'Comment'),
			],
		]),
	]);
	return $form;
})->delete(null);
```

## Связь с моделью

```php
Admin::model(\App\MyModel::class)
```

Если вы используете PHP версии ниже 5.5, можете использовать строковое представление:

```php
Admin::model('App\MyModel')
```

## Заголовок

```php
->title('My Model Title')
```

Заголовок модели и текст элемента меню.

## Указание Alias

```php
->alias('districts')
```

Alias будет использован в url`ах. По умолчанию alias &mdash; множественная форма названия модели в нижнем регистре.

## Указание типа вывода списком

```php
->display(function ()
{
	// specify model display here
})
```

Подробнее смотрите в разделе [списки](displays).

## Указание форм создания и редактирования

Вы можете указать одну форму для создания и редактирования:

```php
->createAndEdit(function ()
{
	// specify model create or edit form here
})
```

Или разные формы:

```php
->create(function ()
{
	// create form
})
->edit(function ()
{
	// edit form
})
```

Подробнее смотрите в разделе [формы](form).

## Запрет на создание новых записей

```php
->createAndEdit(function ($id)
{
	if (is_null($id))
	{
		return null;
	}
	...
})
```

## Запрет на редактирование записей

```php
->createAndEdit(function ($id)
{
	if ( ! is_null($id))
	{
		return null;
	}
	...
})
```

## Запрет на удаление

Вы можете отключить функцию удаления записей:

```php
->delete(null)
```

## Запрет на восстановление

Вы можете отключить функцию восстановления записей (в моделях с soft-delete):

```php
->restore(null)
```