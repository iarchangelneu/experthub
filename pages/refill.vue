<template>
    <div class="paypage">
        <PrevPage></PrevPage>
        <div class="payform">
            <div class="form__body">
                <div class="text-center">
                    <h1>Пополнение баланса</h1>
                </div>
                <div class="type" v-if="accountType == 'buyer'">
                    <NuxtLink to="/refill" class="activeBtn">Пополнение</NuxtLink>
                    <NuxtLink to="/withdrawal">Вывод</NuxtLink>
                </div>
                <p>Порядок действий для пополнения баланса</p>

                <p>1. Подтвердите Ваше согласие с правилами нашей системы</p>

                <label class="custom-checkbox">
                    <input type="checkbox" v-model="checked">
                    <p class="checkbox-text m-0" ref="checked">Принимаю условия <NuxtLink to='/terms'>
                            Пользовательского<br>
                            соглашения
                        </NuxtLink> и
                        <NuxtLink to='/polytics'>Политики конфиденциальности</NuxtLink>

                    </p>
                </label>
                <p>2. Введите сумму, на которую Вы хотите пополнить личный счет, и нажмите на кнопку “Пополнить”. Вы будете
                    переадресованы на сайт платежной системы, где сможете завершить платеж.</p>

                <div class="input">
                    <input type="number" name="cost" id="cost" placeholder="Сумма" v-model="cost">
                    <button ref="inBtn" @click="inMoney">Пополнить</button>
                </div>

                <div class="presets">
                    <button @click="cost = 100" :class="{ activeBtn: cost == 100 }">100 ₸</button>
                    <button @click="cost = 1000" :class="{ activeBtn: cost == 1000 }">1000 ₸</button>
                    <button @click="cost = 2000" :class="{ activeBtn: cost == 2000 }">2000 ₸</button>
                    <button @click="cost = 5000" :class="{ activeBtn: cost == 5000 }">5000 ₸</button>
                    <button @click="cost = 10000" :class="{ activeBtn: cost == 10000 }">10000 ₸</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios'
export default {
    mixins: [global],
    data() {
        return {
            cost: null,
            pathUrl: 'https://experthub.kz',
            accountType: '',
            checked: false,
        }
    },
    methods: {
        inMoney() {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/money/new-pay`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            this.$refs.inBtn.innerHTML = 'ОЖИДАЙТЕ'

            axios
                .post(path, {
                    amount: this.cost
                })
                .then(response => {
                    console.log(response)
                    window.location.href = response.data.url
                    if (response.status = 201) {
                        this.$refs.inBtn.innerHTML = 'ПОПОЛНИТЬ'
                    }
                    if (response.status == 228) {
                        this.$refs.outBtn.innerHTML = response.data.error_msg
                    }
                })
                .catch(error => {
                    console.error(error)
                    this.$refs.inBtn.innerHTML = 'ПОПОЛНИТЬ'
                })
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        if (accType !== 'buyer-account' && accType !== 'seller-account') {
            window.location.href = '/login'
        }
        if (accType == 'buyer-account') {
            this.accountType = 'buyer'

        }
        else if (accType == 'seller-account') {
            this.accountType = 'seller'
        }
        else {
            return
        }

    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Пополнение баланса | Experthub',
    ogTitle: 'Пополнение баланса | Experthub',
    description: 'Пополнение баланса | Experthub',
    ogDescription: 'Пополнение баланса | Experthub',
})
</script>
<style lang="scss" scoped>
.paypage {
    padding: 120px 100px 78px;

    .payform {
        display: flex;
        align-items: center;
        justify-content: center;

        .form__body {
            border-radius: 10px;
            background: #000;
            box-shadow: 0px 0px 15px 0px rgba(0, 0, 0, 0.10);
            padding: 40px;

            .type {
                display: flex;
                margin-bottom: 20px;

                .activeBtn {
                    color: #000 !important;
                    background: #fff !important;
                }

                a {
                    flex: 1;
                    background: transparent;
                    border: 2px solid #fff;
                    border-radius: 10px;
                    padding: 12px 0;
                    display: inline-block;
                    text-align: center;

                    font-size: 24px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 110%;
                    font-family: var(--mon);
                    color: #fff;
                    transition: all .3s ease;

                    &:hover {
                        color: #000;
                        background: #fff;
                    }

                    &:first-child {
                        border-top-right-radius: 0;
                        border-bottom-right-radius: 0;
                        border-right: 0;
                    }

                    &:last-child {
                        border-top-left-radius: 0;
                        border-bottom-left-radius: 0;
                        border-left: 0;
                    }
                }
            }

            .presets {
                display: flex;
                flex-wrap: wrap;
                gap: 10px;
                margin-top: 25px;

                .activeBtn {
                    background: #fff !important;
                    color: #000 !important;
                }

                button {
                    border-radius: 10px;
                    border: 2px solid #fff;
                    background: transparent;
                    padding: 13px 14px;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;
                    transition: all .3s ease;

                    &:hover {
                        color: #000;
                        background: #fff;
                    }
                }
            }

            .input {
                display: flex;
                gap: 20px;

                input {
                    flex: 1;
                    max-width: 227px;
                    padding: 12px 11px;
                    background: transparent;
                    border-radius: 10px;
                    border: 2px solid #fff;
                    text-align: right;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #fff;
                }

                button {
                    flex: 1;
                    border-radius: 10px;
                    border: 2px solid #fff;
                    background: #fff;
                    padding: 12px 0;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--mon);
                    color: #000;
                }
            }

            h1 {
                font-size: 32px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                font-family: var(--mon);
                color: #fff;
                margin-bottom: 15px;
            }

            p {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
                margin-bottom: 25px;
                max-width: 535px;
            }

            a {
                color: #fff;
            }
        }
    }
}
</style>