### How to write End to End and Component Tests with [Cypress](https://www.cypress.io) in Vue

#### Introduction
Is writing tests painful for you? In this tutorial, I am going to explain how to handle UI testing with [Cypress](https://www.cypress.io) and convince you that writing tests is not always so tedious and expensive but fun instead.

[Cypress](https://www.cypress.io) is a purely **JavaScript-based front-end testing tool built for the modern web**. it can test anything that runs in browser and has built in support for testing modern frameworks such as Vue, React and Angular. see a full-list of all supported front-end frameworks [here](https://docs.cypress.io/guides/component-testing/overview#Supported-Frameworks).

We are going to use a Todo app built using Vue as an example, through that we are going to learn;

* How to Install and Setup Cypress.
* Creating a simple Todo App with Vue 3
* How to write End to End test.
* How to write component tests.



####  How to Install and Setup Cypress
1. First thing first lets create new Vue project using Vue CLI.
Install Vue CLI if you don't have it in your machine:

```bash

npm install -g @vue/cli

```

2. create a project(pick `Vue 3,babel,eslint` preset ):

```bash

vue create todo-app

```
3. `cd` into the **todo-app** project and Install cypress with only one command:
_There are no servers, drivers, or any other dependencies to install or configure. You can write your first passing test in 60 seconds_

```bash
 npm install cypress --save-dev

```

3. Edit package.json file in scripts section add a command `"cypress:open": "cypress open"`. see example below.

```json

"scripts": {
    "cypress:open": "cypress open"
  },

```
4. Launch cypress:

```bash

 npm run cypress:open

```
Use the Launch pad to configure both E2E Testing and Component Testing. if you get stuck please visit this [link here](https://docs.cypress.io/guides/getting-started/opening-the-app#The-Launchpad) to cypress official docs on how to configure cypress. 




#### Create a Todo App with Vue 3
Open **todo-app** project in your favourite Editor and add the following single file components:
1. Add BaseTextInput Component `src/components/BaseTextInput.vue` 

```javaScript

<template>
    <form class="add-todo-form">
        <input type="text" class="input" placeholder="Add a new todo" v-model="todo">
        <input type="submit" value="Add" @click="emitNewTodoEvent">
    </form>
</template>
  
<script>
export default {
    data() {
        return {
            todo: ''
        }
    },
    methods: {
        emitNewTodoEvent(e) {
            e.preventDefault();
            this.$emit('newTodo', this.todo);
            this.todo = '';
        }
    }
}
</script>
  
<style scoped>
.input {
    width: 100%;
    padding: 8px 10px;
    border: 1px solid #32485F;
}

.add-todo-form {
    width: 100%;
    display: flex;
    align-items: center;
}

input[type="submit"] {
    margin-left: 5px;
    padding: 8px 10px;
    border: 1px solid #32485F;
    background-color: #32485F;
    color: #fff;
    font-weight: bold;
    cursor: pointer;
}

input[type="submit"]:hover {
    background-color: #00C185;
}
</style>
  

``` 
 BaseTextInput will contain a text input and a button for adding new todos.

2. Add TodoListItem Component `src/components/TodoListItem.vue`

```javaScript

<template>
    <li>
      {{ todo.text }}
      <button @click="$emit('remove', todo.id)">
        X
      </button>
    </li>
  </template>
  
  <script>
  export default {
    props: {
      todo: {
        type: Object,
        required: true
      }
    }
  }
  </script>

```
TodoListItem displays a todo along with a button for removing that Todo


3. Add TodoList component `src/components/TodoList.vue`

```javaScript

<template>
	<div>
		<BaseTextInput 
			@newTodo="addTodo"
		/>
		<ul v-if="todos.length">
			<TodoListItem
				v-for="todo in todos"
				:key="todo.id"
				:todo="todo"
				@remove="removeTodo"
			/>
		</ul>
		<p v-else>
			Nothing left in the list. Add a new todo in the input above.
		</p>
	</div>
</template>

<script>
import BaseTextInput from './BaseTextInput.vue'
import TodoListItem from './TodoListItem.vue'

let nextTodoId = 1

export default {
	components: {
		BaseTextInput, TodoListItem
	},
  data () {
    return {
      todos: [
				{
					id: nextTodoId++,
					text: 'Tests are fun'
				},
				{
					id: nextTodoId++,
					text: 'Write tests'
				},
				{
					id: nextTodoId++,
					text: 'Write more tests'
				}
			]
    }
  },
	methods: {
		addTodo (todo) {
            
			const trimmedText = todo.trim()
			if (trimmedText) {
				this.todos.unshift({
					id: nextTodoId++,
					text: trimmedText
				})
			
			}
		},
		removeTodo (idToRemove) {
			this.todos = this.todos.filter(todo => {
				return todo.id !== idToRemove
			})
		}
	}
}
</script>

``` 

TodoList component lists all todos.

4. Edit App component `src/App.vue` to include code that will render the Todo App.

```javaScript

<template>
	<div id="app">
		<h1>My Todo App!</h1>
		<TodoList/>
	</div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
	components: {
		TodoList
	}
}
</script>

<style>

*, *::before, *::after {
	box-sizing: border-box;
}

#app {
	max-width: 400px;
	margin: 0 auto;
	line-height: 1.4;
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #00C185;
}

h1 {
	text-align: center;
}
</style>

```

Lastly run:
```bash

npm run serve

```
and visit [http://localhost:8080](http://localhost:8080) to view your todo App!


#### End to End testing

#### Component Testing
#### Next Steps
