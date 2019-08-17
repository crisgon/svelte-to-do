<script>
  import TaskInput from "./components/TaskInput.svelte";
  import TaskList from "./components/TaskList.svelte";

  const projectName = "Svelte To Do";
  const src = "https://avatars2.githubusercontent.com/u/23617963?s=40&v=4";

  import { onMount } from "svelte";

  let taskName = "";
  let taskList = [];

  $: taskName = taskName;

  onMount(() => {
    getTaskListOnDataBase();
  });

  async function insertTasksOnDataBase(name) {
    const taskData = { taskName: name, completed: false };
    try {
      const data = await (await fetch(
        "https://svelte-todo-61dba.firebaseio.com/tasks.json",
        {
          method: "POST",
          body: JSON.stringify(taskData),
          headers: { "Content-Type": "application/json" }
        }
      )).json();

      taskList = [...taskList, { id: data.name, taskName: taskData.taskName }];
    } catch (error) {
      console.error(error);
    }
  }

  async function changeTasksStateOnDataBase(event) {
    const {
      detail: { id, completed, name }
    } = event;
    const taskData = { taskName: name, completed };
    try {
      const data = await (await fetch(
        `https://svelte-todo-61dba.firebaseio.com/tasks/${id}.json`,
        {
          method: "PATCH",
          body: JSON.stringify(taskData),
          headers: { "Content-Type": "application/json" }
        }
      )).json();
      const taskIndex = taskList.findIndex(task => task.id === id);
    } catch (error) {
      console.error(error);
    }
  }

  async function getTaskListOnDataBase() {
    try {
      const data = await (await fetch(
        "https://svelte-todo-61dba.firebaseio.com/tasks.json"
      )).json();

      Object.keys(data).forEach(id => {
        taskList = [
          ...taskList,
          { id, taskName: data[id].taskName, completed: data[id].completed }
        ];
      });
    } catch (error) {
      console.error(error);
    }
  }

  async function deleteTaskOnDatabase(event) {
    const {
      detail: { id }
    } = event;
    try {
      const data = await fetch(
        `https://svelte-todo-61dba.firebaseio.com/tasks/${id}.json`,
        {
          method: "DELETE"
        }
      );
      if (data.ok) {
        updateTaskListAfterDelete(id);
      }
    } catch (error) {}
  }

  function updateTaskListAfterDelete(id) {
    taskList = taskList.filter(task => task.id !== id);
  }
</script>

<style>
  .container {
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
  }

  .header {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .image {
    text-align: center;
  }
</style>

<section class="container">
  <header class="header">
    <figure class="image">
      <img {src} alt="Svelte3" />
    </figure>

    <h1 class="title">{projectName}</h1>
    <TaskInput {insertTasksOnDataBase} />
  </header>

  <TaskList
    {taskList}
    deleteTask={deleteTaskOnDatabase}
    completeTask={changeTasksStateOnDataBase} />
</section>
