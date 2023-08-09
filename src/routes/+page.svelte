<script lang='ts'>
	import { onMount } from "svelte"

    let todos: any[] = []
    let todoText = ''

    onMount(() => {
        const exsitingTodos = localStorage.getItem('todos')
        todos = exsitingTodos !== null ? JSON.parse(exsitingTodos) : []
    })

    function addTodo() {
        const newTodo = {id: Date.now(), text: todoText}
        todos = [...todos, newTodo]
        localStorage.setItem('todos', JSON.stringify(todos))
        todoText = ''
    }

    function removeTodo(todo: { id: any; }) {
        todos = todos.filter(t => t.id !== todo.id)
        localStorage.setItem('todos', JSON.stringify(todos))
    }
</script>

<header>
    <h1>Todo List</h1>
</header>
<main>
    <ul>
        {#each todos as todo (todo.id)}
            <li>{todo.text} <button on:click={() => removeTodo(todo)}>❌</button></li>
        {/each}
    </ul>
    <form on:submit|preventDefault={addTodo}>
        <input type='text' bind:value={todoText} placeholder='What do you need to do?'>
        <button type='submit'>Add Todo</button>
    </form>
</main>
<footer>
    <p>Created by <a href="https://golaughlin.github.io/" target='_blank'>Gage O'Laughlin</a></p>
</footer>