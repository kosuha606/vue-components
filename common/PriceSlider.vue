<template>
    <div :id="id">
    </div>
</template>

<script>
    import nouislider from 'nouislider';

    export default {
        name: "PriceSlider",
        props: ['id', 'max', 'min', 'step'],
        data: function () {
            return {
                slider: null
            };
        },
        mounted: function () {
            var slider = document.getElementById(this.id);
            nouislider.create(slider, {
                start: [this.min, this.max],
                tooltips: true,
                connect: true,
                format: {
                    to: function (value) {
                        return value.toFixed(0);
                    },
                    from: function (value) {
                        return value;
                    }
                },
                range: {
                    'min': this.min,
                    'max': this.max
                }
            });
            var vm = this;
            slider.noUiSlider.on('change', function (values) {
                vm.$log(values);
                vm.$emit('change', values);
            });
            this.slider = slider;
        },
        methods: {
            reset: function () {
                this.slider.noUiSlider.set([0, 999999]);
            }
        }
    }
</script>

<style scoped>

</style>