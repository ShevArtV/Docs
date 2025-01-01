# Начало работы

::: warning Особенности
Из-за особенностей обработки HTML в PHP некоторые символы, в частности "+", 
нужно вставлять в шаблон после обработки функцией urlencode(). По этой же причине выносите svg в отдельный файл и
используйте функцию разворачивания.
:::

## Проверьте установленные компоненты

Перейдите в раздел `Пакеты`  и убедитесь, что следующие компоненты установлены:

- pdoTools
- Migx
- pThumb

Если они у вас не установлены, то установите их вручную.

## Проверьте системные настройки

Затем перейдите в системные настройки. Выберите пространство имён `migxpageconfigurator` и убедитесь,
что установлены значения следующих настроек:

- `mpc_config_tv_id`
- `mpc_contacts_page_id`
- `mpc_contacts_tv_id`
- `mpc_static_block_page_id`
- `mpc_tmplvar_ids`

Если какие-то из этих значений отсутствуют, то установка компонента прошла некорректно и вам следует связаться с разработчиком.

## Подготовка к работе

Если работает из IDE с удалённым сервером скачайте файл `assets/components/migxpageconfigurator/css/mpc.css`, это позволит IDE подсказывать какие атрибуты доступны.

## Создайте индексный файл

В папке элементов компонента `pdoTools` (по умолчанию `core/elements/`) создайте папку `pages/`.
В ней создайте файл `index.tpl` с содержимым:

```fenom
{set $path = '!getParsedConfigPath' | snippet:[]}
{if $path}
    {include $path}
{else}
    {$_modx->resource.content}
{/if}
```

## Создайте файлы шаблонов

В папке элементов компонента `pdoTools` (по умолчанию `core/elements/`) создайте папку `templates/`.
В неё нужно поместить верстку всех страниц в виде файлов с расширением `.tpl`.
Общие элементы всех страниц нужно вынести в шаблон-обертку в имени этого файла обязательно должно быть слово `wrapper`.
:::details Пример такой обёртки:

```fenom
<!DOCTYPE html>
<html lang="ru">
<head>

    <title>{$pagetitle}</title>
    <meta name="description" content="{$description}">
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">

    <base href="{$_modx->config.site_url}">

    <link rel="icon" data-mpc-info="favicon" data-mpc-if="$favicon" href="favicon_web.ico" data-mpc-ctx>

    <link rel="apple-touch-icon" sizes="180x180" data-mpc-info="favicon_apple" data-mpc-if="" href="favicon_apple.ico">

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

    <link rel="stylesheet" href="assets/project_files/css/landing/style.css">

    <span data-mpc-unwrap="" data-mpc-info="metrics">{Metrics}</span>
</head>

<body>
<div data-mpc-section="wrapper" class="wrapper">
    <span data-mpc-remove="1" data-mpc-info="site_name">MigxPageConfigurator</span>
    <span data-mpc-remove="1" data-mpc-info="site_name" data-mpc-ctx>WEB_MigxPageConfigurator</span>
    <!-- header -->
    <header class="header index">
        <div class="container">
            <div class="header-content">
                <a href="" class="logotype">
                    <img src="logo.png" data-mpc-info="logo" alt="" width="213" height="48">
                </a>
                <ul class="header-auth">
                    <li data-mpc-contact="phone|header">
                        <p class="footer-contact__icon" data-mpc-cfield="caption">
                            Phone
                        </p>
                        <a href="" data-mpc-cfield="value">
                            <span>8(999)888-77-66</span>
                        </a>
                    </li>
                </ul>
            </div>
        </div>
    </header>

    <!-- wrap -->
    <main class="wrap">
        <!--content-->
        {if !$sections}
            {$content}
        {else}
            {foreach $sections as $section}
                {$section}
            {/foreach}
        {/if}
        <!--content-->
    </main>

    <!-- footer -->
    <footer class="footer">
        <div class="container">

            <div class="footer-top">
                <div class="columns">
                    <div class="column col-6 md-col-12">
                        <h3>{$cp_id | resource: 'pagetitle'}</h3>
                        <ul class="footer-contact">
                            <li data-mpc-contact="phone|footer">
                                <div class="footer-contact__icon">
                                    <img src="assets/project_files/img/phone.svg" data-svg data-mpc-nolazy data-mpc-nothumb data-mpc-cfield="attributes" alt="">
                                </div>
                                <a href="" data-mpc-cfield="value">
                                    <span>8(999)888-77-66</span>
                                </a>
                            </li>
                            <li data-mpc-contact="email|footer">
                                <div class="footer-contact__icon">
                                    <img src="assets/project_files/img/envelope.svg" data-mpc-nothumb data-mpc-nolazy data-svg data-mpc-cfield="attributes" alt="">
                                </div>
                                <a href="" data-mpc-cfield="value">
                                    email@domain.ru
                                </a>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>

            <div class="footer-bottom">
                <div class="footer-logo">
                    <img src="logo_alt.png" data-mpc-info="logo_alt" alt="" width="213" height="48">
                </div>

                <ul class="footer-bottom__list">
                    <li>© {'' | date: 'Y'} Все права защищены</li>
                </ul>
            </div>

        </div>
    </footer>
</div>
</body>
</html>
```

