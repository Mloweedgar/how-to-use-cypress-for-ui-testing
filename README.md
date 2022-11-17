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
https://www.digitalocean.com/community/markdown#?md=%23%23%23+How+to+write+End+to+End+and+Component+Tests+with+%5BCypress%5D%28https%3A%2F%2Fwww.cypress.io%29+in+Vue%0A%0A%23%23%23%23+Introduction%0AIs+writing+tests+painful+for+you%3F+In+this+tutorial%2C+I+am+going+to+explain+how+to+handle+UI+testing+with+%5BCypress%5D%28https%3A%2F%2Fwww.cypress.io%29+and+convince+you+that+writing+tests+is+not+always+so+tedious+and+expensive+but+fun+instead.%0A%0A%5BCypress%5D%28https%3A%2F%2Fwww.cypress.io%29+is+a+purely+**JavaScript-based+front-end+testing+tool+built+for+the+modern+web**.+it+can+test+anything+that+runs+in+browser+and+has+built+in+support+for+testing+modern+frameworks+such+as+Vue%2C+React+and+Angular.+see+a+full-list+of+all+supported+front-end+frameworks+%5Bhere%5D%28https%3A%2F%2Fdocs.cypress.io%2Fguides%2Fcomponent-testing%2Foverview%23Supported-Frameworks%29.%0A%0AWe+are+going+to+use+a+Todo+app+built+using+Vue+as+an+example%2C+through+that+we+are+going+to+learn%3B%0A%0A*+How+to+Install+and+Setup+Cypress.%0A*+Creating+a+simple+Todo+App+with+Vue+3%0A*+How+to+write+End+to+End+test.%0A*+How+to+write+component+tests.%0A%0A%0A%0A%23%23%23%23++How+to+Install+and+Setup+Cypress%0A1.+First+thing+first+lets+create+new+Vue+project+using+Vue+CLI.%0AInstall+Vue+CLI+if+you+don%27t+have+it+in+your+machine%3A%0A%0A%60%60%60bash%0A%0Anpm+install+-g+%40vue%2Fcli%0A%0A%60%60%60%0A%0A2.+create+a+project%28pick+%60Vue+3%2Cbabel%2Ceslint%60+preset+%29%3A%0A%0A%60%60%60bash%0A%0Avue+create+todo-app%0A%0A%60%60%60%0A3.+%60cd%60+into+the+**todo-app**+project+and+Install+cypress+with+only+one+command%3A%0A_There+are+no+servers%2C+drivers%2C+or+any+other+dependencies+to+install+or+configure.+You+can+write+your+first+passing+test+in+60+seconds%21_%0A%0A%60%60%60bash%0A+npm+install+cypress+--save-dev%0A%0A%60%60%60%0A%0A3.+Edit+package.json+file+in+scripts+section+add+a+command+%60%22cypress%3Aopen%22%3A+%22cypress+open%22%60.+see+example+below.%0A%0A%60%60%60json%0A%0A%22scripts%22%3A+%7B%0A++++%22cypress%3Aopen%22%3A+%22cypress+open%22%0A++%7D%2C%0A%0A%60%60%60%0A4.+Launch+cypress%3A%0A%0A%60%60%60bash%0A%0A+npm+run+cypress%3Aopen%0A%0A%60%60%60%0AUse+the+Launch+pad+to+configure+both+E2E+Testing+and+Component+Testing.+if+you+get+stuck+please+visit+this+%5Blink+here%5D%28https%3A%2F%2Fdocs.cypress.io%2Fguides%2Fgetting-started%2Fopening-the-app%23The-Launchpad%29+to+cypress+official+docs+on+how+to+configure+cypress.+%0A%0A%0A%0A%0A%23%23%23%23+Create+a+Todo+App+with+Vue+3%0AOpen+**todo-app**+project+in+your+favourite+Editor+and+add+the+following+single+file+components%3A%0A1.+Add+BaseTextInput+Component+%60src%2Fcomponents%2FBaseTextInput.vue%60+%0A%0A%60%60%60javaScript%0A%0A%3Ctemplate%3E%0A++++%3Cform+class%3D%22add-todo-form%22%3E%0A++++++++%3Cinput+type%3D%22text%22+class%3D%22input%22+placeholder%3D%22Add+a+new+todo%22+v-model%3D%22todo%22%3E%0A++++++++%3Cinput+type%3D%22submit%22+value%3D%22Add%22+%40click%3D%22emitNewTodoEvent%22%3E%0A++++%3C%2Fform%3E%0A%3C%2Ftemplate%3E%0A++%0A%3Cscript%3E%0Aexport+default+%7B%0A++++data%28%29+%7B%0A++++++++return+%7B%0A++++++++++++todo%3A+%27%27%0A++++++++%7D%0A++++%7D%2C%0A++++methods%3A+%7B%0A++++++++emitNewTodoEvent%28e%29+%7B%0A++++++++++++e.preventDefault%28%29%3B%0A++++++++++++this.%24emit%28%27newTodo%27%2C+this.todo%29%3B%0A++++++++++++this.todo+%3D+%27%27%3B%0A++++++++%7D%0A++++%7D%0A%7D%0A%3C%2Fscript%3E%0A++%0A%3Cstyle+scoped%3E%0A.input+%7B%0A++++width%3A+100%25%3B%0A++++padding%3A+8px+10px%3B%0A++++border%3A+1px+solid+%2332485F%3B%0A%7D%0A%0A.add-todo-form+%7B%0A++++width%3A+100%25%3B%0A++++display%3A+flex%3B%0A++++align-items%3A+center%3B%0A%7D%0A%0Ainput%5Btype%3D%22submit%22%5D+%7B%0A++++margin-left%3A+5px%3B%0A++++padding%3A+8px+10px%3B%0A++++border%3A+1px+solid+%2332485F%3B%0A++++background-color%3A+%2332485F%3B%0A++++color%3A+%23fff%3B%0A++++font-weight%3A+bold%3B%0A++++cursor%3A+pointer%3B%0A%7D%0A%0Ainput%5Btype%3D%22submit%22%5D%3Ahover+%7B%0A++++background-color%3A+%2300C185%3B%0A%7D%0A%3C%2Fstyle%3E%0A++%0A%0A%60%60%60+%0A+BaseTextInput+will+contain+a+text+input+and+a+button+for+adding+new+todos.%0A%0A2.+Add+TodoListItem+Component+%60src%2Fcomponents%2FTodoListItem.vue%60%0A%0A%60%60%60javaScript%0A%0A%3Ctemplate%3E%0A++++%3Cli%3E%0A++++++%7B%7B+todo.text+%7D%7D%0A++++++%3Cbutton+%40click%3D%22%24emit%28%27remove%27%2C+todo.id%29%22%3E%0A++++++++X%0A++++++%3C%2Fbutton%3E%0A++++%3C%2Fli%3E%0A++%3C%2Ftemplate%3E%0A++%0A++%3Cscript%3E%0A++export+default+%7B%0A++++props%3A+%7B%0A++++++todo%3A+%7B%0A++++++++type%3A+Object%2C%0A++++++++required%3A+true%0A++++++%7D%0A++++%7D%0A++%7D%0A++%3C%2Fscript%3E%0A%0A%60%60%60%0ATodoListItem+displays+a+todo+along+with+a+button+for+removing+that+Todo%0A%0A%0A3.+Add+TodoList+component+%60src%2Fcomponents%2FTodoList.vue%60%0A%0A%60%60%60javaScript%0A%0A%3Ctemplate%3E%0A%09%3Cdiv%3E%0A%09%09%3CBaseTextInput+%0A%09%09%09%40newTodo%3D%22addTodo%22%0A%09%09%2F%3E%0A%09%09%3Cul+v-if%3D%22todos.length%22%3E%0A%09%09%09%3CTodoListItem%0A%09%09%09%09v-for%3D%22todo+in+todos%22%0A%09%09%09%09%3Akey%3D%22todo.id%22%0A%09%09%09%09%3Atodo%3D%22todo%22%0A%09%09%09%09%40remove%3D%22removeTodo%22%0A%09%09%09%2F%3E%0A%09%09%3C%2Ful%3E%0A%09%09%3Cp+v-else%3E%0A%09%09%09Nothing+left+in+the+list.+Add+a+new+todo+in+the+input+above.%0A%09%09%3C%2Fp%3E%0A%09%3C%2Fdiv%3E%0A%3C%2Ftemplate%3E%0A%0A%3Cscript%3E%0Aimport+BaseTextInput+from+%27.%2FBaseTextInput.vue%27%0Aimport+TodoListItem+from+%27.%2FTodoListItem.vue%27%0A%0Alet+nextTodoId+%3D+1%0A%0Aexport+default+%7B%0A%09components%3A+%7B%0A%09%09BaseTextInput%2C+TodoListItem%0A%09%7D%2C%0A++data+%28%29+%7B%0A++++return+%7B%0A++++++todos%3A+%5B%0A%09%09%09%09%7B%0A%09%09%09%09%09id%3A+nextTodoId%2B%2B%2C%0A%09%09%09%09%09text%3A+%27Tests+are+fun%27%0A%09%09%09%09%7D%2C%0A%09%09%09%09%7B%0A%09%09%09%09%09id%3A+nextTodoId%2B%2B%2C%0A%09%09%09%09%09text%3A+%27Write+tests%27%0A%09%09%09%09%7D%2C%0A%09%09%09%09%7B%0A%09%09%09%09%09id%3A+nextTodoId%2B%2B%2C%0A%09%09%09%09%09text%3A+%27Write+more+tests%27%0A%09%09%09%09%7D%0A%09%09%09%5D%0A++++%7D%0A++%7D%2C%0A%09methods%3A+%7B%0A%09%09addTodo+%28todo%29+%7B%0A++++++++++++%0A%09%09%09const+trimmedText+%3D+todo.trim%28%29%0A%09%09%09if+%28trimmedText%29+%7B%0A%09%09%09%09this.todos.unshift%28%7B%0A%09%09%09%09%09id%3A+nextTodoId%2B%2B%2C%0A%09%09%09%09%09text%3A+trimmedText%0A%09%09%09%09%7D%29%0A%09%09%09%0A%09%09%09%7D%0A%09%09%7D%2C%0A%09%09removeTodo+%28idToRemove%29+%7B%0A%09%09%09this.todos+%3D+this.todos.filter%28todo+%3D%3E+%7B%0A%09%09%09%09return+todo.id+%21%3D%3D+idToRemove%0A%09%09%09%7D%29%0A%09%09%7D%0A%09%7D%0A%7D%0A%3C%2Fscript%3E%0A%0A%60%60%60+%0A%0ATodoList+component+lists+all+todos.%0A%0A4.+Edit+App+component+%60src%2FApp.vue%60+to+include+code+that+will+render+the+Todo+App.%0A%0A%60%60%60javaScript%0A%0A%3Ctemplate%3E%0A%09%3Cdiv+id%3D%22app%22%3E%0A%09%09%3Ch1%3EMy+Todo+App%21%3C%2Fh1%3E%0A%09%09%3CTodoList%2F%3E%0A%09%3C%2Fdiv%3E%0A%3C%2Ftemplate%3E%0A%0A%3Cscript%3E%0Aimport+TodoList+from+%27.%2Fcomponents%2FTodoList.vue%27%0A%0Aexport+default+%7B%0A%09components%3A+%7B%0A%09%09TodoList%0A%09%7D%0A%7D%0A%3C%2Fscript%3E%0A%0A%3Cstyle%3E%0A%0A*%2C+*%3A%3Abefore%2C+*%3A%3Aafter+%7B%0A%09box-sizing%3A+border-box%3B%0A%7D%0A%0A%23app+%7B%0A%09max-width%3A+400px%3B%0A%09margin%3A+0+auto%3B%0A%09line-height%3A+1.4%3B%0A%09font-family%3A+%27Avenir%27%2C+Helvetica%2C+Arial%2C+sans-serif%3B%0A%09-webkit-font-smoothing%3A+antialiased%3B%0A%09-moz-osx-font-smoothing%3A+grayscale%3B%0A%09color%3A+%2300C185%3B%0A%7D%0A%0Ah1+%7B%0A%09text-align%3A+center%3B%0A%7D%0A%3C%2Fstyle%3E%0A%0A%60%60%60%0A%0ALastly+run%3A%0A%60%60%60bash%0A%0Anpm+run+serve%0A%0A%60%60%60%0Aand+visit+%5Bhttp%3A%2F%2Flocalhost%3A8080%5D%28http%3A%2F%2Flocalhost%3A8080%29+to+view+your+todo+App%21%0A%0A%0A%23%23%23%23+End+to+End+testing%0A%0A%23%23%23%23+Component+Testing%0A%23%23%23%23+Next+Steps%0A

#### Component Testing
#### Next Steps
