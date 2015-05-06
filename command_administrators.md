# Администраторы

Используйте эту команду для отображения списка администраторов, создания нового администратора или удаления существующего.

## Использование

Отобразить список администраторов:

```bash
$ php artisan admin:administrators
```

Создание нового администратора:

```bash
$ php artisan admin:administrators --new
```

Изменение пароля для администратора:

```bash
$ php artisan admin:administrators --password
```

Изменение имени администратора:

```bash
$ php artisan admin:administrators --rename
```

Удаление существующего администратора:

```bash
$ php artisan admin:administrators --delete
```