:::

Обратите внимание, что блок с атрибутом `data-mpc-section` является первым дочерним элементом тега `body` и содержит в себе другие части шаблона.
Это структуру необходимо соблюдать для всех обёрток.
Бывают ситуации, когда разные страницы на одном сайте имеют разные шапку и подвал. В этом случае нужно создать вторую обёртку.
В разных обёртках значения `data-mpc-section` должны быть разными, а имена файлов обязательно должны содержать слово `wrapper`.
Шаблоны не являющиеся обёртками на первой строке должны содержать такую запись

```fenom
<!--##{"templatename":"Шаблон-пример","wrapper":"wrapper","icon":"icon-gear","include":"file:pages/index.tpl"}##-->
```

в ней передаются данные для создания шаблона в админке.
Важно строго соблюдать синтаксис, менять можно только значения права от двоеточия между двойными кавычками:

- `templatename` - название шаблона в админке (в начале будет дописано слово Шаблон)
- `wrapper` - имя обёртки должно быть такое же как и в `data-mpc-section`
- `icon` - иконка в админке
- `include` - содержимое шаблона в админке

## Добавьте атрибуты
Атрибуты с "*" требуют полного знакомства со всеми разделами документации. 

### Атрибуты секций

- `data-mpc-section` - ключ секции может содержать только латиницу, цифры и нижнее подчеркивание
- `data-mpc-name` - человекопонятное название секции для контент-менеджера
- `data-mpc-static` - если секция статичная, то установите этот атрибут со значением 1
- `data-mpc-copy` - если секция используется в разных шаблонах, укажите в этом атрибуте путь к фалу шаблона в котором хранится оригинал верстки,
иначе секция будет перезаписываться.

:::info
Статичной называется секция, контент которой не зависит от конкретного ресурса.
:::

### Атрибуты полей

- `data-mpc-field` - ключ поля из тех, что созданы в интерфейсе компонента Migx.
- `data-mpc-lim*` - лимит элементов для полей типа migx.
- `data-mpc-off*` - указывает какое количество элементов от начала пропустить для полей типа migx.
- `data-mpc-item*` - указывает на то, что поле является элементом migx вложенным в migx.
- `data-mpc-remove` - используйте этот, атрибут если поле нужно добавить в секцию, но удалить из вёрстки.
  Например, вы хотите указать путь к файлу со стилями секции
```fenom
<span data-mpc-remove="1" data-mpc-field="css_path">path/to/file.css</span>
```
- `data-mpc-attr` - если нужно вывести атрибут поля динамически, используйте этот атрибут. Например:
```fenom
<p data-mpc-field="titlle" data-mpc-attr="{$title === 'Title' ? 'data-title' : 'data-other'}">Title</p>
```



Или вы добавили своём поле типа migx `my_field` и вам нужно самостоятельно написать его вывод

```fenom
<span data-mpc-remove="1" data-mpc-field="my_field"></span>
{foreach $my_field as $item}
    <h2>{$item.title}</h2>
    <p>{$item.content}</p>
{/foreach}
```

:::details Полный список полей

