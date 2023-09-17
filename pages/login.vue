<template>
    <div class="register">
        <div class="logo">
            <img src="@/assets/img/registerlogo.svg" alt="">
        </div>

        <div class="register__form">
            <div>
                <h1>Авторизация</h1>

                <div class="inputs">
                    <input type="email" placeholder="Эл.почта" v-model="email" ref="email">
                    <input type="password" placeholder="Пароль" v-model="password" ref="password">

                    <div class="registerbtn text-center">
                        <button @click="login()">Войти</button>
                    </div>

                    <div class="text-center haveacc">
                        <span>Еще нет аккаунта? <NuxtLink to="/register">Регистрация</NuxtLink></span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            email: '',
            password: '',
            pathUrl: 'https://experthub.kz',
            error: '',
        }
    },
    methods: {
        login() {
            const path = `${this.pathUrl}/api/main/authorization`
            const csrf = this.getCSRFToken()

            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, { username: this.email, password: this.password })
                .then((res) => {



                    document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                    localStorage.setItem('accountType', res.data.redirect_url)
                    if (res.data.redirect_url == 'buyer-account') {
                        window.location.href = '/'
                    }
                    if (res.data.redirect_url == 'seller-account') {
                        window.location.href = '/seller-account'
                    }


                    console.log(res)
                })
                .catch((error) => {
                    console.log(error);
                    this.error = error.response.data.non_field_errors.toString()
                });
        },
        getCategoryName(categoryId) {
            const category = this.categories.find((c) => c.id === categoryId);
            return category ? category.name : "";
        },
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Авторизация | Experthub',
    ogTitle: 'Авторизация | Experthub',
    description: 'Авторизация | Experthub',
    ogDescription: 'Авторизация | Experthub',
})
</script>
<style lang="scss" scoped>
.register {
    padding: 60px 0 50px;

    @media (max-width: 1024px) {
        padding: 50px 20px;
    }

    .logo {
        display: flex;
        justify-content: center;
    }

    .register__form {
        margin-top: 86px;
        display: flex;
        justify-content: center;
        align-items: center;

        @media (max-width: 1024px) {
            width: 100%;
        }

        div {
            @media (max-width: 1024px) {
                width: 100%;
            }

            .haveacc {
                span {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 110%;
                    font-family: var(--mon);
                    color: #fff;
                }
            }

            .registerbtn {
                padding: 0 100px;

                button {
                    border-radius: 10px;
                    background: #fff;
                    border: 0;
                    padding: 10px;
                    width: 100%;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
                }
            }

            .inputs {
                label {
                    margin: 0 !important;
                }

                margin-top: 30px;
                display: flex;
                flex-direction: column;
                gap: 30px;

                p {
                    font-size: 16px;
                    font-weight: 400;
                }

                a {
                    color: #fff;
                    text-decoration: underline !important;
                }

                input,
                select {
                    display: block;
                    border-radius: 10px;
                    border: 2px solid #fff;
                    padding: 10px 15px;
                    background: transparent;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                }

                option {
                    background: #282828;
                    font-family: var(--mon);
                }
            }

            .usertype {
                display: flex;
                gap: 31px;
                margin-top: 30px;

                .activebtn {
                    color: #000 !important;
                    background: #fff !important;
                }

                button {
                    flex: 1;
                    padding: 10px 0;
                    background: #0072EE;
                    border-radius: 10px;
                    border: 0;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                    transition: all .3s ease;

                    &:hover {
                        color: #000;
                        background: #fff;
                    }
                }
            }

            h1 {
                font-size: 64px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                margin: 0;

                @media (max-width: 1024px) {
                    font-size: 40px;
                    text-align: center;
                }
            }
        }
    }
}
</style>