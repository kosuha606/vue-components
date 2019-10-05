<template>
    <div @change="onSlotChange" :class="'field-wrapper '+(error ? 'field-wrapper-error' : '')">
        <slot></slot>
        <div v-if="error" class="field-error">
            <div v-html="error"></div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "VueValidate",
        props: ['rootforms', 'formname', 'fieldname'],
        data: function () {
            return {
                error: false
            };
        },
        mounted: function () {
            var vm = this;
            this.$root.$on('update', function (root) {
                vm.error = false;
                if (
                    vm.$root.forms.errors[vm.formname] !== undefined
                ) {
                    var errorsData = vm.$root.forms.errors[vm.formname];
                    let fieldNameParts = vm.fieldname.split('.');
                    fieldNameParts.forEach(function (value) {
                        if (!errorsData) {
                            return;
                        }
                        errorsData = errorsData[value];
                    });
                    if (errorsData) {
                        vm.error = errorsData.join('<br>');
                    }
                }
                vm.$forceUpdate();
            });
        },
        computed: {},
        methods: {
            onSlotChange() {
                this.error = false;
            }
        }
    }
</script>

<style scoped>

</style>