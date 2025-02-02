<template>
  <div class="task-item">
    <div class="task-info">
      <span class="description">{{ description }}</span>
      <span class="date">{{ date }}</span>
      <div class="buttons">
        <button @click="toggleEtat" class="btn-status">{{ etat ? "Fait" : "À faire" }}</button>
        <button @click="deleteTask" class="btn-delete">
          <img src="/corbeille.png" alt="Delete" class="delete-icon" />
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    description: {
      type: String,
      required: true,
    },
    etat: {
      type: Boolean,
      required: true,
    },
    date: {
      type: String,
      required: true,
    },
  },
  emits: ['update:etat', 'delete'],
  
  methods: {
    toggleEtat() {
      this.$emit('update:etat', !this.etat); // Émet un événement pour mettre à jour l'état
    },
    deleteTask() {
      this.$emit('delete'); // Émet un événement pour supprimer la tâche
    },
  },
};
</script>

<style scoped>
.task-item {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'Arial', sans-serif;
  width: 90%;
}

.task-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 80%;
  font-size: 16px;
  color: #333;
  flex-wrap: wrap;
  gap: 10px;
}


.task-item:hover {
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.25);
}

.description {
  font-weight: bold;
  flex-grow: 1;
  margin-right: 20px;
  max-height: 50px; 
  overflow-y: auto; 
  padding-right: 5px; 
  word-wrap: break-word; 
}

.date {
  font-size: 14px;
  color: #777;
}

.buttons {
  display: flex;
  gap: 10px;
  margin-left: 10px;
  justify-content: flex-end;
  flex-wrap: wrap;
}

.btn-status {
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

.btn-status:hover {
  background-color: #0077b6;
  transform: scale(1.05);
}

.btn-delete {
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.delete-icon {
  width: 20px;
  height: 20px;
  transition: transform 0.3s ease;
}

.delete-icon:hover {
  transform: scale(1.2);
}
@media screen and (max-width: 768px) {
  .task-item {
    width: 80%;
    flex-direction: column;
    gap: 10px;
    margin: 10px;
    padding: 15px;
  }

  .task-info {
    flex-direction: line;
    align-items: flex-start;
    gap: 8px;
  }

  .description {
 
    margin-right: 0;
    margin-bottom: 8px;
    font-size: 14px;
  }

  .date {
    width: 100%;
    font-size: 12px;
  }

  .buttons {
    width: 100%;
    justify-content: space-between;
    margin-left: 0;
    margin-top: 10px;
  }

  .btn-status {
    padding: 8px 12px;
    font-size: 14px;
  }
}


</style>
