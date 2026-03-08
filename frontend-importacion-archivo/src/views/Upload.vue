<template>
  <div class="upload-container">
    <div class="upload-card">
      <h2 class="upload-title">Subir Archivo</h2>
      <p class="upload-subtitle">Arrastra y suelta o haz clic para seleccionar</p>

      <div class="dropzone" :class="{
        'dropzone-active': isDragOver,
        'dropzone-error': errorMessage,
        'dropzone-success': successMessage
      }" @dragover.prevent="isDragOver = true" @dragleave.prevent="isDragOver = false" @drop.prevent="handleDrop">
       <input
    ref="fileInput"
    type="file"
    class="file-input"
    @change="handleFileUpload"
  />

        <div class="dropzone" :class="{
          'dropzone-active': isDragOver,
          'dropzone-error': errorMessage,
          'dropzone-success': successMessage
        }" @dragover.prevent="isDragOver = true" @dragleave.prevent="isDragOver = false" @drop.prevent="handleDrop"
          @click="triggerFileInput">
          <i class="fas fa-cloud-upload-alt upload-icon"></i>

          <p v-if="!selectedFile" class="dropzone-text">
            <span class="highlight">Haz clic aquí</span> o arrastra tu archivo
          </p>

          <p v-else class="file-name">
            {{ selectedFile.name }}
            <span class="file-size">({{ formatFileSize(selectedFile.size) }})</span>
          </p>

          <button v-if="selectedFile && !isUploading" class="btn btn-primary mt-3" @click="uploadFile">
            <i class="fas fa-upload me-2"></i>Subir ahora
          </button>

          <div v-if="isUploading" class="mt-3">
            <div class="spinner-border text-primary" role="status">
              <span class="visually-hidden">Subiendo...</span>
            </div>
            <p class="uploading-text">Subiendo archivo...</p>
          </div>
        </div>
      </div>

      <!-- Mensajes de feedback -->
      <div v-if="successMessage" class="alert alert-success mt-3">
        {{ successMessage }}
      </div>
      <div v-if="errorMessage" class="alert alert-danger mt-3">
        {{ errorMessage }}
      </div>
    </div>
  </div>
</template>

<script>
import { fileService } from '../services/fileService';

export default {
  name: 'UploadView',
  data() {
    return {
      selectedFile: null,
      isDragOver: false,
      isUploading: false,
      successMessage: '',
      errorMessage: ''
    };
  },
  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        this.selectedFile = file;
        this.successMessage = '';
        this.errorMessage = '';
      }
    },

    handleDrop(event) {
      this.isDragOver = false;
      const file = event.dataTransfer.files[0];
      if (file) {
        this.selectedFile = file;
        this.successMessage = '';
        this.errorMessage = '';
      }
    },

    async uploadFile() {
      if (!this.selectedFile) return;

      this.isUploading = true;
      this.successMessage = '';
      this.errorMessage = '';

      try {

        const formData = new FormData();
        formData.append('file', this.selectedFile);

        await fileService.subir(this.selectedFile);

        this.successMessage = '¡Archivo subido exitosamente! ✅';
        this.selectedFile = null;
        this.$router.push('/list');
      } catch (error) {
        console.error('Error al subir:', error);
        this.errorMessage = error.message || 'Error al subir el archivo. Intenta nuevamente.';
      } finally {
        this.isUploading = false;
      }
    },

    formatFileSize(bytes) {
      if (bytes === 0) return '0 Bytes';
      const k = 1024;
      const sizes = ['Bytes', 'KB', 'MB', 'GB'];
      const i = Math.floor(Math.log(bytes) / Math.log(k));
      return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
    },

    triggerFileInput() {
      this.$refs.fileInput.click();
    }
  }
};
</script>

<style scoped>
.upload-container {
  max-width: 700px;
  margin: 5rem auto;
  padding: 0 1rem;
}

.upload-card {
  background: white;
  border-radius: 1rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  padding: 2.5rem 2rem;
  border: 1px solid #e5e7eb;
}

.upload-title {
  color: #0b2545;
  font-size: 2.25rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  text-align: center;
  letter-spacing: -0.025em;
}

.upload-subtitle {

  text-align: center;
  margin-bottom: 2rem;
  font-size: 1.05rem;
}

.dropzone {
  border: 2px dashed #cbd5e1;
  border-radius: 1rem;
  padding: 3rem 1.5rem;
  text-align: center;
  transition: all 0.3s ease;
  background: #f8fafc;
  position: relative;
  cursor: pointer;
}

.dropzone:hover,
.dropzone-active {
  border-color: #3b82f6;
  background: #eff6ff;
}

.dropzone-error {
  border-color: #ef4444;
  background: #fef2f2;
}

.dropzone-success {
  border-color: #10b981;
  background: #f0fdf4;
}

.file-input {
  display: none;
}

.upload-icon {
  font-size: 3.5rem;
  color: #94a3b8;
  margin-bottom: 1rem;
}

.dropzone-text {
  color: #475569;
  margin: 0;
  font-size: 1.1rem;
}

.highlight {
  color: #2563eb;
  font-weight: 600;
}

.file-name {
  font-weight: 500;
  color: #1e293b;
  margin: 0.5rem 0;
  word-break: break-all;
}

.file-size {
  color: #64748b;
  font-size: 0.95rem;
}

.btn-primary {
  background: #2563eb;
  border: none;
  padding: 0.75rem 1.75rem;
  font-weight: 600;
  border-radius: 0.75rem;
  transition: all 0.2s;
}

.btn-primary:hover {
  background: #1d4ed8;
  transform: translateY(-1px);
}

.alert {
  border-radius: 0.75rem;
  padding: 1rem;
  font-weight: 500;
}

.uploading-text {
  color: #64748b;
  margin-top: 0.75rem;
}

.spinner-border {
  width: 2.5rem;
  height: 2.5rem;
}
</style>