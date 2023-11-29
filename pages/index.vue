<template>
  <div>
    <main class="container mx-auto p-4">
      <section class="text-center">
        <h1 class="text-4xl font-bold mb-4 animate__animated animate__fadeInDown text-blue-500">
          Selamat Datang di Aplikasi Data Penjualan
        </h1>
        <p class="text-lg mb-8 animate__animated animate__fadeInUp text-yellow-500">
          Kami memberikan kemudahan dalam mencatat dan mengelola penjualan 
        </p>
        <NuxtLink to="/tambahdata" class="bg-blue-500 text-white py-2 px-4 rounded-full text-lg transition duration-300 hover:bg-blue-700 animate__animated animate__fadeIn">
          tambahkan data
        </NuxtLink>
        <br>
      </section>

   <!-- Kolom untuk Menampilkan Seluruh Harga Jual, Harga Beli, dan Keuntungan -->
   <div class="mt-10">
        <h2 class="text-3xl font-bold mb-4 animate__animated animate__fadeIn text-blue-500">Statistika</h2>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp">
            <h3 class="text-xl font-bold mb-2">pendapatan</h3>
            <p class="text-gray-600">Total: {{ totalHargaJual }}</p>
          </div>
          <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp">
            <h3 class="text-xl font-bold mb-2">pengeluaran</h3>
            <p class="text-gray-600">Total: {{ totalHargaBeli }}</p>
          </div>
          <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp">
            <h3 class="text-xl font-bold mb-2">Keuntungan</h3>
            <p class="text-gray-600">Total: {{ totalKeuntungan }}</p>
          </div>
        </div>
      </div>



      <!-- Menampilkan 5 Data Terbaru -->
      <div class="mt-10">
  <h2 class="text-3xl font-bold mb-4 animate__animated animate__fadeIn text-blue-500 text-center">Terbaru</h2>
  <div v-if="dataPenjualan && dataPenjualan.length > 0" class="flex flex-wrap -mx-4">
    <div v-for="(item, index) in dataPenjualan.slice(0, 8)" :key="index" class="w-full md:w-1/2 lg:w-1/3 xl:w-1/4 px-4 mb-8">
      <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp">
        <h3 class="text-xl font-bold mb-2">{{ item.nama }}</h3>
        <p class="text-gray-600">Kategori: {{ item.kategori }}</p>
        <p class="text-gray-600">Harga: {{ item.harga_jual }}</p>
      </div>
    </div>
  </div>
  
</div>
      
   
    </main>
    <Footer />
  </div>
</template>

<script>
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = 'https://urwxmwdxgiswqqgpgtko.supabase.co'; 
const supabaseKey = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InVyd3htd2R4Z2lzd3FxZ3BndGtvIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MDEyMjg3NTAsImV4cCI6MjAxNjgwNDc1MH0.7FiBl-rNIqtuLZQ5tvI9XewdQy5VGP200uPecPfHalk';

const supabase = createClient(supabaseUrl, supabaseKey);

export default {
  data() {
    return {
      dataPenjualan: [],
      totalPendapatanMingguIni: 0,
      totalPengeluaranMingguIni: 0,
      totalKeuntunganMingguIni: 0,
      totalHargaJual: 0,
      totalHargaBeli: 0,
      totalKeuntungan: 0,
    };
  },
  methods: {
    async fetchData() {
      try {
        const { data, error } = await supabase.from('penjualan').select('*');
        if (error) {
          throw error;
        }
        this.dataPenjualan = data;
        this.calculateStatistikMingguIni();
        this.calculateTotalHargaKeuntungan();
      } catch (error) {
        console.error('Error fetching data from Supabase:', error);
      }
    },
    calculateProfit(hargaJual, hargaBeli) {
      return hargaJual - hargaBeli;
    },
    calculateStatistikMingguIni() {
      // Hitung total pendapatan, pengeluaran, dan keuntungan minggu ini
      const oneWeekAgo = new Date();
      oneWeekAgo.setDate(oneWeekAgo.getDate() - 7);

      this.totalPendapatanMingguIni = this.dataPenjualan
        .filter(item => new Date(item.tanggal) >= oneWeekAgo)
        .reduce((total, item) => total + item.total_pendapatan, 0);

      this.totalPengeluaranMingguIni = this.dataPenjualan
        .filter(item => new Date(item.tanggal) >= oneWeekAgo)
        .reduce((total, item) => total + item.total_pengeluaran, 0);

      this.totalKeuntunganMingguIni = this.dataPenjualan
        .filter(item => new Date(item.tanggal) >= oneWeekAgo)
        .reduce((total, item) => total + this.calculateProfit(item.harga_jual, item.harga_beli), 0);
    },
    calculateTotalHargaKeuntungan() {
      this.totalHargaJual = this.dataPenjualan.reduce((total, item) => total + item.harga_jual, 0);
      this.totalHargaBeli = this.dataPenjualan.reduce((total, item) => total + item.harga_beli, 0);
      this.totalKeuntungan = this.totalHargaJual - this.totalHargaBeli;
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>

<style scoped>

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.animate__animated {
  animation-duration: 1s;
  animation-fill-mode: both;
}

.animate__fadeIn {
  animation-name: fadeIn;
}

.animate__fadeInUp {
  animation-name: fadeInUp;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
