<template>
    <div class="paypage">
        <PrevPage></PrevPage>
        <div class="payform">
            <div class="form__body">
                <div class="text-center">
                    <h1>Вывод средств</h1>
                </div>
                <div class="type" v-if="accountType == 'buyer'">
                    <NuxtLink to="/refill">Пополнение</NuxtLink>
                    <NuxtLink to="/withdrawal" class="activeBtn">Вывод</NuxtLink>
                </div>
                <p>Порядок действий для вывода средств</p>

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
                <p class="sum">2. Введите сумму, которую вы хотите вывести с личного счета, и нажмите на кнопку «Вывести».
                    Вы будете
                    переадресованы на сайт платёжной системы, где сможете завершить операцию.</p>
                <div class="card">
                    <input type="text" class="mb-3 w-100" name="card" id="card" v-model="cardNumber"
                        :maxlength="cardNumberMaxLength" placeholder="Введите номер карты" @input="formatCardNumber"
                        autocomplete="cc-number">
                    <input type="text" class="mb-3 w-100" name="cardHolder" id="card" v-model="cardHolder"
                        placeholder="Введите владельца карты" autocomplete="cc-name">
                </div>
                <div class="input">
                    <input type="number" name="cost" id="cost" placeholder="Сумма" v-model="cost">
                    <button ref="outBtn" @click="outMoney">Вывести</button>
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
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            cost: null,
            pathUrl: 'https://experthub.kz',
            cardNumber: '',
            cardHolder: '',
            cardNumberMaxLength: 19,
            accountType: '',
            checked: false,
        }
    },
    methods: {
        outMoney() {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/money/pay-return`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            this.$refs.outBtn.innerHTML = 'ОЖИДАЙТЕ'

            axios
                .post(path, {
                    amount: this.cost,
                    card_number: this.cardNumber.replace(/\s/g, ''),
                    cardholder: this.cardHolder
                })
                .then(response => {
                    console.log(response)
                    if (response.status == 200) {
                        this.$refs.outBtn.innerHTML = 'УСПЕШНО'
                    }
                    if (response.status == 228) {
                        this.$refs.outBtn.innerHTML = response.data.error_msg
                    }

                })
                .catch(error => {
                    console.error(error)
                    this.$refs.outBtn.innerHTML = 'ВЫВЕСТИ'
                })
        },
        formatCardNumber() {
            // Удаляем все символы, кроме цифр
            this.cardNumber = this.cardNumber.replace(/\D/g, '');

            // Добавляем разделитель каждые 4 символа
            this.cardNumber = this.cardNumber.replace(/(.{4})/g, '$1 ');

            // Обрезаем карточный номер до максимальной длины
            this.cardNumber = this.cardNumber.slice(0, this.cardNumberMaxLength);
        }
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

    },
    watch: {
        cardNumber(newCardNumber) {
            this.cardHolder = this.cardNumberToHolderMapping[newCardNumber] || "";
        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Вывод средств | Experthub',
    ogTitle: 'Вывод средств | Experthub',
    description: 'Вывод средств | Experthub',
    ogDescription: 'Вывод средств | Experthub',
})
</script>
<style lang="scss" scoped>
.card {
    background: 0;

    input {
        background: transparent;
        border: 2px solid #fff;
        padding: 10px 15px;
        border-radius: 10px;

        font-size: 18px;
        font-family: var(--mon);
        color: #fff;

        @media (max-width: 1024px) {
            font-size: 16px;
        }
    }
}

.paypage {
    padding: 120px 100px 78px;

    @media (max-width: 1024px) {
        padding: 120px 20px 78px;
    }

    .payform {
        display: flex;
        align-items: center;
        justify-content: center;

        @media (max-width: 1024px) {
            margin-top: 30px;
        }

        .form__body {
            border-radius: 10px;
            background: #000;
            box-shadow: 0px 0px 15px 0px rgba(0, 0, 0, 0.10);
            padding: 40px;

            @media (max-width: 1024px) {
                padding: 20px;
            }

            .sum {
                @media (max-width: 1024px) {
                    font-size: 12px !important;
                }
            }

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

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }

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

                @media (max-width: 1024px) {
                    justify-content: center;
                }

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

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }

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

                    @media (max-width: 1024px) {
                        max-width: 150px;
                    }
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

                    @media (max-width: 1024px) {
                        font-size: 16px;
                        padding: 10px 0;
                    }
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

                @media (max-width: 1024px) {
                    font-size: 24px;
                }
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
                white-space: normal;

                @media (max-width: 1024px) {
                    font-size: 16px;
                    margin-bottom: 10px;
                }
            }

            a {
                color: #fff;
            }
        }
    }
}

.custom-checkbox {
    @media (max-width: 1024px) {
        margin-bottom: 10px !important;
    }

    p {
        @media (max-width: 1024px) {
            font-size: 12px !important;
        }
    }
}
</style>