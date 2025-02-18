---
subtitle: Узнайте как динамически обновлять содержимое страницы.
---
# ОБновление фрагментов

Когда AJAX обработчик выполняется, он может подгтовить фрагменты, которые запрошены реквестом, либо принудительно обновить фрагменты дополнительно передав переменные во фрагменты.

## Запросить обновление фрагментов

Браузер клиента может запросить обновление фрагментов с сервера, когда он выполняет запрос AJAX, который считается *запрашивающим обновление контента*. Следующий код отображает фрагмент **mytime** внутри элемента `#myDiv` на странице после вызова `onRefreshTime` [обработчика событий](./handlers.md).

```twig
<div id="myDiv">{% partial 'mytime' %}</div>
```

[API атрибутов данных](./attributes-api.md) использует атрибут `data-request-update`.

```html
<!-- API атрибутов данных -->
<button
    data-request="onRefreshTime"
    data-request-update="mytime: '#myDiv'">
    Выполнить
</button>
```

[JavaScript API](./javascript-api.md) использует опцию конфигурации `update`:

```js
// JavaScript API
$.request('onRefreshTime', {
    update: { mytime: '#myDiv' }
});
```

### Определение `update`

Определение того, что должно быть обновлено, указывается в виде JSON-подобного объекта, где:

- левая сторона (ключ) является **именем фрагмента**
- правая сторона (значение) — это **элемент** для обновления

Следующий код запросит обновление элемента `#myDiv` содержимым фрагмента **mypartial**.

```js
mypartial: '#myDiv'
```

Несколько элементов разделяются запятой.

```js
firstpartial: '#myDiv', secondpartial: '#otherDiv'
```

Если имя фрагмента содержит `/` или `-`, необходимо обернуть названия фрагментов в кавычки.

```js
'folder/mypartial': '#myDiv', 'my-partial': '#myDiv'
```

Элемент для обновления всегда должен быть справа, поскольку он также может быть HTML элементом в JavaScript.

```js
mypartial: document.getElementById('myDiv')
```

### Добавление содержимого в конец или начало элемента

Если перед строкой селектора стоит символ `@`, содержимое, полученное с сервера, будет добавлено в конец элемента вместо замены существующего содержимого. Аналог функции `append` в jQuery.

```js
'folder/append': '@#myDiv'
```

Если перед строкой селектора стоит символ `^`, содержимое будет добавлено в начало элемента. Аналог функции `prepend` в jQuery.

```js
'folder/append': '^#myDiv'
```

## Принудительно обновить содержимое страницы

Для сравнения, [обработчики AJAX](./handlers.md) могут отправлять обновления содержимого в браузер со стороны сервера. Чтобы отправить обновление, обработчик возвращает массив, где ключ — это элемент HTML для обновления (селектор CSS), а значение — это содержимое для обновления.

::: aside
Имя ключа должно начинаться с `#` или `.`, чтобы инициировать обновление содержимого.
:::

В следующем примере элемент на странице элемент **myDiv** будет обновлен, используя содержимое фрагмента **mypartial**. Обработчик `onRefreshTime` вызывает метод `renderPartial` для рендеринга содержимого фрагмента в PHP.

```php
function onRefreshTime()
{
    return [
        '#myDiv' => $this->renderPartial('mypartial')
    ];
}
```

## Передача переменных во фрагмент

В зависимости от контекста выполнения [обработчика событий AJAX](./handlers.md) он делает переменные доступными для фрагментов по-разному.

- Используйте `$this[]` внутри страницы или макета [секция PHP](../themes/themes.md).
- Используйте `$this->page[]` внутри [класса компонента](../themes/components.md).
- Используйте `$this->vars[]` в [админке](../../extend/system/controllers.md).

Эти примеры добавят переменную **result** во фрагмент для каждого контекста:

```php
// Внутри страницы или макета (секция PHP)
$this['result'] = 'Привет мир!';

// Внутри класса компонента
$this->page['result'] = 'Привет мир!';

// Внутри бекенд контроллера или виджета
$this->vars['result'] = 'Привет мир!';
```

Эта переменная доступна внутри фрагмента:

```twig
<!-- Привет мир! -->
{{ result }}
```
