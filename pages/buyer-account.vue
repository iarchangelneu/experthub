<template>
    <div class="account">
        <h1>Личный кабинет</h1>

        <div class="navs">
            <div>
                <button @click="activeTab = 0" :class="{ activeTab: activeTab == 0 }">Мои покупки</button>
                <button @click="activeTab = 1" :class="{ activeTab: activeTab == 1 }">Транзакции</button>
            </div>
            <div>
                <button @click="activeTab = 2" :class="{ activeTab: activeTab == 2 || activeTab == 7 }">Обратная
                    связь</button>
                <button @click="activeTab = 3" :class="{ activeTab: activeTab == 3 }">Аккаунт</button>

            </div>
        </div>

        <div class="mybuys" v-if="activeTab == 0">
            <div class="products">
                <div class="product__item" v-for="item in buys" :key="item.id">
                    <img :src="item.products.main_image" alt="">

                    <div class="product__info">
                        <div class="names">
                            <h1>{{ item.products.name }}</h1>
                            <span>Дата покупки: {{ formatDate(item.date) }} </span>
                            <span>Автор: <span style="text-decoration: underline;">{{ item.seller.user.first_name }}</span>
                            </span>
                        </div>
                        <div class="buttons">
                            <h1>{{ item.products.price.toLocaleString() + ' ₸' }} </h1>

                            <div class="btns">
                                <button @click="createChat(item.seller.id, item.seller.user.first_name)">Чат с
                                    продавцом</button>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>
        <div class="chats" v-if="activeTab == 2">

            <div class="chat__item" v-for="chat in chats" :key="chat.id">
                <div>
                    <h2>{{ chat.seller.user.first_name }}</h2>
                    <!-- <small>23.07.2023 14:47</small> -->
                </div>

                <div class="justify-content-end">
                    <button @click="openChat(chat.id, chat.seller.user.first_name)">Открыть чат</button>
                </div>
            </div>
        </div>

        <div class="profile" v-if="activeTab == 3">
            <div>
                <div class="inputs">
                    <div>
                        <label for="email">E-mail</label>
                        <input type="email" id="email" v-model="email">
                    </div>
                    <div>
                        <label for="password">Пароль</label>
                        <input type="password" id="password" v-model="password">
                    </div>
                </div>
                <div class="text-center">
                    <button @click="logOut()">Выйти из аккаунта</button>
                </div>
            </div>
        </div>

        <TheTrans v-if="activeTab == 1" :transactions="transactions"></TheTrans>
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
            password: '',
            email: '',
            chats: [],
            seller: [],
            pathUrl: 'https://experthub.kz',
            buys: [],
            sendId: null,
            chatId: null,
            myId: null,
            transactions: []
        }
    },
    methods: {
        formatDate(dateString) {
            const date = new Date(dateString);
            const formattedDate = `${date.getDate().toString().padStart(2, '0')}.${(date.getMonth() + 1).toString().padStart(2, '0')}.${date.getFullYear()}`;
            return formattedDate;
        },
        getAccount() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.account = response.data
                    this.myId = response.data.id
                    this.transactions = response.data.transactions
                    this.email = response.data.user.email

                })
                .catch(error => console.log(error));
        },
        openChat(chatId, chatName) {
            this.activeTab = 7;
            this.chatId = chatId;
            this.chatName = chatName
        },
        getBuys() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk/my-purchases`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.buys = response.data
                })
                .catch(error => {
                    console.error(error)
                })
        },
        createChat(id, name) {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/messanger/new-chat`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, {
                    buyer: this.myId,
                    seller: id,
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
                .then(res => {
                    this.chats = res.data
                })
                .catch(error => console.log(error))
        }
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        console.log(accType)
        if (accType == 'buyer-account') {
            this.getAccount()
            this.getChats()
            this.getBuys()
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

    .profile {
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 150px 0;

        .inputs {
            display: flex;
            gap: 20px;

            input,
            label {
                display: block;
            }

            label {
                margin-left: 10px;
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 110%;
                font-family: var(--mon);
                color: #fff;
            }

            input {
                background: transparent;
                border: 2px solid #fff;
                border-radius: 10px;
                padding: 10px 15px;

                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                width: 23.438vw;
            }
        }

        button {
            margin-top: 30px;
            border-radius: 10px;
            background: #0072EE;
            border: 0;
            padding: 10px 30px;

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

    .mybuys {
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
                        margin: 0;
                    }

                    .names {
                        display: flex;
                        flex-direction: column;
                        gap: 30px;
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
                        margin-top: 30px;

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