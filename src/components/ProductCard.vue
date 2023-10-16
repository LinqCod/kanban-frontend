<template>
    <div class="product" style="stylesProduct">
        <div>
            <div><strong>ID:</strong> {{ product.id }}</div>
            <div><strong>Название:</strong> {{ product.title }} </div>
            <div><strong>Цена:</strong> {{ product.price }} </div>
            <div><strong>Описание:</strong> {{ product.description }} </div>
            <div><strong>Категория:</strong> {{ product.category }} </div>
            <div><strong>Оценка:</strong> {{ product.rating.rate }} </div>
        </div>
        <div class="product_btns">
            <my-button
                class="my-button"
                @click="$emit('update', product)"
            >
                Редактировать
            </my-button>
            <my-button
                class="my-button"
                @click="$emit('remove', product)"
            >
                Удалить
            </my-button>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            product: {
                type: Object,
                required: true
            }
        },
        computed: {
            bgColor() {
                switch(this.product.listName) {
                    case 'raw':
                        return 'rgb(255, 88, 58)';
                    case 'inwork':
                        return 'rgb(54, 181, 255)';
                    case 'completed':
                        return 'rgb(136, 255, 39)';
                }
            }
        }
    }
</script>

<style scoped>
    .product {
        padding: 15px;
        border: 2px solid black;
        border-radius: 5px;
        background-color: v-bind(bgColor);
        margin-top: 15px;
        font-size: 12px;
        display: flex;
        flex-direction: column;
    }

    .product_btns {
        display: flex;
        justify-content: end;
        margin-top: 15px;
    }

    .my-button {
        margin-left: 15px;
    }
</style>