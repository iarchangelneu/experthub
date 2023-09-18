<template>
    <div class="sections-menu">
        <span class="menu-point" :class="{ active: activeSection == index }" @click="scrollToSection(index)"
            v-for="(offset, index) in offsets" :key="index" :title="'Go to section ' + (index + 1)">
        </span>
    </div>
    <section class="fullpage blue">

        <div>
            <h2>Объединяем лучших экспертов для вашего успеха</h2>
            <img src="@/assets/img/scroll.svg" alt="" style="cursor: pointer;" @click="scrollToSection(1)">
        </div>
    </section>
    <section class="fullpage black">
        <div class="popular">
            <h1>Популярные эксперты</h1>
            <p>Эксперты платформы представляют собой опытных и квалифицированных специалистов в различных областях права.
                Они обеспечивают клиентов актуальной и профессиональной поддержкой, помогая разрешать вопросы, составлять
                документы и обеспечивать соблюдение законов, что делает платформу надёжным и удобным источником услуг
                для различных потребителей.</p>
            <NuxtLink to="/">Посмотреть все</NuxtLink>

            <div class="popular__slider">
                <swiper :slides-per-view="5" :space-between="30" :navigation="navigation" :breakpoints="breakpoints"
                    :modules="modules">
                    <swiper-slide v-for="item in experts.results" :key="item.id">
                        <div class="popular__item">
                            <img :src="item.photo" alt="">

                            <h1>{{ item.category.category_name }}</h1>
                            <h2>{{ item.user.first_name }}</h2>
                            <p>{{ truncatedDescription(item.description, 70) }}</p>
                            <div class="text-center">
                                <NuxtLink :to="'/expert/' + item.id">Подробнее</NuxtLink>
                            </div>
                        </div>
                    </swiper-slide>

                </swiper>

                <img src="@/assets/img/xyetakakayato.svg" class="xyeta" alt="">

                <img src="@/assets/img/prev.svg" alt="" class="prev">
                <img src="@/assets/img/next.svg" alt="" class="next">

            </div>
        </div>
    </section>
    <section class="fullpage red">
        <div class="about">
            <div class="roll">
                <img src="@/assets/img/textroll.png" alt="" class="roller">
                <img src="@/assets/img/nout.png" class="nout" alt="">
            </div>

            <div class="text">
                <div class="syk">
                    <div class="header">
                        <span :class="{ activeSpan: forUser }" @click="forUser = true">Для пользователя</span>
                        <img src="@/assets/img/line.svg" alt="">
                        <span :class="{ activeSpan: !forUser }" @click="forUser = false">Для эксперта</span>
                    </div>
                    <img src="@/assets/img/lol.svg" alt="">
                </div>
                <div class="body" v-if="forUser">

                    <p> На нашей платформе вы найдёте консультантов, готовых работать с вами индивидуально или командами,
                        в зависимости от ваших потребностей. Мы стремимся обеспечить вас доступом к самым компетентным
                        и квалифицированным экспертам, чтобы помочь вам достичь успеха и преуспеть в вашем бизнесе.</p>
                    <NuxtLink to="/">Подробнее о платформе</NuxtLink>
                </div>
                <div class="body" v-if="!forUser">

                    <p> ExpertHelp - это партнёрство для вас, как для эксперта в своей области. Мы стремимся предоставить
                        вам средства и возможности для успешной и процветающей практики, обеспечивая вас клиентами
                        и инструментами для эффективной работы и реализации ваших знаний</p>
                    <NuxtLink to="/">Подробнее о платформе</NuxtLink>
                </div>
            </div>
        </div>
    </section>
    <section class="fullpage purple">
        <div class="popular">
            <h1>Популярные услуги</h1>
            <p>На нашей платформе предоставляются разнообразные экспертные услуги, чтобы помочь вам и вашему бизнесу достичь
                успеха, расти и развиваться.</p>
            <NuxtLink to="/">Посмотреть все</NuxtLink>

            <div class="popular__slider">
                <swiper :slides-per-view="5" ref="slider" :space-between="30" :breakpoints="breakpoints"
                    :navigation="navigation2" :modules="modules">
                    <swiper-slide v-for="item in products" :key="item.id">
                        <div class="popular__item prodslider">
                            <img :src="item.main_image" alt="">

                            <h1>{{ item.name }}</h1>
                            <!-- <p>{{ truncatedDescription(item.short_description, 50) }}
                            </p> -->
                            <div class="text-center">
                                <h1>{{ item.price.toLocaleString() + ' ₸' }} </h1>
                                <NuxtLink :to="'/product/' + item.id">Подробнее</NuxtLink>
                            </div>

                        </div>
                    </swiper-slide>

                </swiper>

                <img src="@/assets/img/xyetakakayato.svg" class="xyeta" alt="">

                <img src="@/assets/img/prev.svg" alt="" class="prev2">
                <img src="@/assets/img/next.svg" alt="" class="next2">

            </div>
        </div>
    </section>
    <section class="fullpage green">
        <div class="reviews">
            <div class="header">
                <h1>Отзывы</h1>
                <img src="@/assets/img/rev.svg" alt="">
            </div>

            <div class="reviews__slider">
                <swiper :slides-per-view="3" :space-between="43" :breakpoints="breakpoints2" :navigation="navigation3"
                    :modules="modules" @swiper="onSwiper" ref="mySwiper">
                    <swiper-slide v-for="(slide, index) in reviews" :key="index"
                        :class="{ 'scale-down': index === thirdSlideIndex }">
                        <div class="popular__item">
                            <h1>{{ slide.name }}</h1>
                            <p>{{ slide.text }}
                            </p>
                            <p>{{ slide.text2 }}
                            </p>

                            <div class="text-right">
                                <h2>{{ slide.first_name }}</h2>
                            </div>
                        </div>
                    </swiper-slide>
                </swiper>

                <div class="review__nav text-right">
                    <img src="@/assets/img/prev.svg" class="prev3" alt="">
                    <img src="@/assets/img/next.svg" class="next3" alt="">
                </div>
            </div>
        </div>
    </section>
