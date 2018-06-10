<template>
    <div class="todo--item" :class="{pinned: todo.pin, editing: edit}">
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
                       :class="starIconClassObject"
                    ></i>
                </a>
                <a href="#"
                   @click.prevent="edit = !edit"
                >
                    <i class="fa-edit"
                       :class="editIconClassObject"
                    ></i>
                </a>
                <a href="#"
                   @click.prevent="removeTodo"
                >
                    <i class="far fa-trash-alt"></i>
                </a>
            </span>
        </div>

        <transition name="slide-down-transition">
            <div v-if="edit" class="todo--edit">
                <div class="divider"></div>

                <div class="todo--body">
                    <div class="todo--detail">
                        <div class="todo--detail__header text--subtitle">
                            <i class="far fa-calendar-alt icon"></i>Deadline
                        </div>

                        <div class="todo--detail__content">
                            <input type="text">
                            <input type="text">
                        </div>
                    </div>

                    <div class="todo--detail">
                        <div class="todo--detail__header text--subtitle">
                            <i class="far fa-file icon"></i>File
                        </div>

                        <div class="todo--detail__content">
                            <a href="#" class="btn btn--file icon-btn d-flex align-items-center justify-content-center">
                                <i class="fas fa-plus"></i>
                            </a>
                        </div>
                    </div>

                    <div class="todo--detail">
                        <div class="todo--detail__header text--subtitle">
                            <i class="far fa-comment icon"></i>Comment
                        </div>

                        <div class="todo--detail__content">
                            <textarea></textarea>
                        </div>
                    </div>
                </div>

                <div class="todo--actions d-flex">
                    <a href="#" class="btn btn--cancel flex-1 d-flex align-items-center justify-content-center"
                       @click.prevent="cancelEdit"
                    >
                        <i class="fas fa-times"></i>Cancel
                    </a>
                    <a href="#" class="btn btn--save flex-1 d-flex align-items-center justify-content-center"
                       @click.prevent="saveEdit"
                    >
                        <i class="fas fa-plus"></i>Save
                    </a>
                </div>
            </div>
        </transition>
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

        data () {
            return {
                edit: false
            }
        },

        computed: {
            starIconClassObject () {
                return {
                    fas: this.todo.pin,
                    far: !this.todo.pin,
                    'text--yellow': this.todo.pin
                }
            },

            editIconClassObject () {
                return {
                    fas: this.edit,
                    far: !this.edit,
                    'text--blue': this.edit
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
            },

            cancelEdit () {
                this.endEdit()
            },

            saveEdit () {
                this.endEdit()
            },

            endEdit () {
                this.edit = false
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

        .btn--file:hover
            background-color: $blue

    .todo
        &--item
            width: 100%
            background-color: $grey-lighten-3
            border-radius: $border-radius
            transition: 0.3s all ease
            overflow: hidden

            &:not(:last-child)
                margin-bottom: 0.5rem

            &.pinned
                .todo--header
                    background-color: $light-yellow

            &.editing
                box-shadow: 0 4px 4px 0 $grey-lighten-1
                margin: 1.5rem 0

        &--header
            padding: 1.5rem 1.5rem 1.5rem 2rem

            .checkbox
                margin-right: 1rem

            .text--title
                transition: all 0.3s ease

            .shortcuts
                margin-left: 1rem
                font-size: 1.35rem

                > *:not(:last-child)
                    margin-right: 2rem

        &--body
            padding: 1.5rem 4.5rem

        &--detail
            &:not(:last-child)
                margin-bottom: 1.5rem

            &__header
                margin-bottom: 0.5rem

                .icon
                    width: 1.5rem

            &__content
                margin-left: 1.5rem

    input[type=text],
    textarea
        outline: none
        border: none
        padding: 0.5rem 1rem

    input[type=text]
        width: 10rem
        height: 2.25rem

        &:not(:last-child)
            margin-right: 0.5rem
            margin-bottom: 0.5rem

    textarea
        width: 100%
        height: 7.5rem
        resize: none

    .btn
        background-color: white
        height: 3.75rem
        font-size: 1.5rem

        > i
            margin-right: 0.875rem

        &.icon-btn
            border-radius: 2px

            > i
                margin: 0

        &--cancel
            color: $red

        &--save
            color: $white
            background-color: $blue

        &--file
            width: 2rem
            height: 2rem
            background-color: $grey-lighten-1
            color: $white
            font-size: 1rem
</style>
