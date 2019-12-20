<script>
  let value = "world";

  const showAlert = () => {
    alert('押されました');
  }

  // ここからTODO
  import { afterUpdate } from "svelte";

  let newTodo = '';
  let todos = [];

  // 読み込み完了後にwatch
  afterUpdate(() => {
    const storage = localStorage.getItem('todos')
    if (storage) {
      todos = JSON.parse(localStorage.getItem('todos'));
    }
  });

  // todosにタスクを追加
  const addTodo = () => {
    if (newTodo.trim() === '') return;
    // svelteでは、破壊的メソッドで直接値を上書きすることはできない。
    // x push()
    todos = [...todos, {name: newTodo, compFlag: false}];
    localStorage.setItem('todos', JSON.stringify(todos))
    newTodo = '';
  }

  const toggleDone = (index) => {
    todos[index].compFlag = !todos[index].compFlag
    localStorage.setItem('todos', JSON.stringify(todos))
  }

  // todosからタスクを削除
  const removeTodo = (index) => {
    todos = todos.filter((_, i) => i !== index);
    localStorage.setItem('todos', JSON.stringify(todos))
  }

</script>

<style>
section {
  min-height: 150px;
}
.is-done {
  text-decoration: line-through;
}
</style>

<h1>svelte</h1>
<!--
<h2>bind</h2>
<p><input type="text" bind:value={value}>{value}</p>

<h2>on</h2>
<button on:click={showAlert}>click</button>
 -->
<h2>TODO</h2>
<section>
  <form on:submit|preventDefault={addTodo}>
    <input type="text" bind:value={newTodo}>
    <button type="submit">ADD TODO</button>
  </form>
  <ul>
    {#each todos as todo, index}
      <li>
        <span class:is-done={todo.compFlag}>
          {todo.name}
        </span>
        <input
          type="checkbox"
          bind:checked={todo.compFlag}
          on:click={() => toggleDone(index)}
        />
        {#if todo.compFlag}
          <span on:click={() => removeTodo(index)}>[x]</span>
        {/if}
      </li>
    {/each}
  </ul>
</section>
