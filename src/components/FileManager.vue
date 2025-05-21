<script setup>
import { ref, onMounted } from 'vue';

const files = ref([]);
const uploadedFile = ref(null);
const isUploading = ref(false);
const errorMessage = ref('');

// 模拟文件列表获取
const fetchFiles = () => {
  // 实际项目中，这里应该是从后端API获取文件列表
  // 这里使用模拟数据
  files.value = [
    
  ];
};

// 处理文件上传
const handleFileUpload = (event) => {
  const file = event.target.files[0];
  if (!file) return;
  
  uploadedFile.value = file;
  
  // 模拟上传过程
  isUploading.value = true;
  errorMessage.value = '';
  
  // 实际项目中，这里应该是调用API上传文件
  setTimeout(() => {
    isUploading.value = false;
    // 模拟添加新文件到列表
    const newFile = {
      id: files.value.length + 1,
      name: file.name,
      size: `${(file.size / (1024 * 1024)).toFixed(1)}MB`,
      date: new Date().toISOString().split('T')[0]
    };
    files.value.push(newFile);
    uploadedFile.value = null;
  }, 1500);
};

// 删除文件
const deleteFile = (fileId) => {
  // 实际项目中，这里应该是调用API删除文件
  files.value = files.value.filter(file => file.id !== fileId);
};

onMounted(() => {
  fetchFiles();
});
</script>

<template>
  <div class="file-manager">
    <h2>文件管理</h2>
    
    <!-- 文件上传区域 -->
    <div class="upload-area">
      <input 
        type="file" 
        id="file-upload" 
        @change="handleFileUpload" 
        :disabled="isUploading"
        class="file-input"
      />
      <label for="file-upload" class="upload-button">
        <span v-if="!isUploading">选择文件上传</span>
        <span v-else>上传中...</span>
      </label>
      <div v-if="uploadedFile" class="upload-info">
        已选择: {{ uploadedFile.name }}
      </div>
      <div v-if="errorMessage" class="error-message">
        {{ errorMessage }}
      </div>
    </div>
    
    <!-- 文件列表 -->
    <div class="file-list">
      <h3>已上传文件</h3>
      <table v-if="files.length > 0">
        <thead>
          <tr>
            <th>文件名</th>
            <th>大小</th>
            <th>上传日期</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="file in files" :key="file.id">
            <td>{{ file.name }}</td>
            <td>{{ file.size }}</td>
            <td>{{ file.date }}</td>
            <td>
              <button @click="deleteFile(file.id)" class="delete-button">删除</button>
            </td>
          </tr>
        </tbody>
      </table>
      <div v-else class="no-files">
        暂无文件，请上传文件
      </div>
    </div>
  </div>
</template>

<style scoped>
.file-manager {
  width: 100%;
  height: 100%;
  padding: 20px;
  background-color: #f5f5f5;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #333;
  margin-bottom: 20px;
  text-align: center;
}

.upload-area {
  margin-bottom: 20px;
  padding: 20px;
  border: 2px dashed #ccc;
  border-radius: 8px;
  text-align: center;
  background-color: #fafafa;
}

.file-input {
  display: none;
}

.upload-button {
  display: inline-block;
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

.upload-button:hover {
  background-color: #45a049;
}

.upload-info {
  margin-top: 10px;
  font-size: 0.9em;
  color: #666;
}

.error-message {
  margin-top: 10px;
  color: #f44336;
  font-size: 0.9em;
}

.file-list {
  background-color: white;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

h3 {
  color: #555;
  margin-bottom: 15px;
  font-size: 1.2em;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 12px 15px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #f2f2f2;
  font-weight: bold;
  color: #333;
}

tr:hover {
  background-color: #f5f5f5;
}

.delete-button {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8em;
  transition: background-color 0.3s;
}

.delete-button:hover {
  background-color: #d32f2f;
}

.no-files {
  text-align: center;
  padding: 20px;
  color: #999;
  font-style: italic;
}
</style>