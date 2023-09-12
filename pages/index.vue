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
                <swiper :slides-per-view="5" :space-between="30" :navigation="navigation" :modules="modules">
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/pop1.png" alt="">

                            <div class="popular__body">
                                <h1>Юрист</h1>
                                <h2>Елена Туманова</h2>
                                <p>Эксперт обладает глубокими знаниями законодательства и практическим опытом в различных
                                    сферах права</p>
                                <div class="text-center">
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/pop1.png" alt="">

                            <div class="popular__body">
                                <h1>Юрист</h1>
                                <h2>Елена Туманова</h2>
                                <p>Эксперт обладает глубокими знаниями законодательства и практическим опытом в различных
                                    сферах права</p>
                                <div class="text-center">
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/pop1.png" alt="">

                            <div class="popular__body">
                                <h1>Юрист</h1>
                                <h2>Елена Туманова</h2>
                                <p>Эксперт обладает глубокими знаниями законодательства и практическим опытом в различных
                                    сферах права</p>
                                <div class="text-center">
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/pop1.png" alt="">

                            <div class="popular__body">
                                <h1>Юрист</h1>
                                <h2>Елена Туманова</h2>
                                <p>Эксперт обладает глубокими знаниями законодательства и практическим опытом в различных
                                    сферах права</p>
                                <div class="text-center">
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/pop1.png" alt="">

                            <div class="popular__body">
                                <h1>Юрист</h1>
                                <h2>Елена Туманова</h2>
                                <p>Эксперт обладает глубокими знаниями законодательства и практическим опытом в различных
                                    сферах права</p>
                                <div class="text-center">
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/pop1.png" alt="">

                            <div class="popular__body">
                                <h1>Юрист</h1>
                                <h2>Елена Туманова</h2>
                                <p>Эксперт обладает глубокими знаниями законодательства и практическим опытом в различных
                                    сферах права</p>
                                <div class="text-center">
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
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
                <swiper :slides-per-view="5" :space-between="30" :navigation="navigation2" :modules="modules">
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/usl1.png" alt="">

                            <div class="popular__body">
                                <h1>Квартальные документы</h1>
                                <p>Представление бухгалтерской квартальной отчетности является обязанностью всех организаций
                                </p>
                                <div class="text-center">
                                    <h2>23 000 ₸</h2>
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/usl1.png" alt="">

                            <div class="popular__body">
                                <h1>Квартальные документы</h1>
                                <p>Представление бухгалтерской квартальной отчетности является обязанностью всех организаций
                                </p>
                                <div class="text-center">
                                    <h2>23 000 ₸</h2>
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/usl1.png" alt="">

                            <div class="popular__body">
                                <h1>Квартальные документы</h1>
                                <p>Представление бухгалтерской квартальной отчетности является обязанностью всех организаций
                                </p>
                                <div class="text-center">
                                    <h2>23 000 ₸</h2>
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/usl1.png" alt="">

                            <div class="popular__body">
                                <h1>Квартальные документы</h1>
                                <p>Представление бухгалтерской квартальной отчетности является обязанностью всех организаций
                                </p>
                                <div class="text-center">
                                    <h2>23 000 ₸</h2>
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/usl1.png" alt="">

                            <div class="popular__body">
                                <h1>Квартальные документы</h1>
                                <p>Представление бухгалтерской квартальной отчетности является обязанностью всех организаций
                                </p>
                                <div class="text-center">
                                    <h2>23 000 ₸</h2>
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/usl1.png" alt="">

                            <div class="popular__body">
                                <h1>Квартальные документы</h1>
                                <p>Представление бухгалтерской квартальной отчетности является обязанностью всех организаций
                                </p>
                                <div class="text-center">
                                    <h2>23 000 ₸</h2>
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
                            </div>
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="popular__item">
                            <img src="@/assets/img/usl1.png" alt="">

                            <div class="popular__body">
                                <h1>Квартальные документы</h1>
                                <p>Представление бухгалтерской квартальной отчетности является обязанностью всех организаций
                                </p>
                                <div class="text-center">
                                    <h2>23 000 ₸</h2>
                                    <NuxtLink to="/">Подробнее</NuxtLink>
                                </div>
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
                <swiper :slides-per-view="3" :space-between="43" :navigation="navigation3" :modules="modules"
                    @swiper="onSwiper" ref="mySwiper">
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
export default {
    components: {
        Swiper,
        SwiperSlide,
    },
    data() {
        return {
            inMove: false,
            inMoveDelay: 400,
            activeSection: 0,
            reviews: [
                {
                    name: 'Прогнозирование успешных стратегий',
                    text: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    text2: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    first_name: 'Виктория С.',
                },
                {
                    name: 'Прогнозирование успешных стратегий',
                    text: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    text2: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    first_name: 'Виктория С.',
                },
                {
                    name: 'Прогнозирование успешных стратегий',
                    text: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    text2: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    first_name: 'Виктория С.',
                },
                {
                    name: 'Прогнозирование успешных стратегий',
                    text: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    text2: 'Аналитик предоставил нам инсайды, которые помогли нам принимать обоснованные решения. Он был легко доступен для обсуждения результатов анализа и дал нам уверенность в наших действиях',
                    first_name: 'Виктория С.',
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
        };
    },

    methods: {
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

            if (this.touchStartY < currentY) {
                this.moveDown();
            } else {
                this.moveUp();
            }

            this.touchStartY = 0;
            return false;
        }
    },
    mounted() {
        this.calculateSectionOffsets();
        this.thirdSlideIndex = 2;

        window.addEventListener('DOMMouseScroll', this.handleMouseWheelDOM);  // Mozilla Firefox
        window.addEventListener('mousewheel', this.handleMouseWheel, { passive: false }); // Other browsers

        window.addEventListener('touchstart', this.touchStart, { passive: false }); // mobile devices
        window.addEventListener('touchmove', this.touchMove, { passive: false }); // mobile devices
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
useSeoMeta({
    title: 'Главная | Experthub',
    ogTitle: 'Главная | Experthub',
    description: 'Главная | Experthub',
    ogDescription: 'Главная | Experthub',
})
</script>
<style lang="scss" scoped>
.scale-down {
    transform: scaleY(0.85);
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

    .reviews {
        padding: 0 100px;

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
            }

        }

        .reviews__slider {
            position: relative;
            z-index: 10;

            .review__nav {
                position: absolute;
                margin-top: -40px;
                right: 0;
                z-index: 10;

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
                }

                p {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
                    margin-bottom: 5px;
                }

                h2 {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
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

    .about {
        padding: 0 100px 0 28px;
        display: flex;
        align-items: center;
        gap: 76px;

        .text {
            .syk {
                display: flex;
                justify-content: space-between;
                align-items: flex-end;
            }

            .header {
                display: flex;
                gap: 30px;
                align-items: center;

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

            .roller {
                transform: rotate(0deg);
                animation: rotateAnimation 15s linear infinite;
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
            }
        }
    }
}

.black {


    .popular {
        text-align: left;
        width: 100%;

        padding: 129px 200px 124px 100px;

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
            width: 300px;



            img {
                width: 100%;
                height: 275px;
            }

            .popular__body {
                padding: 10px 15px 10px;

                h1 {
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

                a {
                    background: #000;

                    &:hover {
                        background: #373737;
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
            margin-bottom: 20px;
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

    div {
        margin-top: 200px;
        display: flex;
        align-items: center;
        flex-direction: column;
        height: 100%;
        justify-content: space-around;
    }

    h2 {
        font-size: 36px;
        font-style: normal;
        font-weight: 500;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
        margin-top: 450px;
    }
}


.purple {
    .popular {
        text-align: left;
        width: 100%;

        padding: 129px 200px 124px 100px;

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
            width: 300px;



            img {
                width: 100%;
                height: 275px;
            }

            .popular__body {
                padding: 10px 15px 10px;

                h1 {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 600;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
                    margin-bottom: 10px;
                }

                h2 {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 600;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
                    margin-bottom: 10px;
                }

                p {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    color: #000;
                    font-family: var(--mon);
                    margin-bottom: 15px;
                }

                p {
                    margin-bottom: 15px;
                }

                a {
                    background: #000;

                    &:hover {
                        background: #373737;
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
            margin-bottom: 20px;
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