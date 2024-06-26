<template>
  <div id="app" class="container">
    <div class="todo-app">
      <h1 class="title">TODO LIST</h1>
      <div class="input-group">
        <input
          v-model="newTodo"
          @keyup.enter="addNewTodo"
          placeholder="Tambahkan Todo List"
          class="input"
        />
        <button @click="addNewTodo" class="add-button" style="color:white;">Tambah</button>
      </div>
      <ul class="todo-list">
        <li v-for="(todo, index) in todos" :key="index" class="todo-item">
          <div class="todo-text">
            <input
              type="checkbox"
              :checked="todo.completed"
              @change="toggleTodoStatus(index)"
              class="checkbox"
            />
            <span :class="{ 'completed': todo.completed }">{{ todo.text }}</span>
          </div>
          <button @click="removeTodoItem(index)" class="remove-button">Hapus</button>
        </li>
      </ul>
      <p class="incomplete-count">Total : {{ incompleteTodosCount }}</p>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { useTodoStore } from './store/todostore';

export default {
  setup() {
    const newTodo = ref('');
    const todoStore = useTodoStore();

    const addNewTodo = () => {
      if (newTodo.value.trim()) {
        todoStore.addTodo(newTodo.value.trim());
        newTodo.value = '';
      }
    };

    const removeTodoItem = (index) => {
      todoStore.removeTodo(index);
    };

    const toggleTodoStatus = (index) => {
      todoStore.toggleTodo(index);
    };

    const incompleteTodosCount = computed(() => todoStore.incompleteTodosCount);

    return {
      newTodo,
      todos: todoStore.todos,
      addNewTodo,
      removeTodoItem,
      toggleTodoStatus,
      incompleteTodosCount,
    };
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center; 
  min-height: 100vh; 
  background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
}

.todo-app {
  display: flex;
  flex-direction: column;
  /* justify-content: center;  */
  align-items: center; 
  text-align: center; 
  width: 100%; 
  max-width: 600px; 
  padding: 20px;
  margin: auto 0;
  background-color: rgba(255, 255, 255, 0.9); 
  border-radius: 12px; 
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15); 
  transition: transform 0.3s ease, box-shadow 0.3s ease; 
}

.todo-app:hover {
  transform: scale(1.05); 
  box-shadow: 0 0 25px rgba(0, 0, 0, 0.2); 
}

@media (max-width: 768px) {
  .container {
    padding: 0; /* Tidak ada padding */
  }

  .todo-app {
    padding: 15px;
  }
}


.title {
  font-size: 2rem;
  margin-bottom: 20px;
  color: #695171;
}

.title:hover {
  transform: scale(1.05);
  text-shadow: 2px 2px 8px rgba(189, 19, 19, 0.4);
}

.input-group {
  display: flex;
  margin-bottom: 20px;
  width: 100%;
}

.input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.3s ease;
}

.input:focus {
  border-color: #403083;
}

.add-button {
  background: #363585;
  color: #080808;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  margin-left: 10px;
  transition: background 0.3s ease;
}

.add-button:hover {
  background: #444081;
}

.todo-list {
  list-style: none;
  padding: 0;
  width: 100%;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #191818;
  transition: background 0.3s ease;
}

.todo-item:hover {
  background: #f9f9f9;
}

.todo-text {
  display: flex;
  align-items: center;
}

.checkbox {
  margin-right: 10px;
  cursor: pointer;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.remove-button {
  background: none;
  border: none;
  color: #ff4d4d;
  cursor: pointer;
  font-size: 1rem;
  transition: color 0.3s ease;
}

.remove-button:hover {
  color: #ff0000;
}

.incomplete-count {
  margin-top: 20px;
  font-size: 1rem;
  color: #333;
}
</style>
