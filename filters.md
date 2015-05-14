# Фильтры

Доступные тип фильтров:

 * [фильтр по связанной модели](filters#related)
 * [фильтр по полю](filters#field)
 * [фильтр со scope](filters#scope)
 * [произвольный фильтр](filters#custom)

## Обзор фильтров

### Alias

Все фильтры имеют `alias`. Если в запросе есть параметр с именем alias - фильтр будет применен.

Alias можно задать используя метод `alias($alias)`. Если alias не задан - имя фильтра будет использовано в качестве alias.

### Заголовок

Заголовок фильтра будет отображаться в качестве подзаголовка, когда фильтр активен. Некоторые фильтры генерируют заголовок сами.

Вы можете переопределить заголовок фильтра используя метод `title($title)`. Если `$title` - строка, то она будет использована как есть, если замыкание - будет вызвано с параметром запроса:

```php
Filter::field('my_field')->title(function ($value)
{
	return 'Parameter Value: ' . $value;
})
```

### Переопределение параметра запроса

```php
Filter::field('title')->value('TODO category')
```

Создает фильтр по полю `title`. Он игнорирует параметр запроса и переопределяет его значением `'TODO category'`.

**Важно:** параметр запроса должнен иметь значение. Вы не можете применить фильтр используя `categories?title`, но `categories?title=1` или `categories?title=something_else` будут работать.

<hr/>
<a name="#related"></a>
## Фильтр по связанной модели

Необходимо указать поле для сортировки (*related_id в примере*), связанную модель (*App\Related*) и поле связанной модели для отображения в качестве заголовока фильтра (*firstName*). Значение фильтра будет извлечено из параметров запроса.

```php
Filter::related('related_id')->model('App\Related')->display('firstName')
```

<hr/>
<a name="#field"></a>
## Фильтр по полю

Следующий код будет фильтровать данные по параметру из запроса.

```php
Filter::field('published')
```

<hr/>
<a name="scope"></a>
## Фильтр со scope

Этот фильтр будет применять scope к вашему запросу.

```php
Filter::scope('myScope')
```

Вы должны определить scope в вашей модели (подробнее смотрите в [документации laravel scope](http://laravel.com/docs/5.0/eloquent#query-scopes)):

```php
public function scopeMyScope($query, $parameter)
{
	$query->where('myField', $parameter);
}
```

<hr/>
<a name="custom"></a>
## Произвольный фильтр

Вы можете создавать свои собственные фильтры (`$query` будет билдером запросов eloquent, `$parameter` - параметр запроса):

```php
Filter::custom('filter_name')->callback(function ($query, $parameter)
{
	$query->where('myField', $parameter);
})->title('My Filter Title')
```