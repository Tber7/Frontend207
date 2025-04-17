<script setup lang="ts">
  import { ref, onMounted, computed } from 'vue';
  import type { Product } from '../stores/Product';

  const products = ref<Product[]>([]);
  const keyword = ref('');
  const sort = ref('');

  onMounted(async () =>{
    const response = await fetch('http://localhost:3000/products');
    products.value = await response.json();
    console.log(products);
  });

  const filteredAndSortedProducts = computed(() => {
    let filteredProducts = products.value.filter(product =>
      product.name.toLowerCase().includes(keyword.value.toLowerCase())
    );
    if (sort.value === 'asc') {
      filteredProducts.sort((a, b) => a.price - b.price);
    }else{
      filteredProducts.sort((a, b) => b.price - a.price);
    }
    return filteredProducts; 
  });
</script>


<template>
  <div>
    <div class="row md-4 mt-3 d-flex justify-content-between">
      <div class="col-md-2 ms-2 ">
        <input v-model="keyword" type="text" placeholder="Tim kiem san pham" class="form-control">
      </div>
      <div class="col-md-2 me-2">
        <select v-model="sort" class="form-control">
          <option value="" disabled selected>Sap xep theo gia</option>
          <option value="asc">gia tang dan</option>
          <option value="desc">gia giam dan</option>
        </select>
      </div>
    </div>
    <div class="row mt-3"> 
      <div class="col-md-3" v-for="product in filteredAndSortedProducts" :key="product.id">
        <div class="card mb-4">
            <img :src="'/images/'+product.image" class="h-5 w-55">
          <div class="card-body">
            <p><i class="bi bi-hand-thumbs-up"></i>{{ product.name }}</p>
            <p>{{ product.price }}</p>
            <p>{{ product.duration}}</p>
            <div class="text-center">
                        <RouterLink :to="'/detail/'+ product.id" class="btn btn-primary">Mua</RouterLink>
                    </div>
          </div>
        </div>
      </div>
    </div>
  </div>
 
</template>



  

  
 
  
  