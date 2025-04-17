<script  setup lang="ts">
import { ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';
import bcrypt from 'bcryptjs';
import { userLogin } from '../stores/user';


const email = ref('')
const password = ref('');
const router = useRouter();

const login = async () => {
    try {
        const response = await fetch(`http://localhost:3000/user?email=${email.value}`);
        const users = await response.json();

        if (users.length === 0) {
            alert('Người dùng không tồn tại');
            return;
        }

        const user = users[0];
        const isPasswordValid = await bcrypt.compare(password.value, user.password);

        if (isPasswordValid) {
            userLogin({
                email: user.email,
                role: user.role || 'user',
                fullName: user.fullName || ''
            });
            router.push('/');
        } else {
            alert('Đăng nhập thất bại');
        }
    } catch (error) {
        console.error('Login error:', error);
        alert('Đăng nhập thất bại');
    }
};


</script>
<template>
    <main>
        <TheWelcome />
            <main>
                <div>
                    <div class="container-sm w-50 bg-light pt-5 pb-5 rounded-5 mt-5 mb-5 align-items-center">
                        <form @submit.prevent="login" class="needs-validation container w-75" novalidate>
                            <h1>Đăng nhập</h1>
                            <p class="d-flex">Cần tài khoản? <a class="text-primary"><RouterLink class="dropdown-item" to="/signup">Tạo tài khoản mới</RouterLink></a></p>
                            <div class="mb-3 mt-4">
                                <label for="exampleInputEmail1" class="form-label">Tài khoản mail</label>
                                <div class="input-group mb-3">
                                    <input type="email" v-model="email" class="form-control rounded-4" style="height: 50px;" placeholder=""
                                        aria-label="Recipient's username" aria-describedby="basic-addon2" required>
                                </div>
                            </div>
                            <div class=" mb-3">
                                <label for="exampleInputPass" class="form-label">Mật khẩu</label>
                                <input v-model="password" type="text"  class="form-control rounded-4" style="height: 50px;" id="exampleInputPass"
                                    aria-describedby="Password" required>

                            </div>
                            <div class="mb-3 form-check">
                                <input type="checkbox" class="form-check-input" id="exampleCheck1">
                                <label for="exampleCheck1">Ghi nhớ đăng nhập của tôi</label>
                            </div>
                            <button type="submit" class="btn btn-danger rounded-5" style="height: 50px;"><i class="fa-solid fa-spin "><i
                                        class="fa-solid fa-bounce "><i class="fa-solid fa-right-to-bracket fa-shake"></i> </i></i>
                                Đăng nhập</button><br>
                            <br>
                            <a href="DoiMatKhau.html">Quên tài khoản? Quên mật khẩu?</a><br>
                            <a href="">Bạn không thể đăng nhập?</a>
                        </form>
                    </div>
                    </div>
            </main>
    </main>
  </template>
  

  

  
  