# good-articles-translation
Перевод документации поставляемой с библиотекой q (https://github.com/kriskowal/q/)

Если некоторая функция не может вернуть значение или бросить исключение без блокировки, 
вместо них она может вернуть некоторое обещание. Обещание -- это объект, который представляет то 
самое возвращаемое значение или исключение, которое функция вернет через некоторое время. Также 
обещание можно использовать в качестве прокси для [remote object][Q-Connection], чтобы управлять
периодом ожидания (задержкой ответа).

Использование обещаний позволяет ограничить [Pyramid of
Doom][POD]: случай, когда код едет вправо быстрее, чем продвигается вниз (прим.: имеется в виду возможность
ограничения уровней вложенности коллбэков).

[POD]: http://calculist.org/blog/2011/12/14/why-coroutines-wont-work-on-the-web/

```javascript
step1(function (value1) {
    step2(value1, function(value2) {
        step3(value2, function(value3) {
            step4(value3, function(value4) {
                // Do something with value4
            });
        });
    });
});
```

С помощью библиотеки обещаний, Вы привести код к читабельному виду:

```javascript
Q.fcall(promisedStep1)
.then(promisedStep2)
.then(promisedStep3)
.then(promisedStep4)
.then(function (value4) {
    // Do something with value4
})
.catch(function (error) {
    // Handle any error from all above steps
})
.done();
```

