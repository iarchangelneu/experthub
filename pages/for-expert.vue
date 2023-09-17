<template>
    <div class="about">
        <div class="about__left">
            <div>
                <span>документация</span>
            </div>
            <div>
                <span>БИЗНЕС-АНАЛИТИКА</span>
            </div>
            <div>
                <span>МАРКЕТИНГ</span>
                <span>логистика</span>
            </div>
            <div>
                <span>юридические услуги</span>
                <span>финансовый аудит</span>
            </div>
            <div>
                <span>консалтинг</span>
                <span>Бухгалтерия</span>
                <span>страхование</span>
            </div>
        </div>

        <div class="about__right">
            <h1>Для эксперта</h1>

            <p class="mb-3">Платформа ExpertHelp предоставляет экспертам уникальные возможности для успешной практики
                и расширения клиентской базы. Размещая свои услуги на нашей платформе, вы можете ожидать следующих
                преимуществ:</p>
            <ol>
                <li>Доступ к широкой аудитории: ExpertHelp объединяет клиентов со всего мира, что даёт вам возможность
                    привлечь новых клиентов и расширить свою клиентскую базу. Это позволяет вам работать с клиентами,
                    которые нуждаются в вашей экспертизе и специализации.</li>
                <li>Продвижение вашей практики: Мы предоставляем вам возможность создания профиля, который будет виден
                    потенциальным клиентам. Вы можете представить свои навыки, специализацию и опыт, что поможет
                    вам привлечь клиентов, которые ищут именно вас.</li>
                <li>Гибкий график и удалённая работа: ExpertHelp позволяет вам работать с клиентами удалённо,
                    что обеспечивает гибкий график и возможность управлять своей практикой в соответствии с вашими
                    потребностями.</li>
                <li>Рейтинг и отзывы: Клиенты могут оценивать и оставлять отзывы о вашей работе. Позитивные отзывы и высокий
                    рейтинг помогут вам установить доверие среди потенциальных клиентов и увеличить вашу видимость
                    на платформе.
                </li>
                <li>Безопасность и конфиденциальность: Мы обеспечиваем безопасность и конфиденциальность всех консультаций
                    и сделок на платформе, что делает её надёжным местом для предоставления юридических услуг.</li>
                <li>Удобная система оплаты: ExpertHelp предоставляет прозрачную и удобную систему оплаты, обеспечивая
                    быстрое и надёжное получение оплаты за ваши услуги.</li>
            </ol>

            <p>ExpertHelp - это партнёрство для вас, как для юридического эксперта. Мы стремимся предоставить вам средства
                и возможности для успешной и процветающей практики, обеспечивая вас клиентами и инструментами
                для эффективной работы в сфере права.</p>

            <NuxtLink to="/register">Зарегистрироваться</NuxtLink>
        </div>
    </div>
</template>
<script>
// AOS.init();
</script >
<script setup>
useSeoMeta({
    title: 'Для эксперта | Experthub',
    ogTitle: 'Для эксперта | Experthub',
    description: 'Для эксперта | Experthub',
    ogDescription: 'Для эксперта | Experthub',
})
</script>
<style lang="scss" scoped>
.about {
    padding: 150px 100px 113px;
    display: flex;
    align-items: center;
    gap: 80px;

    @media (max-width: 1600px) {
        padding: 150px 50px 113px;
    }

    @media (max-width: 1024px) {
        padding: 120px 20px 50px;
    }

    .about__left {
        display: flex;
        gap: 20px;
        flex-direction: column;
        justify-content: flex-end;
        align-items: flex-end;

        @media (max-width: 1024px) {
            display: none;
        }

        @keyframes loader {
            0% {
                transform: translateX(-700px)
            }

            100% {
                transform: translateX(0px)
            }
        }

        div {
            display: flex;
            gap: 20px;

            animation: loader .2s ease-in-out;

            &:nth-child(2) {
                animation: loader .5s ease-in-out;
            }

            &:nth-child(3) {
                animation: loader .7s ease-in-out;
            }

            &:nth-child(4) {
                animation: loader .9s ease-in-out;
            }

            &:nth-child(5) {
                animation: loader 1s ease-in-out;
            }

            span {
                border-radius: 10px;
                background: #000;
                border: 0;
                padding: 15px 30px;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                text-transform: uppercase;
                font-family: var(--jost);
                color: #fff;

                @media (max-width: 1440px) {
                    padding: 10px 20px;
                    font-size: 16px;
                }
            }
        }
    }

    .about__right {
        max-width: 1031px;

        h1 {
            font-size: 32px;
            font-style: normal;
            font-weight: 600;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            margin-bottom: 30px;

            @media (max-width: 1024px) {
                font-size: 24px;
            }
        }

        ol {
            padding: 0 20px;
        }

        a {
            display: inline-block;
            background: #0072EE;
            border: 0;
            border-radius: 10px;
            padding: 10px 30px;

            font-size: 16px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            transition: all .3s ease;
            margin-top: 30px;

            &:hover {
                background: #004692;
            }
        }

        p,
        li {
            margin: 0;
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
            padding: 0;
        }
    }
}
</style>