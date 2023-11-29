<template>
  <div class="bg relative overflow-x-auto shadow-md sm:rounded-lg">
    <main class="container mx-auto p-4">
      <section>
        <h1 class="text-4xl font-bold mb-4 animate__animated animate__fadeIn">
          Data Penjualan
        </h1>
      </section>

      <NuxtLink to="/tambahdata" class="bg-yellow-500 text-white py-2 px-4 rounded-full text-lg transition duration-300 hover:bg-blue-700">
        Tambahkan Data
      </NuxtLink>

      <!-- Filter Kategori -->
      <section class="mt-5 relative overflow-x-auto">
        <label for="kategoriFilter" class="text-lg font-bold mb-2">Filter : </label>
        <select class=" rounded-full text-lg transition duration-300 bg-blue-400 text-lg font-bold mb-2" id="kategoriFilter" v-model="kategoriFilter" @change="filterDataByCategory">
          <option class="bg-blue-400" value="">Semua Kategori</option>
          <option v-for="kategori in kategoriOptions" :key="kategori" :value="kategori">{{ kategori }}</option>
        </select>
      </section>

      <!-- Tabel Menampilkan Data dari Supabase -->
      <section class="mt-5 relative overflow-x-auto">
        <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-400">
          <thead class="text-xs text-white-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-white-400">
            <tr>
              <th class="border border-gray-300 px-4 py-2">Nama</th>
              <th class="border border-gray-300 px-4 py-2">Kategori</th>
              <th class="border border-gray-300 px-4 py-2">Tanggal</th>
              <th class="border border-gray-300 px-4 py-2">Harga Jual</th>
              <th class="border border-gray-300 px-4 py-2">Harga Beli</th>
              <th class="border border-gray-300 px-4 py-2">Keuntungan</th>
             
              <th class="border border-gray-300 px-4 py-2">Aksi</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in filteredData" :key="item.id">
              <td class="border border-gray-300 px-4 py-2">{{ item.nama }}</td>
              <td class="border border-gray-300 px-4 py-2">{{ item.kategori }}</td>
              <td class="border border-gray-300 px-4 py-2">{{ item.tanggal }}</td>
              
              <td class="border border-gray-300 px-4 py-2">{{ item.harga_jual }}</td>
              <td class="border border-gray-300 px-4 py-2">{{ item.harga_beli }}</td>
              <td class="border border-gray-300 px-4 py-2">{{ calculateProfit(item.harga_jual, item.harga_beli) }}</td>
             
              <td class="border border-gray-300 px-4 py-2">
                <button @click="hapusData(item.id)">Hapus</button> 
             
              </td>
            </tr>
          </tbody>
        </table>
      </section>

      <!-- Total Harga Jual, Harga Beli, dan Keuntungan -->
      <section class="mt-5">
        <h2 class="text-3xl font-bold mb-4 animate__animated animate__fadeIn text-blue-500">statistik</h2>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp">
            <h3 class="text-xl font-bold mb-2">Pendapatan</h3>
            <p class="text-gray-600">Total: {{ totalHargaJual }}</p>
          </div>
          <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp">
            <h3 class="text-xl font-bold mb-2">Pengeluaran</h3>
            <p class="text-gray-600">Total: {{ totalHargaBeli }}</p>
          </div>
          <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp">
            <h3 class="text-xl font-bold mb-2">Keuntungan</h3>
            <p class="text-gray-600">Total: {{ totalKeuntungan }}</p>
          </div>
        </div>
      </section>
    </main>
    <Footer />
  </div>
</template>

<script>
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = 'https://urwxmwdxgiswqqgpgtko.supabase.co'; // Ganti dengan URL Supabase Anda
const supabaseKey = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InVyd3htd2R4Z2lzd3FxZ3BndGtvIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MDEyMjg3NTAsImV4cCI6MjAxNjgwNDc1MH0.7FiBl-rNIqtuLZQ5tvI9XewdQy5VGP200uPecPfHalk'; // Ganti dengan API Key Supabase Anda

const supabase = createClient(supabaseUrl, supabaseKey);

export default {
  data() {
    return {
      dataPenjualan: [],
      kategoriFilter: '',
    };
  },
  computed: {
    // Filtered data based on selected category
    filteredData() {
      if (!this.kategoriFilter) {
        return this.dataPenjualan;
      }
      return this.dataPenjualan.filter(item => item.kategori === this.kategoriFilter);
    },
    // List of unique categories for filter options
    kategoriOptions() {
      const categories = new Set(this.dataPenjualan.map(item => item.kategori));
      return [].concat(Array.from(categories));
    },
    // Total Harga Jual for the filtered data
    totalHargaJual() {
      return this.filteredData.reduce((total, item) => total + item.harga_jual, 0);
    },
    // Total Harga Beli for the filtered data
    totalHargaBeli() {
      return this.filteredData.reduce((total, item) => total + item.harga_beli, 0);
    },
    // Total Keuntungan for the filtered data
    totalKeuntungan() {
      return this.filteredData.reduce((total, item) => total + this.calculateProfit(item.harga_jual, item.harga_beli), 0);
    },
  },
  methods: {
    async fetchData() {
      try {
        const { data, error } = await supabase.from('penjualan').select('*');
        if (error) {
          throw error;
        }
        this.dataPenjualan = data;
      } catch (error) {
        console.error('Error fetching data from Supabase:', error);
      }
    },




    calculateProfit(hargaJual, hargaBeli) {
      return hargaJual - hargaBeli;
    },
    async updateStatus(item) {
      try {
        const { data, error } = await supabase
          .from('penjualan')
          .update({ status: item.status })
          .eq('id', item.id);
        if (error) {
          throw error;
        }
        console.log('Status updated successfully:', data);
      } catch (error) {
        console.error('Error updating status:', error);
      }
    },

  

    async confirmDelete(id) {
      const isConfirmed = window.confirm('Apakah Anda yakin ingin menghapus data ini?');

      if (isConfirmed) {
        await this.hapusData(id);
      }
    },
    async hapusData(id) {
      try {
        const isConfirmed = window.confirm('Apakah Anda yakin ingin menghapus data ini?');

        if (isConfirmed) {
          const { data, error } = await supabase.from('penjualan').delete().eq('id', id);
          if (error) {
            throw error;
          }
          console.log('Data deleted successfully:', data);
          // Refresh data setelah penghapusan
          this.fetchData();
        }
      } catch (error) {
        console.error('Error deleting data:', error);
      }
    },
    
    // Filter data by selected category
    filterDataByCategory() {
      this.fetchData(); // Re-fetch all data
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>


<style scoped>
/* Gaya khusus untuk halaman ini

  
  <style scoped>
  /* Gaya khusus untuk halaman ini */
  /* Animasi menggunakan library animate.css */
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

.bg{
  
  background-color:aqua;
}

  </style>
  