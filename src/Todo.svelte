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

<section class="rounded-3xl ">
  <h1 class="">Hey there!</h1>
  <p>Today's Goal</p>
  <input type="text" bind:value={todoItem} />
  <button on:click={addTodo}>Add</button>

  <ul>
    {#each todos as todo}
      <li class="rounded-3xl bg-black bg-opacity-10	p-5 mb-4">
        <input
          type="checkbox"
          bind:checked={todo.done}
          on:click={toggleComplete(todo)}
        />
        <input
          class:completed={todo.done}
          class="border-0 bg-transparent text-white"
          bind:value={todo.title}
          on:change={updateTodo(todo)}
        />
        <button on:click={deleteTodo(todo)}>X</button>
      </li>
    {:else}
      <p>No goals for today!</p>
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
</section>

<style>
  .completed {
    text-decoration: line-through;
  }
  section {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
    background-color: #2073dd;
  }
  @media (min-width: 640px) {
    section {
      max-width: 400px;
    }
  }
</style>
