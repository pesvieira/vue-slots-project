<script setup>
import ProductCard from '@/components/ProductCard.vue';
import moment from 'moment';

const props = defineProps({
  productList: { type: Array, default: () => [] },
});

const formatDate = (date) => moment(date).format('M/D/YYYY');
</script>

<template>
  <div class="list">
    <ProductCard v-for="product in productList" :leftIcon="product.image">
      <div class="product-container">
        <div class="product-details">
          <div :class="`indicator ${product.status}`"></div>
          <p class="product-text">{{ product.name }}</p>
        </div>
        <p v-if="product.date" class="product-text">{{ formatDate(product.date) }}</p>
      </div>
    </ProductCard>
  </div>
</template>

<style lang="less" scoped>
.list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;

  @media only screen and (min-width: 768px) {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-column-gap: 1rem;
  }
}

.product-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 0.5rem;
  width: 100%;
}

.product-details {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.indicator {
  background-color: #000;
  border-radius: 50%;
  width: 0.625rem;
  height: 0.625rem;
}

.product-text {
  color: #252526;
  font-family: 'Inter';
  font-weight: 400;
  font-size: 0.875rem;
  line-height: 1.375rem;
}

.red {
  background-color: #c63434;
}

.yellow {
  background-color: #e4d33a;
}

.green {
  background-color: #518103;
}
</style>
