… а именно как писать LL парсеры для не очень сложных структур при помощи конструирования сложного парсера из более простых. Изредка возникает необходимость распарсить что то несложное, скажем некую XML-подобную структуру или какой нибудь data URL, и тогда обычно возникает либо простыня хитрого трудно читаемого кода либо зависимость от какой то ещё более сложной и хитрой библиотеки для парсинга. Здесь я собираюсь совместить несколько известных идей (какие то из них попадались на Хабре) и показать как можно просто и лаконично написать довольно сложные парсеры уложившись при этом в совсем немного строчек кода. Для примера я буду писать парсер XML-подобной структуры. И да, я не буду вставлять сюда картинку для привлечения внимания. В статье вообще картинок нет, поэтому читать будет трудно.

#### Основная идея

Она в том, что каждый кусочек входного текста парсится отдельной функцией (назовём её «паттерном») и комбинируя эти функции можно получать более сложные функции которые смогут парсить более сложные тексты. Итак, паттерн — это такой объект у которого есть метод _exec_ который и осуществляет парсинг. Этой функции указывают что и откуда парсить и она возвращает распарсенное и то место где парсинг закончился:

```
var digit = {
    exec: function (str, pos) {
        var chr = str.charAt(pos);
        if (chr >= "0" && chr <= "9") 
            return { res: +chr, end: pos + 1};
    }
};
```

Теперь digit это паттерн парсящий цифры и его можно использовать так:

```
assert.deepEqual(digit.exec("5", 0), { res: 5, end: 1 });
assert.deepEqual(digit.exec("Q", 0), void 0);
```

Почему именно такой интерфейс? Потому что в JS есть встроенный класс RegExp с очень похожим интерфейсом. Для удобства введём класс Pattern (смотрите на него как на аналог RegExp) экземпляры которого и будут представлять собой эти паттерны:

```
function Pattern(exec) {
    this.exec = exec;
}
```

Затем введём несколько простых паттернов которые пригодятся практически в людом более-менее сложном парсере.

#### Простые паттерны

Самый простой паттерн это txt — он парсит фиксированную наперёд заданную строку текста:

```
function txt(text) {
    return new Pattern(function (str, pos) {
        if (str.substr(pos, text.length) == text)
            return { res: text, end: pos + text.length };
    });
}
```

Применяется он так:

```
assert.deepEqual(txt("abc").exec("abc", 0), { res: "abc", end: 3 });
assert.deepEqual(txt("abc").exec("def", 0), void 0);
```

Если бы в JS были конструкторы-по-умолчанию (как в C++), то запись txt(«abc») можно было бы сократить до «abc» в контексте конструирования более сложного паттерна.

Затем введём его аналог для регулярных выражений и назовём его rgx:

```
function rgx(regexp) {
    return new Pattern(function (str, pos) {
        var m = regexp.exec(str.slice(pos));
        if (m && m.index === 0)
            return { res: m[0], end: pos + m[0].length };
    });
}
```

Применяют его так:

```
assert.deepEqual(rgx(/\d+/).exec("123", 0), { res: "123", end: 3 });
assert.deepEqual(rgx(/\d+/).exec("abc", 0), void 0);
```

Опять же, если бы в JS были конструторы-по-умолчанию, то запись rgx(/abc/) можно было сократить до /abc/.

#### Паттерны-комбинаторы

Теперь нужно ввести несколько паттернов которые комбинируют уже имеющиеся паттерны. Самый простой из таких «комбинаторов» это opt — он делает любой паттерн не обязательным, т.е. если исходный паттерн p не может распарсить текст, то opt(p) на этом же тексте скажет, что всё распарсилось, только результат парсинга пуст:

```
function opt(pattern) {
    return new Pattern(function (str, pos) {
        return pattern.exec(str, pos) || { res: void 0, end: pos };
    });
}
```

Пример использования:

```
assert.deepEqual(opt(txt("abc")).exec("abc"), { res: "abc", end: 3 });
assert.deepEqual(opt(txt("abc")).exec("123"), { res: void 0, end: 0 });
```

Если бы в JS была возможна перегрузка операторов и конструкторы-по-умолчанию, то запись opt(p) можно было бы сократить до p || void 0 (это хорошо видно из того как реализован opt).

Следующий по сложности паттерн-комбинатор это exc — он парсит только то, что может распарсить первый паттерн и не может распарсить второй:

```
function exc(pattern, except) {
    return new Pattern(function (str, pos) {
        return !except.exec(str, pos) && pattern.exec(str, pos);
    });
}
```

