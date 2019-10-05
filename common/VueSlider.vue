<template>
    <div :id="id">
        <div v-for="item in iterable">
            <slot v-bind:item="item"></slot>
        </div>
    </div>
</template>

<script>
    import {extend} from 'lodash';
    import { tns } from "../../../node_modules/tiny-slider/src/tiny-slider"

    export default {
        name: "VueSlider",
        props: ['id', 'iterable', 'config'],
        data: function () {
            return {
                slider: null
            };
        },
        mounted: function () {
            var config = {container: '#' + this.id};
            if (this.config) {
                extend(config, this.config);
            } else {
                extend(config, {
                    items: 1,
                    mouseDrag: true,
                    loop: false,
                    responsive: {
                        768: {
                            items: 2
                        },
                        1100: {
                            items: 4
                        }
                    }
                });
            }
            var slider = new tns(config);
            var vm = this;
            setTimeout(function () {
                vm.$root.lazyLoadInstance.update();
            }, 200);
            this.slider = slider;
            vm.$emit('changeslide', vm);
            slider.events.on("indexChanged", function () {
                vm.$emit('changeslide', vm);
            });
        },
        methods: {
            info: function () {
                return this.slider.getInfo();
            },
            next: function () {
                this.slider.goTo('next');
            },
            prev: function () {
                this.slider.goTo('prev');
            }
        }
    }
</script>

<style scoped>

</style>