<template>
    <div class="cart">
        <prevPage></prevPage>
        <h1>Корзина</h1>

        <div class="cart__body">
            <div class="cart__left">
                <div class="cart__item" v-for="item in cart" :key="item.id">
                    <img :src="pathUrl + '/api' + item.products.main_image" class="img" alt="">

                    <div class="item__price">
                        <div>
                            <h1>{{ item.products.name }}</h1>
                            <h2>{{ item.products.price.toLocaleString() + ' ₸' }}</h2>
                        </div>
                        <div class="trash text-right">
                            <img src="@/assets/img/trash.svg" @click="deleteFromCart(item.id)" alt=""
                                style="cursor: pointer;">
                        </div>
                    </div>
                </div>

            </div>
            <div class="cart__right">
                <h1>Подтверждение покупки</h1>
                <span>Количество курсов: {{ cart.length }}</span>
                <span>Итоговая сумма: {{ formatPrice(calculateTotal()) }} ₸ </span>
                <p>Сумма будет списана с вашего счета. Товары отобразятся во вкладке «Мои покупки» в Вашем Личном кабинете.
                </p>
                <button ref="buyBtn" @click="buyProduct()">Подтвердить покупку</button>
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
            cart: [],
            pathUrl: 'https://experthub.kz',
        }
    },

    methods: {
        buyProduct() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/placed-basket`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            this.$refs.buyBtn.innerHTML = 'Оформляем'
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                    this.getCart()
                    if (response.status == 204) {
                        this.$refs.buyBtn.innerHTML = 'Недостаточно средств'
                    }
                    if (response.status == 201) {
                        // this.getBuyer()
                        this.$refs.buyBtn.innerHTML = 'Оплата прошла успешно'

                        setTimeout(() => {
                            window.location.href = '/products'
                        }, 3);
                    }
                })
                .catch(error => {
                    console.error(error)
                })

        },
        calculateTotal() {
            let total = 0;

            this.cart.forEach(item => {
                const { price, discount } = item.products;
                const discountedPrice = price * (1 - discount / 100); // Преобразуем скидку в коэффициент
                total += discountedPrice * item.amount;
            });

            return total;
        },
        formatPrice(price) {
            return price.toLocaleString('ru-RU');
        },
        getCart() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/all-product-basket`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(path)
                .then(response => {
                    this.cart = response.data
                })
                .catch(error => {
                    console.error(error)
                })
        },
        deleteFromCart(id) {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/buyer/delete-product-basket/${id}`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .put(path)
                .then(response => {
                    console.log(response)
                    this.getCart()
                })
                .catch(error => {
                    console.error(error)
                })
        },
    },
    mounted() {
        this.getCart()
        const accType = localStorage.getItem('accountType')
        if (accType === null || accType === undefined) {
            window.location.href = '/login';
        } else if (accType === 'seller-account') {
            window.location.href = '/login';
        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Корзина | Experthub',
    ogTitle: 'Корзина | Experthub',
    description: 'Корзина | Experthub',
    ogDescription: 'Корзина | Experthub',
})
</script>
<style lang="scss" scoped>
.cart {
    padding: 120px 100px 60px;

    @media (max-width: 1600px) {
        padding: 120px 50px 60px;
    }

    @media (max-width: 1024px) {
        padding: 120px 20px 60px;
    }

    .cart__body {
        display: flex;
        justify-content: space-around;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 30px;
        }

        .cart__right {

            h1 {
                font-size: 32px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                margin-bottom: 30px;

                @media (max-width: 1024px) {
                    font-size: 20px;
                    margin-bottom: 10px;
                }
            }

            span {
                margin-bottom: 30px;
                display: block;
                font-size: 20px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;

                @media (max-width: 1024px) {
                    font-size: 16px;
                    margin-bottom: 10px;
                }
            }

            p {
                margin-bottom: 30px;
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                max-width: 485px;

                @media (max-width: 1024px) {
                    font-size: 12px;
                    margin-bottom: 20px;
                }
            }

            button {
                padding: 10px 30px;
                border: 0;
                border-radius: 10px;
                background: #0072EE;

                font-size: 20px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                transition: all .3s ease;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }

                &:hover {
                    background: #004692;
                }
            }
        }

        .cart__left {

            .cart__item {
                display: flex;
                gap: 32px;
                margin-bottom: 30px;

                &:last-child {
                    margin-bottom: 0;
                }

                .img {
                    border-radius: 10px;
                    width: 18.75vw;
                    height: 13.802vw;

                    @media (max-width: 1024px) {
                        width: 165px;
                        height: 165px;
                    }
                }

                .item__price {
                    width: 411.994px;
                    display: flex;
                    flex-direction: column;
                    justify-content: space-between;

                    @media (max-width: 1400px) {
                        width: 300px;
                    }
                }

                div {
                    h1 {
                        font-size: 24px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;
                        margin-bottom: 20px;

                        @media (max-width: 1024px) {
                            font-size: 16px;
                        }
                    }

                    h2 {
                        font-size: 24px;
                        font-style: normal;
                        font-weight: 600;
                        line-height: 130%;
                        font-family: var(--mon);
                        color: #fff;
                        margin: 0;

                        @media (max-width: 1024px) {
                            font-size: 16px;
                        }
                    }
                }
            }
        }
    }

    h1 {
        font-size: 40px;
        font-style: normal;
        font-weight: 500;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
        margin: 10px 0 30px;
    }
}
</style>