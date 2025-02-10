<script setup>
useHead({
    title: "Riwayat",
    meta: [
        {
            name: "description",
            content: "Riwayat",
        },
    ],
});
const supabase = useSupabaseClient();
const visitors = ref([]);

const getRiwayat = async () => {
    const { data } = await supabase.from("absen").select("*").order("id", { ascending: false });
    if (data) visitors.value = data;
};
onMounted(() => {
    getRiwayat();

});
</script>
<template>
    <div class="container">
        <nuxt-link to="/" class="text-decoration-none">
            <div class="d-flex justify-content-start align-items-center mb-4">
                <button class="btn btn-outline-primary d-flex align-items-center gap-2">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        viewBox="0 0 16 16">
                        <path fill-rule="evenodd"
                            d="M11.354 1.646a.5.5 0 0 1 0 .708L5.707 8l5.647 5.646a.5.5 0 0 1-.708.708l-6-6a.5.5 0 0 1 0-.708l6-6a.5.5 0 0 1 .708 0z" />
                    </svg>
                    Kembali
                </button>
            </div>
        </nuxt-link>
        <div class="card shadow-sm">
            <div class="card-header bg-white d-flex justify-content-between align-items-center py-3">
                <h5 class="mb-0">Riwayat Absensi</h5>
                <button class="btn btn-success d-flex align-items-center gap-2" @click="exportToExcel">
                    <i class="fas fa-file-excel"></i>
                    Export Excel
                </button>
            </div>
            <div class="card-body p-0">
                <div class="table-responsive">
                    <table class="table table-hover mb-0">
                        <thead class="table-light">
                            <tr>
                                <th scope="col" class="py-3">No</th>
                                <th scope="col" class="py-3">Tanggal</th>
                                <th scope="col" class="py-3">Nama Siswa</th>
                                <th scope="col" class="py-3">Sekolah</th>
                                <th scope="col" class="py-3">Jurusan</th>
                                <th scope="col" class="py-3">Keterangan</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(visitor, i) in visitors" :key="i">
                                <th scope="row">{{ i + 1 }}.</th>
                                <td>{{ visitor.tgl }}</td>
                                <td>{{ visitor.nama }}</td>
                                <td>{{ visitor.sekolah }}</td>
                                <td>{{ visitor.jurusan }}</td>
                                <td>{{ visitor.ket }}</td>

                            </tr>

                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>



<style scoped>
.container {
    max-width: 1200px;
    margin-top: 10%;
}

.card {
    border-radius: 10px;
    border: none;
}

.card-header {
    border-bottom: 1px solid #eee;
}

.table {
    margin-bottom: 0;
}

.table th {
    font-weight: 600;
    color: #444;
}

.table td,
.table th {
    padding: 1rem;
    vertical-align: middle;
}

.badge {
    padding: 0.5em 0.8em;
    font-weight: 500;
}

.btn-success {
    padding: 0.5rem 1rem;
    font-weight: 500;
}

.btn-outline-primary {
    padding: 0.5rem 1rem;
    font-weight: 500;
    transition: all 0.3s ease;
}

.btn-outline-primary:hover {
    transform: translateX(-5px);
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .container {
        padding: 10px;
    }

    .table td,
    .table th {
        padding: 0.75rem;
    }

    .card-header {
        flex-direction: column;
        gap: 1rem;
    }

    .btn-success {
        width: 100%;
        justify-content: center;
    }

    .btn-outline-primary {
        font-size: 0.9rem;
    }
}
</style>