<template>
    <div class="socials" id="uLoginCustom">
        <div class="socials-item" data-uloginbutton="twitter">
            <img src="/images/pngfigma/socials/tw.png" alt="Вход через Твитер">
        </div>
        <div class="socials-item" data-uloginbutton="google">
            <img src="/images/pngfigma/socials/gg.png" alt="Вход через Google">
        </div>
        <div class="socials-item" data-uloginbutton="facebook">
            <img src="/images/pngfigma/socials/fb.png" alt="Вход через Facebook">
        </div>
        <div class="socials-item" data-uloginbutton="odnoklassniki">
            <img src="/images/pngfigma/socials/ok.png" alt="Вход через Одноклассники">
        </div>
        <div class="socials-item" data-uloginbutton="vkontakte">
            <img src="/images/pngfigma/socials/vk.png" alt="Вход через ВК">
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import scriptjs from 'scriptjs';

    export default {
        name: "Ulogin",
        data: function () {
            return {
                id: 'uLoginCustom'
            };
        },
        mounted: function () {
            let vm = this;
            scriptjs("https://ulogin.ru/js/ulogin.js", function() {
                window.socialLogin = function (token) {
                    var url = '/public/socials/social?token=' + token;
                    axios({
                        method: 'post',
                        url: url
                    }).then(function (response) {
                        if (response.data.result && response.data.result.url) {
                            window.location.href = response.data.result.url;
                        }
                    });
                };
                uLogin.customInit(vm.id, {
                    redirect_uri: '/public/socials/social',
                    display: 'buttons',
                    callback: 'socialLogin'
                });
            });
        },
        methods: {}
    }
</script>

<style scoped>

</style>