<template>
    <div :class="'accordeon'+(visible ? ' open' : '')">
        <div @click.prevent="toggleContent" class="accordeon-title">
            {{ title }}
            <slot name="reset"></slot>
            <div class="accordeon-arrow">
                <img src="/images/pngfigma/arrow-ios-down.png" alt="">
            </div>
        </div>
        <transition name="fade">
            <div class="accordeon-content" v-if="visible">
                <slot v-bind:state="data"></slot>
            </div>
        </transition>
    </div>
</template>

<script>
    export default {
        name: "Accordeon",
        props: ['title'],
        data: function () {
            return {
                visible: false,
                data: {},
                message: ''
            };
        },
        methods: {
            toggleContent: function () {
                this.visible = !this.visible;
            }
        }
    }
</script>

<style scoped>

</style>