<script setup>
import { ref, computed, onBeforeMount } from 'vue'
import ProductsList from '@/components/ProductsList.vue';
import initialProducts from '@/constants/products.js'
import moment from 'moment';

const products = ref([]);
const currentProductsListPage = ref(1);
const numberOfProductsByPage = ref(5);
const numberOfProductsByPageOptions = [5, 10, 20];

const filteredProducts = ref([]);

const resetCurrentProductsListPage = () => {
  currentProductsListPage.value = 1;
};

const getProductsByPage = computed(() => {
  const start = (currentProductsListPage.value - 1) * numberOfProductsByPage.value;
  const end = start + numberOfProductsByPage.value;
  return filteredProducts.value.slice(start, end);
});

const searchText = ref('');
const handleSearch = () => {
  if (searchText.value.length)  {
    filteredProducts.value = products.value.filter((product) => product.name.toLowerCase().includes(searchText.value.toLowerCase()));
  } else {
    filteredProducts.value = [...products.value];
  }

  resetCurrentProductsListPage();
};

const addProductText = ref('');
const addProduct = () => {
  if (!addProductText.value.length) {
    return;
  }

  const newProduct = {
    name: addProductText.value,
    image: 'https://picsum.photos/48',
    status: 'green',
    date: moment(Date.now()),
  };

  let i = 0;
  while (addProductText.value.toLowerCase() > products.value[i].name.toLowerCase()) {
    i += 1;
  }
  products.value.splice(i, 0, newProduct);

  handleSearch();
};

const getListPageDetails = () => {
  const numberOfPages = Math.ceil(filteredProducts.value.length / numberOfProductsByPage.value);
  return `Page <b>${currentProductsListPage.value}</b> of <b>${numberOfPages}</b>`;
};

const getSortDetails = () => (`Sort by <b>Product Name</b>`);

const prevPage = () => {
  if (currentProductsListPage.value === 1) {
    return;
  }

  currentProductsListPage.value -= 1;
};

const nextPage = () => {
  const numberOfPages = Math.ceil(filteredProducts.value.length / numberOfProductsByPage.value);
  if (currentProductsListPage.value === numberOfPages) {
    return;
  }

  currentProductsListPage.value += 1;
};

onBeforeMount(() => {
  initialProducts.map((item) => {
    products.value.push(item);
  });

  products.value.sort((a, b) => (a.name <= b.name ? -1 : 1));

  filteredProducts.value = [...products.value];
})
</script>

<template>
  <div class="main-container">
    <p class="page-title">Products</p>
    
    <div class="products-container">
      <div class="inputs-container">
        <input
          v-model="searchText" 
          class="input-text" 
          type="text"
          placeholder="Find a Product" 
        />
        <button class="buttons search" @click="handleSearch">Search</button>
      </div>

      <div class="inputs-container">
        <input
          v-model="addProductText" 
          class="input-text" 
          type="text"
          placeholder="Product Name" 
        />
        <button class="buttons add-product" @click="addProduct">Add Product</button>
      </div>

      <ProductsList :productList="getProductsByPage"/>
    </div>
    
    <div class="pagination">
      <div class="pagination-sections">
        <a href="#" class="pagination-nav-text" @click="prevPage">Prev</a>
        <a href="#" class="pagination-nav-text" @click="nextPage">Next</a>
      </div>
      
      <div class="pagination-sections">
        <p v-html="getListPageDetails()" class="pagination-text"></p>
        <p v-html="getSortDetails()" class="pagination-text"></p>
        <select v-model="numberOfProductsByPage" class="products-by-page" @change="resetCurrentProductsListPage">
          <option v-for="option in numberOfProductsByPageOptions">{{ option }}</option>
        </select>
      </div>
    </div>
  </div>
</template>

<style lang="less" scoped>
.main-container {
  display: flex;
  flex-direction: column;
  padding: 1rem;
  background-color: #ececec;
  min-height: 100vh;
}

.page-title {
  color: #252526;
  font-size: 1.5rem;
  font-weight: 500;
  line-height: 2rem;
  font-family: 'Bitter';
  margin: 0;
  margin-bottom: 1rem;
}

.products-container {
  display: flex;
  flex-direction: column;
  padding: 1rem;
  background-color: #fff;
  border: 1px solid #e0e2e4;
  border-radius: 0.5rem;
  gap: 0.5rem
}

.inputs-container {
  display: flex;
  gap: 0.5rem;
  width: 100%;
}

.input-text {
  background-color: #fff;
  color: #939597;
  font-family: 'Inter';
  font-weight: 400;
  font-size: 0.875rem;
  line-height: 1.375rem;
  border: 1px solid #e0e2e4;
  border-radius: 0.25rem;
  width: 100%;
  padding: 0 0.5rem;
}

.buttons {
  color: #fff;
  font-family: 'Inter';
  font-weight: 400;
  font-size: 0.75rem;
  line-height: 1rem;
  padding: 0.6875rem 1rem;
  white-space: nowrap;
}

.search {
  background-color: #375673;
}

.add-product {
  background-color: #377364;
}

.pagination {
  display: flex;
  flex-direction: column;
  padding: 0 1rem;
}

.pagination-sections {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
}

.products-by-page {
  border: none;
  background-color: #ececec;
  font-family: 'Inter';
  font-weight: 700;
  font-size: 0.75rem;
  line-height: 1rem;
}

.pagination-text {
  font-family: 'Inter';
  font-weight: 400;
  font-size: 0.75rem;
  line-height: 1rem;
}

.pagination-nav-text {
  color: #000;
  font-family: 'Inter';
  font-weight: 400;
  font-size: 0.75rem;
  line-height: 1rem;
  margin-top: 0.3rem;
}
</style>
