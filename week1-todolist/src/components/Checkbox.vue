<template>
    <label class="checkbox d-flex align-items-center justify-content-center"
         :class="{'checked': checked}"
         @click.stop.prevent="$emit('check')"
    >
        <transition name="scale-transition">
            <i v-show="checked" class="fas fa-check"></i>
        </transition>

        <input type="checkbox" :checked="checked">
    </label>
</template>

<script>
    export default {
        name: 'checkbox',

        props: {
            checked: Boolean
        }
    }
</script>

<style lang="stylus" scoped>
    @import '../stylus/_variables.styl'

    input[type=checkbox]
        display: none

    .checkbox
        width: 1.5rem
        height: 1.5rem
        background-color: $white
        color: $white
        cursor: pointer
        transition: 0.3s background-color ease
        position: relative

        &.checked
            background-color: $blue

    .fa-check
        transform: scale(1)

    .scale-transition
        &-enter-active,
        &-leave-active
            transition: 0.3s transform ease

        &-enter,
        &-leave-to
            transform: scale(0)
</style>
