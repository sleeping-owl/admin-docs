# Типы отображения данных

Доступные типы вывода:

 * [Таблица](#table)
 * [Datatables](#datatables)
 * [Асинхронная Datatables](#datatablesAsync)
 * [Дерево](#tree)
 * [Вывод табами](#tabbed)
 * [Произвольный](#custom)

<a name="table"></a>
## Вывод таблицей

```php
…->display(function ()
{
	$display = AdminDisplay::table();
	// configure display
	return $display;
})
```

### Указание столбцов

Укажите столбцы для отображения в таблице. Подробнее смотрите в разделе [столбцы](columns).

```php
$display->columns([
	Column::string('title')->label('Title'),
	...
]);
```

### Фильтры столбцов

Вы можете указать фильтр для каждого столбца. Подробнее смотрите в разделе [фильтры столбцов](columnfilters).

```php
$display->columnFilters([
	null, // у первого столбца нет фильтра
	ColumnFilter::text()->placeholder('Title'), // у второго столбца тектовый фильтр
])
```

### Eager Loading

Укажите связи для eager load.

```php
$display->with('related', 'other_relation');
```

### Изменение запроса

Вы можете изменять запрос по желанию:

```php
$display->apply(function ($query)
{
	$query->where('my_field', 2);
	$query->orderBy('date', 'desc');
});
```

### Применение scope

Вы можете применить eloquent scope к выводимым данным:

```php
$display->scope('myScope');
```

### Указание фильтров

Вы можете добавить фильтры к отображению. Фильтры будут применены на основе параметров запроса. Подробнее смотрите в разделе [фильтры](filters).

```php
$display->filters([
	Filter::scope('last')->title('Latest News'),
	...
]);
```

### Указание параметров

Эти параметры будут добавлены к ссылкам на добавление и редактирование. Эти параметры могут быть использованы в качестве значений по умолчанию для элементов формы.

```php
$display->parameters([
	'related_id' => 12,
	...
]);
```

### Массовые действия

Вы можете добавить массовые действия:

```php
$display->actions([
	Column::action('export')->value('Export')->icon('fa-share')->target('_blank')->callback(function ($collection)
	{
		dd('You are trying to export:', $collection->toArray());
	}),
	...
]);
```

Для использования массовых действий необходимо добавить `Column::checkbox()` в список столбцов.

<hr/>
<a name="datatables"></a>
## Вывод Datatables

Это вид основан на [табличном](#table) виде и вы можете использовать все возможности табличного вида.

```php
…->display(function ()
{
	$display = AdminDisplay::datatables();
	// configure display
	return $display;
})
```

### Указание сортировки

Вы можете указать сортировку по умолчанию для вашей таблицы в формате сортировки datatables (порядковый номер столбца и направление сортировки):

```php
$display->order([[0, 'desc']]);
```

Вы можете указать сортировку по нескольким столбцам:

```php
$display->order([[0, 'desc'], [2, 'asc']]);
```

### Атрибуты datatables

Вы можете указать атрибуты datatables:

```php
$display->attributes([
	'ordering' => false,
	'stateSave' => true,
]);
```

Поддерживаемые атрибуты:

 - `ordering` &mdash; boolean, включена ли функция сортировки (по умолчанию: true)
 - `stateSave` &mdash; boolean, включена ли функция сохранения состояния (по умолчанию: true)

<hr/>
<a name="datatablesAsync"></a>
## Асинхронный вывод Datatables

Этот вид основан на [datatables](#datatables) виде. Вам не нужно производить каких-либо дополнительных конфигураций, ваша datatables таблица станет асинхронной с этим видом.

```php
…->display(function ()
{
	$display = AdminDisplay::datatablesAsync();
	// configure display
	return $display;
})
```

Этот вид имеет некоторые ограничения:

 * вы не можете сортировать по столбцу не из вашей таблицы (вам необходимо пометить эти столбцы как несортируемые вручную)
 * вы не можете фильтровать по столбцу не из вашей таблицы
 * если вы хотите использовать асинхронный вывод datatables внутри вывода табами, необходимо указать имя таблицы:
   `AdminDisplay::datatablesAsync('my-table-name')`

<hr/>
<a name="tree"></a>
## Вывод деревом

Поддерживается 2 nested-sets пакета:

 * https://github.com/etrepat/baum
 * https://github.com/lazychaser/laravel-nestedset
 
И простое дерево, основанное на полях `parent_id` и `order`.
 
Если у вас установлен один из этих пакетов, вы можете использовать вывод деревом:
 
```php
…->display(function ()
{
	$display = AdminDisplay::tree();
	// configure display
	return $display;
})
```

### Указание отображаемого поля

Вы должны указать поле для вывода в качестве заголовка записи (*по умолчанию 'title'*).

```php
$display->value('myTitleField');
```

### Отключение сортировки

Вы можете включить или выключить сортировку дерева.

```php
$display->reorderable(false);
```

### Простое дерево

Вы можете использовать этот тип дерева, если у вас в таблице есть поля `parent_id` и `order`.

Необходимо указать названия полей и значение корневого parent_id (по умолчанию `null`):

```php
$display->parentField('parent_id');
$display->orderField('order');
$display->rootParentId(0);
```

<hr/>
<a name="tabbed"></a>
## Вывод табами

Вы можете объединить несколько типов выводов используя табы.

```php
…->display(function ()
{
	$display = AdminDisplay::tabbed();
	// configure display
	return $display;
})
```

### Указание табов

Вы должны указать табы:

```php
$display->tabs(function ()
{
	$tabs = [];
	
	$firstTab = AdminDisplay::table();
	// configure first tab display
	$tabs[] = AdminDisplay::tab($firstTab)->label('First Tab')->active(true);
	
	$secondTab = AdminDisplay::datatables();
	// configure second tab display
	$tabs[] = AdminDisplay::tab($secondTab)->label('Second Tab');
	
	$thirdTab = Admin::model('App\MyOtherModel')->display();
	// this tab will be display from 'App\MyOtherModel' configuration
	$tabs[] = AdminDisplay::tab($thirdTab)->label('Third Tab');
	
	return $tabs;
});
```

<hr/>
<a name="custom"></a>
## Произвольный вывод

Вы можете использовать свои собственные view и отображать их как угодно:

```php
…->display(function ()
{
	$rows = App\News::all();
	$model = Admin::model('App\News');
	return view('custom_display', compact('rows', 'model'));
})
```

Url для формы создания можно получить через `$model->createUrl()`.

Url для формы редактирования можно получить через `$model->editUrl($id)`.