Если W(p) это множество текстов которые парсит паттерн p, то W(exc(p, q)) = W(p) \\ W(q). Это удобно например когда нужно распарсить все большие буквы кроме буквы H:

```
var p = exc(rgx(/[A-Z]/), txt("H"));

assert.deepEqual(p.exec("R", 0), { res: "R", end: 1 });
assert.deepEqual(p.exec("H", 0), void 0);
```

Если бы в JS была перегрузка операторов, то exc(p1, p2) можно было бы сократить до p1 — p2 или до !p2 && p1 (для этого, правда, потребуется ввести паттерн-комбинатор all/and который будет работать как оператор &&).

Затем идёт паттерн-комбинатор any — он берёт несколько паттернов и конструирует новый, который парсит то, что парсит первый из данных паттернов. Можно сказать, что W(any(p1, p2, p3, ...)) = W(p1) v W(p2) v W(p3) v…

```
function any(...patterns) {
    return new Pattern(function (str, pos) {
        for (var r, i = 0; i < patterns.length; i++)
            if (r = patterns[i].exec(str, pos))
                return r;
    });
}
```

Я воспользовался конструкцией ...patterns (harmony:rest\_parameters), чтобы избежать корявого кода вроде \[\].slice.call(arguments, 0). Пример использования any:

```
var p = any(txt("abc"), txt("def"));

assert.deepEqual(p.exec("abc", 0), { res: "abc", end: 3 });
assert.deepEqual(p.exec("def", 0), { res: "def", end: 3 });
assert.deepEqual(p.exec("ABC", 0), void 0);
```

Если бы в JS была перегрузка операторов, то any(p1, p2) можно было бы сократить до p1 || p2.

Следующий паттерн-комбинатор это seq — он последовательно парсит текст данной ему последовательностью паттернов и выдаёт массив результатов:

```
function seq(...patterns) {
    return new Pattern(function (str, pos) {
        var i, r, end = pos, res = [];

        for (i = 0; i < patterns.length; i++) {
            r = patterns[i].exec(str, end);
            if (!r) return;
            res.push(r.res);
            end = r.end;
        }

        return { res: res, end: end };
    });
}
```

Применяется он так:

```
var p = seq(txt("abc"), txt("def"));

assert.deepEqual(p.exec("abcdef"), { res: ["abc", "def"], end: 6 });
assert.deepEqual(p.exec("abcde7"), void 0);
```

Если бы в JS была перегрузка операторов, то seq(p1, p2) можно было бы сократить до p1, p2 (перегружен оператор «запятая»).

Ну и наконец паттерн-комбинатор rep — он много раз применяет известный паттерн к тексту и выдаёт массив результатов. Как правило это применяется для парсинга неких однотипных конструкций разделённых скажем запятыми, поэтому rep принимает два аргумента: основной паттерн результаты которого нам интересны и разделяющий паттерн результаты которого отбрасываются:

```
function rep(pattern, separator) {
    var separated = !separator ? pattern :
        seq(separator, pattern).then(r => r[1]);

    return new Pattern(function (str, pos) {
        var res = [], end = pos, r = pattern.exec(str, end);

        while (r && r.end > end) {
            res.push(r.res);
            end = r.end;
            r = separated.exec(str, end);
        }

        return { res: res, end: end };
    });
}
```

Можно добавить ещё пару параметров min и max которые будут контролировать сколько повторений допустимо. Здесь я воспользовался стрелочной функцией r => r\[1\] (harmony:arrow\_functions), чтобы не писать function (z) { return z\[1\] }. Обратите внимание на то как rep сводится к seq при помощи Pattern#then (идея взята у Promise#then):

```
function Pattern(exec) {
    ...

    this.then = function (transform) {
        return new Pattern(function (str, pos) {
            var r = exec(str, pos);
            return r && { res: transform(r.res), end: r.end };
        });
    };
}
```

Этот метод позволяет выводить из одного паттерна другой при помощи применения произвольного преобразования к результатам первого. Кстати, знатоки хаскеля, можно ли сказать что этот Pattern#then делает из паттерна монаду?

Ну а rep применяется таким образом:

```
var p = rep(rgx(/\d+/), txt(","));

assert.deepEqual(p.exec("1,23,456", 0), { res: ["1", "23", "456"], end: 8 });
assert.deepEqual(p.exec("123ABC", 0), { res: ["123"], end: 3 });
assert.deepEqual(p.exec("ABC", 0), void 0);
```

Какой либо внятной аналогии с перегрузкой операторов для rep мне в голову не приходит.

В итоге получается около 70 строчек на все эти rep/seq/any. На этом список паттернов-комбинаторов заканчивается и можно переходить собственно к конструированию паттерна распознающего XML-подобный текст.

