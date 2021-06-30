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
    if (!todoItem) {
      return;
    }
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

<section class="rounded-3xl text-white py-10 px-6">
  <h1 class="text-4xl	font-bold mb-5">Hey there!</h1>
  <p class="text-2xl	mb-2">Today's Goal</p>
  <input type="text" bind:value={todoItem} placeholder="Add new goals" />
  <button on:click={addTodo}>Add</button>

  <ul>
    {#each todos as todo}
      <li
        class="rounded-xl bg-black bg-opacity-10	p-5 mb-4 flex items-center justify-between cursor-pointer hover:shadow-lg transition transform hover:scale-110"
      >
        <div>
          <input
            type="checkbox"
            class="mr-3"
            bind:checked={todo.done}
            on:click={toggleComplete(todo)}
          />
          <input
            class:completed={todo.done}
            class="border-0 bg-transparent"
            bind:value={todo.title}
            on:change={updateTodo(todo)}
          />
        </div>
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
    /* text-align: center; */
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
