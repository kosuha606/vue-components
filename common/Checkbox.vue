<template>
    <div class="checkbox-wrapper">
        <input
                :checked="isChecked"
                ref="checkinput"
                :id="id"
                @change="onChange"
                :value="cvalue"
                :name="name"
                :type="type ? type : 'checkbox'"
        />
        <label :for="id" class="checkbox">
            <div class="checkbox-checked"></div>
        </label>
        <label class="checkbox-label" :for="id">
            <span v-html="label"></span>
        </label>
    </div>
</template>

<script>
    export default {
        name: "Checkbox",
        props: ['label', 'name', 'id', 'value', 'cvalue', 'type', 'checked'],
        data: function () {
            return {};
        },
        computed: {
            isChecked: function () {
                if (this.checked === true) {
                    return true;
                }
                return this.cvalue == this.checked;
            }
        },
        methods: {
            triggerChange: function () {
                this.$refs.checkinput.click();
            },
            onChange: function (e) {
                this.$emit('input', e.target.value);
                this.$emit('change', e.target.value);
            }
        }
    }
</script>

<style scoped>

</style>