</template>
<script>
// Import Swiper Vue.js components
import { Navigation } from 'swiper/modules';
import { Swiper, SwiperSlide } from 'swiper/vue';

// Import Swiper styles
import 'swiper/css';
import 'swiper/css/navigation';
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    components: {
        Swiper,
        SwiperSlide,
    },
    data() {
        return {
            pathUrl: 'https://experthub.kz',
            inMove: false,
            inMoveDelay: 400,
            activeSection: 0,
            breakpoints: {
                320: {
                    slidesPerView: 1,
                    spaceBetween: 10,

                },
                1025: {
                    slidesPerView: 3,
                    spaceBetween: 30,
                },
                1300: {
                    slidesPerView: 4,
                    spaceBetween: 30,
                },
                1600: {
                    slidesPerView: 5,
                    spaceBetween: 30,
                }
            },
            breakpoints2: {
                320: {
                    slidesPerView: 1,
                    spaceBetween: 10,

                },
                1025: {
                    slidesPerView: 3,
                    spaceBetween: 30,
                },
            },
            reviews: [
                {
                    name: 'Прогнозирование успешных стратегий',
                    text: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    text2: 'Аналитик предоставил нам ценные инсайды, которые существенно улучшили наши бизнес-решения. Его легкая доступность для обсуждения результатов анализа дала нам дополнительную уверенность в наших действиях.',
                    first_name: 'Виктория С.',
                },
                {
                    name: 'Со всеми юридическими вопросами сюда',
                    text: 'Юристы продемонстрировали высший уровень знаний и опыта. Они ответили на все мои вопросы и помогли в сложной судебной ситуации. Безусловно, лучшие в своей области',
                    text2: '"Юристы - лучшие в своей области. Они ответили на все мои вопросы, помогли в сложной судебной ситуации, их знания и опыт впечатляют. Большое спасибо за профессионализм и поддержку!',
                    first_name: 'Анатолий В.',
                },
                {
                    name: 'Бухгалтерия без головной боли',
                    text: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    text2: 'Бухгалтер предоставил мне возможность сосредоточиться на моем бизнесе, обеспечивая бесперебойное ведение финансовой документации. Он всегда находится на связи и быстро решает все вопросы. Очень доволен!',
                    first_name: 'Марк Ц.',
                },
                {
                    name: 'Прогнозирование успешных стратегий',
                    text: 'Квартальные документы – это ключевой элемент в нашем бизнесе, и сотрудничество с нашим бухгалтером сделало это процесс максимально гладким. Он всегда вовремя предоставляет необходимую финансовую отчетность, и мы можем полностью доверять точности и надежности этой информации.',
                    text2: 'Это освобождает наше время и ресурсы, позволяя нам сосредотачиваться на стратегическом развитии компании. Спасибо за проделанную работу!',
                    first_name: 'Сергей Б.',
                },

            ],
            offsets: [],
            touchStartY: 0,
            modules: [Navigation],
            navigation: {
                nextEl: '.next',
                prevEl: '.prev'
            },
            navigation2: {
                nextEl: '.next2',
                prevEl: '.prev2'
            },
            navigation3: {
                nextEl: '.next3',
                prevEl: '.prev3'
            },
            forUser: true,
            thirdSlideIndex: -1,
            experts: [],
            products: [],
        };
    },
    methods: {

        truncatedDescription(description, length) {
            const maxLength = length;
            if (description && description.length > maxLength) {
                return description.slice(0, maxLength) + '...';
            } else {
                return description;
            }
        },
        getPopulars() {
            const path = `${this.pathUrl}/api/products/popular-product?amount_products=10`;
            axios
                .get(path)
                .then(response => {
                    this.products = response.data
                })
                .catch(error => {
                    console.log(error)
                })
        },
        getExperts() {
            let url = `${this.pathUrl}/api/seller/all-seller`;
            axios
                .get(url)
                .then(response => {
                    this.experts = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        onSwiper(swiper) {
            swiper.on('slideChange', () => {
                // Получаем индекс третьего слайда (индексация начинается с 0)
                this.thirdSlideIndex = swiper.realIndex + 2;
            });
        },
        calculateSectionOffsets() {
            let sections = document.getElementsByTagName('section');
            let length = sections.length;

            for (let i = 0; i < length; i++) {
                let sectionOffset = sections[i].offsetTop;
                this.offsets.push(sectionOffset);
            }
        },

        handleMouseWheel: function (e) {

            if (e.wheelDelta < 30 && !this.inMove) {
                this.moveUp();
            } else if (e.wheelDelta > 30 && !this.inMove) {
                this.moveDown();
            }

            e.preventDefault();
            return false;
        },

        handleMouseWheelDOM: function (e) {

            if (e.detail > 0 && !this.inMove) {
                this.moveUp();
            } else if (e.detail < 0 && !this.inMove) {
                this.moveDown();
            }

            return false;
        },

        moveDown() {
            this.inMove = true;
            this.activeSection--;

            if (this.activeSection < 0) this.activeSection = this.offsets.length - 1;

            this.scrollToSection(this.activeSection, true);
        },

        moveUp() {
            this.inMove = true;
            this.activeSection++;

            if (this.activeSection > this.offsets.length - 1) this.activeSection = 0;

            this.scrollToSection(this.activeSection, true);
        },

        scrollToSection(id, force = false) {
            if (this.inMove && !force) return false;

            this.activeSection = id;
            this.inMove = true;

            let section = document.getElementsByTagName('section')[id];
            if (section) {
                document.getElementsByTagName('section')[id].scrollIntoView({ behavior: 'smooth' });
            }

            setTimeout(() => {
                this.inMove = false;
            }, this.inMoveDelay);

        },
        touchStart(e) {
            e.preventDefault();

            this.touchStartY = e.touches[0].clientY;
        },
        touchMove(e) {
            if (this.inMove) return false;
            e.preventDefault();

            const currentY = e.touches[0].clientY;
            const deltaY = currentY - this.touchStartY;

            if (Math.abs(deltaY) > 50) {
                this.inMove = true;
                if (deltaY > 0) {
                    this.moveDown();
                } else {
                    this.moveUp();
                }

                setTimeout(() => {
                    this.inMove = false;
                }, 1000); // Здесь вы можете настроить задержку в миллисекундах
            }

            this.touchStartY = 0;
            return false;
        }
    },
    mounted() {
        this.getPopulars()
        this.getExperts()
        this.calculateSectionOffsets();
        this.thirdSlideIndex = 2;

        const screenWidth = window.innerWidth;

        if (screenWidth >= 1025) {
            window.addEventListener('DOMMouseScroll', this.handleMouseWheelDOM);  // Mozilla Firefox
            window.addEventListener('mousewheel', this.handleMouseWheel, { passive: false }); // Other browsers

            window.addEventListener('touchstart', this.touchStart, { passive: false }); // mobile devices
            window.addEventListener('touchmove', this.touchMove, { passive: false }); // mobile devices
        }
    },
    destroyed() {
        window.removeEventListener('DOMMouseScroll', this.handleMouseWheelDOM); // Mozilla Firefox
        window.removeEventListener('mousewheel', this.handleMouseWheel, { passive: false });  // Other browsers

        window.removeEventListener('touchstart', this.touchStart); // mobile devices
        window.removeEventListener('touchmove', this.touchMove); // mobile devices
    }
};
</script>
<script setup>
const isMobile = window.innerWidth < 1024;

useSeoMeta({
    title: 'Главная | Experthub',
    ogTitle: 'Главная | Experthub',
    description: 'Главная | Experthub',
    ogDescription: 'Главная | Experthub',
    viewport: isMobile ? 'width=device-width, initial-scale=1.0' : 'width=1920',
})
</script>

<style lang="scss" scoped>
.prodslider {
    min-height: 450px;
    max-height: 450px;
}

.scale-down {
    transform: scaleY(0.9);
    transform-origin: top center;
    transition: transform 0.3s ease;
}

.fullpage {
    height: 100vh;
    width: 100%;
    background-color: #282828;
}

.green {
    display: flex;
    justify-content: center;
    align-items: left;
    flex-direction: column;
    width: 100%;

    @media (max-width: 1024px) {
        height: auto;
    }

    .reviews {
        padding: 0 100px;

        @media (max-width: 1600px) {
            padding: 0 50px;
        }

        @media (max-width: 1024px) {
            padding: 0 20px;
        }


        .header {
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: flex-end;

            h1 {
                font-size: 40px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                color: #fff;
                font-family: var(--mon);

                @media (max-width: 1024px) {
                    font-size: 24px;
                }
            }

            img {
                @media (max-width: 1024px) {
                    display: none;
                }
            }

        }

        .reviews__slider {
            position: relative;
            z-index: 10;

            .text-right {
                @media (max-width: 1024px) {
                    text-align: center !important;
                }
            }

            .review__nav {
                position: absolute;
                margin-top: -20px;
                right: 0;
                z-index: 10;

                @media (max-width: 1024px) {
                    position: relative;
                    margin-top: 20px;
                }

                img {
                    z-index: 10;
                }
            }


            .popular__item {
                border-radius: 10px;
                background: #F6F6F6;
                padding: 20px;

                h1 {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
                    margin-bottom: 15px;

                    @media (max-width: 1024px) {
                        font-size: 18px;
                    }
                }

                p {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
                    margin-bottom: 5px;

                    @media (max-width: 1024px) {
                        font-size: 14px;
                    }
                }

                h2 {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;

                    @media (max-width: 1024px) {
                        font-size: 18px;
                    }
                }
            }
        }
    }
}

.red {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    flex-direction: column;

    @media (max-width: 1024px) {
        height: auto;
        margin-top: 80px;
    }

    @media (max-width: 380px) {
        margin-top: 250px;
    }

    .about {
        padding: 0 100px 0 28px;
        display: flex;
        align-items: center;
        gap: 76px;

        @media (max-width: 1024px) {
            padding: 30px 20px;

        }

        .text {
            .syk {
                display: flex;
                justify-content: space-between;
                align-items: flex-end;

                img {
                    &:last-child {
                        @media (max-width: 1024px) {
                            display: none;
                        }
                    }
                }
            }

            .header {
                display: flex;
                gap: 30px;
                align-items: center;

                @media (max-width: 1024px) {
                    gap: 14px;
                    justify-content: center;
                    width: 100%;
                }

                .activeSpan {
                    color: #fff !important;
                }

                span {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #737373;
                    transition: all .3s ease;
                    cursor: pointer;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }


                    &:hover {
                        color: #fff;
                    }
                }
            }

            .body {
                margin-top: 45px;

                p {
                    font-size: 32px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                    margin-bottom: 30px;
                    max-width: 933px;

                    @media (max-width: 1600px) {
                        font-size: 25px;
                    }

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }

                a {
                    padding: 10px 30px;
                    border: 0;
                    border-radius: 10px;
                    background: #0072EE;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                    transition: all .3s ease;
                    text-decoration: none;

                    &:hover {

                        background: #004692;
                    }
                }
            }
        }

        .roll {
            position: relative;

            @media (max-width: 1024px) {
                display: none;
            }

            .roller {
                transform: rotate(0deg);
                animation: rotateAnimation 15s linear infinite;

                @media (max-width: 1600px) {
                    display: none;
                }
            }

            @keyframes rotateAnimation {
                0% {
                    transform: rotate(0deg);
                }

                100% {
                    transform: rotate(360deg);
                }
            }

            .nout {
                position: absolute;
                top: 5%;
                left: 5%;

                @media (max-width: 1600px) {
                    position: relative;
                    max-width: 100%;
                }
            }
        }
    }
}

.black {


    .popular {
        text-align: left;
        width: 100%;

        padding: 129px 200px 124px 100px;

        @media (max-width: 1600px) {
            padding: 129px 100px 124px 50px;
        }

        @media (max-width: 1024px) {
            padding: 30px 20px;
        }

        .popular__slider {
            position: relative;

            .xyeta {
                position: absolute;
                bottom: -20px;
                right: -20px;
            }

            .prev {
                position: absolute;
                left: -1.5%;
                top: 40%;
                z-index: 10;
                cursor: pointer;

                @media (max-width: 1024px) {
                    left: 0;
                }
            }

            .next {
                position: absolute;
                right: -1.5%;
                top: 40%;
                z-index: 10;
                cursor: pointer;
            }

        }

        .popular__item {
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

                margin-bottom: 15px;
            }

            .text-center {
                margin-top: auto;
            }

            a {
                margin-bottom: 10px;
                background: #000;
                text-decoration: none;

                &:hover {
                    background: #373737;
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
            margin-bottom: 20px;

            @media (max-width: 1024px) {
                font-size: 24px;
            }
        }

        p {
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            max-width: 1005px;
            margin-bottom: 30px;
        }

        a {
            padding: 10px 30px;
            border: 0;
            background: #0072EE;
            border-radius: 10px;

            font-size: 16px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            transition: all .3s ease;
            display: inline-block;
            text-decoration: none;

            &:hover {
                background: #004692;
            }
        }
    }
}

.blue {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    background: url('@/assets/img/mainsl.png') no-repeat;
    background-size: 100vw 100vh;


    @media (max-width: 500px) {
        background-size: 100% !important;
        background: url('@/assets/img/mainslmob.svg') no-repeat;

        height: auto;
        min-height: 671px;
    }

    div {
        margin-top: 200px;
        display: flex;
        align-items: center;
        flex-direction: column;
        height: 100%;
        justify-content: space-around;

        @media (max-width: 500px) {
            display: none;
        }
    }

    h2 {
        font-size: 36px;
        font-style: normal;
        font-weight: 500;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
        margin-top: 450px;

        @media (max-width: 500px) {
            display: none;
        }
    }
}


.purple {

    @media (max-width: 1024px) {
        height: auto;
        margin-bottom: 20px;
    }

    .popular {
        text-align: left;
        width: 100%;

        padding: 129px 200px 124px 100px;

        @media (max-width: 1600px) {
            padding: 129px 100px 124px 50px;
        }

        @media (max-width: 1024px) {
            padding: 30px 20px;
        }

        .popular__slider {
            position: relative;

            .xyeta {
                position: absolute;
                bottom: -20px;
                right: -20px;
            }

            .prev,
            .prev2 {
                position: absolute;
                left: -1.5%;
                top: 40%;
                z-index: 10;
                cursor: pointer;

                @media (max-width: 1024px) {
                    left: 0;
                }
            }

            .next,
            .next2 {
                position: absolute;
                right: -1.5%;
                top: 40%;
                z-index: 10;
                cursor: pointer;
            }

        }

        .popular__item {
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

                margin-bottom: 15px;
            }

            .text-center {
                margin-top: auto;
            }

            a {
                margin-bottom: 10px;
                background: #000;
                text-decoration: none;

                &:hover {
                    background: #373737;
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
            margin-bottom: 20px;

            @media (max-width: 1024px) {
                font-size: 24px;
            }
        }

        p {
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            max-width: 542px;
            margin-bottom: 30px;
        }

        a {
            padding: 10px 30px;
            border: 0;
            background: #0072EE;
            border-radius: 10px;

            font-size: 16px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            transition: all .3s ease;
            display: inline-block;
            text-decoration: none;

            &:hover {
                background: #004692;
            }
        }
    }
}


.sections-menu {
    position: fixed;
    right: 1rem;
    top: 50%;
    transform: translateY(-50%);
    z-index: 10;

    @media (max-width: 1024px) {
        display: none;
    }
}

.sections-menu .menu-point {
    width: 15px;
    height: 15px;
    background-color: transparent;
    display: block;
    margin: 1rem 0;
    opacity: 1;
    border: 2px solid #fff;
    border-radius: 50%;
    transition: .4s ease-in-out all;
    cursor: pointer;
}

.sections-menu .menu-point.active {
    opacity: 1;
    transform: scale(1.5);
    background-color: #fff;
}

.sections-menu .menu-point:hover {
    opacity: 1;
    transform: scale(1.2);
}
</style>