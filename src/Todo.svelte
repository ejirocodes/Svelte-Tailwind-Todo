<script>
  import { onMount } from 'svelte';

  let todos = [
    {
      title: 'Todo A',
      done: false,
    },
    {
      title: 'Todo B',
      done: true,
    },
    {
      title: 'Todo C',
      done: false,
    },
    {
      title: 'Todo D',
      done: false,
    },
  ];
  let todoItem = '';

  const addTodo = () => {
    // Using push to trigger update
    // todos.push({
    //   title: todoItem,
    //   project: todoItem,
    //   done: false,
    // });
    // todos = todos;

    // Using a more idiomatic solution
    todos = [
      ...todos,
      {
        title: todoItem,
        project: todoItem,
        done: false,
      },
    ];
    todoItem = '';
    console.log({ todos }, { todoItem });
  };
  const deleteTodo = (todo) => {
    // Using filter to trigger update
    todos = todos.filter((to) => to !== todo);
    console.log(todos);

    // Using splice to trigger update
    // const todoIndex = todos.indexOf(todo);
    // todos.splice(todoIndex, 1);
    // console.log({ todos });
    // todos = todos;
  };
  const toggleComplete = (todo) => {
    const todoIndex = todos.indexOf(todo);
    todos[todoIndex].done = !todos[todoIndex].done;
  };

  const updateTodo = (todo) => {
    console.log({ todo });
    console.log({ todos });
  };
</script>

<main>
  <h1 class="p-3 hover:text-red-900">What's up!</h1>
  <p>Today's tasks</p>
  <input type="text" bind:value={todoItem} />
  <button on:click={addTodo}>Add</button>

  <ul>
    {#each todos as todo}
      <li class="todo-item-wrapper">
        <input
          type="checkbox"
          bind:checked={todo.done}
          on:click={toggleComplete(todo)}
        />
        <input
          class:completed={todo.done}
          class="border-0"
          bind:value={todo.title}
          on:change={updateTodo(todo)}
        />
        <span on:click={deleteTodo(todo)}>X</span>
      </li>
    {:else}
      <p>No item in to-do!</p>
    {/each}
  </ul>
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
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
  .completed {
    text-decoration: line-through;
  }
  .todo-item {
  }
</style>
