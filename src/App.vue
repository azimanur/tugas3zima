<template>
  <div class="todo-app">
      <form @submit.prevent="addTodo" class="todo-form">
          <input v-model="newTodo.text" required placeholder="New todo" class="todo-input">
          <button type="submit" class="todo-button">Add Todo</button>
      </form>
  
      <ul class="todo-list">
          <li v-for="todo in todos" :key="todo.id" class="todo-item">
              <input type="checkbox" v-model="todo.done">
              <span v-if="!todo.isEditing" @dblclick="editTodo(todo)" class="todo-text">{{ todo.text }}</span>
              <input v-else v-model="todo.text" @blur="finishEdit(todo)" @keyup.enter="finishEdit(todo)" class="todo-text">
              <button @click="removeTodo(todo)" class="delete-button">X</button>
              <button v-if="!todo.isEditing" @click="editTodo(todo)" class="edit-button">Edit</button>
              <button v-if="todo.isEditing" @click="finishEdit(todo)" class="save-button">Save</button>
          </li>
      </ul>
  </div>
</template>


<script>
export default {
    data() {
        return {
            newTodo: { text: '', done: false },
            todos: [ 
                { id: 1, text: 'Memasak', done: false, isEditing: false },
                { id: 2, text: 'Membuat tugas', done: true, isEditing: false },
                { id: 3, text: 'Shalat', done: true, isEditing: false },
                { id: 4, text: 'Rapat', done: false, isEditing: false },
                { id: 5, text: 'Membaca buku selama 1 jam', done: false, isEditing: false },
                { id: 6, text: 'Jalan-jalan', done: true, isEditing: false }
            ]
        };
    },
    methods: {
        addTodo() {
            if (this.newTodo.text.trim() !== '') {
                this.todos.push({
                    id: this.todos.length + 1,
                    text: this.newTodo.text,
                    done: false,
                    isEditing: false
                });
                this.newTodo.text = '';
            }
        },
        removeTodo(todo) {
            this.todos = this.todos.filter(t => t.id !== todo.id);
        },
        editTodo(todo) {
            todo.isEditing = true;
        },
        finishEdit(todo) {
            todo.isEditing = false;
            this.updateTodo(todo);
        },
        updateTodo(todo) {
            const index = this.todos.findIndex(t => t.id === todo.id);
            if (index !== -1) {
                this.$set(this.todos, index, todo);
            }
        }
    }
};
</script>


<style>
.todo-app {
  max-width: 500px;
  margin: auto;
}

.todo-form {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  font-size: 16px;
}

.todo-button {
  padding: 10px;
  font-size: 16px;
  margin-left: 10px;
}

.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.todo-text {
  flex: 1;
  margin-left: 10px;
  padding: 5px;
}

.delete-button {
  background: red;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}
</style>