#### Парсер XML-подобных текстов

Ограничимся вот такими XML-подобными текстами:

```
<?xml version="1.0" encoding="utf-8"?>
<book title="Book 1">
  <chapter title="Chapter 1">
    <paragraph>123</paragraph>
    <paragraph>456</paragraph>
  </chapter>
  <chapter title="Chapter 2">
    <paragraph>123</paragraph>
    <paragraph>456</paragraph>
    <paragraph>789</paragraph>
  </chapter>
  <chapter title="Chapter 3">
    ...
  </chapter>
</book>
```

Для начала напишем паттерн распознающий именованный атрибут вида name=«value» — он, очевидно, часто встречается в XML:

```
var name = rgx(/[a-z]+/i).then(s => s.toLowerCase());
var char = rgx(/[^"&]/i);
var quoted = seq(txt('"'), rep(char), txt('"')).then(r => r[1].join(''));
var attr = seq(name, txt('='), quoted).then(r => ({ name: r[0], value: r[2] }));
```

Здесь attr парсит именованный атрибут со значением в виде строки, quoted — парсит строку в кавычках, char — парсит одну букву в строке (зачем это писать в виде отдельного паттерна? затем, что потом «научить» этот char парсить т.н. xml entities), ну а name парсит имя атрибута (обратите внимание что парсит он как большие так и малые буквы, но возвращает распарсенное имя где все буквы малые). Применение attr выглядит так:

```
assert.deepEqual(
    attr.exec('title="Chapter 1"', 0), 
    { res: { name: "title", value: "Chapter 1" }, end: 17 });
```

Далее сконструируем паттерн умеющий парсить заголовок вида <?xml… ?>:

```
var wsp = rgx(/\s+/);

var attrs = rep(attr, wsp).then(r => {
    var m = {}; 
    r.forEach(a => (m[a.name] = a.value));
    return m;
});

var header = seq(txt('<?xml'), wsp, attrs, txt('?>')).then(r => r[2]);
```

Здесь wsp парсит один или несколько пробелов, attrs парсит один или несколько именованных атрибутов и возвращает распарсенное в виде словаря (rep возвратил бы массив пар имя-значение, но словарь удобнее, поэтому массив преобразуется в словарь внутри then), а header парсит собственно заголовок и возвращает только атрибуты заголовка в виде того самого словаря:

```
assert.deepEqual(
    header.exec('<?xml version="1.0" encoding="utf-8"?>', 0),
    { res: { version: "1.0", encoding: "utf-8" }, end: ... });
```

Теперь перейдём к распарсиванию конструкций вида <node...>...:

```
var text = rep(char).then(r => r.join(''));
var subnode = new Pattern((str, pos) => node.exec(str, pos));

var node = seq(
    txt('<'), name, wsp, attrs, txt('>'),
    rep(any(text, subnode), opt(wsp)),
    txt('</'), name, txt('>'))
    .then(r => ({ name: r[1], attrs: r[3], nodes: r[5] }));
```

Здесь text парсит текст внутри узла (node) и использует паттерн char который может быть научен распознавать xml entities, subnode парсит внутренний узел (фактически subnode = node) и node парсит узел с атрибутами и внутренними узлами. Зачем такое хитрое определение subnode? Если в определении node сослаться на node напрямую (как то так: node = seq(..., node, ...)) то окажется, что на момент определения node эта переменная ещё пуста. Трюк с subnode позволяет убрать эту циклическую зависимость.

Осталось определить паттерн распознающий весь файл с заголовком:

```
var xml = seq(header, node).then(r => ({ root: r[1], attrs: r[0] }));
```

Применение соответственно такое:

```
assert.deepEqual(
    xml.exec(src), {
        attrs: { version: '1.0', encoding: 'utf-8' },
        root: {
            name: 'book',
            attrs: { title: 'Book 1' },
            nodes: [
                {
                    name: 'chapter',
                    attrs: { title: 'Chapter 1' },
                    nodes: [...]
                },
                ...
            ]
        }
    });
```

Здесь я вызываю Pattern#exec с одним аргументом и смысл этого в том, что я хочу распарсить строку с самого начала и убедиться, что она распарсилась до конца, ну а поскольку она распарсилась до конца, то вернуть достаточно только распарсенное без указателя на то место где парсер остановился (я и так знаю, что это конец строки):

```
function Pattern(name, exec) {
    ...

    this.exec = function (str, pos) {
        var r = exec(str, pos || 0);
        return pos >= 0 ? r : !r ? null : r.end != str.length ? null : r.res;
    };
}
```

