# Emerge.js — Документация

Источник: https://ilyabirman.ru/emerge/documentation/

## Описание

Emerge — скрипт для отложенного показа элементов. Элементы с `class="emerge"` проявляются только после загрузки содержимого (картинок, видео, CSS-фонов).

## Установка

Добавьте в `<head>`:

```html
<script src="/путь/к/emerge.js"></script>
```

Если скрипт подключается в конце документа, в `<head>` добавьте:

```html
<style>.emerge { opacity: 0; }</style>
```

## Использование

```html
<div class="emerge">
  ... Покажется после загрузки ...
</div>
```

## Индикаторы загрузки

```html
<div class="emerge" data-spin="true">
  ... Будет индикатор загрузки ...
</div>
```

### Атрибуты индикатора

| Атрибут | Описание | По умолчанию |
|---|---|---|
| `data-spin-size="24"` | Диаметр в пикселях | 24 |
| `data-spin-color="#000"` | Цвет | Чёрный |
| `data-spin-direction="clockwise"` | Направление вращения | `clockwise` |

### Свой индикатор

```html
<div id="custom-spinner" style="display: none">
  ... Код своего индикатора ...
</div>
```

```html
<div class="emerge" data-spin-element="custom-spinner">
  ... Элемент с кастомным индикатором ...
</div>
```

## Эффекты

| Атрибут | Описание |
|---|---|
| `data-effect="slide"` | Встроенный эффект. Варианты: `relax`, `slide`, `zoom`, `screw` |
| `data-duration="500"` | Длительность анимации в мс (по умолчанию 500) |
| `data-up="20px"` / `data-down="5em"` | Смещение вверх/вниз для `slide` (по умолчанию 20px вверх) |
| `data-left="10%"` / `data-right="5cm"` | Смещение влево/вправо для `slide` (по умолчанию 0) |
| `data-scale="0.5"` | Исходный масштаб для `relax`, `zoom`, `screw` (по умолчанию 0.92 для relax, 0.5 для zoom/screw) |
| `data-angle="90"` | Исходный угол для `screw` в градусах (по умолчанию 90°) |
| `data-origin="top"` | Точка трансформации. По умолчанию `top` для relax, `center center` для zoom/screw |
| `data-opaque="true"` | Элемент изначально непрозрачный |
| `data-style-1` / `data-style-2"` | Кастомные CSS для анимации (игнорируются при `data-effect`) |

### Примеры

```html
<!-- Скольжение -->
<div class="emerge" data-effect="slide">
  ... Появится со скольжением ...
</div>

<!-- Рост от низа -->
<div class="emerge" data-effect="relax" data-scale=".5" data-origin="bottom">
  ... Вырастет вертикально от половины высоты ...
</div>

<!-- Кастомный CSS -->
<div class="emerge"
  data-opaque="true"
  data-style-1="-webkit-transform: rotate3d(1,1,0,90deg)"
  data-style-2="-webkit-transform: rotate3d(0,0,0,0);
    transition: opacity .5s ease-out,
      -webkit-transform 2s cubic-bezier(0.0, 0.0, 0.001, 1.0)"
>
  ... Космический эффект ...
</div>
```

## Порядок и время

| Атрибут | Описание |
|---|---|
| `data-await="element-id"` | Ждать загрузки элемента с указанным id |
| `data-continue="true"` | Ждать загрузки предыдущего элемента `.emerge` в DOM |
| `data-hold="500"` | Задержка в мс перед появлением |
| `data-expose="true"` | Ждать появления элемента в области видимости |

### Примеры

```html
<!-- Ждать предыдущий -->
<div class="emerge">
  ... Тра-ля-ля...
</div>
<div class="emerge" data-continue="true">
  ... Дождётся предыдущего элемента ...
</div>

<!-- Ждать конкретный элемент -->
<div class="emerge" data-await="thing" data-hold="500">
  ... Дождётся элемента thing, появится через 500 мс ...
</div>
<div class="emerge" id="thing">
  ... Появится первым ...
</div>

<!-- Одновременное появление -->
<div class="emerge" id="one" data-await="two">
  ... Появится одновременно со следующим ...
</div>
<div class="emerge" id="two" data-await="one">
  ... Появится одновременно со предыдущим ...
</div>
```

## Событие готовности видео

По умолчанию Emerge ждёт `canplaythrough`. Можно изменить:

```html
<video ... data-emerge-event="loadedmetadata">
  ...
</video>
```

## Повтор анимации

```html
<a href="#" class="emerge-replay">Replay</a>
```

Воспроизводит все анимации страницы заново по клику.
