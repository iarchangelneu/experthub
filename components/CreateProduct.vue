<template>
    <div>
        <div class="course">
            <div class="course__left">
                <div>
                    <label for="name">Название товара</label>
                    <input type="text" id="name" name="name" placeholder="Введите название" ref="name" v-model="name">
                </div>
                <div>
                    <label for="name">Краткое описание</label>
                    <input type="text" id="name" name="name" placeholder="Введите краткое описание" v-model="shortDesc">
                </div>

                <div class="select">
                    <div class="mb-0">
                        <label for="select">Выберите категорию</label>
                        <select name="select" id="select" v-model="selectedCategory" ref="select">
                            <option v-for="(category, index) in categories" :key="index" :value="category.id">{{
                                category.name
                            }}</option>
                        </select>
                    </div>
                    <span v-if="selectedCategory !== null">
                        {{ getCategoryName(selectedCategory) }}
                        <img src="@/assets/img/del.svg" alt="" loading="lazy" @click="clearSelection">
                    </span>
                </div>

                <div class="discount">
                    <div>
                        <label for="name">Введите цену</label>
                        <input type="number" id="name" name="name" placeholder="Цена за товар" v-model="price">
                    </div>
                </div>
                <div>
                    <label for="name">Загрузка изображения</label>
                    <div class="mb-0 fileupload" @dragover.prevent="highlightDropArea"
                        @dragleave.prevent="unhighlightDropArea" @drop.prevent="handleDrop" @click="openFileInput">
                        <span>{{ alert }}</span>
                        <small>Открыть</small>
                        <input type="file" ref="fileInput" style="display: none !important" accept="image/*"
                            @change="handleFileInput" />
                    </div>


                    <div class="uploaded-images">
                        <div v-if="selectedImage" class="uploaded-image">
                            <img :src="selectedImage" alt="Uploaded Image" />
                        </div>
                    </div>
                </div>
            </div>

            <div class="course__right">
                <div>
                    <label for="tabs">Особенности услуги (До 6)</label>
                    <div class="custom-textarea">
                        <textarea class="features-list" сols="30" rows="6" ref="featuresList" v-model="courseFeaturesText"
                            @keydown="handleKeyDown"></textarea>
                    </div>
                </div>
                <div>
                    <label for="tabs">Описание услуги</label>
                    <textarea name="" id="" cols="30" rows="10" v-model="description">

                </textarea>
                </div>
            </div>
        </div>

        <div class="text-center">
            <button ref="createProduct" @click="submitForm">Опубликовать услугу</button>
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
            selectedCategory: null,
            pathUrl: 'https://experthub.kz',
            discount: 0,
            price: 0,
            name: '',
            shortDesc: '',
            description: '',
            alert: 'Перетащите файлы сюда или откройте вручную',
            selectedImage: null, // Сюда будет сохранено выбранное изображение
            selectedFile: null,
            courseFeaturesText: '',
            categories: [
                {
                    id: 1,
                    name: 'Бухгалтерия'
                },
                {
                    id: 2,
                    name: 'Документация'
                },
                {
                    id: 3,
                    name: 'Бизнес-аналитика'
                },
                {
                    id: 4,
                    name: 'Логистика'
                },
                {
                    id: 5,
                    name: 'Юридические услуги'
                },
                {
                    id: 6,
                    name: 'Страхование'
                },
                {
                    id: 7,
                    name: 'Консалтинг'
                },
                {
                    id: 8,
                    name: 'Финансовый аудит'
                },
                {
                    id: 9,
                    name: 'Маркетинг'
                },
            ],
        }
    },

    methods: {
        async submitForm() {
            const csrf = this.getCSRFToken()
            if (this.name.length > 0) {
                this.$refs.name.style.borderColor = '#fff'

                if (this.selectedCategory != null) {
                    this.$refs.select.style.borderColor = '#fff'
                    const path = `${this.pathUrl}/api/seller/seller-lk/add-product/`
                    const formData = new FormData();




                    formData.append('name', this.name);
                    formData.append('category', this.selectedCategory);
                    formData.append('price', this.price);
                    formData.append('description', this.description);
                    formData.append('key_features', this.courseFeaturesText);
                    formData.append('short_description', this.shortDesc);
                    if (this.selectedFile == null) {
                        formData.append('main_image', '');
                    }
                    else {
                        formData.append('main_image', this.selectedFile);
                    }
                    this.$refs.createProduct.disabled = true
                    this.$refs.createProduct.innerHTML = 'СОЗДАЕМ ЗАКАЗ'

                    try {
                        axios.defaults.headers.common['X-CSRFToken'] = csrf;
                        axios.defaults.headers.post['Content-Type'] = 'multipart/form-data';
                        const response = await axios.post(path, formData);
                        console.log('Форма успешно отправлена', response);
                        if (response.status == 201) {
                            this.$refs.createProduct.innerHTML = 'Заказ успешно создан!'
                        }
                    } catch (error) {
                        console.error('Ошибка при отправке формы', error);
                        this.$refs.createProduct.disabled = false
                        this.$refs.createProduct.innerHTML = 'Ошибка при создании заказа'
                    }
                }
                else {
                    this.$refs.select.style.borderColor = 'red'
                }
            }
            else {
                this.$refs.name.style.borderColor = 'red'
            }

        },
        handleKeyDown(event) {
            if (event.key === 'Backspace' && !this.courseFeaturesText) {
                event.preventDefault(); // Предотвращаем удаление последней строки
                return;

            }

            const lines = this.courseFeaturesText.split('\n');
            if (lines.length >= 6 && event.key === 'Enter') {
                event.preventDefault();
                this.$refs.featuresList.style.borderColor = 'red' // Предотвращаем добавление новой строки после 6 строк
                return;
            }
        },
        getCategoryName(categoryId) {
            const category = this.categories.find((c) => c.id === categoryId);
            return category ? category.name : "";
        },
        clearSelection() {
            this.selectedCategory = null;
        },
        limitValue() {
            if (this.discount > 100) {
                this.discount = 100; // Ограничиваем значение до 100
            }
        },
        highlightDropArea(event) {
            // Подсветить область при перетаскивании
            event.currentTarget.classList.add('highlight');
        },
        unhighlightDropArea(event) {
            // Убрать подсветку области при завершении перетаскивания
            event.currentTarget.classList.remove('highlight');
        },
        handleDrop(event) {
            // Обработать файлы, перетащенные на область
            event.preventDefault();
            this.unhighlightDropArea(event);

            const files = event.dataTransfer.files;
            this.handleFileInput(files); // Передаем файлы в метод handleFileInput
        },
        handleFileInput(event) {
            const files = event.target ? event.target.files : event; // Получаем файлы из события
            const file = files[0]; // Получаем первый выбранный файл
            if (file) {
                // Сохраняем выбранное изображение и файл
                this.selectedImage = URL.createObjectURL(file);
                this.selectedFile = file;
            }
        },

        openFileInput() {
            // Проксировать клик на скрытый input для выбора файлов
            this.$refs.fileInput.click();
        },
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Добавление услуги | Experthub',
    ogTitle: 'Добавление услуги | Experthub',
    description: 'Добавление услуги | Experthub',
    ogDescription: 'Добавление услуги | Experthub',
})
</script>
<style lang="scss" scoped>
.text-center {
    button {
        margin-top: 30px;
        padding: 10px 30px;
        border: 0;
        border-radius: 10px;
        background: #0072EE;

        font-size: 16px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
        transition: all .3s ease;

        &:hover {
            background: #004692;
        }
    }
}

