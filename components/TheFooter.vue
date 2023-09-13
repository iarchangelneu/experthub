<template>
    <footer v-if="!hideFooterOnPages.includes($route.name)">
        <img src="@/assets/img/footerline.svg" class="img-fluid" alt="">

        <div class="footer__body">
            <div class="footer__flex">
                <img src="@/assets/img/footerlogo.svg" alt="">

                <div class="links">
                    <div>
                        <NuxtLink to="/about">О платформе</NuxtLink>
                        <NuxtLink to="/for-expert">Для эксперта</NuxtLink>
                    </div>
                    <div>
                        <NuxtLink to="/terms">Пользовательское соглашение</NuxtLink>
                        <NuxtLink to="/polytics">Политика конфиденциальности</NuxtLink>
                    </div>
                </div>
            </div>

            <div class="text-center">
                <small>ТОО WORK 111, 230440033562, Казахстан, город Астана, район Алматы, улица Қалибек Қуанышбаев, дом
                    15/2, кв. 111, почтовый индекс 010000</small>
                <small>Почта поддержки : support@experthub.kz</small>
            </div>
        </div>
    </footer>
</template>
<script>
export default {
    data() {
        return {
            hideFooterOnPages: ['login', 'register', 'index'],
        }
    }
}
</script>
<style lang="scss" scoped>
footer {
    padding: 0 100px 30px;

    .footer__body {
        margin-top: 42px;

        .text-center {
            margin-top: 47px;


            small {
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                display: block;
            }
        }

        .footer__flex {
            display: flex;
            justify-content: space-between;
        }

        .links {
            display: flex;
            gap: 78px;

            div {
                display: flex;
                flex-direction: column;
                gap: 25px;

                a {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                }
            }
        }
    }
}
</style>