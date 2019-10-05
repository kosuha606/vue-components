<template>
    <div class="card-tabs">
        <div class="card-tabs-header">
            <div @click.prevent="activate(item)"
                 :class="'card-tabs-header-item '+(isActiveItem(item) ? 'card-tabs-active' : '')"
                 v-for="item in header">
                {{ item.label }}
            </div>
        </div>
        <div class="card-tabs-content">
            <div :class="'card-tabs-content-item '+(isActiveItem(item) ? 'card-tabs-active' : '')"
                 v-for="item in header">
                <slot v-bind:item="item"></slot>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Tabs",
        props: ['header', 'selected'],
        data: function () {
            return {
                activeItemId: false
            };
        },
        mounted: function () {
            if (this.header[0]) {
                this.activeItemId = this.header[0].id;
            }
            if (this.selected) {
                this.activeItemId = this.selected;
            }
        },
        methods: {
            activate: function (item) {
                this.activeItemId = item.id;
                this.$emit('tabchanged', item);
            },
            isActiveItem: function (item) {
                return this.activeItemId === item.id;
            }
        }
    }
</script>

<style scoped>

</style>