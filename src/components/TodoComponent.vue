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
  width: 95%;
  margin-bottom: 10px;
  padding: 10px;
}

.task-info {
  display: flex;
  align-items: center;
  width: 100%;
  font-size: 16px;
  color: #333;
}

.description {
  font-weight: bold;
  flex: 1;
  max-height: 50px;
  overflow-y: auto;
  padding-right: 5px;
  word-wrap: break-word;
}

.actions-container {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-left: auto;
  white-space: nowrap;
}

.date {
  font-size: 14px;
  color: #333;
  margin-right: 10px;
}

.buttons {
  display: flex;
  gap: 10px;
  align-items: center;
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
    width: 85%;
    padding: 5px;
  }

  .task-info {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
    width: 100%;
  }

  .description {
    width: 100%;
    max-height: none;
    margin-right: 0;
    font-size: 14px;
  }

 

 
  .btn-delete{
    margin-right: auto;
  }

  .btn-status {
    padding: 8px 12px;
    font-size: 14px;
  }
}


</style>
