<template>
    
    
   
  <!-- Navbar -->
  <nav class="navbar navbar-expand-lg" style="background-color: #0bbf3b;">
    <div class="container-fluid" style="max-width: 1500px; margin: 0 auto;">
      <a class="navbar-brand" href="#" style="color: white;">MyTaskList</a>
      
      <!-- Mostrar el nombre del usuario y el botón de cerrar sesión -->
      <div class="navbar-right">
        <span v-if="user" style="color: white; font-weight: bold; margin-right: 20px;">{{ user }}</span>
        <button v-if="user" @click="logout" class="btn btn-danger">Cerrar sesión</button>
      </div>
    </div>
    
    
  </nav>
  
  


  
  
  <div class="task-list">
    <h1>Welcome to TaskList</h1>

    <!-- Input para agregar nuevas tareas -->
    <div class="input-container">
      <input v-model="newTask" type="text" placeholder="Agregar tarea..." />
      <button @click="addTask">Agregar tarea</button>
    </div>

    <!-- Contenedor de las columnas Kanban -->
    <div class="task-container">
      <div class="task-column" v-for="(tasks, column) in tasks" :key="column">
        <h2>{{ column === 'todo' ? 'Por hacer' : column === 'inProgress' ? 'En progreso' : 'Hecho' }}</h2>

        <!-- Lista de tareas en cada columna -->
        <ul class="task-list">
          <li v-for="task in tasks" :key="task.id" class="task-item">
            <span>{{ task.text }}</span>
            <div v-if="column === 'todo'">
              <input type="checkbox" v-model="selectedTasks[task.id]" />
            </div>
            <div>
              <button class="edit" @click="editTask(task, column)">Editar</button>
              <button class="delete" @click="deleteTask(task, column)">Eliminar</button>
            </div>
          </li>
        </ul>

        <!-- Botones para mover tareas entre columnas -->
        <div class="task-actions" v-if="column === 'todo'">
          <button @click="moveSelectedTasksToProgress">Mover tareas seleccionadas a En Progreso</button>
        </div>
        <div class="task-actions" v-if="column === 'inProgress'">
          <button @click="moveTaskToDone">Mover a Hecho</button>
        </div>
      </div>
    </div>
  </div>














</template>

<script>
export default {
  data() {
    return {
      newTask: '', // Campo para capturar el texto de la nueva tarea
      tasks: {
        todo: [], // Tareas "Por hacer"
        inProgress: [], // Tareas "En progreso"
        done: [] // Tareas "Hecho"
      },
      selectedTasks: {} // Objeto para almacenar las tareas seleccionadas
    };
  },
  methods: {
    // Función para agregar una nueva tarea
    addTask() {
      const trimmedTask = this.newTask.trim(); // Eliminar espacios antes y después
      if (trimmedTask !== '') {
        const task = {
          id: Date.now(),
          text: trimmedTask
        };
        this.tasks.todo.push(task); // Agregar a la columna "Por hacer"
        this.newTask = '';  // Limpiar el campo de entrada
      } else {
        alert("Por favor, ingrese una tarea válida.");
      }
    },

    // Función para editar una tarea
    editTask(task, column) {
      const newText = prompt('Edita tu tarea:', task.text);
      if (newText !== null && newText.trim() !== '') {
        task.text = newText; // Actualizar el texto de la tarea
      }
    },

    // Función para eliminar una tarea
    deleteTask(task, column) {
      const index = this.tasks[column].indexOf(task); 
      if (index > -1) {
        this.tasks[column].splice(index, 1); // Eliminar la tarea de la columna
        this.$delete(this.selectedTasks, task.id); // Eliminar también de la lista de tareas seleccionadas
      }
    },

    // Función para mover tareas seleccionadas de "Por hacer" a "En progreso"
    moveSelectedTasksToProgress() {
      const selectedIds = Object.keys(this.selectedTasks).filter(id => this.selectedTasks[id]);
        
      selectedIds.forEach(id => {
        const task = this.tasks.todo.find(t => t.id === parseInt(id));
        if (task) {
          this.moveTaskToProgress(task); // Mover tarea a "En progreso"
        }
      });

      // Limpiar las tareas seleccionadas después de moverlas
      this.selectedTasks = {};
    },

    // Función para mover una tarea de "Por hacer" a "En progreso"
    moveTaskToProgress(task) {
      // Mover la tarea de "todo" a "inProgress"
      this.tasks.inProgress.push(task);
      this.deleteTask(task, 'todo'); // Eliminarla de la columna 'todo'
    },

    // Función para mover una tarea de "En progreso" a "Hecho"
    moveTaskToDone(task) {
      // Mover la tarea de "inProgress" a "done"
      this.tasks.done.push(task);
      this.deleteTask(task, 'inProgress'); // Eliminarla de la columna 'inProgress'
    }
  }
};
</script>

