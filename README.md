<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.2/css/all.min.css" integrity="sha512-HK5fgLBL+xu6dm/Ii3z4xhlSUyZgTT9tuc/hSrtw6uzJOvgRr2a9jyxxT1ely+B+xFAmJKVSTbpM/CuL7qxO8w==" crossorigin="anonymous" />
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@200&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        My to do list
    </header>
    <form>
        <input type="text" class="todo-input">
        <button type="submit" class="todo-button">
            <i class="fas fa-plus-square"></i>
        </button>
        <div class="select">
            <select name="todos" class="filter-todo">
                <option value="all">All</option>
                <option value="completed">Completed</option>
                <option value="uncompleted">Uncompleted</option>
            </select>
        </div>
    </form>
    <div class="todo-container">
        <ul class="todo-list"></ul>
    </div>
<script src="app.js"></script>
</body>
</html>


CSS
* {
    margin: 0 0;
    padding: 0 0;
    box-sizing: border-box;
}
body {
    background-image: linear-gradient(120deg, #e2c35d, #d88771);
    color: white;
    font-family: "Poppins", sans-serif;
    min-height: 100vh;
}

header {
    font-size: 1.5rem;
}

header, form {
    min-height: 20vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

form input, form button {
    padding: 0.5rem;
    font-size: 1.5rem;
    background: white;
    border: none;
}

form button {
    color: #d88771;
    background: white;
    cursor: pointer;
    transition: all 0.3s ease;
}

form button:hover {
    background: #d88771;
    color: white;
}

.todo-container {
    display: flex;
    align-items: center;
    justify-content: center;
}

.todo-list {
    min-width: 30%;
    list-style: none;
}

.todo {
    margin: 0.5rem;
    background: white;
    font-size: 1.5rem;
    color: black;
    justify-content: space-between;
    align-items: center;
    display: flex;
    transition: all 1s ease;
}

.todo li {
    flex: 1
}

.trash-btn, .complete-btn {
    background: #ff6f47;
    cursor: pointer;
    padding: 1rem;
    border: none;
    color: white;
    font-size: 1rem;
}

.fall {
    transform: translateY(8rem) rotateZ(20deg);
    opacity: 0;
}

.complete-btn {
    background: rgb(73,274,73);
}

.todo-item {
    padding: 0rem 0.5rem;
}

.completed {
    text-decoration: line-through;
    opacity: 0.5;
}

.fa-check, .fa-trash {
    pointer-events: none;
}

select {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    outline: none;
    border: none;
}

.select{
    margin: 1rem;
    position: relative;
    overflow: hidden;
}

select {
    color: #ff6f47;
    width: 10rem;
    cursor: pointer;
    padding: 1rem;
}

.select::after {
    content: "\25BC";
    position: absolute;
    background: #ff6f47;
    top: 0;
    right: 0;
    padding: 1rem;
    pointer-events: none;
    transition: all 0.3s ease;
}

.select:hover::after {
    background: white;
    color: #ff6f47;
}


jAVA SCRIPT

let todoButton = document.querySelector('.todo-button')
let todoList = document.querySelector('.todo-list')
let todoInput= document.querySelector('.todo-input')
let filterTodo = document.querySelector('.filter-todo')



document.addEventListener('DOMContentLoaded', getTodos)
todoButton.addEventListener('click', addTodo)
todoList.addEventListener('click', deleteCheck)
filterTodo.addEventListener('click', filterTodos)


function addTodo(event) {
    event.preventDefault();
    const todoDiv = document.createElement('div')
    todoDiv.classList.add("todo")
    const newTodo = document.createElement('li')
    newTodo.innerText = todoInput.value
    newTodo.classList.add("todo-item")
    todoDiv.appendChild(newTodo)
    saveLocalTodo(todoInput.value)
    todoInput.value = ""
    const completedButton = document.createElement('button')
    completedButton.innerHTML = '<i class="fas fa-check"></i>'
    completedButton.classList.add('complete-btn')
    todoDiv.appendChild(completedButton)
    //Create TRASH BUTTON
    const trashButton = document.createElement('button')
    trashButton.innerHTML = '<i class="fas fa-trash"></i>'
    trashButton.classList.add('trash-btn')
    todoDiv.appendChild(trashButton)
    //APPEND TO LIST
    todoList.appendChild(todoDiv)
}

function deleteCheck(e) {
    //Trash button
    const item = e.target
    if(item.classList[0] === "trash-btn") {
        const todo = item.parentElement
        //Animation
        todo.classList.add("fall")
        removeTodo(todo)
        todo.addEventListener('transitionend', function() {
            todo.remove()
        })
    }
    //Check button
    if(item.classList[0] === 'complete-btn') {
        const todo = item.parentElement
        todo.classList.toggle("completed")
    }
}

function filterTodos(e) {
    const todos = todoList.childNodes
    todos.forEach(function (todo) {
        console.log(todos)
        switch (e.target.value) {
            case "all":
                todo.style.display = 'flex';
                break;
            case "completed":
                if(todo.classList.contains('completed')) {
                    todo.style.display = 'flex'
                } else {
                    todo.style.display = 'none'
                }
                break;
            case "uncompleted":
                if (!todo.classList.contains('completed')) {
                    todo.style.display = 'flex'
                } else {
                    todo.style.display = 'none'
                }
                break
        }
    })
}

function saveLocalTodo(todo) {
    let todos;
    if (localStorage.getItem("todos") === null) {
        todos = [];
    } else {
        todos = JSON.parse(localStorage.getItem("todos"))
    }

    todos.push(todo)
    localStorage.setItem('todos', JSON.stringify(todos));
}

function getTodos() {
    let todos;
    if (localStorage.getItem("todos") === null) {
        todos = [];
    } else {
        todos = JSON.parse(localStorage.getItem("todos"))
    }
    todos.forEach(function (todo) {
        const todoDiv = document.createElement('div')
        todoDiv.classList.add("todo")
        //Create Li
        const newTodo = document.createElement('li')
        newTodo.innerText = todo
        newTodo.classList.add("todo-item")
        todoDiv.appendChild(newTodo)
        todoInput.value = ""
        //Create MARK BUTTON
        const completedButton = document.createElement('button')
        completedButton.innerHTML = '<i class="fas fa-check"></i>'
        completedButton.classList.add('complete-btn')
        todoDiv.appendChild(completedButton)
        //Create TRASH BUTTON
        const trashButton = document.createElement('button')
        trashButton.innerHTML = '<i class="fas fa-trash"></i>'
        trashButton.classList.add('trash-btn')
        todoDiv.appendChild(trashButton)
        //APPEND TO LIST
        todoList.appendChild(todoDiv)
    })
}

function removeTodo(todo) {
    let todos;
    if (localStorage.getItem("todos") === null) {
        todos = [];
    } else {
        todos = JSON.parse(localStorage.getItem("todos"))
    }
    const todoIndex = todo.children[0].innerText;
    todos.splice(todoIndex.indexOf(todoIndex), 1)
    localStorage.setItem('todos', JSON.stringify(todos))
}







