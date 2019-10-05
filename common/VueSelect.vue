<template>
    <select :id="id" class="select">
        <option :value="option.value" :selected="selected==option.value" v-for="option in options">
            {{ option.text }}
        </option>
    </select>
</template>

<script>
    import slimselect from 'slim-select';

    export default {
        name: "VueSelect",
        props: ['id', 'options', 'selected'],
        data: function () {
            return {};
        },
        mounted: function () {
            var vm = this;
            var select = new slimselect({
                select: '#' + this.id,
                searchPlaceholder: 'Поиск',
                onChange: function () {
                    vm.onChange(select);
                }
            });
        },
        methods: {
            onChange: function (select) {
                this.$emit('change', select);
            }
        }
    }
</script>

<style scoped>

</style>