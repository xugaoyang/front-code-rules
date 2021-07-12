# front-code-rules

前端代码规范。

> 集百家之所长，融百家之所思，扬百家之所名

#### js 规范

##### 注释

##### 空格

##### 逗号

- 禁止前置逗号，eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html)

```javascript
// bad
const story = [once, upon, aTime]

// good
const story = [once, upon, aTime]

// bad
const hero = {
  firstName: 'Ada',
  lastName: 'Lovelace',
  birthYear: 1815,
  superPower: 'computers',
}

// good
const hero = {
  firstName: 'Ada',
  lastName: 'Lovelace',
  birthYear: 1815,
  superPower: 'computers',
}
```

- 需要额外结尾逗号。eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html)

```diff
// bad - 没有结尾逗号的 git diff
const hero = {
      firstName: 'Florence',
-     lastName: 'Nightingale'
+     lastName: 'Nightingale',
+     inventorOf: ['coxcomb chart', 'modern nursing']
};

// good - 有结尾逗号的 git diff
const hero = {
      firstName: 'Florence',
      lastName: 'Nightingale',
+     inventorOf: ['coxcomb chart', 'modern nursing'],
};
```

```javascript
// bad
const hero = {
  firstName: 'Dana',
  lastName: 'Scully',
}

const heroes = ['Batman', 'Superman']

// good
const hero = {
  firstName: 'Dana',
  lastName: 'Scully',
}

const heroes = ['Batman', 'Superman']

// bad
function createHero(firstName, lastName, inventorOf) {
  // does nothing
}

// good
function createHero(firstName, lastName, inventorOf) {
  // does nothing
}

// good (注意，逗号不应出现在使用了 ... 操作符后的参数后面)
function createHero(firstName, lastName, inventorOf, ...heroArgs) {
  // does nothing
}

// bad
createHero(firstName, lastName, inventorOf)

// good
createHero(firstName, lastName, inventorOf)

// good  (注意，逗号不应出现在使用了 ... 操作符后的参数后面)
createHero(firstName, lastName, inventorOf, ...heroArgs)
```

##### 分号

- 默认在行尾不添加分号

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

- 用小驼峰命名法来命名你的对象、函数、实例。eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html)

  ```javascript
  // bad
  const OBJEcttsssss = {}
  const this_is_my_object = {}
  function c() {}

  // good
  const thisIsMyObject = {}
  function thisIsMyFunction() {}
  ```

- 用大驼峰命名法来命名类。eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html)

  ```javascript
  // bad
  function user(options) {
    this.name = options.name
  }

  const bad = new user({
    name: 'nope',
  })

  // good
  class User {
    constructor(options) {
      this.name = options.name
    }
  }

  const good = new User({
    name: 'yup',
  })
  ```

- 不要用前置或后置下划线。eslint: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html)

  > 为什么？JavaScript 没有私有属性或私有方法的概念。尽管前置下划线通常的概念上意味着私有，事实上，这些属性是完全公有的，因此这部分也是你的 API 的内容。这一概念可能会导致开发者误以为更改这个不会导致崩溃或者不需要测试。如果你想要什么东西变成私有，那就不要让它在这里出现。

  ```javascript
  // bad
  this.__firstName__ = 'Panda'
  this.firstName_ = 'Panda'
  this._firstName = 'Panda'

  // good
  this.firstName = 'Panda'
  ```

- 不要保存引用 `this`，用箭头函数或 [函数绑定——Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)。

  ```javascript
  // bad
  function foo() {
    const self = this
    return function () {
      console.log(self)
    }
  }

  // bad
  function foo() {
    const that = this
    return function () {
      console.log(that)
    }
  }

  // good
  function foo() {
    return () => {
      console.log(this)
    }
  }
  ```