```json
{
  "id": "ID",
  "section_name": "Название",
  "file_name": "Имя файла секции",
  "hide_section": "Скрыть секцию?",
  "copy_from_origin": "Копировать содержимое из Типов страниц?",
  "is_static": "Это статичная секция?",
  "position": "Порядковый номер на странице",
  "title": "Заголовок",
  "subtitle": "Подзаголовок",
  "content": "Текст",
  "btn_text": "Текст кнопки",
  "target": "Целевое окно",
  "resources": "ID ресурсов",
  "limit": "Лимит",
  "preview": "Превью",
  "bg_img": "Фоновая картинка",
  "img": [
    "Картинка",
    {
      "src": "Путь к файлу",
      "alt": "Альтернативный текст",
      "width": "Ширина",
      "height": "Высота"
    }
  ],
  "list_images": [
    "Список картинок",
    {
      "preview": "Превью",
      "img": "Картинка"
    }
  ],
  "list_simple_img": [
    "Cписок с картинкой",
    {
      "content": "Содержимое",
      "img": "Картинка",
      "preview": "Превью"
    }
  ],
  "list_double_img": [
    "Двойной список с картинкой",
    {
      "title": "Заголовок",
      "content": "Содержимое",
      "img": "Картинка",
      "preview": "Превью"
    }
  ],
  "list_triple_img": [
    "Тройной список с картинкой",
    {
      "title": "Заголовок",
      "subtitle": "Подзаголовок",
      "content": "Содержимое",
      "img": "Картинка",
      "preview": "Превью"
    }
  ],
  "list_simple_images": [
    "Простой список с картинками",
    {
      "title": "Заголовок",
      "content": "Содержимое",
      "list_images": "Изображения"
    }
  ],
  "list_double_images": [
    "Двойной список с картинками",
    {
      "title": "Заголовок",
      "content": "Содержимое",
      "list_images": "Изображения"
    }
  ],
  "list_triple_images": [
    "Тройной список с картинками",
    {
      "title": "Заголовок",
      "subtitle": "Подзаголовок",
      "content": "Содержимое",
      "list_images": "Изображения"
    }
  ],
  "picture": [
    "Изображение",
    {
      "img": "Основное изображение",
      "sources": "Источники",
      "preview": "Превью"
    }
  ],
  "list_pictures": [
    "Список изображений",
    {
      "preview": "Превью",
      "picture": "Изображение"
    }
  ],
  "list_simple_picture": [
    "Простой список с изображением",
    {
      "content": "Содержимое",
      "picture": "Изображение"
    }
  ],
  "list_double_picture": [
    "Двойной список с изображением",
    {
      "title": "Заголовок",
      "content": "Содержимое",
      "picture": "Изображение"
    }
  ],
  "list_triple_picture": [
    "Тройной список с изображением",
    {
      "title": "Заголовок",
      "subtitle": "Подзаголовок",
      "content": "Содержимое",
      "picture": "Изображение"
    }
  ],
  "list_simple_pictures": [
    "Простой список с изображениями",
    {
      "content": "Содержимое",
      "list_pictures": "Список изображений"
    }
  ],
  "list_double_pictures": [
    "Двойной список с изображениями",
    {
      "title": "Заголовок",
      "content": "Содержимое",
      "list_pictures": "Список изображений"
    }
  ],
  "list_triple_pictures": [
    "Тройной список с изображениями",
    {
      "title": "Заголовок",
      "subtitle": "Подзаголовок",
      "content": "Содержимое",
      "list_pictures": "Список изображений"
    }
  ],
  "list_simple": [
    "Простой список",
    {
      "content": "Содержимое"
    }
  ],
  "list_double": [
    "Двойной список",
    {
      "title": "Заголовок",
      "content": "Содержимое"
    }
  ],
  "list_triple": [
    "Тройной список",
    {
      "title": "Заголовок",
      "subtitle": "Подзаголовок",
      "content": "Содержимое"
    }
  ],
  "list_of_lists": [
    "Список списков",
    {
      "list_triple_img": "Тройной список с картинкой",
      "title": "Заголовок",
      "block_id": "ID блока"
    }
  ],
  "video": [
    "Видео",
    {
      "src": "Путь к файлу",
      "autoplay": "Начинать проигрывание автоматически",
      "controls": "Отображать элементы управления",
      "loop": "Зациклить воспроизведение",
      "muted": "Загрушить звук",
      "preload": "Как видео должно загружаться при загрузке страницы",
      "height": "Высота",
      "width": "Ширина",
      "poster": "Постер",
      "sources": "Источники"
    }
  ],
  "audio": [
    "Аудио",
    {
      "src": "Путь к файлу",
      "autoplay": "Начинать проигрывание автоматически",
      "controls": "Отображать элементы управления",
      "loop": "Зациклить воспроизведение",
      "muted": "Загрушить звук",
      "preload": "Как аудио должно загружаться при загрузке страницы",
      "sources": "Источники"
    }
  ],
  "list_videos": [
    "Список видео",
    {
      "path": "Путь к файлу",
      "video": "Видео"
    }
  ],
  "list_audios": [
    "Список аудио",
    {
      "path": "Путь к файлу",
      "audio": "Аудио"
    }
  ],
  "css_file_path": "Путь к файлу стилей",
  "pt_pc": "Внуттрений отступ сверху (ПК)",
  "pb_pc": "Внуттрений отступ снизу (ПК)",
  "pt_mob": "Внуттрений отступ сверху (МОБ)",
  "pb_mob": "Внуттрений отступ снизу (МОБ)"
}
```
:::

