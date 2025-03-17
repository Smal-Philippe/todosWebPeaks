<script>
    import Cell from "./cell.svelte";
    import Icon from "./icon.svelte";
    import Priority from "./priority.svelte";
    import { createEventDispatcher } from "svelte";

    export let todo;
    let old_priority = todo.priority
    const dispatch = createEventDispatcher();

    const toggle_status = () => {
        todo.done = !todo.done
        item_change("update")
    }

    const item_change = (type) => {
        dispatch("change", {type: type, id: todo.id})
    }

    const edit_task = () => {
        document.getElementById(todo.id).blur()
        item_change("update")
    }

    $: {
        if (todo.priority != old_priority) {
            old_priority = todo.priority;
            item_change("update")
        }
    }

</script>

<Cell>
    {todo.id}
</Cell>
<Cell>
    {#if !todo.done}
        <Icon handler={toggle_status} nome={"circle"} color={"red"}/>
    {:else}
        <Icon handler={toggle_status} nome={"task_alt"}/>
    {/if}
    
</Cell>
<Cell>
    <input type="text"
            class="todo-item-input-text {todo.done ? "text-done" : ""}"
            id={todo.id}
            placeholder="Inserisci il nuovo ToDo"
            bind:value={todo.task} 
            on:change={edit_task} />
</Cell>
<Cell>
    <Priority disabled={todo.done} bind:prio={todo.priority}/>
</Cell>
<Cell last>
    <Icon nome="delete_forever" handler = {() => item_change("delete")} color={"red"}/>
</Cell>

<style>
    .todo-item-input-text {
        border: none;
        width: 90%;
        height: 30px;
        font-size: 20px;
        color: #525252;
    }

    .todo-item-input-text:focus {
        background-color: rgb(89, 155, 216);
        color: rgb(78, 67, 67);
        padding: 4px;
        padding-left: 10px;
    }

    .text-done {
        text-decoration: line-through;
        opacity: 0.3;
    }
</style>