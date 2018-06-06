<template>
    <ul class="filter--container d-flex align-items-center">
        <li v-for="filter in filters"
            :key="`filter_${filter.text}`"
            class="filter--item text--title text-center height-100 d-flex justify-content-center align-items-center"
            :class="{selected: filter.value === value}"
            @click="changeFilter(filter)"
        >
            <span>{{ filter.text }}</span>
            <div class="indicator"></div>
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

        &--item
            font-weight: 400
            color: $dark-blue
            flex: 1
            cursor: pointer
            transition: 0.3s color ease
            position: relative
            width: 100%

            &.selected
                color: $white
                font-weight: 500

                .indicator
                    height: 5px

    .indicator
        width: 80%
        position: absolute
        bottom: 0
        left: 10%
        background-color: $dark-blue
        height: 0
        transition: 0.3s height ease
</style>
