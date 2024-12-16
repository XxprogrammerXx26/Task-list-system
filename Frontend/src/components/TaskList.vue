<template>
  
  <nav class="navbar navbar-expand-lg" style="background-color: #0bbf3b;">
    <div class="container-fluid">
      <a class="navbar-brand text-white" href="#">MyTaskList</a>
      
      
      <div class="navbar-right">
        <span v-if="user" class="text-white font-weight-bold mr-3">{{ user }}</span>
        <button v-if="user" @click="logout" class="btn btn-warning logout-btn">Cerrar sesión</button> 
      </div>
    </div>
  </nav>

  <div class="task-list container mt-5">
    <h1 class="text-center">Welcome to TaskList</h1>

   
    <div class="input-container row justify-content-center mt-4">
      <div class="col-12 col-md-8">
        <div class="input-group">
          <input v-model="newTask" type="text" class="form-control" placeholder="Agregar tarea..." />
          <button @click="addTask" class="btn btn-success">Agregar tarea</button>
        </div>
      </div>
    </div>

    <!-- Contenedor de las columnas Kanban -->
    <div class="task-container row justify-content-center mt-4">
      <div class="task-column col-12 col-md-4 mb-4" v-for="(tasks, column) in tasks" :key="column">
        <h2>{{ column === 'todo' ? 'Por hacer' : column === 'inProgress' ? 'En progreso' : 'Hecho' }}</h2>

        <!-- Lista de tareas en cada columna -->
        <ul class="list-unstyled">
          <li v-for="task in tasks" :key="task.id" class="task-item d-flex justify-content-between align-items-center mb-3 p-3 bg-light rounded shadow-sm">
            <span>{{ task.text }}</span>
            <div v-if="column === 'todo'">
              <input type="checkbox" v-model="selectedTasks[task.id]" />
            </div>
            <div>
              <button class="btn btn-primary btn-sm edit-btn" @click="editTask(task, column)">Editar</button> <!-- Botón de editar azul predeterminado -->
              <button class="btn btn-danger btn-sm delete-btn" @click="deleteTask(task, column)">Eliminar</button> <!-- Botón de eliminar rojo predeterminado -->
            </div>
          </li>
        </ul>

        <!-- Botones para mover tareas entre columnas -->
        <div class="task-actions" v-if="column === 'todo'">
          <button @click="moveSelectedTasksToProgress" class="btn btn-secondary w-100">Mover tareas seleccionadas a En Progreso</button>
        </div>
        <div class="task-actions" v-if="column === 'inProgress'">
          <button @click="moveTaskToDone" class="btn btn-secondary w-100">Mover a Hecho</button>
        </div>
      </div>
    </div>
  </div>








  <br>
  <br>
  <div class="card text-center">
  <div class="card-header">
    Featured
  </div>
  <div class="card-body">
    <h5 class="card-title">Special title treatment</h5>
    <p class="card-text">With supporting text below as a natural lead-in to additional content.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
  <div class="card-footer text-body-secondary">
    2 days ago
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
      selectedTasks: {}, // Objeto para almacenar las tareas seleccionadas
      user: 'Leo@gmail.com' // Simulación de un usuario logueado
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
    },

    // Función para cerrar sesión
    logout() {
      this.user = null; // Eliminar la información del usuario
      localStorage.removeItem('user'); // Opcional, si estás usando almacenamiento local
      this.$router.push('/login'); // Redirigir al login o página de inicio
    }
  },

  // Simulación de la sesión del usuario al cargar la página
  mounted() {
    const storedUser = localStorage.getItem('user'); // Si usas almacenamiento local
    if (storedUser) {
      this.user = storedUser;
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
  padding: 40px 20px;
  min-height: 100vh;
}

h1 {
  font-size: 2.5em;
  margin-bottom: 30px;
  color: #333;
  font-weight: 700;
}

/* Input Container */
.input-container input {
  font-size: 18px;
}

input {
  padding: 15px;
  font-size: 16px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  padding: 12px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:active {
  transform: translateY(2px);
}

/* Task Columns */
.task-column {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  min-height: 300px;
  box-sizing: border-box;
}

h2 {
  text-align: center;
  margin-bottom: 30px;
  color: #333; 
  font-size: 1.4em;
  font-weight: 700;
}

/* List of Tasks */
.task-item {
 background-color: #e0e0e0; 
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

/* Estilo para botones Editar, Eliminar y Cerrar sesión */
.edit-btn {
  background-color: #007bff; /* Azul para editar */
  color: white;
}

.delete-btn {
  background-color: #dc3545; /* Rojo para eliminar */
  color: white;
}

.logout-btn {
  background-color: #ff5733; /* Naranja llamativo para cerrar sesión */
  color: white;
}

/* Eliminar el hover */
button:hover {
 /* background-color: inherit;  */
 
  opacity: 1; /* No cambia la opacidad */
}

button:focus {
  outline: none; /* Eliminar el contorno cuando el botón es seleccionado */
}

/* Media Queries para hacer el diseño responsive con Bootstrap */
@media (max-width: 768px) {
  .task-container {
    flex-direction: column;
  }

  .task-column {
    margin-bottom: 20px;
  }

  h1 {
    font-size: 2em;
  }

  h2 {
    font-size: 1.2em;
  }

  .task-item {
    font-size: 14px;
  }

  .navbar-right {
    flex-direction: column;
  }
}
</style>
