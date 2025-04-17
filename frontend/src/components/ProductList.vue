<script setup lang="ts">
import type { Product } from '../stores/Product';

defineProps<{
    title: string;
    products : Product[];

}>();
</script>

<template>
    <div>
        <h3>{{ title }}</h3>
        <div class="row">
            <div class="col-md-3" v-for="product in products" :key="product.id">
                <div class="card mb-4">
                    <img :src="'/images/'+product.image" class="card-img-top" alt="">
                <div class="card-body">
                    <h5  class="card-title"> <i class="bi bi-hand-thumbs-up"></i> {{ product.name }}  </h5>
                    <p>{{ product.price.toLocaleString() }} VND</p>
                    <p>{{ product.description.toLocaleString() }}</p>
                    <div class="text-center">
                        <RouterLink :to="'/services/'+ product.id" class="btn btn-primary">Mua</RouterLink>
                    </div>
                </div>
                </div>
            </div>
        </div>
    </div>
</template>