- `export default`导出模块 A，则这个文件名也叫`A.\*`， `import`时候的参数也叫`A`。 大小写完全一致。

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

  export default makeStyleGuide
  ```

- 当你 export 一个结构体/类/单例/函数库/对象 时用大驼峰。

  ```javascript
  const AirbnbStyleGuide = {
    es6: {},
  }

  export default AirbnbStyleGuide
  ```

- 简称和缩写应该全部大写或全部小写。

  > 为什么？名字都是给人读的，不是为了去适应计算机算法。

  ```javascript
  // bad
  import SmsContainer from './containers/SmsContainer'

  // bad
  const HttpRequests = [
    // ...
  ]

  // good
  import SMSContainer from './containers/SMSContainer'

  // good
  const HTTPRequests = [
    // ...
  ]

  // also good
  const httpRequests = [
    // ...
  ]

  // best
  import TextMessageContainer from './containers/TextMessageContainer'

  // best
  const requests = [
    // ...
  ]
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

      // 允许但不够语义化

      export const apiKey = 'SOMEKEY';

      // 在大多数情况下更好

      export const API_KEY = 'SOMEKEY';

      // bad - 不必要的大写键，没有增加任何语义

      export const MAPPING = {
        KEY: 'value'
      };

      // good

      export const MAPPING = {
        key: 'value'
      };
      ```

#### css 规范

1. [stylelint](https://stylelint.io/)

#### html 规范

#### vue 规范

- 组件名为多个单词

```
// bad
Vue.component('todo', {
  // ...
})

export default {
  name: 'Todo',
  // ...
}

// good
Vue.component('todo-item', {
  // ...
})

export default {
  name: 'TodoItem',
  // ...
}

```

- 组件数据， 组件的**data**必须是一个函数

```
// bad
Vue.component('some-comp', {
  data: {
    foo: 'bar'
  }
})

export default {
  data: {
    foo: 'bar'
  }
}

// good
Vue.component('some-comp', {
  data: function () {
    return {
      foo: 'bar'
    }
  }
})

export default {
  data () {
    return {
      foo: 'bar'
    }
  }
}
```


- prop定义，尽量详细，至少需要指定类型

```
// bad
props: ['status']

// good
props: {
  status: String
}

// better
props: {
  status: {
    type: String,
    required: true,
    validator: function (value) {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].indexOf(value) !== -1
    }
  }
}
```

- v-for设置键值，总是用 key 配合 v-for

```
// bad
<ul>
  <li v-for="todo in todos">
    {{ todo.text }}
  </li>
</ul>

// good
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

- v-for和v-if避免使用，**永远不要把 v-if 和 v-for 同时用在同一个元素上**

```
// bad
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

// good
// users --> activeUsers,用计算属性筛一次，过滤出显示条件的数据
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

- 组件样式作用域， scoped/module/BEM

```
// bad
<template>
  <button class="btn btn-close">X</button>
</template>

<style>
.btn-close {
  background-color: red;
}
</style>

// good
<template>
  <button class="button button-close">X</button>
</template>

<!-- 使用 `scoped` attribute -->
<style scoped>
.button {
  border: none;
  border-radius: 2px;
}

.button-close {
  background-color: red;
}
</style>

<template>
  <button :class="[$style.button, $style.buttonClose]">X</button>
</template>

<!-- 使用 CSS Modules -->
<style module>
.button {
  border: none;
  border-radius: 2px;
}

.buttonClose {
  background-color: red;
}
</style>

<template>
  <button class="c-Button c-Button--close">X</button>
</template>

<!-- 使用 BEM 约定 -->
<style>
.c-Button {
  border: none;
  border-radius: 2px;
}

.c-Button--close {
  background-color: red;
}
</style>
```

- 单文件组件文件名首字母大写

```
// bad
components/
| - myComponent

// good
components/
| - MyComponent
```

- 紧密耦合的组件名

```
// bad
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue

// good
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue

```

- 完整单词的组件名

```
// bad
components/
|- SdSettings.vue

// good
components/
|- StudentDashboardSettings.vue

```
- 在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。

```
// bad
props: {
  'greeting-text': String
}
<WelcomeMessage greetingText="hi"/>

// good
props: {
  greetingText: String
}
<WelcomeMessage greeting-text="hi"/>
```

- 多个 attribute 的元素应该分多行撰写，每个 attribute 一行

```
// bad
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">

<MyComponent foo="a" bar="b" baz="c"/>

// good
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>

<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```

#### 工具

1. 命名查询：[codeIf](https://unbug.github.io/codelf/)

#### 参考资料：

1. [airbnb](https://github.com/airbnb/javascript)
2. [airbnb 中文版](https://github.com/lin-123/javascript)
3. [prettier](https://prettier.io/docs/en/index.html)
4. [prettier 中文版](https://www.prettier.cn/docs/index.html)
5. [eslint](https://eslint.bootcss.com/)
6. [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)
7. [clean-code-javascript 中文版](https://github.com/alivebao/clean-code-js)
8. [凹凸实验室](https://guide.aotu.io/index.html)
9. [vue风格指南](https://cn.vuejs.org/v2/style-guide/index.html)
