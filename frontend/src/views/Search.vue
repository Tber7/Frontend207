<script setup lang="ts">
import { ref, onMounted, computed, watch } from 'vue';
import { useRoute } from 'vue-router';
import type { Product } from '../stores/Product';

const products = ref<Product[]>([]);
const route = useRoute();
const keyword = ref(String(route.query.tukhoa || ""));

onMounted(async()=>{
    products.value=await fetch('http://localhost:3000/products').then(response=>response.json());
})

const filteredProducts = computed(()=>{
    return products.value.filter(product => product.name.toLowerCase().includes(keyword.value.toLowerCase()));
    return products.value.filter(product => product.price.toString().includes(keyword.value.toString()));
})

watch(()=>route.query.tukhoa,(newQuery)=>{
    keyword.value = String(newQuery || "");
})

</script>
<template>
    <div>
        <h4>Ket qua tim kiemmcho tu khoa: {{keyword}}</h4><br>
        <div class="row mt-3"> 
            <div class="col-md-3" v-for="product in filteredProducts" :key="product.id">
                <div class="card mb-4">
                <img :src="'/images/'+product.image" class="card-img-top" alt="">
                <div class="card-body">
                    <p>{{ product.name }}</p>
                    <p>{{ product.price }}</p>
                    <button class="btn btn-primary">Dat tourz</button>
                </div>
                </div>
            </div>
        </div>
    </div>
</template>