# Расширение Авторазиации
Данное расширение являеться новым для фреймворка MyUCP,
и совместимо только с версией 5.7 и выше. Работает при помощи
новой системы расширений которая была введена в новой версии.

## Установка
Расширение идет предустановленым в базовой сборке фреймворка.
Если у вас его нет то, для установки расширения вам нужно загрузить архив последней
версии и загрузить файлы с заменой.

**Важное примечание:** если у вас уже есть модель с названием Model,
вы можете оставить свою.

После загрузки файлов вам нужно указать алиас расширения в `configs/main.php`:

```php
'extensions'   =>  [
    ...


    // Список расширений которые будут инициализированы при запуске приложения
    // Обязательно должны реализовывать класс BootExtensionable
    'boot'  =>  [
        ...
        
        "Auth" => \Extensions\Auth\Auth::class,
    ],
],
```

После нужно указать путь для автозагрузки главного класса в `configs/autoload_classess.php`:

```php
\Extensions\Auth\Auth::class => EXTENSIONS_DIR . 'Auth/Auth.php',
```

Так же для работы расширения нужно еще одно расширение **Validate**, которое так же предустановлено
в базовой сборке фреймворка.

После загрузки всех файлов и указания путей, нужно настроить
конфигурационный файл расширения `configs/auth.php`

### Конфигурационный файл

В данном поле вы указываете название таблицы, где храняться ваши пользователи:
```php
"table" => "users",
```

В массиве `rows` указаны названия полей необходимые для работы расширения, вы так же можете заменить их на те которые указаны у вас в таблице:
```php
"rows" => [
    "id" => "user_id",
    "password" => "user_password",
    "email" => "user_email",
],
```

В поле `redirect` вы указываете адрес куда будет совершенна переадрессация после успешной авторизации пользователем
```php
"redirect" => "/",
```

## Доступные методы
Вы так же можете использовать некоторые методы расширения для получения некоторых данных или совершения действий авторизации или регистрации.

`login($email, $password)` - данный метод позволяет проводить авторизацию пользователя.
`register($email, $password, $password_repeat)` - данный метод позволяет проводить регистраци пользователя.  
`auth()` - данный метод позволяет получить данные авторизированного пользователя, если пользователь не авторизирован будет возвращен `false`  
`::user()` - данный метод является статическим и полной копией метода `auth()`  
`::check()` - данный метод является статическим и позволяет узнать статус пользователя, возвращает `true` в случае если пользователь авторизирован и `false` если не прошел авторизации.

## Виды (шаблоны)

В качестве форм авторизации и регистрации используются Zara шаблоны, которые предзагружены при инициализации расширения
вы так же можете перезаписать их, просто создав в директории `resources/views/` свои, с аналогичными названиями, они будут загружены вместо шаблонов расширения.

`auth.common` - шаблон каркас, который имеет в себе подключаемые стили и скрипты и основную разметку страниц.   
`auth.login` - шаблон с формой авторизации, расширяем шаблоном `auth.common`   
`auth.register` - шаблон с формой регистрации, расширяем шаблоном `auth.common`

## Лицензия

Расширение **Auth** является программных обеспечением с открытым исходным кодом, под [лицензией MIT](https://opensource.org/licenses/MIT).