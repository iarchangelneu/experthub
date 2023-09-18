<template>
    <header v-if="!hideHeaderOnPages.includes($route.name)">
        <div class="pcheader">
            <div class="crap">

                <div class="headermen" v-if="!ifHome.includes($route.name)">
                    <div class="trepik">
                        <NuxtLink to="/withdrawal">
                            <img src="@/assets/img/cash.svg" alt="" loading="lazy">
                        </NuxtLink>
                        <NuxtLink to="/cart">
                            <img src="@/assets/img/cart.svg" v-if="accountType == 'buyer'" alt="" loading="lazy"
                                style="cursor: pointer;">
                        </NuxtLink>


                    </div>
                    <NuxtLink to="/">
                        <img src="@/assets/img/headerlogo.svg" alt="" loading="lazy">
                    </NuxtLink>
                    <div class="trepik">
                        <NuxtLink :to="this.accountUrl">
                            <img src="@/assets/img/acc.svg" alt="" loading="lazy">
                        </NuxtLink>
                        <div class="burg">
                            <input id="menu__toggle" class="d-none" type="checkbox" />
                            <label class="menu__btn mt-2" for="menu__toggle" @click="menuOpen = !menuOpen">
                                <span></span>
                            </label>
                        </div>
                    </div>
                </div>
                <div class="headermen" v-else>
                    <div class="trepik">
                        <a href="/withdrawal">
                            <img src="@/assets/img/cash.svg" alt="" loading="lazy">
                        </a>
                        <a href="/cart">
                            <img src="@/assets/img/cart.svg" v-if="accountType == 'buyer'" alt="" loading="lazy"
                                style="cursor: pointer;">
                        </a>


                    </div>
                    <a href="/">
                        <img src="@/assets/img/headerlogo.svg" alt="" loading="lazy">
                    </a>
                    <div class="trepik">
                        <a :href="this.accountUrl">
                            <img src="@/assets/img/acc.svg" alt="" loading="lazy">
                        </a>
                        <div class="burg">
                            <input id="menu__toggle" class="d-none" type="checkbox" />
                            <label class="menu__btn mt-2" for="menu__toggle" @click="menuOpen = !menuOpen">
                                <span></span>
                            </label>
                        </div>
                    </div>
                </div>
            </div>

            <div class="headermenu" :class="{ open: menuOpen }">
                <div class="menu__body">
                    <div>
                        <NuxtLink to="/experts">Эксперты</NuxtLink>
                        <NuxtLink to="/products">Услуги</NuxtLink>
                        <NuxtLink to="/about">О платформе</NuxtLink>
                        <NuxtLink to="/for-expert">Для Эксперта</NuxtLink>
                    </div>
                    <div>
                        <NuxtLink to="/terms">Пользовательское соглашение</NuxtLink>
                        <NuxtLink to="/polytics">Политика конфиденциальности</NuxtLink>
                    </div>
                    <NuxtLink to="/login" class="reg" v-if="accountType == ''">Вход/Регистрация</NuxtLink>
                    <span v-if="userBalance !== null">{{ userBalance == null ? '0 ₸' : userBalance.toLocaleString()
                        + ' ₸' }}</span>
                </div>
            </div>
        </div>
    </header>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios'
export default {
    mixins: [global],
    data() {
        return {
            menuOpen: false,
            cartOpen: false,
            hideHeaderOnPages: ['login', 'register'],
            pathUrl: 'https://experthub.kz',
            userBalance: null,
            accountType: '',
            ifHome: ['index'],
        }
    },
    methods: {
        getBuyer() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.userBalance = response.data.balance

                })
                .catch(error => console.log(error));
        },
        getSeller() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/seller/seller-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.userBalance = response.data.balance

                })
                .catch(error => console.log(error));
        },

    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        if (accType == 'buyer-account') {
            this.getBuyer()
            this.accountType = 'buyer'
            setInterval(() => {
                this.cartLength = localStorage.getItem('cartLength')
            }, 1);
        }
        else if (accType == 'seller-account') {
            this.getSeller()
            this.accountType = 'seller'
        }
        else {
            console.log('not authorized')
        }

    }
}
</script>
<style lang="scss" scoped>
.cartOpen {
    transform: scaleY(1) !important;
    opacity: 1 !important;
}

