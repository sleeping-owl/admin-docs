# Установка

 1. Добавьте данный пакет в ваш composer.json и запустите `composer update`:

	```json
	"sleeping-owl/admin": "dev-development"
	```

 2. После обновления composer, добавьте service provider в ваш `config/app.php`

	    'SleepingOwl\Admin\AdminServiceProvider',

 3. Добавьте это в настройку фасадов `config/app.php`:

		'Admin'         => 'SleepingOwl\Admin\Admin',
		'AdminAuth'     => 'SleepingOwl\AdminAuth\Facades\AdminAuth',
		'Column'        => 'SleepingOwl\Admin\Columns\Column',
		'ColumnFilter'  => 'SleepingOwl\Admin\ColumnFilters\ColumnFilter',
		'Filter'        => 'SleepingOwl\Admin\Filter\Filter',
		'AdminDisplay'  => 'SleepingOwl\Admin\Display\AdminDisplay',
		'AdminForm'     => 'SleepingOwl\Admin\Form\AdminForm',
		'AdminTemplate' => 'SleepingOwl\Admin\Templates\Facade\AdminTemplate',
		'FormItem'      => 'SleepingOwl\Admin\FormItems\FormItem',

 4. Запустите эту команду в терминале (если вы хотите узнать, что конкретно делает эта команда, посмотрите [документацию по команде install](install)):

	```bash
	$ php artisan admin:install
	```
 5. Готово! Теперь зайдите по адресу `<your_site_url>/admin` и авторизуйтесь при помощи данных по умолчанию `admin` / `SleepingOwl`.