Собственно весь парсер в 20 строчек (не забываем про те 70 которые реализуют rep, seq, any и пр.):

```
var name = rgx(/[a-z]+/i).then(s => s.toLowerCase());
var char = rgx(/[^"&]/i);
var quoted = seq(txt('"'), rep(char), txt('"')).then(r => r[1].join(''));
var attr = seq(name, txt('='), quoted).then(r => ({ name: r[0], value: r[2] }));

var wsp = rgx(/\s+/);

var attrs = rep(attr, wsp).then(r => {
    var m = {}; 
    r.forEach(a => (m[a.name] = a.value));
    return m;
});

var header = seq(txt('<?xml'), wsp, attrs, txt('?>')).then(r => r[2]);

var text = rep(char).then(r => r.join(''));
var subnode = new Pattern((str, pos) => node.exec(str, pos));

var node = seq(
    txt('<'), name, wsp, attrs, txt('>'),
    rep(any(text, subnode), opt(wsp)),
    txt('</'), name, txt('>'))
    .then(r => ({ name: r[1], attrs: r[3], nodes: r[5] }));

var xml = seq(header, node).then(r => ({ root: r[1], attrs: r[0] }));
```

С перегрузкой операторов в JS (или в C++) это выглядело бы как то так:

```
var name = rgx(/[a-z]+/i).then(s => s.toLowerCase());
var char = rgx(/[^"&]/i);
var quoted = ('"' + rep(char) + '"').then(r => r[1].join(''));
var attr = (name + '=' + quoted).then(r => ({ name: r[0], value: r[2] }));

var wsp = rgx(/\s+/);

var attrs = rep(attr, wsp).then(r => {
    var m = {}; 
    r.forEach(a => (m[a.name] = a.value));
    return m;
});

var header = ('<?xml' + wsp + attrs + '?>').then(r => r[2]);

var text = rep(char).then(r => r.join(''));
var subnode = new Pattern((str, pos) => node.exec(str, pos));

var node = ('<' + name + wsp + attrs + '>' + rep(text | subnode) + (wsp | null) + '</' + name + '>')
    .then(r => ({ name: r[1], attrs: r[3], nodes: r[5] }));

var xml = (header + node).then(r => ({ root: r[1], attrs: r[0] }));
```

Стоит отметить, что каждый var здесь строго соответствует одному правилу ABNF и потому если надо что то распарсить по описанию в RFC (а там любят ABNF), то перенос тех правил — дело механическое. Более того, поскольку сами правила ABNF (а также EBNF и PEG) строго формальны, то можно написать парсер этих правил и затем, вместо вызовов rep, seq и пр. писать что то такое:

```
var dataurl = new ABNF('"data:" mime ";" attrs, "," data', {
    mime: /[a-z]+\/[a-z]+/i,
    attrs: ...,
    data: /.*/
}).then(r => ({ mime: r[1], attrs: r[3], data: r[5] }));
```

И применять как обычно:

```
assert.deepEqual(
    dataurl.exec('data:text/plain;charset="utf-8",how+are+you%3f'),
    { mime: "text/plain", attrs: { charset: "utf-8" }, data: "how are you?" });
```

#### Ещё немного буков

Почему Pattern#exec возвращает null/undefined если распарсить ничего не удалось? Почему бы не бросать исключение? Если использовать исключения таким образом, то парсер станет медленнее раз в двадцать. Исключения хороши для исключительных случаев.

Описанный способ позволяет писать LL парсеры которые подходят не для всех целей. Например если надо распарсить XML вида

```
<book attr1="..." attr2="..." очень много атрибутов ???
```

То в том месте где стоит ??? может оказаться как /> так и просто >. Теперь если LL парсер пытался всё это распарсить как <book ...> а на месте ??? оказался />, то парсер отбросит всё то что он так долго парсил и начнёт заново в предположении, что это <book .../>. LR парсеры лишены этого недостатка, но писать их трудно.

Также LL парсеры плохо подходят для разбора разнообразных синтаксических/математических выражений где есть операторы с разным приоритетом и т.п. LL парсер конечно можно написать, но он будет несколько запутан и будет работать медленно. LR парсер будет запутан сам по себе, но будет быстр. Поэтому такие выражения удобно парсить т.н. алгоритмом Пратта который неплохо [объяснил](http://javascript.crockford.com/tdop/tdop.html) Крокфорд (если эта ссылка у вас фиолетовая, то в парсерах вы наверно разбираетесь лучше меня и вам наверно было очень скучно читать всё это).

Надеюсь кому нибудь это пригодится. В своё время я писал парсеры разной степени корявости и описанный выше способ был для меня открытием.