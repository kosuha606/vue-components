<template>
    <div class="mt-25">
        <div class="doc-upload__title-block">
            <p class="doc-upload__title">{{ title }}</p>
            <form action="" v-if="canShowUploadLabel">
                <label class="doc-upload__upload">
                    <input @change="onUpload" name="UploadDocForm[file]" type="file">
                    Загрузить
                </label>
            </form>
        </div>
        <div v-if="error" class="field-error">{{ error }}</div>
        <div class="doc-upload__file" v-if="canShowUploadProccess()">
            <div class="doc-upload__file-icon">
                <img :src="mainAssetPath+'/advanced/elny-master/build/images/doc-upload/icon-doc.svg'" alt="Файл">
            </div>
            <div class="doc-upload__file-info">
                <p class="doc-upload__file-title">
                    {{ uploadedName }}
                </p>
                <div class="doc-upload__controls">
                    <button @click="onDeleteUploadFile" class="doc-upload__delete" type="button" title="Удалить">
                        <svg class="doc-upload__delete-icon">
                            <use xlink:href="#delete-small"></use>
                        </svg>
                    </button>
                    <div v-if="inUploadProgress" class="doc-upload__progress-bar">
                        <div class="doc-upload__progress" style="width: 90%"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import preloaderx from '../../vuex/PreLoader';

    export default {
        name: "UploadDocField",
        props: ['value', 'title'],
        data() {
            return {
                mainAssetPath: main_asset_path,
                inUploadProgress: false,
                uploadedName: false,
                response: false,
                error: '',
            }
        },
        computed: {
            /**
             * Можно ли показать кнопку Загрузить?
             */
            canShowUploadLabel() {
                return !this.response || this.response.result === false;
            },
        },
        methods: {
            canShowUploadProccess() {
                return (this.value || this.inUploadProgress || this.uploadedName) && !this.error;
            },
            onUpload(e) {
                this.uploadedName = e.target.value;
                this.inUploadProgress = true;
                this.error = false;
                var form = e.target.parentNode.parentNode;
                var vm = this;
                // debugger
                var data = this.$root.retrieveFormData(form);
                var formData = new FormData();
                var header = { headers: {'content-type': 'multipart/form-data'}};
                // debugger
                for (var key in data) {
                    formData.append(key, data[key]);
                }
                axios.post(
                    api_routes.fileUploadDoc,
                    formData,
                    header
                ).then(function (response) {
                    vm.inUploadProgress = false;
                    vm.response = response.data;
                    if (response.data.result) {
                        vm.$emit('input', response.data.file.name);
                        vm.$emit('change', response.data.file.name);
                    } else {
                        vm.error = response.data.error.file.join(' ');
                    }
                }).catch(function (error) {
                    vm.$log(error);
                    preloaderx.dispatch('hide');
                    vm.$root.componentRef('popup_information', 'show', {text: 'Произошла системная ошибка при загрузке файла', title: 'Ошибка', zindex: 9999});
                });
            },
            onDeleteUploadFile() {
                this.$emit('input', null);
                this.response = false;
                this.uploadedName = null;
            }
        }
    }
</script>

<style scoped>

</style>