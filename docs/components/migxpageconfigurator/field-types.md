# Справочник типов полей

::: warning Раздел в работе (TODO)
Здесь будет полный перечень типов полей из `mpc_base` (значения `data-mpc-ftype`) с описанием **вложенных полей** каждого типа и их настроек. Например:

- **`image`** — вложенные поля `src`, `alt`, `title`, `width`, `height` (обращение `{$field[0].src}` и т.д.);
- **`picture` / `video` / `audio`** — свои наборы вложенных полей;
- **`listbox` / `option` / `checkbox`** — опции (`inputOptionValues`), формат `data-mpc-values`;
- скалярные (`text`, `textarea`, `richtext`, `number`, `date`, `email`, `url`, `file`, `tag`) — без вложенных полей.

Заполнить по прототипам из конфигурации `mpc_base` (сверять с кодом — см. `SectionProcessor::makeFieldDef` / `synthesizeScalar`).
:::
