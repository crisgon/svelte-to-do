<script>
  export let taskList;
  export let deleteTask;
  export let completeTask;

  import TaskItem from "./TaskItem.svelte";
</script>

<style>
  .task-list {
    padding: 0;
  }
</style>

<ul class="task-list">
  {#each taskList as task (task.id)}
    <TaskItem
      id={task.id}
      name={task.taskName}
      completed={task.completed}
      on:delete={e => deleteTask(e)}
      on:complete={e => completeTask(e)} />
  {:else}
    <span>Nothing here...</span>
  {/each}
</ul>