.course {
    margin-top: 34px;
    display: flex;
    gap: 20px;
    width: 100%;

    @media (max-width: 1024px) {
        flex-direction: column;
        margin-top: 20px;
    }

    label,
    input {
        display: block;
    }

    label {
        margin-left: 10px;
        font-size: 16px;
        font-style: normal;
        font-weight: 500;
        line-height: 110%;
        font-family: var(--mon);
        color: #fff;
    }

    .uploaded-images {
        display: flex;
        flex-wrap: wrap;
        gap: 9px;
        margin-top: 20px;

        .uploaded-image {
            max-width: auto !important;
            width: auto !important;
        }

        img {
            width: 259px;
            height: 177px;

            @media (max-width: 1024px) {
                width: 165px;
                height: 110px;
            }
        }
    }

    input,
    select {
        background: transparent;
        border: 2px solid #fff;
        border-radius: 10px;
        padding: 10px;
        width: 100%;

        font-size: 16px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
    }

    .course__left {
        max-width: 527px;

        @media (max-width: 1024px) {
            max-width: 100%;
            width: 100%;
        }

        .highlight {
            border: 2px dashed green !important;
            cursor: pointer;
            /* Добавьте курсор-указатель, чтобы показать, что область можно нажать */
        }

        .fileupload {
            border-radius: 10px;
            border: 2px solid #fff;
            padding: 10px;
            display: flex;
            justify-content: space-between;
            cursor: pointer;


            span {
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
            }

            small {
                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                text-decoration-line: underline;
                font-family: var(--mon);
                color: #fff;
            }
        }

        width: 100%;

        div {
            margin-bottom: 20px;
            max-width: 527px;

            @media (max-width: 1024px) {
                max-width: 100%;
                width: 100%;
            }

            &:last-child {
                margin-bottom: 0;
            }
        }

        .discount {
            display: flex;
            gap: 10px;

            div {
                flex: 1;
            }
        }

        .select {
            display: flex;
            align-items: flex-end;
            flex-wrap: wrap;
            gap: 10px;

            @media (max-width: 1024px) {
                flex-direction: column;
                align-items: flex-start;
            }

            span {
                flex: 1;
                padding: 11px 10px;
                margin-top: 4px;
                border: 2px solid #fff;
                border-radius: 10px;

                display: flex;
                align-items: center;
                justify-content: space-between;
                gap: 10px;
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;

                @media (max-width: 1024px) {}

                img {
                    cursor: pointer;
                }
            }
        }
    }

    .course__right {
        width: 100%;

        .custom-textarea {
            width: 100%;
            background: transparent;

        }

        .features-list {
            outline: none;
            white-space: pre-wrap;
            width: 100%;
        }

        textarea {
            width: 100%;
            border-radius: 10px;
            border: 2px solid #fff;
            background: transparent;
            padding: 20px;
            color: #fff;
        }

        div {
            margin-bottom: 20px;

            &:last-child {
                margin-bottom: 0;
            }
        }
    }
}
</style>