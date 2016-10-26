# Airbnb JavaScript Style Guide() {

*用更合理的方式寫 JavaScript*

## <a name="table-of-contents">目錄</a>

  1. [類型](#types)
  1. [對象](#objects)
  1. [數組](#arrays)
  1. [字符串](#strings)
  1. [函數](#functions)
  1. [屬性](#properties)
  1. [變量](#variables)
  1. [提升](#hoisting)
  1. [比較運算符 & 等號](#comparison-operators--equality)
  1. [塊](#blocks)
  1. [註釋](#comments)
  1. [空白](#whitespace)
  1. [逗號](#commas)
  1. [分號](#semicolons)
  1. [類型轉化](#type-casting--coercion)
  1. [命名規則](#naming-conventions)
  1. [存取器](#accessors)
  1. [構造函數](#function Object() { [native code] }s)
  1. [事件](#events)
  1. [模塊](#modules)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 兼容性](#ecmascript-5-compatibility)
  1. [測試](#testing)
  1. [性能](#performance)
  1. [資源](#resources)
  1. [誰在使用](#in-the-wild)
  1. [翻譯](#translation)
  1. [JavaScript 風格指南說明](#the-javascript-style-guide-guide)
  1. [與我們討論 JavaScript](#chat-with-us-about-javascript)
  1. [貢獻者](#contributors)
  1. [許可](#license)

## <a name="types">類型</a>

  - **原始值**: 存取直接作用於它自身。

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **複雜類型**: 存取時作用於它自身值的引用。

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 回到頂部](#table-of-contents)**

## <a name="objects">對象</a>

  - 使用直接量創建對象。

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - 不要使用[保留字](http://es5.github.io/#x7.6.1)作為鍵名，它們在 IE8 下不工作。[更多信息](https://github.com/airbnb/javascript/issues/61)。

    ```javascript
    // bad
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - 使用同義詞替換需要使用的保留字。

    ```javascript
    // bad
    var superman = {
      class: 'alien'
    };

    // bad
    var superman = {
      klass: 'alien'
    };

    // good
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ 回到頂部](#table-of-contents)**

## <a name="arrays">數組</a>

  - 使用直接量創建數組。

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - 向數組增加元素時使用 Array#push 來替代直接賦值。

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - 當你需要拷貝數組時，使用 Array#slice。[jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - 使用 Array#slice 將類數組對象轉換成數組。

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="strings">字符串</a>

  - 使用單引號 `''` 包裹字符串。

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - 超過 100 個字符的字符串應該使用連接符寫成多行。
  - 注：若過度使用，通過連接符連接的長字符串可能會影響性能。[jsPerf](http://jsperf.com/ya-string-concat) & [討論](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - 程序化生成的字符串使用 Array#join 連接而不是使用連接符。尤其是 IE 下：[jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // bad
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        // use direct assignment in this case because we're micro-optimizing.
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="functions">函數</a>

  - 函數表達式：

    ```javascript
    // 匿名函數表達式
    var anonymous = function() {
      return true;
    };

    // 命名函數表達式
    var named = function named() {
      return true;
    };

    // 立即調用的函數表達式（IIFE）
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  - 永遠不要在一個非函數代碼塊（if、while 等）中聲明一個函數，把那個函數賦給一個變量。瀏覽器允許你這麼做，但它們的解析表現不一致。
  - **注：** ECMA-262 把 `塊` 定義為一組語句。函數聲明不是語句。[閱讀對 ECMA-262 這個問題的說明](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)。

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - 永遠不要把參數命名為 `arguments`。這將取代函數作用域內的 `arguments` 對象。

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

**[⬆ 回到頂部](#table-of-contents)**



## <a name="properties">屬性</a>

  - 使用 `.` 來訪問對象的屬性。

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - 當通過變量訪問屬性時使用中括號 `[]`。

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="variables">變量</a>

  - 總是使用 `var` 來聲明變量。不這麼做將導致產生全局變量。我們要避免汙染全局命名空間。

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - 使用 `var` 聲明每一個變量。
    這樣做的好處是增加新變量將變的更加容易，而且你永遠不用再擔心調換錯 `;` 跟 `,`。

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // （跟上面的代碼比較一下，看看哪裡錯了）
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - 最後再聲明未賦值的變量。當你需要引用前面的變量賦值時這將變的很有用。

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - 在作用域頂部聲明變量。這將幫你避免變量聲明提升相關的問題。

    ```javascript
    // bad
    function () {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function () {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad - 不必要的函數調用
    function () {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // good
    function () {
      var name;

      if (!arguments.length) {
        return false;
      }

      name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="hoisting">提升</a>

  - 變量聲明會提升至作用域頂部，但賦值不會。

    ```javascript
    // 我們知道這樣不能正常工作（假設這裡沒有名為 notDefined 的全局變量）
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 但由於變量聲明提升的原因，在一個變量引用後再創建它的變量聲明將可以正常工作。
    // 注：變量賦值為 `true` 不會提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 解釋器會把變量聲明提升到作用域頂部，意味著我們的例子將被重寫成：
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - 匿名函數表達式會提升它們的變量名，但不會提升函數的賦值。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  - 命名函數表達式會提升變量名，但不會提升函數名或函數體。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 當函數名跟變量名一樣時，表現也是如此。
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - 函數聲明提升它們的名字和函數體。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 瞭解更多信息在 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ 回到頂部](#table-of-contents)**



## <a name="comparison-operators--equality">比較運算符 & 等號</a>

  - 優先使用 `===` 和 `!==` 而不是 `==` 和 `!=`.
  - 條件表達式例如 `if` 語句通過抽象方法 `ToBoolean` 強制計算它們的表達式並且總是遵守下面的規則：

    + **對象** 被計算為 **true**
    + **Undefined** 被計算為 **false**
    + **Null** 被計算為 **false**
    + **布爾值** 被計算為 **布爾的值**
    + **數字** 如果是 **+0、-0 或 NaN** 被計算為 **false**，否則為 **true**
    + **字符串** 如果是空字符串 `''` 被計算為 **false**，否則為 **true**

    ```javascript
    if ([0]) {
      // true
      // 一個數組就是一個對象，對象被計算為 true
    }
    ```

  - 使用快捷方式。

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - 瞭解更多信息在 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ 回到頂部](#table-of-contents)**


## <a name="blocks">塊</a>

  - 使用大括號包裹所有的多行代碼塊。

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function () { return false; }

    // good
    function () {
      return false;
    }
    ```

  - 如果通過 `if` 和 `else` 使用多行代碼塊，把 `else` 放在 `if` 代碼塊關閉括號的同一行。

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ 回到頂部](#table-of-contents)**


## <a name="comments">註釋</a>

  - 使用 `/** ... */` 作為多行註釋。包含描述、指定所有參數和返回值的類型和值。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - 使用 `//` 作為單行註釋。在評論對象上面另起一行使用單行註釋。在註釋前插入空行。

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this.type || 'no type';

      return type;
    }
    ```

  - 給註釋增加 `FIXME` 或 `TODO` 的前綴可以幫助其他開發者快速瞭解這是一個需要複查的問題，或是給需要實現的功能提供一個解決方式。這將有別於常見的註釋，因為它們是可操作的。使用 `FIXME -- need to figure this out` 或者 `TODO -- need to implement`。

  - 使用 `// FIXME:` 標註問題。

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - 使用 `// TODO:` 標註問題的解決方式。

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="whitespace">空白</a>

  - 使用 2 個空格作為縮進。

    ```javascript
    // bad
    function () {
    ∙∙∙∙var name;
    }

    // bad
    function () {
    ∙var name;
    }

    // good
    function () {
    ∙∙var name;
    }
    ```

  - 在大括號前放一個空格。

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - 在控制語句（`if`、`while` 等）的小括號前放一個空格。在函數調用及聲明中，不在函數的參數列表前加空格。

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - 使用空格把運算符隔開。

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - 在文件末尾插入一個空行。

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function (global) {
      // ...stuff...
    })(this);↵
    ```

  - 在使用長方法鏈時進行縮進。使用前面的點 `.` 強調這是方法調用而不是新語句。

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - 在塊末和新語句前插入空行。

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo: function () {
      },
      bar: function () {
      }
    };
    return obj;

    // good
    var obj = {
      foo: function () {
      },

      bar: function () {
      }
    };

    return obj;
    ```


**[⬆ 回到頂部](#table-of-contents)**

## <a name="commas">逗號</a>

  - 行首逗號: **不需要**。

    ```javascript
    // bad
    var story = [
        once
      , upon
      , aTime
    ];

    // good
    var story = [
      once,
      upon,
      aTime
    ];

    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - 額外的行末逗號：**不需要**。這樣做會在 IE6/7 和 IE9 怪異模式下引起問題。同樣，多餘的逗號在某些 ES3 的實現裡會增加數組的長度。在 ES5 中已經澄清了 ([source](http://es5.github.io/#D))：

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="semicolons">分號</a>

  - **使用分號。**

    ```javascript
    // bad
    (function () {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function () {
      var name = 'Skywalker';
      return name;
    })();

    // good (防止函數在兩個 IIFE 合併時被當成一個參數
    ;(function () {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [瞭解更多](http://stackoverflow.com/a/7365214/1712802).

**[⬆ 回到頂部](#table-of-contents)**


## <a name="type-casting--coercion">類型轉換</a>

  - 在語句開始時執行類型轉換。
  - 字符串：

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - 使用 `parseInt` 轉換數字時總是帶上類型轉換的基數。

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - 如果因為某些原因 `parseInt` 成為你所做的事的瓶頸而需要使用位操作解決[性能問題](http://jsperf.com/coercion-vs-casting/3)時，留個註釋說清楚原因和你的目的。

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - **注：** 小心使用位操作運算符。數字會被當成 [64 位值](http://es5.github.io/#x4.3.19)，但是位操作運算符總是返回 32 位的整數（[source](http://es5.github.io/#x11.7)）。位操作處理大於 32 位的整數值時還會導致意料之外的行為。[討論](https://github.com/airbnb/javascript/issues/109)。最大的 32 位整數是 2,147,483,647：

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - 布爾:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="naming-conventions">命名規則</a>

  - 避免單字母命名。命名應具備描述性。

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - 使用駝峰式命名對象、函數和實例。

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var o = {};
    function c() {}

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - 使用帕斯卡式命名構造函數或類。

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - 不要使用下劃線前/後綴。

  > 為什麼？JavaScript 並沒有私有屬性或私有方法的概念。雖然使用下劃線是表示「私有」的一種共識，但實際上這些屬性是完全公開的，它本身就是你公共接口的一部分。這種習慣或許會導致開發者錯誤的認為改動它不會造成破壞或者不需要去測試。長話短說：如果你想要某處為「私有」，它必須不能是顯式提出的。

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';
    ```

  - 不要保存 `this` 的引用。使用 Function#bind。

    ```javascript
    // bad
    function () {
      var self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function () {
      var that = this;
      return function () {
        console.log(that);
      };
    }

    // bad
    function () {
      var _this = this;
      return function () {
        console.log(_this);
      };
    }

    // good
    function () {
      return function () {
        console.log(this);
      }.bind(this);
    }
    ```

  - 給函數命名。這在做堆棧軌跡時很有幫助。

    ```javascript
    // bad
    var log = function (msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **注：** IE8 及以下版本對命名函數表達式的處理有些怪異。瞭解更多信息到 [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/)。

  - 如果你的文件導出一個類，你的文件名應該與類名完全相同。
    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // in some other file
    // bad
    var CheckBox = require('./checkBox');

    // bad
    var CheckBox = require('./check_box');

    // good
    var CheckBox = require('./CheckBox');
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="accessors">存取器</a>

  - 屬性的存取函數不是必須的。
  - 如果你需要存取函數時使用 `getVal()` 和 `setVal('hello')`。

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - 如果屬性是布爾值，使用 `isVal()` 或 `hasVal()`。

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - 創建 get() 和 set() 函數是可以的，但要保持一致。

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function set(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function get(key) {
      return this[key];
    };
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="function Object() { [native code] }s">構造函數</a>

  - 給對象原型分配方法，而不是使用一個新對象覆蓋原型。覆蓋原型將導致繼承出現問題：重設原型將覆蓋原有原型！

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - 方法可以返回 `this` 來實現方法鏈式使用。

    ```javascript
    // bad
    Jedi.prototype.jump = function jump() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function setHeight(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    Jedi.prototype.jump = function jump() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function setHeight(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - 寫一個自定義的 `function toString() { [native code] }()` 方法是可以的，但是確保它可以正常工作且不會產生副作用。

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.function toString() { [native code] } = function function toString() { [native code] }() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="events">事件</a>

  - 當給事件附加數據時（無論是 DOM 事件還是私有事件），傳入一個哈希而不是原始值。這樣可以讓後面的貢獻者增加更多數據到事件數據而無需找出並更新事件的每一個處理器。例如，不好的寫法：

    ```js
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function (e, listingId) {
      // do something with listingId
    });
    ```

    更好的寫法：

    ```js
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function (e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ 回到頂部](#table-of-contents)**


## <a name="modules">模塊</a>

  - 模塊應該以 `!` 開始。這樣確保了當一個不好的模塊忘記包含最後的分號時，在合併代碼到生產環境後不會產生錯誤。[詳細說明](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - 文件應該以駝峰式命名，並放在同名的文件夾裡，且與導出的名字一致。
  - 增加一個名為 `noConflict()` 的方法來設置導出的模塊為前一個版本並返回它。
  - 永遠在模塊頂部聲明 `'use strict';`。

    ```javascript
    // fancyInput/fancyInput.js

    !function (global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="jquery">jQuery</a>

  - 使用 `$` 作為存儲 jQuery 對象的變量名前綴。

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - 緩存 jQuery 查詢。

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - 對 DOM 查詢使用層疊 `$('.sidebar ul')` 或 父元素 > 子元素 `$('.sidebar > ul')`。 [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - 對有作用域的 jQuery 對象查詢使用 `find`。

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="ecmascript-5-compatibility">ECMAScript 5 兼容性</a>

  - 參考 [Kangax](https://twitter.com/kangax/) 的 ES5 [兼容表](http://kangax.github.com/es5-compat-table/).

**[⬆ 回到頂部](#table-of-contents)**


## <a name="testing">測試</a>

  - **Yup.**

    ```javascript
    function () {
      return true;
    }
    ```

**[⬆ 回到頂部](#table-of-contents)**


## <a name="performance">性能</a>

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ 回到頂部](#table-of-contents)**


## <a name="resources">資源</a>


**推薦閱讀**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**工具**

  - Code Style Linters
    + [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**其它風格指南**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)
  - [JavaScript Standard Style](https://github.com/feross/standard)

**其它風格**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**進一步閱讀**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**書籍**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net) - Marijn Haverbeke
  - [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) - Kyle Simpson

**博客**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**播客**

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)


**[⬆ 回到頂部](#table-of-contents)**

## <a name="in-the-wild">誰在使用</a>

  這是一個使用本風格指南的組織列表。給我們發 pull request 或開一個 issue 讓我們將你增加到列表上。

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **General Electric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Nordic Venture Family**: [CodeDistillery/javascript](https://github.com/CodeDistillery/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/javascript)
  - **Super**: [SuperJobs/javascript](https://github.com/SuperJobs/javascript)
  - **SysGarage**: [sysgarage/javascript-style-guide](https://github.com/sysgarage/javascript-style-guide)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## <a name="translation">翻譯</a>

  這份風格指南也提供了其它語言的版本：

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese(Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese(Simplified)**: [sivan/javascript](https://github.com/sivan/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

## <a name="the-javascript-style-guide-guide">JavaScript 風格指南說明</a>

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name="chat-with-us-about-javascript">與我們討論 JavaScript</a>

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## <a name="contributors">貢獻者</a>

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## <a name="license">許可</a>

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ 回到頂部](#table-of-contents)**

# };
