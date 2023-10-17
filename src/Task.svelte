<script>
  export let task;
  export let onDelete;
  export let onComplete;
</script>

<div class="task">
  <p>{task}</p>
  <button on:click={onDelete}>Eliminar</button>
  {#if onComplete}
    <button on:click={onComplete}>Completar</button>
  {/if}
</div>

<style>
  .task {
    margin-bottom: 10px;
    border: 1px solid #ccc;
    padding: 10px;
    text-align: center;
  }
</style>



