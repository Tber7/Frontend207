<!-- src/components/CategoriesView.vue -->
<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';
import type { Category } from '../stores/Category';

const categories = ref<Category[]>([]);
const editingCategory = ref<Category | null>(null);
const showModal = ref(false);
const keyword = ref('');

onMounted(async () => {
  await fetchCategories();
});

async function fetchCategories() {
  // Giả lập API, thay bằng API thực tế của bạn
  const response = await fetch('http://localhost:3000/categories');
  categories.value = await response.json();
}

const filteredCategories = computed(() => {
  return categories.value.filter(category =>
    category.name.toLowerCase().includes(keyword.value.toLowerCase())
  );
});

const openEditModal = (category?: Category) => {
  editingCategory.value = category 
    ? { ...category } 
    : { id: 0, name: '', description: '', isActive: true };
  showModal.value = true;
};

const saveCategory = async () => {
  try {
    if (editingCategory.value) {
      // Chuẩn hóa dữ liệu trước khi gửi
      const categoryData = {
        ...editingCategory.value,
        id: editingCategory.value.id || undefined, // Gửi undefined nếu là tạo mới
        isActive: Boolean(editingCategory.value.isActive)
      };

      const method = editingCategory.value.id ? 'PUT' : 'POST';
      const url = editingCategory.value.id 
        ? `http://localhost:3000/categories/${editingCategory.value.id}`
        : 'http://localhost:3000/categories';

      const response = await fetch(url, {
        method,
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(categoryData),
      });

      if (!response.ok) {
        const errorData = await response.json();
        throw new Error(errorData.message || 'Lỗi không xác định');
      }

      await fetchCategories();
      showModal.value = false;
    }
  } catch (error) {
    console.error('Lỗi khi lưu danh mục:', error);
    alert('Lỗi: ' + (error instanceof Error ? error.message : 'Có lỗi xảy ra'));
  }
};

const deleteCategory = async (id: string | number) => {  // Cho phép cả string và number
  if (confirm('Bạn có chắc muốn xóa danh mục này?')) {
    try {
      const response = await fetch(`http://localhost:3000/categories/${id}`, {
        method: 'DELETE',
      });

      if (response.ok) {
        await fetchCategories();
      } else {
        const errorData = await response.json();
        console.error('Lỗi từ server:', errorData);
        alert('Xóa không thành công: ' + (errorData.message || 'Lỗi không xác định'));
      }
    } catch (error) {
      console.error('Lỗi khi xóa:', error);
      alert('Có lỗi xảy ra khi xóa danh mục');
    }
  }
};

const toggleStatus = async (category: Category) => {
  try {
    const updatedCategory = { ...category, isActive: !category.isActive };
    const response = await fetch(`http://localhost:3000/categories/${category.id}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(updatedCategory),
    });

    if (response.ok) {
      await fetchCategories();
    }
  } catch (error) {
    console.error('Error toggling category status:', error);
  }
};
</script>

<template>
  <div class="container mt-4">
    <div class="card">
      <div class="card-header d-flex justify-content-between align-items-center">
        <h2>Category Management</h2>
        <div>
          <input v-model="keyword" type="text" class="form-control d-inline-block w-auto" placeholder="Search...">
          <button @click="openEditModal()" class="btn btn-primary ms-2">
            <i class="bi bi-plus-circle"></i> Add Category
          </button>
        </div>
      </div>
      
      <div class="card-body">
        <table class="table table-striped table-hover">
          <thead>
            <tr>
              <th>Name</th>
              <th>Description</th>
              <th>Status</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="category in filteredCategories" :key="category.id">
              <td>{{ category.name }}</td>
              <td>{{ category.description || '-' }}</td>
              <td>
                <span :class="['badge', category.isActive ? 'bg-success' : 'bg-secondary']">
                  {{ category.isActive ? 'Active' : 'Inactive' }}
                </span>
              </td>
              <td>
                <button @click="toggleStatus(category)" class="btn btn-sm btn-outline-secondary me-1">
                  <i :class="['bi', category.isActive ? 'bi-eye-slash' : 'bi-eye']"></i>
                </button>
                <button @click="openEditModal(category)" class="btn btn-sm btn-outline-warning me-1">
                  <i class="bi bi-pencil-square"></i>
                </button>
                <button @click="deleteCategory(category.id)" class="btn btn-sm btn-outline-danger">
                  <i class="bi bi-trash"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Category Modal -->
    <div v-if="showModal" class="modal fade show" style="display: block; background-color: rgba(0,0,0,0.5)">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ editingCategory?.id ? 'Edit Category' : 'Add New Category' }}</h5>
            <button type="button" class="btn-close" @click="showModal = false"></button>
          </div>
          <div class="modal-body">
            <form v-if="editingCategory">
              <div class="mb-3">
                <label class="form-label">Name</label>
                <input v-model="editingCategory.name" type="text" class="form-control" required>
              </div>
              <div class="mb-3">
                <label class="form-label">Description</label>
                <textarea v-model="editingCategory.description" class="form-control" rows="3"></textarea>
              </div>
              
              <div class="mb-3 form-check" v-if="editingCategory.id">
                <input v-model="editingCategory.isActive" type="checkbox" class="form-check-input">
                <label class="form-check-label">Active</label>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="showModal = false">Cancel</button>
            <button type="button" class="btn btn-primary" @click="saveCategory">Save</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1050;
}

.modal-dialog {
  margin: 1.75rem auto;
}

.table th, .table td {
  vertical-align: middle;
}

.btn-sm {
  padding: 0.25rem 0.5rem;
  font-size: 0.875rem;
}
</style>