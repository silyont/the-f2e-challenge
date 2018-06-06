<template>
    <div class="todo--item" :class="{pinned: todo.pin}">
        <div class="todo--header d-flex align-items-center">
            <checkbox :checked="todo.completed" @check="toggleComplete"></checkbox>

            <span class="flex-1 text--title"
                  :class="{strikethrough: todo.completed, 'placeholder--color': todo.completed}"
            >
                {{ todo.title }}
            </span>

            <span class="shortcuts d-flex align-items-center">
                <a href="#"
                   @click.prevent="togglePin"
                >
                    <i class="fa-star"
                       :class="starClassObject"
                    ></i>
                </a>
                <a href="#"
                   @click.prevent=""
                >
                    <i class="far fa-edit"></i>
                </a>
                <a href="#"
                   @click.prevent="removeTodo"
                >
                    <i class="far fa-trash-alt"></i>
                </a>
            </span>
        </div>
    </div>
</template>

<script>
    import Checkbox from './Checkbox'

    export default {
        name: 'todo-item',

        components: {
            Checkbox
        },

        props: {
            todo: Object
        },

        computed: {
            starClassObject () {
                return {
                    fas: this.todo.pin,
                    far: !this.todo.pin,
                    'text--yellow': this.todo.pin
                }
            }
        },

        methods: {
            togglePin () {
                this.$emit('toggle-pin')
            },

            toggleComplete () {
                this.$emit('toggle-complete')
            },

            removeTodo () {
                this.$emit('remove')
            }
        }
    }
</script>

<style lang="stylus" scoped>
    @import '../stylus/_variables.styl'

    @media (hover:hover)
        .fa-star:hover
            color: $yellow

        .fa-edit:hover
            color: $blue

        .fa-trash-alt:hover
            color: $red

    .todo
        &--item
            width: 100%
            padding: 1.5rem 1.5rem 1.5rem 2rem
            background-color: $grey-lighten-3
            border-radius: $border-radius

            &:not(:last-child)
                margin-bottom: 0.5rem

            &.pinned
                background-color: $light-yellow

        &--header
            .checkbox
                margin-right: 1rem

            .text--title
                transition: all 0.3s ease

            .shortcuts
                margin-left: 1rem
                font-size: 1.35rem

                > *:not(:last-child)
                    margin-right: 2rem
</style>
