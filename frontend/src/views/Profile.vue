<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { email, isAuthenticated, userLogin, userLogout } from '../stores/user';
import { useRouter } from 'vue-router';
import ProductList from '../components/ProductList.vue';
import type { User } from '../stores/user'; 

const users = ref<User | null> (null);

const user = ref<any>({});

const router = useRouter();

onMounted(async() =>{
    const users = await fetch(`http://localhost:3000/user?email=${email.value}`)
                                .then(respones => respones.json());

    user.value = users[0];
    console.log(user);

});

const handelLogout=() => {
    userLogout();
    router.push('login')
}

</script>

<template>
    <div class=" container-sm w-50 bg-light pt-5 pb-5 rounded-5 mt-5 mb-5 align-items-center">
        <div v-if="user" class="row">
            <div class="col-6">
                <img src="/images/gal_1.jpg" style="border-radius: 200px" class="d-block w-50 " alt="...">
            </div>
            <div class="col-6">
                <h2>{{user.email}}</h2>
                <h2>{{user.fullname}}</h2>
                <h2>{{user.address}}</h2>
                <h2>{{user.phone}}</h2>
            </div>
        </div>

    </div>
</template>