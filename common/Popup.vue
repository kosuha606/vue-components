<template>
    <transition name="slide-fade">
        <div v-if="visible" class="popups-wrapper" @click="hide"
             :style="'display:'+(visible ? 'block' : 'none')+(data.zindex ? ';z-index:'+data.zindex : '')">
            <slot v-bind:data="data"></slot>
        </div>
    </transition>
</template>

<script>
    import Components from '../../bundled_async_components_simple';

    export default {
        name: "Popup",
        props: ['type', 'title'],
        components: Components,
        data: function () {
            return {
                visible: false,
                sidebars: function () {
                    sidebar++;
                    this.$log(sidebar);
                    return sidebar;
                },
                data: {}
            };
        },
        computed: {
            rootSidebars: function () {
                return this.$root.sidebars;
            }
        },
        methods: {
            popup: function (e) {
                this.$log(e.target);
                e.stopPropagation();
            },
            getId: function () {
                return this._uid;
            },
            closeAllSidebars: function () {
                this.$root.closeAllSidebars();
            },
            showSidebar: function (id) {
                this.$root.showSidebar(id);
            },
            hideSidebar: function (id) {
                this.$root.hideSidebar(id);
            },
            calculateMiddle() {
                var self = this;
                let $npopup = self.$el.querySelector('.n-popup');
                let bodyHeight = window.innerHeight;
                let elHeight = $npopup.offsetHeight;
                if ((bodyHeight-elHeight) > 200) {
                    $npopup.classList.add('n-popup-middle');
                } else {
                    $npopup.classList.remove('n-popup-middle');
                }
                self.$log(elHeight, bodyHeight);
            },
            show: function (data) {
                var self = this;
                this.visible = true;
                if (data) {
                    this.data = data;
                }

                setTimeout(function () {
                    self.calculateMiddle();
                }, 50);

                setTimeout(function () {
                    self.$emit('show', self);
                }, 250);

            },
            hide: function (e) {
                if (e.target === undefined || e.target.classList.contains('popups-wrapper') || e.target.classList.contains('popup-close')) {
                    this.visible = false;
                    this.$emit('hide', this);
                }
            }
        }
    }
</script>

<style scoped>

</style>