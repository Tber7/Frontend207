<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

interface Product {
  id?: number;
  name: string;
  price: number;
  discounted: number;
  image: string;
  category: number;
  quantity: number;
  // Các trường khác giữ nguyên
}

const API_URL = 'http://localhost:3000/products';
const products = ref<Product[]>([]);
const editingProduct = ref<Product>({
  name: '',
  price: 0,
  discounted: 0,
  image: '',
  category: 1,
  quantity: 0
});
const showModal = ref(false);

// Lấy danh sách sản phẩm
const fetchProducts = async () => {
  try {
    const res = await axios.get(API_URL);
    products.value = res.data;
  } catch (err) {
    console.error('Lỗi khi lấy dữ liệu:', err);
  }
};

// Xử lý upload ảnh
const handleImageUpload = (e: Event) => {
  const input = e.target as HTMLInputElement;
  if (input.files && input.files[0]) {
    const reader = new FileReader();
    reader.onload = (event) => {
      editingProduct.value.image = event.target?.result as string;
    };
    reader.readAsDataURL(input.files[0]);
  }
};

// Lưu sản phẩm
const saveProduct = async () => {
  try {
    if (!editingProduct.value.name || editingProduct.value.price <= 0) {
      alert('Vui lòng điền đầy đủ thông tin');
      return;
    }

    if (editingProduct.value.id) {
      await axios.put(`${API_URL}/${editingProduct.value.id}`, editingProduct.value);
    } else {
      await axios.post(API_URL, editingProduct.value);
    }
    
    await fetchProducts();
    showModal.value = false;
    resetForm();
  } catch (err) {
    console.error('Lỗi khi lưu sản phẩm:', err);
  }
};

// Reset form
const resetForm = () => {
  editingProduct.value = {
    name: '',
    price: 0,
    discounted: 0,
    image: '',
    category: 1,
    quantity: 0
  };
};

// Mở modal chỉnh sửa
const openEditModal = (product?: Product) => {
  if (product) {
    editingProduct.value = { ...product };
  } else {
    resetForm();
  }
  showModal.value = true;
};

// Xóa sản phẩm
const deleteProduct = async (id: number) => {
  if (confirm('Bạn chắc chắn muốn xóa?')) {
    try {
      await axios.delete(`${API_URL}/${id}`);
      await fetchProducts();
    } catch (err) {
      console.error('Lỗi khi xóa:', err);
    }
  }
};

onMounted(() => {
  fetchProducts();
});
</script>

<template>
  <div class="container">
    <button @click="openEditModal()" class="btn btn-primary">Thêm sản phẩm</button>

    <table class="table">
      <thead>
        <tr>
          <th>Ảnh</th>
          <th>Tên</th>
          <th>Giá</th>
          <th>Hành động</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td><img :src="'/images/' + product.image" width="70" class="img-fluid" alt=""></td>
          <td>{{ product.name }}</td>
          <td>{{ product.price }}</td>
          <td>
            <button @click="openEditModal(product)" class="btn btn-sm btn-warning">Sửa</button>
            <button @click="deleteProduct(product.id!)" class="btn btn-sm btn-danger">Xóa</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Modal thêm/sửa sản phẩm -->
    <div v-if="showModal" class="modal" style="display: block; background: rgba(0,0,0,0.5)">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ editingProduct.id ? 'Sửa sản phẩm' : 'Thêm sản phẩm' }}</h5>
            <button @click="showModal = false" class="btn-close"></button>
          </div>
          <div class="modal-body">
            <div class="mb-3">
              <label class="form-label">Tên sản phẩm</label>
              <input v-model="editingProduct.name" class="form-control" required>
            </div>
            <div class="mb-3">
              <label class="form-label">Giá</label>
              <input v-model.number="editingProduct.price" type="number" class="form-control" required>
            </div>
            <div class="mb-3">
              <label class="form-label">Ảnh sản phẩm</label>
              <input type="file" @change="handleImageUpload" class="form-control" accept="image/*">
              <img v-if="editingProduct.image" :src="editingProduct.image" width="100" class="mt-2">
            </div>
          </div>
          <div class="modal-footer">
            <button @click="showModal = false" class="btn btn-secondary">Đóng</button>
            <button @click="saveProduct" class="btn btn-primary">Lưu</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.img-thumbnail {
  max-height: 70px;
  object-fit: cover;
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1050;
}
</style>