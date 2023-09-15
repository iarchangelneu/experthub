<template>
    <div class="register">
        <div class="logo">
            <img src="@/assets/img/registerlogo.svg" alt="">
        </div>

        <div class="register__form">
            <div>
                <h1>Регистрация</h1>

                <div class="usertype">
                    <button @click="userType = 'buyer'" :class="{ activebtn: userType == 'buyer' }">Пользователь</button>
                    <button @click="userType = 'seller'" :class="{ activebtn: userType == 'seller' }">Эксперт</button>
                </div>

                <div class="inputs">
                    <input type="email" placeholder="Эл.почта" v-model="email" ref="email">
                    <input type="password" placeholder="Пароль" v-model="password" ref="password">
                    <input type="password" placeholder="Повторите пароль" v-model="repeat__password" ref="repeat__password">
                    <input type="text" placeholder="Имя и фамилия" v-model="name" ref="name">
                    <select name="select" id="select" v-model="selectedCategory" ref="select" v-if="userType == 'seller'">
                        <option value="0" disabled selected>Специализация</option>
                        <option v-for="(category, index) in categories" :key="index" :value="category.id">{{ category.name
                                                    }}
                        </option>

                    </select>


                    <label class="custom-checkbox">
                        <input type="checkbox" v-model="checked">
                        <p class="checkbox-text m-0" ref="checked">Принимаю условия <NuxtLink to='/terms'>
                                Пользовательского<br>
                                соглашения
                            </NuxtLink> и
                            <NuxtLink to='/polytics'>Политики конфиденциальности</NuxtLink>

                        </p>
                    </label>
                    <small>{{ error }}</small>
                    <div class="registerbtn text-center">
                        <button @click="register">Зарегистироваться</button>
                    </div>

                    <div class="text-center haveacc">
                        <span>Уже есть аккаунт? <NuxtLink to="/login">Вход</NuxtLink></span>
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
            userType: 'seller',
            checked: false,
            selectedCategory: 0,
            email: '',
            password: '',
            repeat__password: '',
            name: '',
            checked: false,
            error: '',
            pathUrl: 'https://experthub.kz',
            categories: [
                {
                    id: 1,
                    name: 'Бухгалтерия'
                },
                {
                    id: 2,
                    name: 'Документация'
                },
                {
                    id: 3,
                    name: 'Бизнес-аналитика'
                },
                {
                    id: 4,
                    name: 'Логистика'
                },
                {
                    id: 5,
                    name: 'Юридические услуги'
                },
                {
                    id: 6,
                    name: 'Страхование'
                },
                {
                    id: 7,
                    name: 'Консалтинг'
                },
                {
                    id: 8,
                    name: 'Финансовый аудит'
                },
                {
                    id: 9,
                    name: 'Маркетинг'
                },
            ],
        }
    },
    methods: {
        register() {
            const buyer = `${this.pathUrl}/api/main/registration/buyer`
            const seller = `${this.pathUrl}/api/main/registration/seller`
            const csrf = this.getCSRFToken()

            if (this.email !== '') {
                this.error = ''
                this.$refs.email.style.borderColor = '#fff'

                if (this.password != 0 && this.password == this.repeat__password) {
                    this.$refs.password.style.borderColor = '#fff'
                    this.$refs.repeat__password.style.borderColor = '#fff'
                    this.error = ''

                    if (this.checked) {
                        this.$refs.checked.style.color = '#fff'
                        if (this.name !== '') {
                            this.$refs.name.style.borderColor = '#fff'
                            this.error = ''
                            if (this.userType == 'seller') {
                                if (this.selectedCategory > 0) {


                                    this.$refs.select.style.borderColor = '#fff'
                                    this.error = ''
                                    axios.defaults.headers.common['X-CSRFToken'] = csrf;
                                    axios
                                        .post(seller, { first_name: this.name, password: this.password, username: this.email, email: this.email, category: this.selectedCategory })
                                        .then((res) => {

                                            document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                                            console.log(res)
                                            localStorage.setItem('accountType', res.data.redirect_url)
                                            window.location.href = res.data.redirect_url
                                        })
                                        .catch((error) => {
                                            if (error.response.data.detail) {
                                                this.error = error.response.data.detail
                                            }
                                            this.error = 'Ошибка на стороне сервера'
                                            console.log(error.response);
                                        });
                                }
                                else {
                                    this.error = 'Выберите специализацию'
                                    this.$refs.select.style.borderColor = 'red'
                                }

                            }
                            else {
                                axios.defaults.headers.common['X-CSRFToken'] = csrf;
                                this.error = ''
                                axios
                                    .post(buyer, { first_name: this.name, email: this.email, password: this.password, username: this.email, email: this.email })
                                    .then((res) => {

                                        document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                                        console.log(res)
                                        localStorage.setItem('accountType', res.data.redirect_url)
                                        window.location.href = '/'
                                    })
                                    .catch((error) => {
                                        console.log(error.responseS);
                                        this.error = error.response.data.detail
                                    });
                            }

                        }
                        else {
                            this.error = 'Вы не заполнили имя'
                            this.$refs.name.style.borderColor = 'red'
                        }
                    }
                    else {
                        this.$refs.checked.style.color = 'red'
                        this.error = 'Вы не согласились с условиями'
                    }
                }
                else {
                    this.error = 'Пароли не совпадают'
                    this.$refs.password.style.borderColor = 'red'
                    this.$refs.repeat__password.style.borderColor = 'red'
                }
            }
            else {
                this.error = 'Вы не указали почту'
                this.$refs.email.style.borderColor = 'red'
            }
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
    title: 'Регистрация | Experthub',
    ogTitle: 'Регистрация | Experthub',
    description: 'Регистрация | Experthub',
    ogDescription: 'Регистрация | Experthub',
})
</script>
<style lang="scss" scoped>
.register {
    padding: 60px 0 50px;

    .logo {
        display: flex;
        justify-content: center;
    }

    small {
        color: red;
        font-family: var(--mon);
    }

    .register__form {
        margin-top: 86px;
        display: flex;
        justify-content: center;
        align-items: center;

        div {
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
                padding: 0 48.5px;

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
            }
        }
    }
}
</style>