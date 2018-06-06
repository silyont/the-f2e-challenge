<template>
    <div id="app">
        <header>
            <div class="container height-100">
                <todo-filters v-model="selectedFilter" :filters="filters"></todo-filters>
            </div>
        </header>

        <main>
            <div class="container">
                <div class="todo--item">
                </div>
            </div>
        </main>
    </div>
</template>

<script>
    import TodoFilters from './components/TodoFilters'

    export default {
        name: 'app',

        components: {
            TodoFilters
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

                selectedFilter: 'all'
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

    main
        background-color: $grey-lighten-2
</style>
