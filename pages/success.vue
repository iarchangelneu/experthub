<template>
    <div class="success">
        <div>
            <div class="text">
                <h1>Транзакция прошла успешно</h1>
                <img src="@/assets/img/bsuccess.svg" alt="">
            </div>

            <div class="text-center">
                <NuxtLink to="/">На главную</NuxtLink>
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
            pathUrl: 'https://experthub.kz',
        }
    }
    ,
    methods: {
        sendRequest(reference) {
            const token = this.getAuthorizationCookie();
            const path = `${this.pathUrl}/api/money/success/${reference}`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                    if (response.status == 200) {
                        //  window.location.href = '/'
                    }
                })
                .catch(error => {
                    console.log(error);
                });
        },
    },
    mounted() {
        const url = window.location.href;
        const match = url.match(/order_pay_main_experthub_(\d+)/);

        if (match) {
            this.extractedValue = match[0];
            console.log(this.extractedValue);

            this.sendRequest(match[0])
        }
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Успех | Experthub',
    ogTitle: 'Успех | Experthub',
    description: 'Успех | Experthub',
    ogDescription: 'Успех | Experthub',
})
</script>
<style lang="scss" scoped>
.success {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;

    @media (max-width: 1024px) {
        margin: 120px 0 50px;
    }

    div {
        .text {
            display: flex;
            align-items: center;
            gap: 15px;

            h1 {
                font-size: 24px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                margin: 0;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }
        }

        .text-center {
            a {
                display: inline-block;
                padding: 10px 30px;
                border: 0;
                border-radius: 10px;
                background: #0072EE;
                margin-top: 70px;

                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;

                @media (max-width: 1024px) {
                    margin-top: 40px;
                }

                transition: all .3s ease;

                &:hover {
                    background: #004692;
                }
            }
        }
    }
}
</style>