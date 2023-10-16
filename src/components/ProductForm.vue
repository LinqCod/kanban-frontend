<template>
    <form @submit.prevent>
        <h4>Создание карточки продукта</h4>
        <my-input
            class="my-input"
            v-model="product.title"
            type="text"
            placeholder="Название"
        />
        <my-input
            class="my-input"
            v-model="product.price"
            type="number" 
            step="any"
            min="0"
            placeholder="Цена"
        />
        <my-input
            class="my-input"
            v-model="product.description"
            type="text"
            placeholder="Описание"
        />
        <my-input
            class="my-input"
            v-model="product.category"
            type="text"
            placeholder="Категория"
        />
        <my-input
            class="my-input"
            v-model="product.rate"
            type="number"
            step="0.1"
            min="0"
            max="5"
            placeholder="Оценка"
        />
        <my-button
            @click="createProduct"
            style="align-self: flex-end; margin-top: 15px;"
            >
            Создать
        </my-button>
    </form>
</template>

<script>
    export default {
        props: {
            list: {
                type: String, 
                required: true
            }
        },
        data() {
            return {
                product: {
                    title: '',
                    price: 0,
                    description: '',
                    category: '',
                    rate: 0,
                    listName: ''
                }
            }
        },
        methods: {
            createProduct() {
                this.product.id = Date.now();
                this.product.listName = this.list;
                this.$emit('create', this.product)
                this.product = {
                    title: '',
                    price: 0,
                    description: '',
                    category: '',
                    rate: 0,
                    listName: ''
                }
            }
        }
    }
</script>

<style scoped>
    form {
        display: flex;
        flex-direction: column;
    }

    .my-input {
        margin-top: 15px;
    }
</style>