<template>
    <div class="product">
        <prevPage></prevPage>
        <div v-if="product.length <= 0"></div>
        <div class="product__body" v-else>
            <h1>{{ product.name }}</h1>

            <div class="product__info">
                <div class="image">
                    <img :src="product.main_image" alt="">

                    <div class="cat">
                        <h2>Категория: </h2>
                        <span>{{ product.category.category_name }}</span>
                    </div>

                    <ul class="dot-list">
                        <li v-for="(feature, index) in product.key_features.split('\r\n')" :key="index">{{ feature }}</li>
                    </ul>
                </div>

                <div class="description">
                    <p>{{ product.short_description }}</p>

                    <div class="price">
                        <h2>{{ product.price.toLocaleString() + ' ₸' }}</h2>
                        <button ref="cartBtn" @click="addToCart()" v-if="accType == 'buyer'">Купить</button>
                    </div>

                    <div v-html="product.description"></div>
                </div>

                <div class="author">
                    <span>Автор: <NuxtLink :to="'/expert/' + product.seller.id"
                            style="text-decoration: underline !important; color: #fff;">{{
                                seller.first_name }}</NuxtLink></span>
                    <span>Рейтинг: {{ rating }} <img src="@/assets/img/star.svg" alt=""></span>
                    <span>Количество услуг на сайте: {{ count }}</span>
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
            productId: this.$route.params.id,
            product: [],
            seller: [],
            pathUrl: 'https://experthub.kz',
            category: '',
            rating: null,
            count: null,
            apiResponse: '',
            accType: '',
        }
    },
    methods: {
        addToCart() {
            const path = `${this.pathUrl}/api/buyer/add-product-basket`
            const csrf = this.getCSRFToken()
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, {
                    products: this.product.id,
                    amount: 1,
                })
                .then(response => {
                    if (response.status == 201) {
                        this.$refs.cartBtn.innerHTML = 'Добавлен в корзину'
                        this.$refs.cartBtn.disabled = true
                        this.$refs.cartBtn.classList.add('disabled')
                        this.getCart()
                    }
                    else {
                        this.$refs.cartBtn.innerHTML = 'Произошла ошибка, попробуйте еще раз'
                        this.$refs.cartBtn.disabled = false

                    }
                    console.log(response)
                })
                .catch(error => {
                    console.error(error)
                })
        },

        getProduct() {
            const path = `${this.pathUrl}/api/products/detail-product/${this.productId}`
            axios
                .get(path)
                .then(response => {
                    this.product = response.data
                    this.seller = response.data.seller.user
                    this.rating = response.data.seller.rating
                    this.category = response.data.category.category_name
                    this.count = response.data.seller.amount_products
                })
                .catch(error => {
                    console.error(error)
                })
        },
    },
    mounted() {
        this.getProduct()

        const accType = localStorage.getItem('accountType')
        if (accType == 'buyer-account') {
            this.accType = 'buyer'
        }
        else if (accType == 'seller-account') {
            this.accType = 'seller'
        }
        else {
            console.log('not authorized')
        }
    }

}
</script>
<script setup>
useSeoMeta({
    title: 'Услуга | Experthub',
    ogTitle: 'Услуга | Experthub',
    description: 'Услуга | Experthub',
    ogDescription: 'Услуга | Experthub',
})
</script>
<style lang="scss" scoped>
.product {
    padding: 110px 100px 60px;

    @media (max-width: 1600px) {
        padding: 110px 50px 60px;
    }

    @media (max-width: 1024px) {
        padding: 110px 20px 60px;
    }

    .product__info {
        display: flex;
        align-items: flex-start;

        @media (max-width: 1024px) {
            flex-direction: column;
        }

        .author {
            margin-left: 90px;

            @media (max-width: 1024px) {
                margin: 30px 0 0;
            }

            span {
                display: flex;
                align-items: center;
                gap: 5px;
                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                margin-bottom: 10px;

                &:last-child {
                    margin-bottom: 0;
                }
            }
        }

        .description {
            margin-left: 30px;
            max-width: 1038px;


            @media (max-width: 1024px) {
                margin: 0;
            }


            .price {
                display: flex;
                align-items: center;
                gap: 30px;
                margin: 30px 0 25px;

                h2 {
                    font-size: 36px;
                    font-style: normal;
                    font-weight: 600;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                    margin: 0;
                }

                button {
                    padding: 10px 30px;
                    background: #0072EE;
                    border: 0;
                    border-radius: 10px;
                    transition: all .3s ease;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;

                    &:hover {
                        background: #004692;
                    }

                }
            }

            p,
            div {
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                margin: 0;
            }
        }

        .image {
            img {
                border-radius: 10px;
                width: 16.667vw;
                height: 14.323vw;
                object-fit: cover;

                @media (max-width: 1024px) {
                    width: 165px;
                    height: 165px;
                }
            }

            .dot-list {
                margin-top: 20px;
                list-style-type: none;
                padding: 0;



                li {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    color: #fff;
                    font-family: var(--mon);
                    max-width: 300px;

                    position: relative;
                    display: flex;
                    align-items: flex-start;
                    white-space: normal;
                    margin-bottom: 10px;

                    &:last-child {
                        margin-bottom: 0;
                    }
                }

                li::before {
                    content: "";
                    min-width: 10px;
                    min-height: 10px;
                    background: #fff;
                    border-radius: 50%;
                    margin-right: 15px;
                    margin-top: 5px;
                }

            }

            .cat {
                margin-top: 30px;
                display: flex;
                gap: 5px;
                align-items: center;

                h2 {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 600;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                    margin: 0;
                }

                span {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
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

        @media (max-width: 1024px) {
            font-size: 24px;
        }
    }
}
</style>