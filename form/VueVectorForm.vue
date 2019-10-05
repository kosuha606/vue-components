<template>
    <div class="vue-vector-form">
        <slot v-bind:component="component"></slot>
    </div>
</template>

<script>
    import {clone} from 'lodash';

    /**
     * VectorForm - форма хранящая в себе неограниченное кол-во форм любого типа
     *
     * В этом компоненте описан механизм взаимодействия с такими формами
     */
    export default {
        name: "VueVectorForm",
        props: ['formName'],
        data: function() {
            return {
                component: this
            };
        },
        computed: {
            rootVectorFormData() {
                return this.$root.forms.data.vector_form.data;
            },
            rootVectorFormActive() {
                return this.$root.forms.data.vector_form.activeForm;
            },
            rootVectorFormForms() {
                return this.$root.forms.data.vector_form.forms;
            }
        },
        methods: {
            test() {
                return 'test';
            },
            /**
             * Активная форма вектора
             * @returns {*}
             */
            vectorActiveForm() {
                return this.rootVectorFormData[this.formName][this.rootVectorFormActive[this.formName]];
            },
            /**
             * Получить индекс активной формы
             */
            vectorActiveFormIndex() {
                return this.rootVectorFormActive[this.formName];
            },
            /**
             * Добавить данные к форме
             * и установоить индекс активной формы на новую форму
             */
            vectorFormAdd(labelKey, labelValue) {
                let newItem = clone(this.rootVectorFormForms[this.formName]);
                newItem[labelKey] = labelValue;
                this.rootVectorFormData[this.formName].push(newItem);
                this.rootVectorFormActive[this.formName] = this.rootVectorFormData[this.formName].length-1;
            },
            /**
             * Всего записей в форме
             * @returns {*}
             */
            vectorFormTotal() {
                return this.rootVectorFormData[this.formName].length;
            },
            /**
             * Удалить данные типа формы
             * и поставить индекс на первую форму
             */
            vectorFormRemove() {
                this.rootVectorFormData[this.formName].splice(this.rootVectorFormActive[this.formName], 1);
                this.rootVectorFormActive[this.formName] = 0;
            },
            /**
             * Изменение активной формы
             * @param event
             */
            vectorOnChangeActiveForm(event) {
                this.rootVectorFormActive[this.formName] = event.target.value;
                this.$forceUpdate();
            },
            /**
             * Получить карту значений типа формы
             * @param to
             */
            vectorFormMap(to) {
                let result = [];
                this.rootVectorFormData[this.formName].forEach(function(value, index) {
                    result.push({
                        index: index,
                        to: value[to]
                    });
                });
                return result;
            },
        }
    }
</script>

<style scoped>

</style>