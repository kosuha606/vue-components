<template>
    <div class="alert-holder" v-show="visible">
        <div class="alert">
            <span v-html="message"></span>
            <a @click="hide" href="#">&times;</a>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Alert",
        data: function () {
            return {
                visible: false,
                message: ''
            };
        },
        methods: {
            show: function (message) {
                var vm = this;
                this.message = message;
                this.visible = true;
                setTimeout(function () {
                    vm.$emit('show', vm);
                }, 250);
            },
            hide: function () {
                this.visible = false;
                this.$emit('hide', this);
            }
        }
    }
</script>

<style scoped>

</style>