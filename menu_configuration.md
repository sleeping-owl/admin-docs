# Конфигурация меню

Конфигурация меню SleepingOwl Admin по умолчанию располагается в `app/admin/menu.php`.

Вот простой пример как может выглядеть конфигурация меню:

```
Admin::menu()->url('/')->label('Start Page')->icon('fa-dashboard');
Admin::menu('App\User')->icon('fa-user');
Admin::menu()->label('Subitems')->icon('fa-book')->items(function ()
{
	Admin::menu(\Acme\Models\Bar\User::class)->icon('fa-user');
	Admin::menu(\Acme\Models\Foo::class)->label('my label');
	Admin::menu()->url('about')->label('About');
});
```
	
## Создание элемента меню для модели

```php
Admin::menu(\App\MyModel::class)
```

Если вы используете PHP до версии 5.5 вы можете использовать строковое значение:

```php
Admin::menu('App\MyModel')
```

Модель должна быть зарегистрирована в SleepingOwl Admin. Подробности смотрите в [конфигурации модели](Model_Configuration.html).

Заголовок элемента меню будет взят из заголовка модели, но вы можете установить свой заголовок используя метод `label()`.

Адрес элемента меню будет ссылкой на указанную модель.

## Создание элемента меню с произвольным адресом

```php
Admin::menu()->url('my-url')->label('My Label')
```

Url должен быть зарегистрирован в адресах административного раздела. Подробнее смотрите в разделе [роуты административного интерфейса](Routes_Configuration.html).


## Создание элемента меню для действия контроллера

```php
Admin::menu()->url('my-url')->uses('\App\HTTP\Controllers\MyController@getAction')
```

Вы должны указать адрес для данного элемента используя `url()` и указать действие контроллера используя `uses()`.

## Заголовок элемента меню

```php
->label('My Label')
```

## Иконка элемента меню

```php
->icon('fa-bank')
```

Вы можете использвать [Font Awesome 4.1.0](http://fontawesome.io) классы иконок.

## Вложенные меню

```php
->items(function()
{
	// ...
})
```

Вы можете создавать подменю, без ограничения глубины вложенности.