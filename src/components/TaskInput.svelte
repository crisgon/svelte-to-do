<script>
  export let insertTasksOnDataBase;

  let taskName = "";
  let disabledBtn = true;

  $: disabledBtn = taskName.trim().length <= 5;

  function createNewTask(name) {
    insertTasksOnDataBase(name);
    taskName = "";
  }
</script>

<style>
  .btn {
    background-color: #ff3e00;
    color: #fff;
    cursor: pointer;
  }

  .btn:disabled {
    cursor: not-allowed;
  }
</style>

<div class="task-input">
  <input type="text" bind:value={taskName} />
  <button
    disabled={disabledBtn}
    class="btn"
    on:click={() => createNewTask(taskName)}>
    create
  </button>

</div>
