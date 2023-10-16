<template>
    <div class="app">
        <h1>Канбан система продуктов</h1>
        <div class="app-container">
            <div class="raw products-container"
                @drop="onDrop($event, 'raw')"   
                @dragenter.prevent
                @dragover.prevent 
            >
                <h3>
                    Необработанные
                </h3>
                <div class="options-container">
                    <my-button @click="showDialog(null, 'raw', 'create')">Добавить продукт</my-button>
                    <my-select
                        class="my-select"
                        v-model="rawSort"
                        :options="sortOptions"
                    />
                </div>  
                <product-list 
                    :products="getSortedProducts('raw', this.rawSort)"
                    @remove="removeProduct"
                    @update="showDialog($event, 'raw', 'update')"
                    v-if="!isProductsLoading"
                />
                <div
                    v-else
                    style="margin-top: 15px;"
                >
                    Загрузка продуктов...
                </div>
            </div>
            <div class="inwork products-container"
                @drop="onDrop($event, 'inwork')"   
                @dragenter.prevent
                @dragover.prevent 
            >
                <h3>
                    В работе
                </h3>
                <div class="options-container">
                    <my-button @click="showDialog(null, 'inwork', 'create')">Добавить продукт</my-button>
                    <my-select
                        class="my-select"
                        v-model="inworkSort"
                        :options="sortOptions"
                    />
                </div>  
                <product-list 
                    :products="getSortedProducts('inwork', this.inworkSort)"
                    @remove="removeProduct"
                    @update="showDialog($event, 'inwork', 'update')"
                    v-if="!isProductsLoading"
                />
                <div
                    v-else
                    style="margin-top: 15px;"
                >
                    Загрузка продуктов...
                </div>
            </div>
            <div class="completed products-container"
                @drop="onDrop($event, 'completed')"   
                @dragenter.prevent
                @dragover.prevent 
            >
                <h3>
                    Завершенные
                </h3>
                <div class="options-container">
                    <my-button @click="showDialog(null, 'completed', 'create')">Добавить продукт</my-button>
                    <my-select
                        class="my-select"
                        v-model="completedSort"
                        :options="sortOptions"
                    />
                </div>  
                <product-list 
                    :products="getSortedProducts('completed', this.completedSort)"
                    @remove="removeProduct"
                    @update="showDialog($event, 'completed', 'update')"
                    v-if="!isProductsLoading"
                />
                <div
                    v-else
                    style="margin-top: 15px;"
                >
                    Загрузка продуктов...
                </div>
            </div>
            <my-dialog v-model:show="dialogVisible">
                <product-form
                    v-if="formType === 'create'"
                    :list="currentAddingProductType"
                    @create="createProduct"
                />
                <edit-product-form
                    v-else-if="formType === 'update'"
                    @update="updateProduct"
                    :product="productToUpdate"
                />
            </my-dialog>
        </div>
    </div>
</template>
  
<script>
    import ProductForm from '@/components/ProductForm';
    import EditProductForm from '@/components/EditProductForm';
    import ProductList from '@/components/ProductList';
    import axios from 'axios';

    export default {
        components: {ProductForm, EditProductForm, ProductList},
        data() {
            return {
                currentProductType: 'raw',
                currentAddingProductType: 'raw',
                products: [],
                dialogVisible: false,
                productToUpdate: {
                    id: 0,
                    title: '',
                    price: 0,
                    description: '',
                    category: '',
                    rate: 0,
                    listName: ''
                },
                formType: '',
                isProductsLoading: false,
                rawSort: '',
                inworkSort: '',
                completedSort: '',
                sortOptions: [
                    {value: 'rateAsc', name: 'По возрастанию рейтинга'},
                    {value: 'rateDesc', name: 'По убыванию рейтинга'}
                ]
            }
        },
        methods: {
            createProduct(product) {
                this.products.push(product);
                this.dialogVisible = false;
            },
            removeProduct(product) {
                this.products = this.products.filter(p => p.id !== product.id);
            },
            updateProduct(product) {
                const id = this.products.findIndex(p => {
                            return p.id === product.id
                        });
                this.products[id] = product;

                this.dialogVisible = false;
            },
            showDialog(product, type, formType) {
                if (formType === 'update' && product != null) {
                    this.productToUpdate = product;
                } else {
                    this.currentAddingProductType = type;
                }
                this.currentProductType = type;
                this.formType = formType;
                this.dialogVisible = true;
            },
            async fetchProducts() {
                try {
                    this.isProductsLoading = true;
                    const response = await axios.get('http://localhost:8080/api/v1/products');
                    this.products = response.data.map(obj => ({ ...obj, listName: 'raw' }));
                } catch(e) {
                   alert('Ошибка получения данных с апишки') 
                } finally {
                    this.isProductsLoading = false;
                }
            },
            getSortedProducts(listName, sortOrder) {
                const pr = this.products.filter((p) => p.listName === listName);
                switch(sortOrder) {
                    case 'rateAsc':
                        return pr.sort((p1, p2) => {
                            return p1['rate'] < p2['rate'] ? -1 :
                                p1['rate'] > p2['rate'] ? 1 : 0; 
                        });
                    case 'rateDesc':
                        return pr.sort((p1, p2) => {
                            return p1['rate'] > p2['rate'] ? -1 :
                                p1['rate'] < p2['rate'] ? 1 : 0; 
                        });
                }
                
                return pr;
            },
            onDrop(event, list) {
                const itemID = event.dataTransfer.getData('itemID');
                const item = this.products.find((item) => item.id == itemID);
                item.listName = list;
            }
        },
        mounted() {
            this.fetchProducts();
        }
    }
</script>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box; 
        font-family: Nunito, sans-serif;
    }

    body {
        background: rgb(238, 238, 238);
    }

    .app {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 25px 0;
    }

    .app-container {
        padding: 20px;
        width: 75%;
        display: flex;
    }

    .options-container {
        display: flex;
        justify-content: space-between;
        margin-top: 15px;
        width: 75%;
    }

    .products-container {
        margin: 0 8px;
        padding: 25px;
        width: 33%;
        height: fit-content;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .raw, .inwork, .completed {
        border-radius: 25px;
        background: white;
    } 

    .my-select {
        margin-left: 15px;
    }
</style>