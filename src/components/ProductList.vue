<template>
    <div 
        class="productList"
    >
        <div 
            v-if="products.length > 0"
        >
            <product-card
                v-for="product in products"
                :product="product"
                :key="product.id"
                @remove="$emit('remove', product)"
                @update="$emit('update', product)"
                draggable="true"
                @dragstart="startDrag($event, product)"
            />
        </div>
        <h2 
            v-else
            style="color: red;"    
        >
            <strong>Список продуктов пуст</strong>
        </h2>
    </div>
</template>

<script>
    import ProductCard from '@/components/ProductCard';

    export default {
        components: {ProductCard},
        props: {
            products: {
                type: Array,
                required: true
            }
        },
        methods: {
            startDrag(event, item) {
                console.log(item);
                event.dataTransfer.dropEffect = 'move';
                event.dataTransfer.effectAllowed = 'move';
                event.dataTransfer.setData('itemID', item.id);
            }
        }
    }
</script>

<style scoped>
    .productList {
        display: flex;
        flex-direction: column;
        width: 75%;
    }
</style>