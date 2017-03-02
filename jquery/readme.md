# Не повезет

## Условие задачи

> — Говорят, разработали новую библиотеку NoPassword.js для защиты данных на стороне клиента
>
> — Да, я про неё слышал, её даже в популярный фреймворк jQuery встроили
>
> — Кстати, я тут попробовал встроить её на свой веб-сайт. Протестируешь? http://ctf4.nsychev.ru

## Решение

Очевидно, что `alert` выдается не магией, а функцией `parseDate` при нажатии на кнопку. Учитывая то, что функции `parseDate` вообще не существует в JavaScript, должно 
создаться впечатление, что эта функция реализована в файле `jquery.min.js`, который совсем незаметно загружается вместе со страницей. Первая (и единственная, кстати)
строчка этого кода содержит выражение `(p, a, c, k, e, d)` и намекает на обфускацию. Дальше было два варианта дальнейших действий:

1. Воспользоваться сервисом деобфускации — вроде даже правильно работает.

2. Заметить, что всё, что выдает этот код, выполняется внутри `eval`, а значит, можно заменить его, к примеру, на `alert`.

Оригинальный код выглядел так:

```javascript
    function parseDate(l) {
      m = parseInt(l);
      if (m % 13462462 == 542523 && m > 542523) {
        alert('UPML_AY563429');
      } else {
        alert('Nope!');
      }
    }
```

Флаг, конечно, очевиден (**UPML_AY563429**), но если вы хотите его получить "более честно", то можете ввести 13462462 + 542523 = **14004985** и получить `alert` с флагом.