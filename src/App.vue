<template>
  <div class="container" id="tasklist">
    <h1 class="header"> TO-DO APP </h1>

    <!-- Form -->
    <form @submit.prevent="addTodo" class="add-item">
      <div class="input-group">
        <input type="text" autocomplete="off" placeholder="Add new task" class="input" v-model="newTodo" required>
        <button type="submit" class="submit-task" :title="'Add'"></button>
      </div>
    </form>

    <!-- List -->
    <ul class="list">
      <li v-for="(todo, index) in todos" :key="index"  class="list-item">
        <input type="checkbox">
        <span @click="editTodo(index)" title="Edit item">{{ todo.text }}</span>
        <span @click="deleteTodo(index)" class="delete-btn" title="Delete item"></span>
      </li>
    </ul>

    <!-- Edit Modal -->
    <div class="modal fade" id="editTodoModal" tabindex="-1" aria-labelledby="editTodoModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="editTodoModalLabel">Edit Todo</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <input v-model="editingTodo.text" type="text" class="form-control" required>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" @click="saveEditingTodo">Save changes</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        newTodo: '',
        todos: [
          { text: 'Call A' },
          { text: 'Submit assignment' }
        ],
        editingTodo: { text: '', index: null },
      };
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim() !== '') {
          this.todos.push({ text: this.newTodo });
          this.newTodo = '';
        }
      },
      deleteTodo(index) {
        this.todos.splice(index, 1);
      },
      editTodo(index) {
        this.editingTodo = { text: this.todos[index].text, index };
        // eslint-disable-next-line no-undef
        $('#editTodoModal').modal('show');
      },
      saveEditingTodo() {
        if (this.editingTodo.text.trim() !== '') {
          this.todos[this.editingTodo.index].text = this.editingTodo.text;
          // eslint-disable-next-line no-undef
          $('#editTodoModal').modal('hide');
        }
      },
    },
  };
</script>

<style>
#app {
  width: 100vw;
  display: flex;
  justify-content: center;
}
* {
  box-sizing: border-box;
}

:root {
  --checkbox-color: rgb(202, 60, 60);
  --checkbox-shadow: rgba(238, 156, 167, 0.2);
  --add-button: rgba(255, 255, 255, 0.7);
  --add-button-shadow: rgba(238, 156, 167, 0.4);
}
body {
  width: 100%;
  height: 100vh;
  padding: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: linear-gradient(rgba(0, 0, 0, 0.8),rgba(0, 0, 0, 0.8)), url('/public/images/todo.jpg');
  background-size: cover;
  font-family: sans-serif;
  overflow: hidden;
}
.input {
  outline: none;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin-bottom: 5px;
}
.container {
  max-width: 480px;
  width: 100%;
  max-height: 100%;
  background-color: rgba(255, 255, 255, 0.3);
  padding: 25px;
  border-radius: 25px;
  overflow: auto;
  color: #222;
}
.header {
  text-align: center;
  font-size: 20px;
  line-height: 32px;
  margin: 0 0 12px 0;
  color: #272727;
}
.add-item {
  height: 40px;
  font-size: 14px;
  display: flex;
}
.input-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  border-bottom: 1px solid #272727;
  /* width: 100%;
  padding: 0 5px;
  outline: none;
  border: none;
  background-color: transparent;
  margin-right: 15px;
  color: #272727;
  box-shadow: none;
  border-radius: 0; */
}
.action-btn {
  margin-left: 10px;
  cursor: pointer;
  color: red;
}
input::placeholder {
  color: #272727;
}
.submit-task {
  width: 16px;
  height: 16px;
  flex-shrink: 0;
  border: none;
  color: #272727;
  background: var(--add-button);
  background-image: url('/public/images/submit.png');
  background-size: 18px;
  background-position: center;
  background-repeat: no-repeat;
  border-radius: 50%;
  cursor: pointer;
  box-shadow: 0 0 12px 0 rgba(238, 156, 167, 0.4);
}
.list-item {
  background-color: rgba(255, 255, 255, 0.7);
  background-repeat: no-repeat;
  background-position: center;
  background-size: 0;
  display: flex;
  align-items: center;
  padding: 8px;
  border-radius: 25px;
  margin-bottom: 12px;
}
.delete-btn {
  padding: 10px 20px;
  border-radius: 4px;
  margin-left: auto;
  margin-right: 5px;
  display: block;
  width: 16px;
  height: 16px;
  background-image: url('/public/images/delete.png');
  background-repeat: no-repeat;
  background-size: 16px;
  background-position: center;
  cursor: pointer;
}
/*
.list-item input {
  width: 16px;
  height: 16px;
  border: 1px solid #272727;
  border-radius: 50%;
  
  
  
  background-size: 0;
  transition: 0.2s;
  margin-right: 8px;
  margin-top: 4px;
  flex-shrink: 0;
  appearance: none;
}
.list-item-label {
  display: flex;
  align-items: flex-start;
  color: #272727;
  margin-right: 8px;
  font-size: 14px;
  line-height: 24px;
  position: relative;
  transition: 0.2s;
  cursor: pointer;
}
.list-item input:hover {
  border-color: var(--checkbox-color);
  box-shadow: 0 0 0 3px var(--checkbox-shadow);
}
.list-item input:checked {
  background-size: 10px;
  border: 1px solid var(--checkbox-color);
  background-color: var(--checkbox-color);
}
.list-item input:checked + span {
  color: #272727;
  text-decoration: line-through #272727;
}
.list-item-label {
  display: flex;
  align-items: flex-start;
  color: #272727;
  margin-right: 8px;
  font-size: 14px;
  line-height: 24px;
  position: relative;
  transition: 0.2s;
  cursor: pointer;
}

*/
</style>
