<template>
  <div class="container mt-4">
    <h2 class="mb-3 text-center text-primary">Subir Archivo</h2>

    <!-- Formulario de subida -->
    <div class="mb-3 d-flex align-items-center gap-2">
      <input type="file" class="form-control" @change="handleFileUpload" />
      <button class="btn btn-primary" @click="uploadFile" :disabled="!selectedFile">
        <i class="fas fa-upload me-1"></i> Subir Archivo
      </button>
      <span v-if="selectedFile" class="text-truncate">
        {{ selectedFile.name }}
      </span>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'UploadView',
  data() {
    return {
      selectedFile: null
    }
  },
  methods: {
    handleFileUpload(event) {
      this.selectedFile = event.target.files[0]
    },
    async uploadFile() {
      if (!this.selectedFile) {
        alert('Selecciona un archivo primero')
        return
      }

      try {
        const formData = new FormData()
        formData.append('file', this.selectedFile)

        await axios.post('http://localhost:9090/files/upload', formData)

        // Limpiar el input
        this.selectedFile = null

        alert('Archivo subido con éxito ✅')
      } catch (error) {
        console.error('Error al subir archivo:', error)
        alert('Error al subir archivo ❌')
      }
    }
  }
}
</script>

<style scoped>
.mb-3 {
  margin-bottom: 1rem;
}

.gap-2 {
  gap: 0.5rem;
}

.form-control {
  border-radius: 0.5rem;
}

.btn-primary {
  display: flex;
  align-items: center;
}

.text-truncate {
  max-width: 200px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>