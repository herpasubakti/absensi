<script setup>
useHead({
    title: "Form",
    meta: [
        {
            name: "description",
            content: "Form ",
        },
    ],
});

const supabase = useSupabaseClient();
const router = useRouter();

// Initialize form data
const form = ref({
    nama: '',
    sekolah: '',
    jurusan: '',
    bidang: '',
    ket: '',
    tgl: new Date().toISOString().split('T')[0] // Add current date
});

const formErrors = ref({
    nama: false,
    sekolah: false,
    jurusan: false,
    bidang: false,
    ket: false
});

const kirimData = async () => {
    try {
        // Reset errors
        Object.keys(formErrors.value).forEach(key => formErrors.value[key] = false);

        // Check for empty fields
        let hasError = false;
        Object.keys(form.value).forEach(key => {
            if (!form.value[key] && key !== 'tgl') { // Skip date validation
                formErrors.value[key] = true;
                hasError = true;
            }
        });

        if (hasError) {
            return; // Stop if there are errors
        }

        // Submit if all fields are filled
        const { error } = await supabase
            .from('absen')
            .insert([form.value]);

        if (error) throw error;

        // Reset form after successful submission
        Object.keys(form.value).forEach(key => {
            if (key !== 'tgl') form.value[key] = '';
        });

        // Navigate to riwayat page
        await navigateTo('/riwayat');

    } catch (error) {
        console.error('Error submitting form:', error);
        alert('Terjadi kesalahan saat mengirim data');
    }
};
onMounted(async () => {
    try {
        const { data, error } = await supabase.from('absen').select('*').limit(1);
        if (error) throw error;
        console.log('Supabase connection successful');
    } catch (err) {
        console.error('Supabase connection error:', err);
    }
});
</script>

<template>
    <div class="container">
        <div class="d-flex justify-content-end mb-3">
            <nuxt-link to="/riwayat" class="text-decoration-none">
                <button class="btn btn-outline-primary d-flex align-items-center gap-2">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        viewBox="0 0 16 16">
                        <path d="M8 3.5a.5.5 0 0 0-1 0V9a.5.5 0 0 0 .252.434l3.5 2a.5.5 0 0 0 .496-.868L8 8.71V3.5z" />
                        <path d="M8 16A8 8 0 1 0 8 0a8 8 0 0 0 0 16zm7-8A7 7 0 1 1 1 8a7 7 0 0 1 14 0z" />
                    </svg>
                    Riwayat Absensi
                </button>
            </nuxt-link>
        </div>
        <div class="card shadow-lg">
            <div class="card-header text-white" style="background-color: #18a5d0;">
                <h4 class="mb-0 text-center">Form Absensi</h4>
            </div>
            <div class="card-body">
                <form @submit.prevent="kirimData">
                    <div class="mb-4">
                        <label class="form-label">Nama Siswa</label>
                        <select v-model="form.nama" class="form-select">
                            <option value="" selected disabled>Pilih nama siswa</option>
                            <option value="Herpa Ardi Subakti">Herpa Ardi Subakti</option>
                            <option value="Fauzan Firdaus">Fauzan Firdaus</option>
                            <!-- <option value="3">Three</option> -->
                        </select>
                        <div class="invalid-feedback" v-if="formErrors.nama">
                            Nama siswa wajib diisi
                        </div>
                    </div>

                    <div class="mb-4">
                        <label class="form-label">Sekolah</label>
                        <select v-model="form.sekolah" class="form-select">
                            <option value="" selected disabled>Pilih Sekolah</option>
                            <option value="SMKN 4 Tasikmalaya">SMKN 4 Tasikmalaya</option>
                            <!-- <option value="2">Two</option> -->
                            <!-- <option value="3">Three</option> -->
                        </select>
                        <div class="invalid-feedback" v-if="formErrors.sekolah">
                            Sekolah wajib diisi
                        </div>
                    </div>

                    <div class="mb-4">
                        <label class="form-label">Jurusan</label>
                        <select v-model="form.jurusan" class="form-select">
                            <option value="" selected disabled>Pilih jurusan</option>
                            <option value="PPLG">PPLG</option>
                            <!-- <option value="2">Two</option> -->
                            <!-- <option value="3">Three</option> -->
                        </select>
                        <div class="invalid-feedback" v-if="formErrors.jurusan">
                            Jurusan wajib diisi
                        </div>
                    </div>
                    <div class="mb-4">
                        <label class="form-label">Bidang</label>
                        <select v-model="form.bidang" class="form-select">
                            <option value="" selected disabled>Pilih Bidang</option>
                            <option value="Permukiman">Permukiman</option>
                            <!-- <option value="2">Two</option> -->
                            <!-- <option value="3">Three</option> -->
                        </select>
                        <div class="invalid-feedback" v-if="formErrors.bidang">
                            Bidang wajib diisi
                        </div>
                    </div>

                    <div class="mb-4">
                        <label class="form-label">Keterangan</label>
                        <select v-model="form.ket" class="form-select">
                            <option value="" selected disabled>Pilih Keterangan</option>
                            <option value="Hadir">Hadir</option>
                            <option value="Sakit">Sakit</option>
                            <option value="Izin">Izin</option>
                            <option value="Alpha">Alpha</option>
                        </select>
                        <div class="invalid-feedback" v-if="formErrors.ket">
                            Keterangan wajib diisi
                        </div>
                    </div>

                    <button type="submit" class="btn btn-primary w-100">Submit</button>
                </form>
            </div>
        </div>
    </div>
</template>

<style scoped>
.container {
    margin-top: 80px;
    padding: 20px;
    max-width: 100%;
}

.card {
    max-width: 500px;
    width: 95%;
    margin: 0 auto;
    border-radius: 15px;
    border: none;
    transition: all 0.3s ease;
}

/* Responsive adjustments */
@media (max-width: 576px) {
    .container {
        padding: 10px;
        margin-top: 70px;
    }

    .card {
        width: 100%;
    }

    .card-body {
        padding: 1.5rem;
    }

    .card-header {
        padding: 1.25rem;
        border-radius: 15px;

    }

    .form-select {
        padding: 0.6rem;
    }
}

.form-select:focus {
    border-color: #86b7fe;
    box-shadow: 0 0 0 0.25rem rgba(13, 110, 253, 0.25);
}

.form-label {
    font-weight: 500;
    margin-bottom: 0.5rem;
}

.btn-primary {
    padding: 0.75rem;
    font-weight: 500;
    border-radius: 8px;
    transition: all 0.3s ease;
    background-color: #18a5d0 !important;
    border: none;
    position: relative;
    overflow: hidden;
}

.btn-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(24, 165, 208, 0.3);
    background-color: #1590b6 !important;
}

.btn-primary:active {
    transform: translateY(0);
    box-shadow: 0 2px 8px rgba(24, 165, 208, 0.3);
}

.btn-primary:focus {
    box-shadow: 0 0 0 3px rgba(24, 165, 208, 0.25);
}
</style>