## Vue Frame ##

The main functions of Vue Frame are completed in the directory named dist in the project. Ever since Vue2.2, vue.js in the dist has been seperated into 8 similar JavaScript files such as `Vue.common.js`, `Vue.esm.js` and `Vue.js`. However their main functions remain the same. All these files are distinguished by their construction methods: Completed Construction and Runtime Construction. Every Web project can make use of the frames provided by dist to render their page, a simple use case is as the following:

```
<!DOCTYPE html>

<html lang="en">

  <head>

    <meta charset="utf-8">

    <title>Vue.js markdown editor example</title>

    <link rel="stylesheet" href="style.css">

    <script src="https://unpkg.com/marked@0.3.6"></script>

    <script src="https://unpkg.com/lodash@4.16.0"></script>

    <!-- Delete ".min" for console warnings in development -->

    <script src="../../dist/vue.min.js"></script>

  </head>

  <body>



    <div id="editor">

      <textarea :value="input" @input="update"></textarea>

      <div v-html="compiledMarkdown"></div>

    </div>



    <script>

      new Vue({

        el: '#editor',

        data: {

          input: '# hello'

        },

        computed: {

          compiledMarkdown: function () {

            return marked(this.input, { sanitize: true })

          }

        },

        methods: {

          update: _.debounce(function (e) {

            this.input = e.target.value

          }, 300)

        }

      })

    </script>



  </body>

</html>
```

As you can see, in the line 19, this case refers the vue.min.js in the dist to render the page. In the `body` of the `HTML` code, we firstly define a div style whose id equals "editor". Then we initialize a instance of Vue including element, data and some functions. Since Vue belongs to responsive system, whenever these components change, the viewGrid will respond, that is, matching those new values of these components. We have already known that each Vue project begins with creating a new instance of Vue by function `Vue`. Therefore we can say that the whole project is completed during the lifecycle of Vue instance. In the open-source project Vue from the GitHub, we can find it that in the dist directory, every vue.js define `function initLifecycle (vm)` clearly to initialize the lifecycle of the instances, and `vm` is short for ViewModel which means the instance of Vue. The code of function is as the following:

```
function initLifecycle (vm) {
  var options = vm.$options;

  // locate first non-abstract parent
  var parent = options.parent;
  if (parent && !options.abstract) {
    while (parent.$options.abstract && parent.$parent) {
      parent = parent.$parent;
    }
    parent.$children.push(vm);
  }

  vm.$parent = parent;
  vm.$root = parent ? parent.$root : vm;

  vm.$children = [];
  vm.$refs = {};

  vm._watcher = null;
  vm._inactive = null;
  vm._directInactive = false;
  vm._isMounted = false;
  vm._isDestroyed = false;
  vm._isBeingDestroyed = false;
}
```
Above this is just a beginning of the Vue project. The completed the lifecycle includes init, create, mount, update and destroy. As you can see in the following picture.

![lifecycle.png-48.8kB][1]

We can quickly build our web project by Vue Frame, which not only supprots `JavaScript`, `HTML`, but also allows more high-level language like  `TypeScript` to make it easy for developers to make pertinency choices.   


  [1]: http://static.zybuluo.com/Anneyino/hlmgs7hj8d7h9udjyzm6pzjb/lifecycle.png