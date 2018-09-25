# Vue Instance #

author: zhaoshihan 

studentID:2016302580149

## How to create Vue instance ##

The vue grammar is similar to the javascript, so if we can grasp the knowledge of Html, css, js. It must be easier for us to understand the examples.

Besides, we can download the release version of vue, add to the script src in the <head\> table of html to implement the examples below. For myself, I use the vue.min.js to test the effect.


- according to the documentation, there are several examples about vue instance
	
	>A Vue application consists of a root Vue instance created with new Vue, optionally organized into a tree of nested, reusable components. For example, a todo app’s component tree might look like this:


	```
	根实例
	└─ TodoList
	   ├─ TodoItem
	   │  ├─ DeleteTodoButton
	   │  └─ EditTodoButton
	   └─ TodoListFooter
	      ├─ ClearTodosButton
	      └─ TodoListStatistics
	```
- template method include
	the documentation provides 6 instances to create the basic components
	
	1. the first one use template render for the dynamic website display, like this:

			<div id="app">
				{{ message }}
			</div>
	
			var app = new Vue({
				el: '#app',
				data: {
				message: 'Hello Vue!'
				}
			})

	2. the second one use v-bind table to implement the reactive effect, like this:
	
			<div id="app-2">
			  <span v-bind:title="message">
			    Hover your mouse over me for a few seconds to see my dynamically bound title!
			  </span>
			</div>

			var app2 = new Vue({
			  el: '#app-2',
			  data: {
			    message: 'You loaded this page on ' + new Date().toLocaleString()
			  }
			})

	3. the third one use v-if table to decide whether or not display the div, like css add attribute to certain tables:

			<div id="app-3">
			  <span v-if="seen">Now you see me</span>
			</div>


			var app3 = new Vue({
			  el: '#app-3',
			  data: {
			    seen: true
			  }
			})

	4. the fourth one use todo to render the ordered list, like this:

			<div id="app-4">
			  <ol>
			    <li v-for="todo in todos">
			      {{ todo.text }}
			    </li>
			  </ol>
			</div>

			var app4 = new Vue({
			  el: '#app-4',
			  data: {
			    todos: [
			      { text: 'Learn JavaScript' },
			      { text: 'Learn Vue' },
			      { text: 'Build something awesome' }
			    ]
			  }
			})

	5. the fifth one use js function to manipulate the message, in this case it just like the string function in python, all of them are script language:

			<div id="app-5">
			  <p>{{ message }}</p>
			  <button v-on:click="reverseMessage">Reverse Message</button>
			</div>

			var app5 = new Vue({
			  el: '#app-5',
			  data: {
			    message: 'Hello Vue.js!'
			  },
			  methods: {
			    reverseMessage: function () {
			      this.message = this.message.split('').reverse().join('')
			    }
			  }
			})

	6. the sixth one use v-model label to implement the binding input:

			<div id="app-6">
			  <p>{{ message }}</p>
			  <input v-model="message">
			</div>

			var app6 = new Vue({
			  el: '#app-6',
			  data: {
			    message: 'Hello Vue!'
			  }
			})