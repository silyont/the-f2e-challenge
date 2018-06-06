<template>
    <div id="app" class="d-flex flex-column">
        <header>
            <div class="container height-100 fluid">
                <todo-filters v-model="selectedFilter" :filters="filters"></todo-filters>
            </div>
        </header>

        <main class="flex-1">
            <div class="container">
                <div class="todo--add">
                    <span class="icon"><i class="fas fa-plus"></i></span>
                    <span class="text--title placeholder">Add Task</span>
                </div>

                <todo-item v-for="(todo, index) in filteredTodoList"
                           :key="todo.id"
                           :todo="todo"
                           @toggle-pin="toggleTodoPin(todo)"
                           @toggle-complete="toggleComplete(todo)"
                           @remove="removeTodo(index)"
                ></todo-item>

                <div class="todo--status text--status">{{ numberOfProgressingTask }} tasks left.</div>
            </div>
        </main>
    </div>
</template>

<script>
    import TodoFilters from './components/TodoFilters'
    import TodoItem from './components/TodoItem'

    export default {
        name: 'app',

        components: {
            TodoFilters,
            TodoItem
        },

        data () {
            return {
                filters: [
                    {
                        text: 'My Tasks',
                        value: 'all'
                    },
                    {
                        text: 'In Progress',
                        value: 'progress'
                    },
                    {
                        text: 'Completed',
                        value: 'completed'
                    }
                ],

                selectedFilter: 'all',

                todos: [
                    {
                        id: 1,
                        title: 'How are you',
                        completed: false,
                        pin: false
                    },
                    {
                        id: 2,
                        title: 'Wow',
                        completed: true,
                        pin: false
                    }
                ]
            }
        },

        computed: {
            numberOfProgressingTask () {
                return this.todos.filter(todo => !todo.completed).length
            },

            filteredTodoList () {
                return this.todos.filter(todo => {
                    return this.selectedFilter === 'all' ? true : this.selectedFilter === 'completed' ? todo.completed : !todo.completed
                }).sort((a, b) => {
                    return a.pin === b.pin ? 0 : a.pin ? -1 : 1
                })
            }
        },

        methods: {
            toggleTodoPin (todo) {
                todo.pin = !todo.pin
            },

            toggleComplete (todo) {
                todo.completed = !todo.completed
            },

            removeTodo (index) {
                this.todos.splice(index, 1)
            }
        }
    }
</script>

<style lang="stylus">
    @import 'stylus/_variables.styl'

    header
        background-color: $blue
        width: 100%
        height: 76px

    .todo--add
        width: 100%
        border: 2px solid $grey-lighten-1
        padding: 1.125rem 2rem
        color: $grey-lighten-1
        background-color: $grey-lighten-3
        border-radius: $border-radius
        margin: 1.5rem 0

        .icon
            width: 1.5rem
            margin-right: 1rem
            font-size: 1.5rem

        .placeholder
            color: $grey-lighten-1
            font-weight: 400

    .todo--status
        margin-top: 0.5rem
        margin-left: 2rem
</style>
