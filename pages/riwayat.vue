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
import * as XLSX from 'xlsx';

const exportToExcel = () => {
    // Prepare the data for export
    const exportData = visitors.value.map((visitor, index) => ({
        'No': index + 1,
        'Tanggal': visitor.tgl,
        'Nama Siswa': visitor.nama,
        'Sekolah': visitor.sekolah,
        'Jurusan': visitor.jurusan,
        'Bidang': visitor.bidang,
        'Keterangan': visitor.ket
    }));

    // Create worksheet
    const ws = XLSX.utils.json_to_sheet(exportData);

    // Create workbook
    const wb = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(wb, ws, 'Absensi');

    // Generate file name with current date
    const fileName = `Absensi_${new Date().toISOString().split('T')[0]}.xlsx`;

    // Export to Excel
    XLSX.writeFile(wb, fileName);
};

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
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        viewBox="0 0 16 16">
                        <path
                            d="M14 14V4.5L9.5 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2zM9.5 3A1.5 1.5 0 0 0 11 4.5h2V14a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1h5.5v2z" />
                        <path
                            d="M4.603 14.087a.81.81 0 0 1-.438-.42c-.195-.388-.13-.776.08-1.102.198-.307.526-.568.897-.787a7.68 7.68 0 0 1 1.482-.645 19.697 19.697 0 0 0 1.062-2.227 7.269 7.269 0 0 1-.43-1.295c-.086-.4-.119-.796-.046-1.136.075-.354.274-.672.65-.823.192-.077.4-.12.602-.077a.7.7 0 0 1 .477.365c.088.164.12.356.127.538.007.188-.012.396-.047.614-.084.51-.27 1.134-.52 1.794a10.954 10.954 0 0 0 .98 1.686 5.753 5.753 0 0 1 1.334.05c.364.066.734.195.96.465.12.144.193.32.2.518.007.192-.047.382-.138.563a1.04 1.04 0 0 1-.354.416.856.856 0 0 1-.51.138c-.331-.014-.654-.196-.933-.417a5.712 5.712 0 0 1-.911-.95 11.651 11.651 0 0 0-1.997.406 11.307 11.307 0 0 1-1.02 1.51c-.292.35-.609.656-.927.787a.793.793 0 0 1-.58.029zm1.379-1.901c-.166.076-.32.156-.459.238-.328.194-.541.383-.647.547-.094.145-.096.25-.04.361.01.022.02.036.026.044a.266.266 0 0 0 .035-.012c.137-.056.355-.235.635-.572a8.18 8.18 0 0 0 .45-.606zm1.64-1.33a12.71 12.71 0 0 1 1.01-.193 11.744 11.744 0 0 1-.51-.858 20.801 20.801 0 0 1-.5 1.05zm2.446.45c.15.163.296.3.435.41.24.19.407.253.498.256a.107.107 0 0 0 .07-.015.307.307 0 0 0 .094-.125.436.436 0 0 0 .059-.2.095.095 0 0 0-.026-.063c-.052-.062-.2-.152-.518-.209a3.876 3.876 0 0 0-.612-.053zM8.078 7.8a6.7 6.7 0 0 0 .2-.828c.031-.188.043-.343.038-.465a.613.613 0 0 0-.032-.198.517.517 0 0 0-.145.04c-.087.035-.158.106-.196.283-.04.192-.03.469.046.822.024.111.054.227.09.346z" />
                    </svg>
                    Download Semua Data
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
                                <th scope="col" class="py-3">Bidang</th>
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
                                <td>{{ visitor.bidang }}</td>
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
    transition: all 0.3s ease;
}

.btn-success:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(40, 167, 69, 0.2);
}

.btn-success:active {
    transform: translateY(0);
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