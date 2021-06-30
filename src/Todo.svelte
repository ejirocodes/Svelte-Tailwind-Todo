<script>
  import { onMount } from 'svelte';
  let deletIcon = 'images/trash.svg';
  let addIcon = 'images/plus.svg';
  import axios from 'axios';

  let isError = null;
  let todoItem = '';
  let todos = [];

  const getTodos = async () => {
    try {
      const res = await axios.get('http://localhost:1337/todos');
      todos = res.data;
    } catch (e) {
      isError = e;
    }
  };

  const addTodo = async () => {
    // Using push to trigger update
    // todos.push({
    //   title: todoItem,
    //   project: todoItem,
    //   isCompleted: false,
    // });
    // todos = todos;

    try {
      if (!todoItem) return alert('please add a goal for today!');

      const res = await axios.post('http://localhost:1337/todos', {
        todoItem,
        isCompleted: false,
      });
      // Using a more idiomatic solution
      todos = [...todos, res?.data];
      todoItem = '';
    } catch (e) {
      isError = e;
    }

    // Using a more idiomatic solution
    // todos = [
    //   ...todos,
    //   {
    //     todoItem: todoItem,
    //     project: todoItem,
    //     isCompleted: false,
    //   },
    // ];
    // todoItem = '';
  };

  const deleteTodo = async (todo) => {
    try {
      await axios.delete(`http://localhost:1337/todos/${todo.id}`);
      todos = todos.filter((to) => to.id !== todo.id);
    } catch (e) {
      isError = e;
    }

    // Using filter to trigger update
    // todos = todos.filter((to) => to !== todo);

    // Using splice to trigger update
    // const todoIndex = todos.indexOf(todo);
    // todos.splice(todoIndex, 1);
    // todos = todos;
  };

  const toggleComplete = async (todo) => {
    const todoIndex = todos.indexOf(todo);
    try {
      const { data } = await axios.put(
        `http://localhost:1337/todos/${todo.id}`,
        {
          isCompleted: !todo.isCompleted,
        }
      );
      todos[todoIndex].isCompleted = data.isCompleted;
    } catch (e) {
      isError = e;
    }
  };

  const updateTodo = async (todo) => {
    const todoIndex = todos.indexOf(todo);
    try {
      const { data } = await axios.put(
        `http://localhost:1337/todos/${todo.id}`,
        {
          todoItem: todo.todoItem,
        }
      );
      todos[todoIndex].todoItem = data.todoItem;
    } catch (e) {
      isError = e;
    }
  };
  onMount(async () => {
    await getTodos();
  });
</script>

<section class="rounded-3xl text-white py-10 px-6">
  <h1 class="text-4xl	font-bold mb-5">Hey there!</h1>
  {#if todos.length > 0}
    <p class="text-2xl mb-4">Today's Goal</p>
  {/if}
  {#if isError}
    <p class="text-xl mb-2 text-red-600">{isError}</p>
  {/if}
  <ul>
    {#each todos as todo}
      <li
        class="rounded-xl bg-black bg-opacity-10	p-5 mb-4 flex items-center justify-between cursor-pointer hover:shadow-lg transition transform hover:scale-110"
      >
        <div class="flex items-center w-full">
          <input
            type="checkbox"
            class="mr-3"
            bind:checked={todo.isCompleted}
            on:click={toggleComplete(todo)}
          />
          <input
            class:completed={todo.isCompleted}
            class="border-0 bg-transparent w-full"
            bind:value={todo.todoItem}
            on:change={updateTodo(todo)}
          />
        </div>
        <button on:click={deleteTodo(todo)} class="border-0"
          ><img src={deletIcon} alt="delete todo" class="w-6 " /></button
        >
      </li>
    {:else}
      <p>No goals for today!</p>
    {/each}
  </ul>
  <input
    type="text"
    bind:value={todoItem}
    class="w-full rounded-xl bg-white border-0 outline-none bg-opacity-10 p-4 shadow-lg mt-4"
    placeholder="Add new goals"
  />
  <button
    on:click={addTodo}
    class="my-5  p-5 bg-black text-white rounded-xl w-full hover:bg-opacity-60 transition border-0 capitalize flex items-center justify-center"
    ><span><img src={addIcon} alt="add todo" class="w-6 mr-4" /></span>Add new
    todo</button
  >
  <div class="text-center">
    <p>
      Completed Tasks: {todos.filter((todo) => {
        return todo.isCompleted;
      }).length}
    </p>
    <p>
      Pending Tasks: {todos.filter((todo) => {
        return todo.isCompleted === false;
      }).length}
    </p>
  </div>
</section>

<style>
  .completed {
    text-decoration: line-through;
  }
  section {
    max-width: 240px;
    margin: 0 auto;
    background-color: #2073dd;
  }
  @media (min-width: 640px) {
    section {
      max-width: 430px;
    }
  }
</style>
