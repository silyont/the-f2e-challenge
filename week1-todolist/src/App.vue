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

                <draggable :list="todos" :move="checkMove" @start="onStart" @end="onEnd">
                    <todo-item v-for="todo in filteredTodoList"
                               :key="todo.id"
                               :todo="todo"
                               @toggle-pin="toggleTodoPin(todo)"
                               @toggle-complete="toggleComplete(todo)"
                               @remove="removeTodo(todo)"
                    ></todo-item>
                </draggable>

                <div class="todo--status text--status">{{ numberOfProgressingTask }} tasks left.</div>
            </div>
        </main>
    </div>
</template>

<script>
    import TodoFilters from './components/TodoFilters'
    import TodoItem from './components/TodoItem'
    import draggable from 'vuedraggable'

    export default {
        name: 'app',

        components: {
            TodoFilters,
            TodoItem,
            draggable
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
                    },
                    {
                        id: 3,
                        title: '124215',
                        completed: true,
                        pin: false
                    },
                    {
                        id: 4,
                        title: 'asgagsasg',
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
                })
            }
        },

        methods: {
            toggleTodoPin (todo) {
                todo.pin = !todo.pin

                this.todos = this.todos.sort((a, b) => {
                    return a.pin === b.pin ? 0 : a.pin ? -1 : 1
                })
            },

            toggleComplete (todo) {
                todo.completed = !todo.completed
            },

            removeTodo (target) {
                this.todos.splice(this.todos.findIndex(todo => todo.id === target.id), 1)
            },

            checkMove ({ draggedContext, relatedContext }) {
                const relatedElement = relatedContext.element
                const draggedElement = draggedContext.element

                return relatedElement && draggedElement.pin === relatedElement.pin
            },

            onStart ({ item }) {
                item.classList.add('hide')
            },

            onEnd ({ item }) {
                item.classList.remove('hide')
            }
        }
    }
</script>

<style lang="stylus">
    @import 'stylus/_variables.styl'

    .hide
        opacity: 0

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
