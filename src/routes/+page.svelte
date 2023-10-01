<script>
    //javascript
    let tasks = [];
    let newTask = '';
    let edit = false;
    let taskIdToEdit = null;

    function addTask() {
        if (newTask !=='') {
            const uniqueId = Date.now() + Math.random(); // Generate a unique ID
            tasks = [...tasks, { id: uniqueId, text: newTask, completed: false }];
            newTask = '';
        }
    }

    function removeTask(taskId) {
        tasks = tasks.filter((task) => task.id !== taskId);
    }

    function editTask(task) {
        edit = true;
        newTask = task.text; // Set the text of the task to the input field
        taskIdToEdit = task.id; // Optionally, store the ID of the task being edited (if needed)
}

    function updateTask() {
        if (newTask !== '' && taskIdToEdit !== null) {
            tasks = tasks.map((task) => task.id === taskIdToEdit ? { ...task, text: newTask } : task);
            edit = false;
            taskIdToEdit = null;
            newTask = '';
        }
    }

</script>

<main>
    <!-- HTML -->
    <h1 class="todo-header">To Do List</h1>

    <form class="todo-form">
        <input
            class="todo-input"
            type="text"
            placeholder="Add a new task"
            bind:value={newTask}
            on:keydown={(event)=> {
                if(event.key === 'Enter') addTask();
            }}
        />
        {#if edit === false}
            <button class="add-button" on:click={addTask}>Add Task</button>
        {:else}
            <button class="update-button" on:click={() => updateTask()}>Update Task</button>
        {/if}
    </form>

    <ul class="todo-list">
        {#each tasks as task}
          <li class="todo-item">
            <div class="task-container">
              <input
                class="todo-checkbox"
                type="checkbox"
                bind:checked={task.completed}
              />
              <span class="todo-text">{task.text}</span>
            </div>
            <div class="button-container">
              <button class="edit-button" on:click={() => editTask(task)}>Edit</button>
              <button class="delete-button" on:click={() => removeTask(task.id)}>Delete</button>
            </div>
          </li>
        {/each}
      </ul>
</main>

<style>
    :global(body) {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background-color: #f6f7fb;
        padding: 0;
        display: flex;
        justify-content: center; 
        align-items: center;
        height: 100vh;
    }
    
    main {
      width: 350px;
      background-color: #29272729;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      padding: 20px;
    }
  
    /* Header Styles */
    .todo-header {
      text-align: center;
      font-size: 24px;
      margin-bottom: 20px;
      color: #333;
    }
  
    /* Form Styles */
    .todo-form {
      display: flex;
      align-items: center;
      margin-bottom: 20px;
    }
  
    .todo-input {
      flex-grow: 1;
      padding: 10px;
      font-size: 16px;
      border: none;
      border-bottom: 1px solid #ccc;
      outline: none;
    }
  
    .add-button,
    .update-button {
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 4px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.2s;
    }
  
    .add-button:hover,
    .update-button:hover {
      background-color: #0056b3;
    }
  
    /* List Styles */
    .todo-list {
      list-style: none;
      padding: 0;
    }
  
    .todo-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      background-color: #fff;
      border: 1px solid #e0e0e0;
      border-radius: 4px;
      padding: 10px;
      margin-bottom: 10px;
    }
  
    .todo-text {
      flex-grow: 1;
      font-size: 16px;
      color: #333;
    }
  
    .todo-checkbox {
      margin-right: 10px;
    }
  
    /* Button Styles */
    .edit-button,
    .delete-button {
      background-color: #dc3545;
      color: #fff;
      border: none;
      border-radius: 4px;
      padding: 5px 10px;
      font-size: 14px;
      cursor: pointer;
      transition: background-color 0.2s;
    }
  
    .edit-button:hover,
    .delete-button:hover {
      background-color: #c82333;
    }
  </style>