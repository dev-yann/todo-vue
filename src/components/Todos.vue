<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input type="text" class="new-todo" placeholder="add todo" v-model="newTodo" @keyup.enter="addTodo"/>
        </header>
        <section class="main">
            <!-- tous cocher -->
            <input type="checkbox" class="toggle-all" v-model="allDone"/>
            <ul class="todo-list">

                <!-- todo.completed true alors ajout de la class completed -->
                <li class="todo" v-for="todo in filteredTodo" v-bind:class="{completed : todo.completed, editing: todo === editing}">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle"/>
                        <label @dblclick="editTodo(todo)">{{todo.name}}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" v-focus="todo === editing" @blur="doneEdit" @keyup.esc="cancelEdit"/>
                </li>
            </ul>
        </section>
        <footer v-show="one" class="footer">


                <span class="todo-count"><strong> {{nbtache}} </strong> tâche à faire</span>

        </footer>

        <!-- Les filtres -->
        <footer v-show="one" class="footer">
            <ul class="filters">
                <li><a href="#" :class="{selected : this.filter === 'all' }" @click.prevent="filter = 'all'">Toutes les taches</a></li>
                <li><a href="#" :class="{selected : this.filter === 'todo' }" @click.prevent="filter = 'todo'">Toutes les taches à faire</a></li>
                <li><a href="#" :class="{selected : this.filter === 'done' }" @click.prevent="filter = 'done'">Toutes les taches effectuées</a></li>
            </ul>
        </footer>
        <button class="clear-completed" v-show="done" @click="deleteDone">Delete all todo done</button>
    </section>
</template>


<script>
    import vue from 'vue'
    export default {
      data () {
        return {
          todos: [],
          newTodo: '',
          completed: false,
          filter: 'all',
          index: 0,
          justOne: false,
          editing: null,
          oldTodo: null
        }
      },
      methods: {
        cancelEdit () {
          // le but c'est de recuperer la valeur initial avant le changement

          this.editing.name = this.oldTodo
          this.doneEdit()
        },
        doneEdit () {
          this.editing = null
        },
        editTodo (todo) {
          this.editing = todo
          this.oldTodo = todo.name
        },
        addTodo () {
          if (this.newTodo !== '') {
            this.todos.push({
              completed: false,
              name: this.newTodo,
              index: this.todos.length
            })
            this.newTodo = ''
          }
        },
        deleteTodo (todo) {
          // suprimer du tableau
          this.todos.splice(todo.index, 1)
        },
        deleteDone () {
          // avec foreach ca marche pas alors filtre...
          this.todos = this.todos.filter(todo => !todo.completed)
        }
      },
      computed: {
        maj () {
          this.todos.forEach((todo, index) => {
            todo.index = index
          })
        },
        done () {
          // todo: retourne vrai si une tache à été check
          if (this.todos.filter(todo => todo.completed).length > 0) {
            return true
          }
        },
        one () {
          if (this.todos.length > 0) {
            return true
          }
        },
        allDone: {
          get () {
            return this.nbtache === 0
          },
          set (value) {
            this.todos.forEach((todo) => {
              todo.completed = value
            })
          }
        },
        nbtache () {
          // retourne le nombre de tache à false
          return this.todos.filter(todo => !todo.completed).length
        },
        filteredTodo () {
          if (this.filter === 'todo') {
            return this.todos.filter(todo => !todo.completed)
          } else if (this.filter === 'done') {
            return this.todos.filter(todo => todo.completed)
          } else {
            return this.todos
          }
        }
      },
      directives: {
        // el : l'element sur lequel est bindé la directive et la valeur
        focus (el, val) {
          if (val) {
            vue.nextTick(_ => {
              el.focus()
            })
          }
        }
      }
    }
</script>
<!-- Insertion du css -->
<style src="./todo.css"></style>