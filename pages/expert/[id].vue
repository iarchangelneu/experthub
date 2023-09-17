<template>
    <div v-if="seller.length <= 0"></div>
    <div class="expert" v-else>
        <prevPage></prevPage>

        <h1>{{ seller.user.first_name }}</h1>

        <div class="expert__info">
            <div class="avatar">
                <img :src="seller.photo" alt="">
            </div>
            <div class="expert__about">
                <div class="cat">
                    <h2>Категория:</h2>
                    <p>{{ seller.category.category_name }}</p>
                </div>
                <h2>Описание:</h2>
                <div v-html="seller.description"></div>
            </div>
        </div>

        <div class="expert__products">
            <h3>Услуги эксперта</h3>

            <div class="products__list">
                <div class="products__item" v-for="item in seller.products" :key="item.id">
                    <img :src="item.main_image" alt="">

                    <h1>{{ item.name }}</h1>

                    <p>Эксперт обладает глубокими знаниями законодательства и практическим опытом в различных
                        сферах права</p>
                    <div class="text-center">
                        <h1>{{ item.price.toLocaleString() + ' ₸' }}</h1>
                        <NuxtLink :to="'/product/' + item.id">Подробнее</NuxtLink>
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
            productId: this.$route.params.id,
            seller: [],
            text: 'Я профессиональный юрист с обширным опытом в области юриспруденции. Я обладаю глубокими знаниями законодательства и специализированными навыками, которые позволяют мне эффективно консультировать клиентов, готовить правовые документы, вести переговоры и представлять интересы клиентов в суде.',
        }
    },
    methods: {
        getSeller() {
            const path = `${this.pathUrl}/api/seller/this-seller/${this.productId}`
            axios
                .get(path)
                .then(response => {
                    this.seller = response.data
                })
                .catch(error => {
                    console.error(error)
                })
        },
    },
    mounted() {
        this.getSeller()
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Эксперт | Experthub',
    ogTitle: 'Эксперт | Experthub',
    description: 'Эксперт | Experthub',
    ogDescription: 'Эксперт | Experthub',
})
</script>

<style lang="scss" scoped>
.expert {
    padding: 110px 100px 60px;

    @media (max-width: 1600px) {
        padding: 110px 50px 60px;
    }

    @media (max-width: 1024px) {
        padding: 110px 20px 60px;
    }

    .expert__products {

        .products__list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(318px, 1fr));
            gap: 31.85px;
            grid-auto-flow: dense;

            @media (max-width: 1024px) {
                gap: 20px;
            }

            .products__item {
                border-radius: 10px;
                background: #F6F6F6;
                width: 100%;
                display: flex;
                flex-direction: column;




                img {
                    width: 100%;
                    height: 275px;
                    border-top-left-radius: 10px;
                    border-top-right-radius: 10px;
                    object-fit: cover;
                }

                h1 {
                    padding: 10px 15px 0;
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 600;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
                    margin-bottom: 5px;
                }

                h2,
                p {
                    padding: 0 15px;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    color: #000;
                    font-family: var(--mon);
                    margin-bottom: 10px;

                }

                p {

                    margin-bottom: 15px;
                }

                .text-center {
                    margin-top: auto;
                }

                a {
                    margin-bottom: 10px;
                    background: #000;
                    text-decoration: none;
                    padding: 10px 30px;
                    border: 0;
                    border-radius: 10px;
                    transition: all .3s ease;
                    display: inline-block;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;

                    &:hover {
                        background: #373737;
                    }
                }
            }
        }

        h3 {
            font-size: 24px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            margin: 80px 0 30px;

            @media (max-width: 1024px) {
                margin: 30px 0 30px;
            }
        }
    }

    .expert__info {
        display: flex;
        gap: 30px;
        align-items: flex-start;

        @media (max-width: 1024px) {
            flex-direction: column;
        }

        .expert__about {
            .cat {
                display: flex;
                gap: 5px;
            }

            h2 {
                font-size: 16px;
                font-style: normal;
                font-weight: 600;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
            }

            p,
            div {
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
            }

            div {
                max-width: 1095px;
            }
        }

        .avatar {
            img {
                object-fit: cover;
                border-radius: 10px;
                width: 16.667vw;
                height: 14.323vw;

                @media (max-width: 1024px) {
                    width: 165px;
                    height: 165px;
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
