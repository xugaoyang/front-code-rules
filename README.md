# front-code-rules
前端代码规范。
> 集百家之所长，融百家之所思，扬百家之所名

#### js规范
##### 命名规范
- 避免用一个字母命名，让你的命名有意义。eslint: [`id-length`](http://eslint.org/docs/rules/id-length)

```javascript
// bad
function q() {
  // ...
}

// good
function query() {
  // ...
}

```
 -  用小驼峰命名法来命名你的对象、函数、实例。eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```
  - 用大驼峰命名法来命名类。eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```
  - 不要用前置或后置下划线。eslint: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html)

    > 为什么？JavaScript 没有私有属性或私有方法的概念。尽管前置下划线通常的概念上意味着私有，事实上，这些属性是完全公有的，因此这部分也是你的 API 的内容。这一概念可能会导致开发者误以为更改这个不会导致崩溃或者不需要测试。如果你想要什么东西变成私有，那就不要让它在这里出现。

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';
    ```

  - 不要保存引用 `this`，用箭头函数或 [函数绑定——Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)。

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```
  - export default` 导出模块A，则这个文件名也叫 `A.*`， `import` 时候的参数也叫 `A`。 大小写完全一致。

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // bad
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // bad
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // good
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  - 当你 export-default 一个函数时，函数名用小驼峰，文件名需要和函数名一致。

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide;
    ```

  - 当你 export 一个结构体/类/单例/函数库/对象 时用大驼峰。

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```

  - 简称和缩写应该全部大写或全部小写。

    > 为什么？名字都是给人读的，不是为了去适应计算机算法。

    ```javascript
    // bad
    import SmsContainer from './containers/SmsContainer';

    // bad
    const HttpRequests = [
      // ...
    ];

    // good
    import SMSContainer from './containers/SMSContainer';

    // good
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const requests = [
      // ...
    ];
    ```

  - 你可以用全大写字母设置静态变量，他需要满足三个条件。

    1. 导出变量；
    1. 是 `const` 定义的， 保证不能被改变；
1. 这个变量是可信的，他的子属性都是不能被改变的。

    > 为什么？这是一个附加工具，帮助开发者去辨识一个变量是不是不可变的。UPPERCASE_VARIABLES 能让开发者知道他能确信这个变量（以及他的属性）是不会变的。

    - 对于所有的 `const` 变量呢？ —— 这个是不必要的。大写变量不应该在同一个文件里定义并使用， 它只能用来作为导出变量。
- 那导出的对象呢？ —— 大写变量处在 `export` 的最高级(例如：`EXPORTED_OBJECT.key`) 并且他包含的所有子属性都是不可变的。（译者注：即导出的变量是全大写的，但他的属性不用大写）

    ```javascript
    // bad
const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

    // bad
export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

    // bad
export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

    ```

// ---

    // 允许但不够语义化
export const apiKey = 'SOMEKEY';

    // 在大多数情况下更好
export const API_KEY = 'SOMEKEY';

// ---

    // bad - 不必要的大写键，没有增加任何语义
    export const MAPPING = {
      KEY: 'value'
};

    // good
    export const MAPPING = {
      key: 'value'
    };
    ```


#### css规范
1. [stylelint](https://stylelint.io/)


#### html规范

#### 命名规则

#### 工具
1. 命名查询：[codeIf](https://unbug.github.io/codelf/)





#### 参考资料：
1. [airbnb](https://github.com/airbnb/javascript)
2. [airbnb中文版](https://github.com/lin-123/javascript)
3. [prettier](https://prettier.io/docs/en/index.html)
4. [prettier中文版](https://www.prettier.cn/docs/index.html)
5. [eslint](https://eslint.bootcss.com/)
6. [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)
7. [clean-code-javascript中文版](https://github.com/alivebao/clean-code-js)
8. [凹凸实验室](https://guide.aotu.io/index.html)
