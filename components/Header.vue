<template>
    <nav class="navbar bg-body-tertiary fixed-top">
        <div class="container-fluid d-flex justify-content-center align-items-center">
            <a class="navbar-brand d-flex align-items-center" href="#">
                <img src="assets/image.webp" alt="Logo" width="50" height="50" class="d-inline-block me-2">
                <h4 class="mb-0 header-title">Absensi Siswa PKL PUPR</h4>
            </a>
        </div>
    </nav>
</template>

<style scoped>
.navbar {
    background-color: #18a5d0 !important;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    z-index: 1000;
}

.header-title {
    color: white;
    font-weight: 600;
    letter-spacing: 0.5px;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
    font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
    transition: all 0.3s ease;
}

.header-title:hover {
    transform: scale(1.02);
}
</style>