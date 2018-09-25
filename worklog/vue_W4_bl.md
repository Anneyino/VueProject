# vue_W4_bl


---

Vue.js is a library for building web interfaces. It provides bindings between data(PJSO, Plain JavaScript Object)and views(DOM objects, or HTML templates). If modify the data, the view is automatically updated.


---
## REACTIVITY
Vue.js allows declarative rendering of data into DOM with a concise template syntax.

```eg
@HTML
<div id="app">
    {{ message }}
</div>
```

```eg
@JAVASCRIPT
var app=new Vue({
    el:'#app',
    data:{
        message:'Hello Vue!'
    }
})
```

Open the browser's JavaScript console and modify the `app.message` value, the above exampe will be updated accordingly.

Vue has converted the object and made it **reactive**.

---
## COMPONENTS
When it comes to structuring complex interfaces, Vue takes an approach that is very similar to React: it’s components all the way down. Let’s make our example a reusable component:

```eg
var Example = Vue.extend({
  template: '<div>{{ message }}</div>',
  data: function () {
    return {
      message: 'Hello Vue.js!'
    }
  }
})

// register it with the tag <example>
Vue.component('example', Example)
```

Now we can use the component in other template simply as a custom element:

```
<example></example>
```

Components can contain other components, and they form a tree that represents your UI. To make them actually composable, Vue components can also:

* Define how it expects to receive data from its parent using `props`;
* Emit custom events to trigger actions in parent scope;
* Compose parent injected content with its own template using `slot`.

```
@HTML
<div id="app-7">
  <ol>
    <!--
      现在我们为每个 todo-item 提供 todo 对象
      todo 对象是变量，即其内容可以是动态的。
      我们也需要为每个组件提供一个“key”，稍后再
      作详细解释。
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id">
    </todo-item>
  </ol>
</div>
```

```
@JS
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: '蔬菜' },
      { id: 1, text: '奶酪' },
      { id: 2, text: '随便其它什么人吃的东西' }
    ]
  }
})
```

result:

> 1. 蔬菜
> 2. 奶酪
> 3. 随便其它什么人吃的东西


In summary, vue.js is an approachable, versatile and performant Javascript framework.