.cart {
    padding: 60px;
    background: #FFF;

    position: fixed;
    width: 30%;
    z-index: 200;
    right: 0;
    height: 100vh;
    transition: all .3s ease;
    transform: scaleY(0);
    opacity: 0;

    @media (max-width: 1600px) {
        width: 40%;
    }

    @media (max-width: 1400px) {
        width: 50%;
    }

    @media (max-width: 1024px) {
        width: 100%;
        padding: 20px;
    }



    .cart__body {
        margin-top: 75px;

        @media (max-width: 1024px) {

            width: 100%;
        }

        a {
            margin-top: 40px;
            display: inline-block;
            text-decoration: none;
            text-align: center;
            padding: 12px 57px;
            border-radius: 10px;
            background: #000;

            font-size: 20px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;

            @media (max-width: 1024px) {
                font-size: 16px;
                padding: 10px 30px;
            }

            &:hover {
                background: linear-gradient(90deg, #462885 0.64%, #A021A7 100%);
            }
        }

        .cart__item {
            display: flex;
            margin-bottom: 20px;

            @media (max-width: 1024px) {
                width: 100%;
                justify-content: space-between;
            }

            img {
                max-width: 176px;
                max-height: 122px;

                @media (max-width: 1024px) {
                    max-width: 120px;
                }
            }

            &:last-child {
                margin-bottom: 0;
            }

            h3 {
                font-size: 14px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #000;
                margin-left: 10px;
                margin-bottom: 0;
            }

            div {
                display: flex;
                flex-direction: column;
                align-items: flex-end;
                gap: 35px;

                small {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #000;
                    white-space: nowrap;
                }
            }
        }
    }

    h2 {
        font-size: 40px;
        font-style: normal;
        font-weight: 500;
        line-height: normal;
        font-family: var(--mon);
        color: #000;
        margin: 0;

        @media (max-width: 1024px) {
            font-size: 25px;
        }
    }
}

header {
    z-index: 150;
    display: flex;
    justify-content: center;
    position: fixed;
    padding: 0 19.635vw 0;
    width: 100%;

    @media (max-width: 1024px) {
        padding: 0 20px 0;
    }




    .pcheader {
        width: 100%;
        height: auto;
        border-radius: 15px;
        border-top-right-radius: 0;
        border-top-left-radius: 0;
        background: rgba(164, 164, 164, 0.13);
        box-shadow: -40.83333px 40.83333px 40.83333px 0px rgba(255, 255, 255, 0.13) inset, 40.83333px -40.83333px 40.83333px 0px rgba(165, 165, 165, 0.13) inset;
        backdrop-filter: blur(44.91666793823242px);
        padding: 12px 25px;
        position: relative;
        transition: all .3s ease;

        @media (max-width: 1024px) {
            padding: 12px 10px;
        }

        .crap {
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: all .3s ease;
        }

        .headermen {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 0 30px;
            width: 100%;
            transition: all .3s ease;

            @media (max-width: 380px) {
                gap: 0 20px;
            }

            .trepik {
                display: flex;
                gap: 40px;

                @media (max-width: 1024px) {
                    gap: 20px;
                }
            }
        }

        .headermenu {
            position: absolute;
            // border-top: 1px solid #CDCDCD;
            margin-top: 12px;
            transition: all .1s ease;
            transform: scaleY(0) !important;
            opacity: 0;
            height: 0;

            .menu__body {
                padding: 30px;
                display: flex;
                justify-content: space-between;
                align-items: flex-start;

                span {
                    font-family: var(--mon);
                    color: #fff;
                    font-size: 16px;
                    font-weight: 400;
                }

                @media (max-width: 1024px) {
                    flex-direction: column;
                    padding: 15px;
                    gap: 15px;
                }

                div {
                    display: flex;
                    flex-direction: column;
                    gap: 25px;

                    @media (max-width: 1024px) {
                        gap: 15px;
                    }

                    a {
                        text-decoration: none;
                        font-size: 20px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 16px;
                        }
                    }


                }

                .reg {
                    text-decoration: none;
                    display: inline-block;
                    text-align: center;
                    padding: 10px 30px;

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
                }
            }
        }

        .open {
            position: relative;
            transform: scaleY(1) !important;
            opacity: 1;
            height: auto;
        }
    }
}

#menu__toggle {
    opacity: 0;
}

.menu__btn {
    display: flex !important;
    /* используем flex для центрирования содержимого */
    align-items: center;
    /* центрируем содержимое кнопки */
    width: 35px;
    height: 20px;
    cursor: pointer;
    z-index: 100000000;
    color: #fff;
    position: relative;
    transform: rotate(180deg);
}

.menu__btn>span {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #000;
}

.menu__btn>span::before,
.menu__btn>span::after {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #000;
}

.menu__btn>span::before {
    content: '';
    top: -8px;
}

.menu__btn>span::after {
    content: '';
    top: 8px;
}

#menu__toggle:checked~.menu__btn>span {
    transform: rotate(45deg);
    background-color: #000;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::before {
    top: 0;
    transform: rotate(0);
    background-color: #000;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::after {
    top: 0;
    transform: rotate(90deg);
    background-color: #000;
    border-radius: 10px;
}

#menu__toggle:checked~.menu__box {
    visibility: visible;
    top: 0;
}

.menu__btn>span,
.menu__btn>span::before,
.menu__btn>span::after {
    transition-duration: .25s;
}
</style>