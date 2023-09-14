<template>
    <div class="account">
        <h1>Личный кабинет</h1>

        <div class="navs">
            <div>
                <button @click="activeTab = 0" :class="{ activeTab: activeTab == 0 || activeTab == 5 }">Мои услуги</button>
                <button @click="activeTab = 1" :class="{ activeTab: activeTab == 1 }">Мои продажи</button>
            </div>
            <div>
                <button @click="activeTab = 2" :class="{ activeTab: activeTab == 2 }">Транзакции</button>
                <button @click="activeTab = 3" :class="{ activeTab: activeTab == 3 }">Обратная связь</button>
            </div>
            <div>
                <button @click="activeTab = 4" :class="{ activeTab: activeTab == 4 }">Аккаунт</button>
            </div>
        </div>

        <div class="myproducts" v-if="activeTab == 0">
            <div class="text-center">
                <button @click="activeTab = 5">Добавить товар</button>
            </div>

            <div class="products">
                <div class="product__item" v-for="item in products" :key="item.id">
                    <img :src="pathUrl + '/api' + item.main_image" alt="">

                    <div class="product__info">
                        <div class="names">
                            <h1>{{ item.name }}</h1>
                        </div>
                        <div class="buttons">
                            <h1>{{ item.price.toLocaleString() + ' ₸' }}</h1>

                            <div class="btns">
                                <button @click="openEditTab(item.id)">Изменить</button>
                                <NuxtLink :to="'/product/' + item.id">Страница товара</NuxtLink>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>

        <div class="mysales" v-if="activeTab == 1">
            <div class="products">
                <div class="product__item" v-for="item in sales" :key="item.id">
                    <img :src="pathUrl + '/api' + item.products.main_image" alt="">

                    <div class="product__info">
                        <div class="names">
                            <h1>{{ item.products.name }}</h1>
                            <span>Дата продажи: {{ formatDate(item.date) }} </span>
                        </div>
                        <div class="buttons">
                            <h1>{{ item.products.price.toLocaleString() + ' ₸' }} </h1>

                            <div class="btns">
                                <button @click="createChat(item.buyer.id, item.buyer.user.first_name)">Чат с
                                    покупателем</button>
                                <NuxtLink :to="'/product/' + item.products.id">Страница товара</NuxtLink>
                            </div>
                        </div>
                    </div>
                </div>


            </div>
        </div>
        <div class="chats" v-if="activeTab == 3">

            <div class="chat__item" v-for="chat in chats" :key="chat.id">
                <div>
                    <h2>{{ chat.buyer.user.first_name }}</h2>
                    <!-- <small>23.07.2023 14:47</small> -->
                </div>

                <div class="justify-content-end">
                    <button @click="openChat(chat.id, chat.buyer.user.first_name)">Открыть чат</button>
                </div>
            </div>
        </div>

        <div class="accs" v-if="activeTab == 4">
            <div class="account__profile">
                <div class="profile__left">
                    <div class="avatar w-100">
                        <div class="avik">
                            <label for="fileInput" class="editava ml-0" @click="openFileInput">
                                <span>Изменить фото</span>
                            </label>
                            <input type="file" id="fileInput" ref="fileInput" accept="image/*"
                                style="display: none !important;" @change="handleFileChange">
                            <!-- Используем avatarUrl для отображения выбранного изображения или дефолтного изображения -->
                            <img :src="avatarUrl" alt="" loading="lazy">
                        </div>

                    </div>
                    <div class="data">
                        <label for="name">Отображаемое Имя</label>
                        <input type="text" placeholder="Ваше имя" class="w-100" v-model="name">
                    </div>
                    <div class="data">
                        <label for="name">E-mail</label>
                        <input class="w-100" type="email" placeholder="E-mail" v-model="email">
                    </div>

                    <div class="data">
                        <label for="name">Пароль</label>
                        <input class="w-100" type="password" placeholder="Пароль">
                    </div>

                </div>
                <div class="profile__right w-100">
                    <label for="desc">Описание профиля</label>
                    <textarea name="desc" id="desc" cols="30" rows="17" v-model="description"></textarea>


                    <div class="buttons">
                        <button @click="editAccount()" ref="edit">Сохранить изменения</button>
                        <button @click="logOut()">Выйти из аккаунта</button>
                    </div>
                </div>


            </div>




        </div>


        <TheTrans v-if="activeTab == 2" :transactions="transactions"></TheTrans>
        <CreateProduct v-if="activeTab == 5"></CreateProduct>
        <EditProduct v-if="activeTab == 6" :productId="sendId"></EditProduct>
        <TheMessanger v-if="activeTab == 7" :chatId="chatId" :name="chatName"></TheMessanger>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            activeTab: 0,
            account: [],
            description: '',
            name: '',
            sales: [],
            photo: '',
            products: [],
            chats: [],
            chatName: '',
            sendId: null,
            chatId: null,
            transactions: [],
            category: null,
            avatar: null,
            pathUrl: 'https://experthub.kz',
            myId: null,
        }
    },
    computed: {
        avatarUrl() {
            if (this.avatar) {
                // Используем URL объекта File, если avatar существует
                return URL.createObjectURL(this.avatar);
            } else {
                // Используем дефолтное изображение, если avatar не существует
                return this.photo;
            }
        },
    },
    methods: {
        formatDate(dateString) {
            const date = new Date(dateString);
            const formattedDate = `${date.getDate().toString().padStart(2, '0')}.${(date.getMonth() + 1).toString().padStart(2, '0')}.${date.getFullYear()}`;
            return formattedDate;
        },
        openEditTab(productId) {
            this.activeTab = 6;
            this.sendId = productId;
        },
        openChat(chatId, chatName) {
            this.activeTab = 7;
            this.chatId = chatId;
            this.chatName = chatName
        },
        createChat(id, name) {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/messanger/new-chat`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, {
                    buyer: id,
                    seller: this.myId,
                })
                .then(response => {
                    const chatId = response.data.chat_id
                    this.openChat(chatId, name)
                })
                .catch(error => console.log(error))
        },
        getChats() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/messanger/all-chats`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(path)
                .then(response => {
                    this.chats = response.data
                })
                .catch(error => {
                    console.log(error)
                })
        },
        getAccount() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/seller/seller-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.account = response.data
                    this.description = response.data.description
                    this.name = response.data.user.first_name
                    this.sales = response.data.my_sales
                    this.photo = this.pathUrl + '/api' + response.data.photo
                    this.products = response.data.products
                    this.transactions = response.data.transactions
                    this.category = response.data.category.id
                    this.myId = response.data.id

                })
                .catch(error => console.log(error));
        },
        editAccount() {

            const path = `${this.pathUrl}/api/seller/seller-lk/edit/`
            const csrf = this.getCSRFToken()

            const user = {
                first_name: this.name,
                email: this.email,
            };
            const formData = new FormData();
            formData.append('user.[first_name]', this.name);
            formData.append('user.[email]', this.email);

            // Добавляем остальные данные
            formData.append('description', this.description);
            if (this.avatar == null) {
                formData.append('photo', '');
            }
            else {
                formData.append('photo', this.avatar);
            }
            formData.append('category', this.category);

            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            this.$refs.edit.innerHTML = 'Сохраняем'

            axios
                .put(path, formData)
                .then((res) => {
                    if (res.status == 200) {
                        this.$refs.edit.innerHTML = 'Успешно'
                        this.name = res.data.user.first_name
                        this.description = res.data.description
                        this.email = res.data.user.email
                    }
                    else {
                        this.$refs.edit.innerHTML = 'Ошибка'
                    }
                })
                .catch((error) => {
                    console.error(error);

                });
        },
        openFileInput() {
            this.$refs.fileInput.click();
        },
        handleFileChange(event) {
            const file = event.target.files[0];
            if (file) {
                this.avatar = file;
                event.target.value = null;
                console.log("Выбран файл:", file);
            }
        }
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        console.log(accType)
        if (accType == 'seller-account') {
            this.getAccount()
            this.getChats()
        }
        else {
            window.location.href = '/login'
        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Личный кабинет | Experthub',
    ogTitle: 'Личный кабинет | Experthub',
    description: 'Личный кабинет | Experthub',
    ogDescription: 'Личный кабинет | Experthub',
})
</script>
<style lang="scss" scoped>
.account {
    padding: 110px 100px 60px;

    .accs {
        .buttons {
            display: flex;
            justify-content: flex-start;
            align-items: center;
            gap: 20px;
            margin-top: 30px;

            @media (max-width: 1024px) {
                flex-direction: column;
                align-items: flex-start;

            }

            button,
            a {
                padding: 10px 30px;
                border: 0;
                border-radius: 10px;
                background: #0072EE;

                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                transition: all .3s ease;

                &:hover {
                    background: #004692;
                }

                @media (max-width: 1024px) {
                    flex: 1;
                }
            }
        }
    }

    .account__profile {
        margin-top: 38px;
        display: flex;
        gap: 54px;

        @media (max-width: 1024px) {
            flex-direction: column;
            margin-top: 20px;
        }

        label,
        input {
            display: block;
        }

        label {
            font-size: 16px;
            font-style: normal;
            font-weight: 500;
            line-height: 110%;
            font-family: var(--mon);
            color: #fff;
            margin-left: 10px;
        }

        input {
            border: 2px solid #fff;
            border-radius: 10px;
            padding: 10px;
            width: 336px;

            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            background: transparent;

            @media (max-width: 1024px) {
                width: 100%;
                max-width: 100%;
                display: inline;
            }
        }


        .profile__left {
            //  width: 100%;

            .data {
                margin-top: 20px;
            }

            .avatar {
                display: flex;
                gap: 20px;
                align-items: start;

                @media (max-width: 1024px) {
                    flex-direction: column;
                    align-items: center;
                    width: 100%;
                }

                .avik {
                    border-radius: 10px;
                }

                img {
                    width: 16.667vw;
                    height: 14.323vw;
                    object-fit: cover;

                    @media (max-width: 1024px) {
                        width: 150px;
                        height: 150px;
                    }
                }

                div {
                    position: relative;
                    overflow: hidden;

                    &:last-child {
                        @media (max-width: 1024px) {
                            width: 100%;
                        }
                    }



                    .editava {
                        position: absolute;
                        display: flex !important;
                        justify-content: center;
                        align-items: center;
                        transition: all .3s ease;
                        transform: scaleY(0);
                        width: 100%;
                        height: 100%;
                        border-radius: 10px;
                        background: rgba(246, 246, 246, 0.30);
                        cursor: pointer;

                        top: 0;

                        span {
                            display: flex;
                            align-items: center;
                            justify-content: center;
                            padding: 10px;
                            background: #fff;
                            border-radius: 10px;

                            font-size: 10.755px;
                            font-style: normal;
                            font-weight: 500;
                            line-height: 130%;
                            text-decoration-line: underline;
                            font-family: var(--mon);
                            color: #000;
                            cursor: pointer;
                        }

                    }

                    &:hover .editava {
                        transform: scaleY(1)
                    }
                }
            }
        }

        .profile__right {
            textarea {
                width: 100%;
                // max-width: ;
                display: block;
                background: transparent;
                border-radius: 10px;
                border: 2px solid #fff;
                padding: 20px;

                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
            }
        }
    }

    .chats {
        margin-top: 38px;
        display: flex;
        flex-wrap: wrap;
        gap: 30px;

        @media (max-width: 1400px) {
            gap: 20px;
        }

        @media (max-width: 1024px) {
            margin-top: 20px;
        }

        .chat__item {
            border-radius: 10px;
            background: #000;
            box-shadow: 0px 0px 15px 0px rgba(0, 0, 0, 0.10);
            padding: 30px 20px;
            width: 546px;

            @media (max-width: 1024px) {
                width: 100%;
                padding: 15px;
            }

            div {
                display: flex;
                justify-content: space-between;
                align-items: center;
                width: 100%;

                @media (max-width: 380px) {
                    flex-direction: column;
                    align-items: flex-start;
                    gap: 10px;
                }

                h2 {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;
                    margin: 0;

                    @media(max-width: 1024px) {
                        font-size: 16px;
                    }
                }

                small {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;

                    @media(max-width: 1024px) {
                        font-size: 16px;
                    }
                }
            }

            .justify-content-end {
                justify-content: flex-start !important;
            }

            button {
                padding: 10px 30px;
                border: 0;
                border-radius: 10px;
                background: #F6F6F6;
                margin-top: 30px;

                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                color: #000;

            }
        }
    }

    .mysales {
        margin-top: 30px;

        .products {
            .product__item {
                width: 100%;
                display: flex;
                gap: 30px;
                align-items: flex-start;
                margin-bottom: 30px;

                &:last-child {
                    margin-bottom: 0;
                }

                .product__info {
                    width: 100%;
                    display: flex;
                    justify-content: space-between;

                    h1 {
                        font-size: 24px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;
                    }

                    span {
                        font-size: 20px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;
                    }

                    .buttons {
                        text-align: right;
                    }

                    button,
                    a {
                        background: #0072EE;
                        border-radius: 10px;
                        border: 0;
                        padding: 10px 20px;

                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;

                        transition: all .3s ease;

                        &:hover {
                            background: #004692;
                        }
                    }

                    button {
                        margin-right: 30px;
                    }
                }

                img {
                    width: 18.75vw;
                    height: 13.802vw;
                    border-radius: 10px;
                    object-fit: cover;
                }
            }
        }
    }

    .myproducts {
        margin-top: 30px;

        .products {
            margin-top: 50px;

            .product__item {
                width: 100%;
                display: flex;
                gap: 30px;
                align-items: flex-start;
                margin-bottom: 30px;

                &:last-child {
                    margin-bottom: 0;
                }

                .product__info {
                    width: 100%;
                    display: flex;
                    justify-content: space-between;

                    h1 {
                        font-size: 24px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;
                    }

                    .buttons {
                        text-align: right;
                    }

                    button,
                    a {
                        background: #0072EE;
                        border-radius: 10px;
                        border: 0;
                        padding: 10px 20px;

                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;

                        transition: all .3s ease;

                        &:hover {
                            background: #004692;
                        }
                    }

                    button {
                        margin-right: 30px;
                    }
                }

                img {
                    width: 18.75vw;
                    height: 13.802vw;
                    border-radius: 10px;
                    object-fit: cover;
                }
            }
        }

        .text-center {
            button {
                padding: 10px 30px;
                background: #0072EE;
                border-radius: 10px;
                border: 0;

                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                transition: all .3s ease;

                &:hover {
                    background: #004692;
                }
            }
        }
    }

    .navs {
        display: flex;
        justify-content: center;
        gap: 20px;

        div {
            display: flex;
            gap: 20px;

            .activeTab {
                background: #0072EE !important;
                color: #fff !important;
            }

            button {
                background: #000;
                border-radius: 10px;
                border: 0;
                padding: 10px 15px;

                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;

                transition: all .3s ease;

                &:hover {
                    background: #0072EE;
                    color: #fff;
                }
            }
        }
    }

    h1 {
        font-size: 40px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        font-family: var(--mon);
        color: #fff;
        margin-bottom: 40px;


    }
}
</style>