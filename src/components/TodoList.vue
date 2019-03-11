<template>
  <div>
      <input 
        type="text" 
        class="todo-input" 
        placeholder="Create new To-do Task"
        v-model="newTodo"
        @keyup.enter="addTodo">
        
      <div 
        v-for="(todo, index) in todos" 
        :key="todo.id" 
        class="todo-item">

        <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed">
            <div v-if="!todo.editing" @dblclick="editTodo(todo)" 
                class="todo-item-label" 
                :class="{ completed : todo.completed }">
                {{ todo.title}}</div>
            <input v-else class="todo-item-edit" type="text" 
            v-model="todo.title"
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)" v-focus>
        </div>

        <div class="remove-item" @click="removeTodo(index)">
            &times;
        </div>
        
      </div>
      <div class="extra-container">
        <div>{{ remaining }} items left</div>
      </div>

      <div class="extra-container">
        <div id="wrapper">
            <button @click="clearCompleted">Clear Completed</button>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      todos: [
          {
              'id': 1,
              'title': 'Take out trash',
              'completed': false,
              'editing': false,
          },
          {
              'id': 2,
              'title': 'Buy food',
              'completed': false,
              'editing': false,
          },
      ]
    }
  },

  computed: {
      remaining() {
          return this.todos.filter(todo => !todo.completed).length
      }
  },

  directives: {
      focus: {
          inserted: function(el) {
              el.focus()
          }
      }
  },
methods: {
    addTodo() {

        if (this.newTodo.trim().length == 0) { // making sure its not empty
            return
        }

        this.todos.push({
            id: this.idForTodo,
            title: this.newTodo,
            completed: false,
        })

        this.newTodo = ''
        this.idForTodo++
        alert('To do task created!')
    },

    editTodo(todo) {
        todo.editing = true
    },

    doneEdit(todo) {
        todo.editing = false
    },

    removeTodo(index) {
        this.todos.splice(index, 1)
    },

    clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed)
    }
}

}
</script>

<style>

.todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
}

.todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.remove-item {
    cursor: pointer;
    margin-left: 14px;
}

.todo-item-left {
    display:flex;
    align-items: center;
}

.todo-item-label {
    padding: 10px;
    border: 1px solid #ccc;
    margin-left: 12px;
}

.todo-item-edit {
    font-size: 24px;
    color: black;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
}

.completed {
    text-decoration: line-through;
    color: black;
}

.extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid black;
    padding-top: 14px;
    margin-bottom: 14px
}

#wrapper {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

button {
  width: 200px;
}

.active {
    background: lightgreen;
}

</style>
