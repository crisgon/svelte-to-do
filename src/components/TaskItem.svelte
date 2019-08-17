<script>
  export let id;
  export let name;
  export let completed;

  import { fly } from "svelte/transition";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
</script>

<style>
  .completed {
    opacity: 0.3;
    transition: opacity 0.2s;
  }

  .completed:hover {
    opacity: 1;
  }

  .completed > .task-name {
    text-decoration: line-through;
    color: #2ed573;
  }

  .task {
    list-style: none;
    padding: 1rem 0;
    border-bottom: 1px solid #f1f2f6;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .task-name {
    width: 750px;
  }

  .checkbox {
    margin: 0;
  }

  .remove {
    width: 50px;
    text-align: right;
    text-decoration: none;
  }

  .remove:hover {
    cursor: pointer;
    color: tomato;
  }
</style>

<li
  class="task"
  class:completed
  in:fly={{ x: 50, duration: 800 }}
  out:fly={{ x: 50, duration: 800 }}>
  <span class="task-name">{name}</span>
  <input
    type="checkbox"
    bind:checked={completed}
    class="checkbox"
    on:change={() => dispatch('complete', { id, name, completed })} />
  <span
    role="button"
    class="remove"
    on:click={() => dispatch('delete', { id })}>
    ❌
  </span>
</li>