<style scoped>
/* Fuente importada desde Google Fonts */
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

/* Estilos Generales */
.task-list {
  font-family: 'Roboto', sans-serif; /* Usamos la fuente Roboto */
  background-color: #f0f0f0;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 40px 20px; /* Aumento el padding superior e inferior */
  min-height: 100vh;
}

h1 {
  font-size: 2.5em; /* Aumento el tamaño del título */
  margin-bottom: 30px;
  color: #333;
  text-align: center;
  font-weight: 700; /* Negrita para el título */
}

/* Input Container */
.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 30px; /* Aumento el espacio entre input y columnas */
}

input {
  width: 60%; /* Cambio el tamaño del input para mayor padding */
  padding: 15px;
  font-size: 18px;
  border-radius: 5px;
  border: 1px solid #ccc;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  font-family: 'Roboto', sans-serif;
}

button {
  padding: 12px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  margin-left: 15px;
  font-size: 16px;
  font-family: 'Roboto', sans-serif;
}

button:hover {
  background-color: #45a049;
  transform: translateY(-2px);
}

button:active {
  transform: translateY(2px);
}

/* Contenedor de las columnas Kanban */
.task-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  width: 100%;
  flex-wrap: wrap;
  gap: 20px; /* Espacio entre las columnas */
}

.task-column {
  width: 30%;
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  min-height: 300px;
  box-sizing: border-box; /* Para incluir el padding en el ancho total */
}

h2 {
  text-align: center;
  margin-bottom: 30px; /* Mayor espacio entre el título y las tareas */
  color: #333;
  font-size: 1.4em;
  font-family: 'Roboto', sans-serif;
  font-weight: 700;
}

/* Lista de Tareas */
ul {
  list-style-type: none;
  padding: 0;
}

.task-item {
  background-color: #e0e0e0;
  padding: 20px;
  margin-bottom: 15px;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease-in-out;
  font-family: 'Roboto', sans-serif;
}

.task-item:hover {
  transform: translateY(-5px);
}

.task-item div {
  display: flex;
  gap: 10px; /* Espacio entre los botones de editar y eliminar */
}

/* Botones de Edición y Eliminación */
button.edit,
button.delete {
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
  border: none;
  color: white;
}

button.edit {
  background-color: #007bff;
}

button.delete {
  background-color: #ff4757;
}

button.edit:hover,
button.delete:hover {
  opacity: 0.8;
}

/* Botones de Mover */
.task-actions button {
  padding: 12px 20px;
  background-color: #007bff;
  color: white;
  border-radius: 5px;
  border: none;
  margin-top: 20px;
  width: 100%;
  cursor: pointer;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
}

.task-actions button:hover {
  background-color: #0062cc;
  transform: translateY(-2px);
}

.task-actions button:active {
  transform: translateY(2px);
}

/* Responsividad */
@media screen and (max-width: 768px) {
  .task-container {
    flex-direction: column;
    align-items: center;
  }

  .task-column {
    width: 80%;
    margin-bottom: 20px;
  }
}

@media screen and (max-width: 480px) {
  input {
    width: 75%;
  }

  .task-column {
    width: 90%;
  }

  button {
    width: 100%;
    margin-left: 0;
  }
}
</style>