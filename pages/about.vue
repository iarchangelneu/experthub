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
            <h1>О платформе</h1>

            <p class="mb-3">ExpertHelp - это вдохновляющая онлайн-платформа, созданная для того, чтобы помочь
                предпринимателям
                и предприимчивым людям достичь успеха в мире бизнеса. На ExpertHelp собраны опытные эксперты и профессионалы
                со всех уголков бизнес-сферы, готовые поделиться своими знаниями и навыками, чтобы помочь другим реализовать
                свои бизнес-идеи и управлять успешными предприятиями.</p>
            <p class="mb-3">Основные особенности ExpertHelp:</p>
            <ol>
                <li>Широкий спектр экспертиз: На платформе представлены эксперты и менторы со знаниями в различных
                    областях
                    бизнеса, включая управление, маркетинг, финансы, стратегическое планирование, технологии и другие.
                    Вы найдёте специалистов, готовых помочь вам в любом аспекте вашего бизнеса.</li>
                <li>Онлайн консультации: ExpertHelp предоставляет возможность проводить онлайн-консультации, вебинары
                    и обмен
                    опытом с экспертами, что позволяет учиться и развиваться в удобное для вас время и месте.</li>
                <li>Кастомизированные рекомендации: Эксперты адаптируют свои советы и рекомендации к вашей конкретной
                    ситуации,
                    помогая вам разработать стратегии и планы, которые наилучшим образом подходят для вашего бизнеса.</li>
                <li>Сообщество и сеть контактов: ExpertHelp помогает вам создать ценные связи и сеть контактов
                    в бизнес-сфере.
                    Вы можете обмениваться опытом с другими предпринимателями и находить партнёров для совместных проектов.
                </li>
                <li>Успех и рост: Миссия ExpertHelp - помочь вам достичь успеха и роста вашего бизнеса.
                    Эксперты
                    нашей
                    платформы готовы стать вашими наставниками и проводниками к успеху.</li>
            </ol>

            <p>Наша платформа — это место, где знания и опыт сливаются воедино, чтобы поддержать вас в вашем
                предпринимательском пути. Независимо от того, являетесь ли вы новичком в мире бизнеса или опытным
                предпринимателем, мы предоставляет вам доступ к ресурсам и менторству, необходимым для успешного бизнеса.
                Добро пожаловать в ExpertHelp, где ваш успех — наш приоритет.</p>

            <NuxtLink to="/register">Зарегистрироваться</NuxtLink>
        </div>
    </div>
</template>
<script>
// AOS.init();
</script >
<script setup>
useSeoMeta({
    title: 'О платформе | Experthub',
    ogTitle: 'О платформе | Experthub',
    description: 'О платформе | Experthub',
    ogDescription: 'О платформе | Experthub',
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