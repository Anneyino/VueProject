# development viewpoints_SA_bl

标签（空格分隔）： vue

---

#Development View
The development viewpoint describes the architecture that supports the software development process. It aims to provide an overview of the structure of Vue.js for stakeholders like production engineers, software developers and testers. This viewpoints concerns about module organization, conmmon processing,  standardization of design and testing, instrument and codeline organization.
##Module Organization
tbc
##Codeline Models
This section will explain how the directory of Vue.js is structured and coordinated via configuration management and how it is built and tested regularly. The overall structure of the directory hierarchy of Vue.js is organized as follows.

**Table-** *source code structure of Vue.js*
|Directory|Descriptions|
|-|-|
|scripts|contains build-related scripts and configuration files.|
|dist|contains built files for distribution. Note this directory is only updated when a release happens; they do not reflect the latest changes in development branches.|
|flow|contains type declarations for Flow. These declarations are loaded globally and you will see them used in type annotations in normal source code.|
|packages|contains `vue-server-renderer` and `vue-template-compiler`, which are distributed as separate NPM packages. They are automatically generated from the source code and always have the same version with the main vue package.|
|test|contains all tests. The unit tests are written with Jasmine and run with Karma. The e2e tests are written for and run with Nightwatch.js.|
|src|contains the source code, obviously. The codebase is written in ES2015 with Flow type annotations.|
||so|kj|
|server|contains code related to server-side rendering.
|platforms|contains platform-specific code. Entry files for dist builds are located in their respective platform directory.Each platform module contains three parts: `compiler`, `runtime` and `server`, corresponding to the three directories above. Each part contains platform-specific modules/utilities which are then imported and injected to the core counterparts in platform-specific entry files. For example, the code implementing the logic behind `v-bind:class` is in `platforms/web/runtime/modules/class.js` - which is imported in `entries/web-runtime.js` and used to create the browser-specific vdom patching function.|
|sfc|contains single-file component (`*.vue` files) parsing logic. This is used in the `vue-template-compiler` package.|
|shared|contains utilities shared across the entire codebase.|
|types|contains TypeScript type definitions|

Here are the subdirectories of `/src`

**Table-** *source code structure of Vue/src*
|Directory|File|Description|
|-|-|-|
|compiler|| contains code for the template-to-render-function compiler|
||parser|converts template strings to element ASTs|
||optimizer.js|detects static trees for vdom render optimization|
||codegen|generate render function code from element ASTs|
|core||contains universal, platform-agnostic runtime code.|
||observer|contains code related to the reactivity system.|
||vdom|contains code related to vdom element creation and patching.|
||instance| contains Vue instance constructor and prototype methods.|
||global-api| as the name suggests.|
||components|universal abstract components. Currently keep-alive is the only one.|

##Concurrency Viewpoint
The concurrency viewpoint describes the concurrency structure of the system, mapping functional elements to concurrency units to clearly identify the parts of the system that can execute concurrently, and shows how this is coordinated and controlled.This involves defining the parts of the system that can run at the same time and how this is controlled (e.g., defining how the system’s functional elements are packaged into operating system processes and how the processes coordinate their execution). It is depicted via system-level concurrency modesl and state models.

**Figure-** *operating mechanism diagram of Vue.js*
![operating mechanism diagram of Vue.js](http://jbcdn2.b0.upaiyun.com/2018/10/dff999209d238b33e224fa20f0ee606b.png)
When we create a new Vue() in main.js, Vue will call the constructor's _init() method, which is defined in the initMixin() method of `core / instance / index.js` .
A series of initialization settings are made to the current vm instance in the _init() method. The data/props is reconciled when the state method initState(vm) is initialized. This is to set the getter/setter to the object that needs to be responsive by the Object.defineProperty() method. Collection), to achieve the purpose of data changes to drive view changes.

Finally, check if there is an el attribute on vm.\$options, and if so, use the vm.\$mount method to mount vm to form a connection between the data layer and the view layer. This is why you need to manually vm.$mount('#app') if you don't provide the el option.

We see that the created hook is called before the $mount is mounted, so we can't manipulate the DOM until the created hook fires, because it hasn't been rendered to the DOM yet.

As we all know, vue.js is a framework based on MVVC model.
![MVVM model](http://s8.sinaimg.cn/bmiddle/003hjcN4zy76N3fUEIL87&690)

MVVM is divided into three parts: M (Model, model layer), V (View, view layer), VM (ViewModel, V and M connected bridge, can also be seen as a controller)
1. M: model layer, mainly responsible for business data related;
2. V: view layer, as the name suggests, responsible for view correlation, which is subdivided into html and css layer;
3. VM: V and M communication bridge, responsible for monitoring M or V modification, is the key to achieve two-way binding of MVVM;

ViewModel is the core of Vue.js, it is a Vue instance. A Vue instance acts on an HTML element. This element can be either the `<body>` element of HTML or an element with an `<id>` specified.
We see the DOM Listeners and Data Bindings in the above diagram as two tools, which are the key to achieving two-way binding.
From the View side, the DOM Listeners tool in the ViewModel will help us monitor the changes in the DOM elements on the page, and if there are changes, change the data in the Model;
From the Model side, when we update the data in the Model, the Data Bindings tool will help us update the DOM elements in the page.

It has been learned that vue is data binding through data hijacking. The most core method is to implement hijacking of attributes through Object.defineProperty(), to achieve the purpose of monitoring data changes, to achieve two-way binding of mvvm. To determine, you must implement the following:
1. Implement a data listener Observer that can listen to all properties of the data object. If there is a change, you can get the latest value and notify the subscriber 
2.  Implement an instruction parser Compile. Scan and parse the instructions of each element node, replace the data according to the instruction template, and bind the corresponding update function 
3. implement a Watcher, as a bridge connecting Observer and Compile, can subscribe to and receive notification of each attribute change , execute the corresponding callback function bound by the instruction, thereby updating the view 
4. mvvm entry function, and integrating the above three

![MVVM binding](https://ss0.baidu.com/6LVYsjip0QIZ8Aqbn9fN2DC/timg?pa&quality=100&size=b640_10000&sec=1539610034&di=cb9e3d59ca132b8eaa4df322389a33c8&ref=http%3A%2F%2Fwww%2Ebitscn%2Ecom%2Fschool%2FJavascript%2F201612%2F763846%2Ehtml&src=http%3A%2F%2Fimg%2Ebitscn%2Ecom%2Fupimg%2Fallimg%2Fc161216%2F14QUaZ0%2D4AI%2Ejpg)
