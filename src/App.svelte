<script>
  export let name;
  let todos = [
    {
      title: 'Todo A',
      project: 'Project A',
      done: false,
    },
    {
      title: 'Todo B',
      project: 'Project B',
      done: true,
    },
    {
      title: 'Todo C',
      project: 'Project C',
      done: false,
    },
    {
      title: 'Todo D',
      project: 'Project D',
      done: false,
    },
  ];
  let todoItem = '';
  const addTodo = () => {
    todos.push(todoItem);
    console.log({ todos }, { todoItem });
  };
</script>

<main>
  <h1>Hello {name}!</h1>
  <p>
    Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p>
  <input type="text" bind:value={todoItem} />
  <button on:click={addTodo}>Add</button>
  <p>
    Completed Tasks: {todos.filter((todo) => {
      return todo.done;
    }).length}
  </p>
  <p>
    Pending Tasks: {todos.filter((todo) => {
      return todo.done === false;
    }).length}
  </p>
  <ul>
    {#each todos as todo}
      <li>{todo.title}</li>
    {:else}
      <p>No item in to-do!</p>
    {/each}
  </ul>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
