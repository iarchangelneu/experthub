<template>
    <div class="catalog">
        <h1>Каталог услуг</h1>

        <div class="catalog__filters">
            <p>На страницах экспертов вы сможете увидеть список всех оказываемых ими услуг</p>
            <div class="buttons">
                <button @click="filters = !filters" :class="{ activebtn: filters }">
                    <span>Фильтры</span>
                    <img src="@/assets/img/filterarrow.svg" alt="">
                </button>
                <button @click="sort = !sort" :class="{ activebtn: sort }">
                    <span>Сортировка</span>
                    <img src="@/assets/img/filterarrow.svg" alt="">
                </button>
            </div>
            <div class="sort__body" :class="{ activeBlock: sort }">
                <span @click="activeSort = 1, sortBy('price')" :class="{ activeSpan: activeSort == 1 }">Сначала
                    дешевле</span>
                <span @click="activeSort = 2, sortBy('-price')" :class="{ activeSpan: activeSort == 2 }">Сначала
                    дороже</span>
                <span @click="activeSort = 3, sortBy('-date_activate')" :class="{ activeSpan: activeSort == 3 }">Самые
                    новые</span>
                <span @click="activeSort = 4, sortBy('date_activate')" :class="{ activeSpan: activeSort == 4 }">Самые
                    старые</span>
            </div>
            <div class="filter__body" :class="{ activeBlock: filters }">
                <div>
                    <label class="custom-checkbox" v-for="(category, index) in categories1" :key="index">
                        <input type="checkbox" :value="index + 1" v-model="selectedCategories" @change="applyFilters">
                        <p class="checkbox-text m-0">{{ category }}</p>
                    </label>
                </div>
                <div>
                    <label class="custom-checkbox" v-for="(category, index) in categories2" :key="index">

                        <input type="checkbox" :value="index + 8" v-model="selectedCategories" @change="applyFilters">
                        <p class="checkbox-text m-0">{{ category }}</p>
                    </label>
                </div>

                <div>
                    <h2>Цена</h2>

                    <div class="price">
                        <input type="number" placeholder="от" v-model="minPrice" @input="applyFilters">
                        <img src="@/assets/img/price.svg" alt="">
                        <input type="number" placeholder="до" v-model="maxPrice" @input="applyFilters">
                    </div>
                </div>
            </div>
        </div>

        <div class="catalog__list">


            <div class="catalog__item" v-for="item in catalog.results" :key="item.id">
                <img :src="item.main_image" alt="">

                <h1>{{ item.name }}</h1>
                <p>{{ item.short_description }}</p>
                <div class="text-center">
                    <h1>{{ item.price.toLocaleString() + ' ₸' }}</h1>
                    <NuxtLink :to="'/product/' + item.id">Подробнее</NuxtLink>
                </div>
            </div>

        </div>

        <div class="pag text-center">
            <button ref="showmore" @click="loadMoreProducts()">Показать еще</button>
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
            filters: false,
            sort: false,
            activeSort: 0,
            catalog: [],
            selectedSort: 0,
            selectedCategories: [],
            minPrice: null,
            maxPrice: null,
            pathUrl: 'https://experthub.kz',
            categories1: ['Бухгалтерия', 'Документация', 'Бизнес-аналитика', 'Логистика', 'Юридические услуги', 'Страхование', 'Консалтинг'],
            categories2: ['Финансовый аудит', 'Маркетинг'],
        }
    },
    methods: {
        getCatalog() {
            const queryParams = new URLSearchParams(window.location.search);
            const categoryParam = queryParams.get('category');

            let url = `${this.pathUrl}/api/products/all-product`;
            if (categoryParam) {
                url += `?category__in=${categoryParam}`;
            }

            axios
                .get(url)
                .then(response => {
                    this.catalog = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        loadMoreProducts() {
            if (this.catalog.next) {
                this.$refs.showmore.innerHTML = 'Загружаем'
                axios
                    .get(this.catalog.next)
                    .then(response => {
                        // Добавляем новые продукты к существующим
                        this.catalog.results.push(...response.data.results);
                        this.catalog.next = response.data.next;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            }
            else {
                this.$refs.showmore.innerHTML = 'Больше ничего нет (;'
            }
        },
        sortBy(ordering) {
            this.sort = false
            const path = `${this.pathUrl}/api/products/all-product?ordering=${ordering}`;
            axios
                .get(path)
                .then(response => {
                    this.catalog = response.data;

                })
                .catch(error => {
                    console.error(error);
                });
        },

        applyFilters() {
            const params = new URLSearchParams();
            if (this.minPrice !== null) {
                params.append('price__gte', this.minPrice);
            }
            if (this.maxPrice !== null) {
                params.append('price__lte', this.maxPrice);
            }

            if (this.selectedCategories.length > 0) {
                params.append('category__in', this.selectedCategories.join(','));
            }
            if (this.search) {
                params.append('name__icontains', this.search);
            }

            this.fetchFilteredProducts(params);
        },
        fetchFilteredProducts(params) {
            const path = `${this.pathUrl}/api/products/all-product?${params.toString()}`;
            axios
                .get(path)
                .then(response => {
                    this.catalog = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
    },
    mounted() {
        this.getCatalog()
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Услуги | Experthub',
    ogTitle: 'Услуги | Experthub',
    description: 'Услуги | Experthub',
    ogDescription: 'Услуги | Experthub',
})
</script>
<style lang="scss" scoped>
.catalog {
    padding: 110px 100px 60px;

    .pag {
        margin-top: 30px;

        button {
            padding: 10px 30px;
            border: 0;
            background: #0072EE;
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

    .catalog__list {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(318px, 1fr));
        gap: 31.85px;
        grid-auto-flow: dense;
        gap: 31.85px;

        .catalog__item {
            border-radius: 10px;
            background: #F6F6F6;
            margin-top: 30px;
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
                margin-top: auto;
                margin-bottom: 15px;
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

    label,
    input {
        display: block !important;
    }

    .catalog__filters {
        display: flex;
        align-items: center;
        justify-content: space-between;
        position: relative;

        .activeBlock {
            transform: rotate3d(1, 1, 1, 0deg) !important;
        }

        .sort__body {
            position: absolute;
            right: 0;
            top: 0;
            margin-top: 70px;
            border-radius: 10px;
            padding: 30px 20px;
            background: #000;
            transform: rotate3d(1, 1, 1, 120deg);
            transition: all .3s ease;

            display: flex;
            flex-direction: column;
            gap: 5px;

            .activeSpan {
                background: #0072EE;
                border-radius: 10px;
                width: 100%;
            }

            span {
                font-size: 20px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                padding: 10px 10px;
                color: #fff;
                font-family: var(--mon);
                transition: all .3s ease;
                cursor: pointer;
            }
        }

        .filter__body {
            position: absolute;
            padding: 30px;
            background: #000;
            right: 0;
            top: 0;
            margin-top: 70px;
            border-radius: 10px;
            transform: rotate3d(1, 1, 1, 120deg);

            transition: all .3s ease;

            display: flex;
            gap: 30px;
        }

        h2 {
            font-size: 24px;
            font-style: normal;
            font-weight: 600;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            margin-bottom: 10px;
        }

        .price {
            display: flex;
            gap: 5px;

            input {
                max-width: 140px;
                padding: 6px 10px;
                background: transparent;
                border-radius: 10px;
                border: 2px solid #fff;

                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
            }
        }

        p {
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            margin: 0;
        }

        .buttons {
            display: flex;
            gap: 30px;

            .activebtn {
                background: #004692;

                img {
                    transform: rotate(90deg);
                }
            }


            button {
                border-radius: 10px;
                padding: 10px 30px;
                background: #0072EE;
                border: 0;
                display: flex;
                align-items: center;
                gap: 5px;

                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                transition: all .3s ease;

                img {
                    transition: all .3s ease;
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
    }
}
</style>