# Работа с полями

## Простые поля
При обработке шаблонов компонент забирает в качестве значения всё содержимое тега для которого установлен атрибут `data-mpc-field`.
Исключения составляют теги, у которых есть атрибуты `href`, `src`, а так же тег `source`. 
Если таким тегам установить атрибут `data-mpc-field`, то компонент заберет в качестве значения содержимое атрибута  `src`/`href`/`srcset`. 

Подробнее о работе с изображениями читайте в разделе [Работа с изображениями](media)

Рассмотрим пример того как можно добавить в админку ссылку на ресурс и подпись к ней.
В шаблоне:
```fenom
<a href="1" data-mpc-field="target"><span data-mpc-field="btn_text" data-mpc-unwrap="1">Title</span></a>
```
После обработки:
```fenom
<a href="{$target | url}">{$btn_text}</a>
```

Если вам это кажется сложным, непонятным или вы не любите магию, то никто не запрещает делать в шаблоне так:
```fenom
<span  data-mpc-field="target" data-mpc-remove="1">1</span> 
<a href="{$target | url}">Title</a>
```

:::info Обратите внимание:
1. Поскольку значение атрибута `href` является числом, после обработки к нему будет применён модификатор url.
2. `span` с атрибутом `data-mpc-remove="1"` будет удалён при обработке.
:::

## Вложенные поля
Часто встречаются ситуации, когда в админку нужно передать какой-то список. В компоненте для этого предусмотрено множество полей:
- текстовые списки 
- списки с одной картиной
- списки с одним тегом picture
- списки с несколькими картинками
- списки с несколькими тегами picture
- списки с одним тегом video
- списки с одним тегом audio
- списки только с img, или picture, или video, или audio
- список списков

Кроме того, вы можете создать свои поля.

В шаблоне разметка для таких полей будет выглядеть так:
```fenom
<ul data-mpc-field="list_of_lists">
    <li data-mpc-item="">
        <h5 data-mpc-field-1="title">Title1</h5>
        <ul data-mpc-field-1="list_triple_img">
            <li data-mpc-item-1>
                <h5 data-mpc-field-2="title">Title12</h5>
                <h6 data-mpc-field-2="subtitle">Subtitle12</h6>
                <p data-mpc-field-2="content">Content12</p>
                <img data-mpc-field-2="img" src="https://i.pinimg.com/736x/80/7d/2b/807d2b9987f0d35a1036b1597c3deb74.jpg" width="50" height="50" alt="Радуга">
            </li>
        </ul>
    </li>
    <li data-mpc-item="">
        <h5 data-mpc-field-1="title">Title12</h5>
        <ul data-mpc-field-1="list_triple_img">
            <li data-mpc-item-1>
                <h5 data-mpc-field-2="title">Title122</h5>
                <h6 data-mpc-field-2="subtitle">Subtitle122</h6>
                <p data-mpc-field-2="content">Content22</p>
                <img data-mpc-field-2="img" src="https://i.pinimg.com/736x/f3/d9/2d/f3d92dcd1cd6f66daa196bfd255ac41d.jpg" width="50" height="50" alt="Радуга">
            </li>
        </ul>
    </li>
</ul>
```
Стоит обратить внимание на:
1. Атрибут `data-mpc-item` не имеет значения.
2. Цифра в конце имени атрибута обозначает уровень вложенности, самый верхний уровень - нулевой - не обозначается индексом.
3. Уровень вложенности у атрибутов `data-mpc-field` всегда на 1 больше, чем у атрибута `data-mpc-item` тега его непосредственного родителя.
4. Теги с атрибутом `data-mpc-field` должны быть вложены в тег с атрибутом `data-mpc-item`.

Например, дана вёрстка:
```fenom
<div class="row">
    <p class="col-12">Title 1</p>
    <p class="col-6">Title2</p>
    <p class="col-6">Title3</p>
</div>
```
Первый элемент должен быть отображён не так как остальные. Есть два способа этого добиться.
С использованием `data-mpc-lim` и `data-mpc-off`
```fenom
<div class="row">
    <p class="col-12">{$list_simple[0].content}</p>
    <div data-mpc-unwrap="1" data-mpc-field="list_simple" data-mpc-lim="2" data-mpc-off="1">
        <div data-mpc-item data-mpc-unwrap="1"><p class="col-6" data-mpc-field-1="content">Title 1</p></div>
        <div data-mpc-item data-mpc-unwrap="1"><p class="col-6" data-mpc-field-1="content">Title2</p></div>
        <div data-mpc-item data-mpc-unwrap="1"><p class="col-6" data-mpc-field-1="content">Title3</p></div>
    </div>
</div>
```
Результат первого варианта
```fenom
<div class="row">
    <p class="col-12">{$list_simple[0].content}</p>
    {foreach $list_simple as $item1 index=$i1 last=$l1}
        {set $c1 = 0}
        {if $i1 >= 1 AND $c1 < 2}
            {set $c1 = $c1 + 1}
            <p class="col-6">{$item1.content}</p>
        {/if}
    {/foreach}
</div>
```

Или с использованием условия
```fenom
<div class="row" data-mpc-field="list_double">
    <div data-mpc-item data-mpc-unwrap="1"><p class="{$i1 > 0 ? 'col-6' : 'col-12'}" data-mpc-field-1="content">Title 1</p></div>
    <div data-mpc-item data-mpc-unwrap="1"><p class="col-6" data-mpc-field-1="content">Title2</p></div>
    <div data-mpc-item data-mpc-unwrap="1"><p class="col-6" data-mpc-field-1="content">Title3</p></div>
</div>
```

Результат второго варианта
```fenom
<div class="row">
    {foreach $list_double as $item1 index=$i1 last=$l1}
        <p class="{$i1 > 0 ? 'col-6' : 'col-12'}">{$item1.content}</p>
    {/foreach}
</div>
```

:::info Обратите внимание:
1. Поля с атрибутом `data-mpc-unwrap="1"` после обработки лишились своих оборачивающих тегов.
:::

## Добавление своих полей в секцию

1. Перейдите в интерфейс компонента Migx.
2. В конфигурацию `mpc_base` добавьте поле.
3. Если добавленное поле имеет тип `migx` и использует уникальную конфигурацию не забудьте её создать. 
