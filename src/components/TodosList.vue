<template>
  <div class="app-container">
    <h1 class="title">Todos</h1>
    <div class="content">
      <!-- Section gauche : Formulaire d'ajout -->
      <div class="left-panel">
        <h2>Ajouter une tâche</h2>
        <div class="form-container">
          <input type="text" v-model="newTask.description" placeholder="Entrez une tâche" class="input-field" />
          <input type="date" v-model="newTask.date" class="input-field"  ref="dateInput"/>
          <button @click="addTask" class="btn-primary">Ajouter</button>
        </div>
      </div>

      <!-- Section droite : Liste des tâches -->
      <div class="right-panel">
        <h2>Liste des tâches</h2>
        <div class="filters">
          <button :class="['btn-filter', { active: currentFilter === 'all' }]" @click="setFilter('all')">
            Toutes
          </button>
          <button :class="['btn-filter', { active: currentFilter === 'todo' }]" @click="setFilter('todo')">
            À faire
          </button>
          <button :class="['btn-filter', { active: currentFilter === 'done' }]" @click="setFilter('done')">
            Faites
          </button>
        </div>
        <div class="task-list">
          <div v-for="(task, index) in filteredTasks" :key="index" class="task-item">
            <div v-if="!task.isEditing" class="task-content" @dblclick="enableEditing(index)">
              <ToDoComponent :description="task.description" :etat="task.etat" :date="task.date"
                @update:etat="updateTaskEtat(index, $event)" @delete="deleteTask(index)" />
            </div>
            <div v-else class="task-edit">
              <input type="text" v-model="task.description" @keyup.enter="saveEdit(index)" @blur="saveEdit(index)"
                class="edit-input" />
            </div>
          </div>
        </div>
        <div class="delete-buttons">
          <button @click="deleteAllTask" class="btn-danger">Supprimer toutes</button>
          <button @click="deleteDoneTask" class="btn-danger">Supprimer les faites</button>
        </div>
        <footer v-if="tasks.length > 0" class="footer">
          <span>Tâches à faire : {{ remainingTasks }}</span>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
import ToDoComponent from './TodoComponent.vue';


export default {
  name: 'TodosList',
  components: { ToDoComponent},
  data() {
    return {
      newTask: { description: '', date: '' },
      tasks: [],
      currentFilter: 'all',
    };
  },
  computed: {
    filteredTasks() {
      if (this.currentFilter === 'todo') {
        return this.tasks.filter(task => !task.etat);
      } else if (this.currentFilter === 'done') {
        return this.tasks.filter(task => task.etat);
      }
      return this.tasks;
    },
    remainingTasks() {
      return this.tasks.filter(task => !task.etat).length;
    },
  },
  methods: {
    addTask() {
      if (this.newTask.description.trim() === '') {
        alert('Veuillez entrer une description de tâche.');
        return;
      }
      if (!this.newTask.date) {
        alert('Veuillez sélectionner une date.');
        return;
      }
      const today = new Date().setHours(0, 0, 0, 0);
      const enteredDate = new Date(this.newTask.date).setHours(0, 0, 0, 0);
      if (enteredDate < today) {
        alert('Veuillez sélectionner une date valide(pas dans le passé).');
        return;
      }
      this.tasks.push({
        description: this.newTask.description,
        etat: false,
        date: this.newTask.date,
        isEditing: false,
      });
      this.newTask.description = '';
      this.newTask.date = '';
    },
    focusDateInput() {
    this.$refs.dateInput.focus();
  },
    setFilter(filter) {
      this.currentFilter = filter;
    },
    deleteAllTask() {
      this.tasks = [];
    },
    deleteDoneTask() {
      this.tasks = this.tasks.filter(task => !task.etat);
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    updateTaskEtat(index, newEtat) {
      this.tasks[index].etat = newEtat;
    },
    enableEditing(index) {
      this.tasks[index].isEditing = true;
    },
    saveEdit(index) {
      this.tasks[index].isEditing = false;
    },
  },
};
</script>

<style>
.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  font-family: 'Poppins', sans-serif;
  box-sizing: border-box;
  margin-top: -40px;
 
}

.title {
  font-size: 2.5rem;
  font-weight: bold;
  margin: 10px 0;
  text-align: center;
}

.content {
  display: grid;
  grid-template-columns: 300px 1fr; 
  gap: 20px;
  width: 100%;
  height: 80%;
  box-sizing: border-box;
}

.left-panel {
  background: #ffffff;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  padding: 20px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 300px; 
  height: 100%; 
}

.right-panel {
  background: #ffffff;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  padding: 20px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  width: 600px; 
  height: 100%; 
}


h2 {
  color: #333;
  font-weight: bold;
  margin-bottom: 20px;  
  margin-top: 5px;
  text-align: center;
}

.form-container {
  display: flex;
  flex-direction: column;
  gap: 15px;
  width: 100%;
}



.btn-primary {
  background-color: #00b4d8;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 15px;
  cursor: pointer;
}




.input-field {
  width: 90%;
  padding: 12px;
  border: 2px solid #ccc;
  border-radius: 10px;
  font-size: 16px;
  transition: all 0.3s ease;

  
}


.input-field:focus {
  border-color: #00b4d8;
  outline: none;
  background-color: #f0f8ff;
}

.btn-primary {
  background-color: #00b4d8;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  transition: transform 0.3s ease, background-color 0.3s ease;
}

.btn-primary:hover {
  background-color: #0077b6;
  transform: scale(1.05);
}


.filters {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  gap: 10px;
}

.btn-filter {
  flex: 1;
  padding: 10px 12px;
  background: #e9d8fd;
  border: 2px solid transparent;
  border-radius: 10px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
  color: #4a4a4a;
  transition: all 0.3s ease;
}

.btn-filter.active {
  background: #00b4d8;
  color: white;
  border-color: #00b4d8;
}

.btn-filter:hover {
  background: #90e0ef;
  color: #333;
}

.task-list {
  flex-grow: 1;
  width: 100%;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  
}

.task-edit .edit-input {
  width: 100%;
  padding: 10px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 10px;
}

.btn-status {
  background: #00b4d8;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 8px 12px;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-status:hover {
  background: #0077b6;
}

.delete-buttons {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

.btn-danger {
  background-color: #dc3545;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  transition: transform 0.3s ease, background-color 0.3s ease;
}

.btn-danger:hover {
  background-color: #b02a37;
  transform: scale(1.05);
}

.footer {
  text-align: center;
  margin-top: 20px;
  font-size: 16px;
  color: #4a4a4a;
  padding-top: 15px;
  border-top: 1px solid #e9d8fd;
}


</style>
