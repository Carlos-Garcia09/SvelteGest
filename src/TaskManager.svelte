<script>
  import Task from './Task.svelte';
  import { onMount } from 'svelte';

  let newTask = '';
  let tasks = [];
  let completedTasks = [];

  function addTask() {
    tasks = [...tasks, { title: newTask, completed: false }];
    newTask = '';
  }

  function deleteTask(index, completed) {
    if (completed) {
      completedTasks = completedTasks.filter((_, i) => i !== index);
    } else {
      tasks = tasks.filter((_, i) => i !== index);
    }
  }

  function completeTask(index) {
    const task = tasks[index];
    tasks = tasks.filter((_, i) => i !== index);
    completedTasks = [...completedTasks, { title: task.title, completed: true }];
  }

  onMount(() => {
    const incompleteContainer = document.getElementById('incomplete-container');
    const completeContainer = document.getElementById('complete-container');

    incompleteContainer.ondragover = (event) => event.preventDefault();
    incompleteContainer.ondrop = (event) => {
      const taskId = event.dataTransfer.getData('text/plain');
      const task = completedTasks[taskId];
      tasks = [...tasks, { title: task.title, completed: false }];
      completedTasks = completedTasks.filter((_, i) => i != taskId);
    };

    completeContainer.ondragover = (event) => event.preventDefault();
    completeContainer.ondrop = (event) => {
      const taskId = event.dataTransfer.getData('text/plain');
      const task = tasks[taskId];
      completedTasks = [...completedTasks, { title: task.title, completed: true }];
      tasks = tasks.filter((_, i) => i != taskId);
    };
  });
</script>

<div class="container">
  <h2>Gestor de Tareas Personales</h2>

  <div class="input-container">
    <input bind:value={newTask} placeholder="Nueva tarea..." />
    <button on:click={addTask}>Agregar</button>
  </div>

  <div class="columns">
    <div class="column" id="incomplete-container">
      <h3>Tareas por completar</h3>
      {#each tasks as task, i}
        <div
          draggable="true"
          on:dragstart={() => {
            event.dataTransfer.setData('text/plain', i);
          }}
        >
          <Task
            task={task.title}
            onDelete={() => deleteTask(i, false)}
            onComplete={() => completeTask(i)}
            key={i}
          />
        </div>
      {/each}
    </div>
    <div class="column complete-column" id="complete-container">
      <h3>Tareas completadas</h3>
      {#each completedTasks as task, i}
        <div
          draggable="true"
          on:dragstart={() => {
            event.dataTransfer.setData('text/plain', i);
          }}
        >
          <Task
            task={task.title}
            onDelete={() => deleteTask(i, true)}
            key={i}
          />
        </div>
      {/each}
    </div>
  </div>
</div>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  h2 {
    text-align: center;
  }

  .input-container {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }

  input {
    margin-right: 10px;
  }

  .columns {
    display: flex;
  }

  .column {
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin: 0 10px;
  }

  .column h3 {
    text-align: center;
  }

  .complete-column {
    background-color: #d1f7d4; /* Color verde para las tareas completadas */
  }
</style>




