<template>
    <ul class="filter--container">
        <li v-for="filter in filters"
            :key="`filter_${filter.text}`"
            class="filter--item text--title text-center"
            :class="{selected: filter.value === value}"
            @click="changeFilter(filter)"
        >
            {{ filter.text }}
        </li>
    </ul>
</template>

<script>
    export default {
        name: 'todo-filters',

        props: {
            value: null,
            filters: Array
        },

        methods: {
            changeFilter (filter) {
                if (filter.value === this.value) {
                    return
                }

                this.$emit('input', filter.value)
            }
        }
    }
</script>

<style lang="stylus" scoped>
    @import '../stylus/_variables.styl'

    .filter
        &--container
            margin: 0
            padding: 0
            list-style-type: none
            width: 100%
            height: 100%
            display: flex
            align-items: center

        &--item
            font-weight: 400
            color: $dark-blue
            flex: 1
            cursor: pointer
            transition: 0.3s all ease

            &.selected
                color: $white
                font-weight: 500
</style>
