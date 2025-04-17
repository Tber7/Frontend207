<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import type { Product } from '../stores/Product';

const product = ref<Product | null> (null);
const route = useRoute();

onMounted(async()=>{
    product.value = await fetch(`http://localhost:3000/products/${route.params.id}`).then(response =>response.json());
    console.log(product.value);
})

</script>

<template>
    <div v-if="product" class="row">
            <div class="col-6 text-center">
                <img class="img-fluid" width="400" height="1000"  :src="'/images/'+ product.image">
            </div>
            <div class="col-6">
                <h2>{{ product.name }}</h2>  
                <p>Giá: {{ product.price.toLocaleString() }}$</p>  
                <p>Giảm Giá Còn: {{ product.discounted.toLocaleString() }}$</p>
                <p>Nhà Sản Xuất: {{ product.manufacturer }}</p>  
                <p>Khối Lượng: {{ product.weight }}</p>  
                <p>Kích Thước: {{ product.size }}</p>  
                <p>Nam Châm: {{ product.magnetic }}</p>
                <p>Mô Tả: {{ product.details }}</p>
  
                <button class="btn btn-primary">Đặt ngay</button>
            </div>
    </div>
    <div v-else>
        <h1>Loading...</h1>
    </div>
</template>