### Атрибуты медиа

- `data-mpc-nolazy` - если нужно, чтобы к конкретному меда-файлу не применялась ленивая загрузка, укажите значение 1
- `data-mpc-nothumb` - если нужно, чтобы pThumb не обрабатывал картинку, укажите значение 1. 
- `data-mpc-thumb` - если хотите для конкретного изображения передать особыва параметры генерацию миниатюры, укажите их в значение атрибута.
```fenom
<img data-mpc-field="img" src="https://i.pinimg.com/736x/2b/d7/27/2bd7274a962e509da7dd8ed5b27549f7.jpg" width="500" height="400" data-mpc-thumb="ra=1&f=png" alt="Это озеро"> 
```

### Атрибуты чанков
- `data-mpc-chunk` - вырезает всё содержимое блока в файл чанка
- `data-mpc-include` - вставляет чанк через include 
- `data-mpc-parse*` - вставляет этот чанк parseChunk

```fenom
<div data-mpc-chunk="common/chunk_2.tpl" data-mpc-parse="['resources' => '{$resources}']">
    <p>Parsed chunk {$resources}</p>
</div> 
```

### Атрибуты сниппетов
- `data-mpc-snippet*` - вставляет в блок вызоа указанного сниппета с параметрами из пресета 

```fenom
<div data-mpc-snippet="!pdoResources|default">
    <a href="{$uri}" data-mpc-chunk="pdoresources/default/item.tpl">{$pagetitle}</a>
</div>
```

### Атрибуты контактов
- `data-mpc-contact*` - атрибут для указания типа и расположения контакта через |, должен стоят у тега обрамляющего сам контакта
- `data-mpc-cfield*` - указывает на конкретное поле контакта

```fenom
<li data-mpc-contact="phone|footer">
    <div class="footer-contact__icon">
        <img src="assets/project_files/img/phone.svg" data-mpc-nothumb data-svg data-mpc-cfield="attributes" alt="">
    </div>
    <a href="" data-mpc-cfield="value">
        <span>8(999)888-77-66</span>
    </a>
</li>
```

### Атрибуты пользовательских настроек

Вы можете установить компонент `ClientConfig` создать необходимые поля и автоматически заполнить их информацией из верстки.

- `data-mpc-info*` - помещает информацию внутри тега в админку и заменяет её соответсвующим плейсхолдером.
- `data-mpc-ctx*` - позволяет указать для какого контекста сохранить данную настройку, по умолчанию  `web`.

```fenom
<link rel="icon" data-mpc-info="favicon" data-mpc-if="$favicon" href="favicon_web.ico" data-mpc-ctx>
```

### Общие атрибуты

Данные атрибуты можно использовать с различными сущностями.

- `data-mpc-if*` - позволяет задать простое условие вывода.
- `data-mpc-symbol*` - позволяет указать первый символ, актуально если нужно чтобы сущность была обработана парсером Fenom
уже в момент отдачи страницы на фронт. По умолчанию `{` - сущность будет обработана на пререндере, 
можно указать `##` - сущность будет обработана парсером Fenom в момент отдачи страницы на фронт.

## Выполните команду в терминале

```bash
php -d display_errors -d error_reporting=E_ALL public_html/core/components/migxpageconfigurator/console/mgr_tpl.php web examples.tpl 1
```
Команда принимает 3 аргумента:
1. Контекст
2. Имя файла шаблона
3. Обновлять ли данные в админке

:::info
Если вы хотите обработать сразу все шаблоны, выполните команду без аргументов или передав только контекст.
:::
