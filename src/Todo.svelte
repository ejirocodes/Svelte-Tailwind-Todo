<script>
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
    console.log(todos);
  };
</script>

<main>
  <h1 class="p-3 hover:text-red-900">What's up!</h1>
  <p>Today's tasks</p>
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
      <li class="todo-item" on:click={toggleComplete(todo)}>
        <input
          type="checkbox"
          class="form-checkbox text-indigo-600"
          bind:checked={todo.done}
        />
        <span class:completed={todo.done}>{todo.title} </span>
        <span on:click={deleteTodo(todo)}>X</span>
      </li>
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
  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
  .completed {
    text-decoration: line-through;
  }
  @keyframes strike {
    from {
      text-decoration-color: transparent;
    }
    to {
      text-decoration-color: auto;
    }
  }
  .todo-item {
    /* transition: all cubic-bezier(0.175, 0.885, 0.32, 1.275) 0.2s; */
  }
  .todo-item:hover {
    text-decoration: line-through;
    animation: strike 0.2s linear;
  }
</style>
