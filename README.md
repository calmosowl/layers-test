# Пример использования @layer

Мы решили использовать ui библиотеку на своём проекте. Библиотека уже использует @layer. В ней есть компонент card и папка utils, в которой находится файл text-align. Всё это импортируется в файл main.css.

```
@layer card, utils;

@import "components/card.css" layer(card);
@import "utils/text-align.css" layer(utils);
```

Подключаем библиотеку к проекту, импортируя файл main.css в наш styles.css.

`@import "ui-library/main.css";`

В библиотеке компонент card имеет такое правило:

```
.card {
  padding: 0.5rem 1rem;
  border: 1px solid;
}
```

`text-align` не задан, поэтому текст выравнивается по левому краю. Но мы хотим чтобы по умолчанию текст выравнивался по правому краю. Дописываем файл styles.css

```
.card {
  text-align: right;
}
```

Но теперь возникает проблемма: библиотечный утилити-класс `.text-left` из файла text-align.css перестал работать, поскольку наше правило с выравниванием по правому краю находится ниже по каскаду. Обе карточки теперь выравнены по правому краю.

```
<div class="card">
  Right-aligned card
</div>

<div class="card text-left">
  Left-aligned card
</div>
```

Мы можем исправить такое поведение внеся изменения в каскадные слои. Перепишем наше правило используя `@layer card`.

```
@layer card {
  .card {
    text-align: right;
  }
}
```

Теперь правило с выравниванием по правому краю находится в слое `card`. Поскольку слой `utils` объявлен после слоя `card` `@layer card, utils;`, правила из этого слоя будут находится в каскаде ниже и в карточке с классом `card text-left` применится правило их файла text-align:

```
.text-left {
  text-align: left;
}
```