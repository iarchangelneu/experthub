<template>
    <div class="transactions text-center">
        <table class="table table-borderless">
            <thead>
                <tr>
                    <th scope="col">Тип</th>
                    <th scope="col">Цена</th>
                    <th scope="col">Дата</th>
                    <th scope="col">Статус</th>
                    <th scope="col">Состояние счета</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in transactions.slice().reverse()" :key="item.id">
                    <td>{{ item.type_operation }}</td>
                    <td> {{ item.amount.toLocaleString() }} ₸</td>
                    <td>{{ formatDate(item.date) }}</td>
                    <td :class="{ success: item.paid == 1, failure: item.paid == 0 }">{{ item.paid == 1 ? 'Совершено' :
                        item.paid == 0 ? 'Отменено' : item.paid }}</td>
                    <td>{{ calculateAmountNow(item) }} ₸ </td>
                </tr>
            </tbody>
        </table>

        <div class="mobtrans">

            <div class="mob__body" v-for="item in transactions.slice().reverse()" :key="item.id">
                <div class="paddingblya">
                    <div class="body__header">
                        <small>{{ item.type_operation }}</small>
                        <span>{{ formatDate(item.date) }}</span>
                        <img src="@/assets/img/success.svg" alt="">
                    </div>
                    <div class="body__footer">
                        <div>
                            <small>Цена</small>
                            <span>+ {{ item.amount.toLocaleString() }} ₸</span>
                        </div>
                        <div>
                            <small>Состояние счета</small>
                            <span>{{ calculateAmountNow(item) }} ₸</span>
                        </div>
                    </div>
                </div>
                <hr>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        transactions: [],
    },
    data() {
        return {}
    },

    methods: {
        calculateAmountNow(item) {
            if (item.type_operation === 'ВЫВОД' && item.paid === 0) {
                return (item.amount_now + item.amount).toLocaleString();
            } else if (item.type_operation === 'ПОПОЛНЕНИЕ' && item.paid === 0) {
                return (item.amount_now - item.amount).toLocaleString();
            } else {
                return item.amount_now.toLocaleString();
            }
        },
        formatDate(dateTime) {
            // Создаем объект Date из строки времени
            const date = new Date(dateTime);

            // Получаем отдельные компоненты даты и времени
            const day = String(date.getDate()).padStart(2, '0');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const year = String(date.getFullYear()).slice(-2);
            const hours = String(date.getHours()).padStart(2, '0');
            const minutes = String(date.getMinutes()).padStart(2, '0');

            // Собираем строку в нужном формате
            return `${day}.${month}.${year} ${hours}:${minutes}`;
        },
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Транзакции | Experthub',
    ogTitle: 'Транзакции | Experthub',
    description: 'Транзакции | Experthub',
    ogDescription: 'Транзакции | Experthub',
})
</script>
<style lang="scss" scoped>
hr {
    border-top: 1px solid #fff !important;
}

.transactions {
    margin-top: 38px;

    .mobtrans {
        display: none;

        border-radius: 30px;
        background: #000;

        .mob__body {

            &:last-child {
                hr {
                    display: none;
                }
            }

            .paddingblya {

                padding: 30px;


                .body__header {
                    display: flex;
                    justify-content: space-between;

                    small {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        font-family: var(--mon);
                        color: #fff;
                        opacity: 0.5;
                    }

                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        font-family: var(--mon);
                        color: #fff;
                        white-space: nowrap;
                    }
                }

                .body__footer {
                    display: flex;
                    justify-content: space-between;
                    margin-top: 15px;
                    text-align: left;

                    small {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        font-family: var(--mon);
                        color: #fff;
                        opacity: 0.5;
                        text-align: left;
                    }

                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        font-family: var(--mon);
                        color: #fff;
                        display: block;
                        text-align: left;
                        margin-top: 10px;
                        //   white-space: nowrap;
                    }

                }
            }
        }

        @media (max-width: 1024px) {
            display: block;
        }
    }

    table {
        @media (max-width: 1024px) {
            display: none;
        }

        th {
            font-size: 20px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            font-family: var(--mon);
            color: #FFF;
            opacity: 0.5;
        }

        td {
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            font-family: var(--mon);
            color: #fff;


        }

        .success {
            color: #03B820 !important;
        }

        .failure {
            color: #EA0505 !important;
        }
    }


}
</style>