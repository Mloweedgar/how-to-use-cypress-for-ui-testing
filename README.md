### Introduction to End to End and Component Testing with [Cypress](https://www.cypress.io) in Vue

#### Introduction
Is writing tests painful for you? In this article, I am going to explain how to handle UI testing with [Cypress](https://www.cypress.io) and convince you that writing tests is not always so tedious and expensive but fun instead.

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


#### End to End testing

#### Component Testing
#### Next Steps
