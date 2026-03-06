<template>
  <div class="d-flex flex-column justify-content-center align-items-center vh-100 ">
    <div class="card shadow-lg p-5 text-center home-card">
      <h1 class="mb-4 file-title">
        <i class="fas fa-file-import bounce-icon"></i> Gestión de Archivos
      </h1>

      <p class="text-muted mb-4">
        Administra tus archivos de forma rápida y segura.
      </p>
      <div class="d-flex justify-content-center gap-3 flex-wrap">
        <button class="btn custom-btn-upload btn-lg" @click="$router.push('/upload')">
          <i class="fas fa-upload me-2"></i> Subir Archivo
        </button>

        <button class="btn custom-btn-upload btn-lg" @click="$router.push('/list')">
          <i class="fas fa-list me-2"></i> Listado de Archivos
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView'
}
</script>

<style scoped>
.file-title {
  color: #0b2545;
  font-weight: 700;
  font-size: 2.5rem;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
  align-items: center;

}

.file-title i {
  color: #0b2545;
}

.home-card {
  border-radius: 1rem;
  max-width: 500px;
}

.custom-btn-upload {
  background-color: #0b2545;
  color: #ffffff;
  border: none;
  transition: background-color 0.3s;
}

.custom-btn-upload:hover {
color: #ffffff;
  background-color: #071b33;
}

.btn-lg {
  font-size: 1.25rem;
  padding: 0.75rem 1.5rem;
}
</style>