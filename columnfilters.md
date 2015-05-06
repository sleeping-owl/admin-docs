# Фильтры столбцов

Создает новый столбец типа `{type}`:

```php
ColumnFilter::{type}()
```

## Поддерживаемые типы

 - [text](#text)
 - [select](#select)
 - [date](#date)
 - [range](#range)

-----
<a name="text"></a>
## Text

Столбец будет фильтроваться по значению тектового поля.

```php
ColumnFilter::text()
```

### Placeholder

Вы можете добавить placeholder к полю:
 
```php
ColumnFilter::text()->placeholder('Title')
```

-----
<a name="select"></a>
## Select

Столбец будет фильтроваться по значению из выпадающего списка.

```php
ColumnFilter::select()
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

### Placeholder

Вы можете добавить placeholder к полю:
 
```php
ColumnFilter::select()->placeholder('Country')
```

-----
<a name="date"></a>
## Date

Столбец будет фильтроваться по дате.

```php
ColumnFilter::date()
```

### Placeholder

Вы можете добавить placeholder к полю:
 
```php
ColumnFilter::date()->placeholder('Title')
```

### Format

Вы можете указать используемый формат даты/времени:

```php
ColumnFilter::date()->format('d.m.Y')
```

-----
<a name="range"></a>
## Range

Столбец будет отфильтрован от одного значения до другого.

```php
ColumnFilter::range()
```

### Диапазон дат

Вы можете фильтровать по диапазону дат:
 
```php
ColumnFilter::range()->from(
	ColumnFilter::date()->format('d.m.Y')->placeholder('From Date')
)->to(
	ColumnFilter::date()->format('d.m.Y')->placeholder('To Date')
)
```

### Диапазон чисел

Вы можете фильтровать по диапазону чисел:

```php
ColumnFilter::range()->from(
	ColumnFilter::text()->placeholder('From')
)->to(
	ColumnFilter::text()->placeholder('To')
)
```