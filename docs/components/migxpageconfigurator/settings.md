# Справочник системных настроек

Все системные настройки компонента имеют префикс `mpc_`. Ниже — сводка по группам. Отдельные настройки подробно разобраны в тематических разделах (ссылки в колонке «Подробнее»).

::: info Раздел в разработке
Ниже — план раздела. Таблицы наполняются итеративно из `_build/elements/settings.php`.
:::

## Пути и базовые

<!-- mpc_path_to_chunks, mpc_path_to_sections, mpc_path_to_create, mpc_common_config_name, mpc_base_section_name, mpc_dev_mode -->

## Изображения и lazy-load

<!-- mpc_lazyload_attr, mpc_thumb_format — см. раздел «Работа с изображениями» -->

## Статические блоки и типы страниц

<!-- mpc_static_block_page_id — см. «Статические секции» -->

## Лексиконы и языки

См. подробности в разделе [Лексиконы и мультиязычность](lexicons).

| Настройка | По умолчанию | Назначение |
|-----------|--------------|------------|
| `mpc_use_lexicons` | `0` | включить режим лексиконов |
| `mpc_translated_content` | `text,contact` | какие типы контента переводимы |
| `mpc_exclude_lexicons_filename` | `…/services/exclude_lexicons.inc.php` | файл со списком исключений |
| `mpc_lexicon_filename_field` | `alias` | поле ресурса для имени lexicon-файла |
| `mpc_lexicon_path` | `…/lexicon/` | папка с lexicon-файлами |
| `mpc_lexicons_namespace` | `migxpageconfigurator` | namespace лексиконов MODX |
| `mpc_default_language` | `ru` | язык по умолчанию |
| `mpc_available_languages` | *(пусто)* | список языков (CSV) |
| `mpc_contact_lexicon_fields` | `caption` | переводимые под-поля контактов |

## Контакты

<!-- mpc_contacts_page_id, mpc_contacts_tv_name, mpc_contacts_tv_id, mpc_email, mpc_phone_format — см. «Работа с контактами» -->

## CLI / манифесты

<!-- mpc_manifests_path — см. «Консольные команды» -->
