<template>
    <div>
      <main class="container mx-auto p-4">
        <section>
          <h1 class="text-4xl font-bold mb-4 animate__animated animate__fadeIn">
            Data Penjualan Pulsa
          </h1>
        </section>
  
        <NuxtLink to="/tambahdata" class="bg-yellow-500 text-white py-2 px-4 rounded-full text-lg transition duration-300 hover:bg-blue-700">
          tambahkan data
        </NuxtLink>

        <!-- Tabel Menampilkan Data dari Supabase -->
        <section class="mt-5">
          <table class="min-w-full border border-gray-300">
            <thead>
              <tr>
                <th class="border border-gray-300 px-4 py-2">Nama</th>
                <th class="border border-gray-300 px-4 py-2">Kategori</th>
                <th class="border border-gray-300 px-4 py-2">Tanggal</th>
                <th class="border border-gray-300 px-4 py-2">Waktu</th>
                <th class="border border-gray-300 px-4 py-2">Harga Jual</th>
                <th class="border border-gray-300 px-4 py-2">Harga Beli</th>
                <th class="border border-gray-300 px-4 py-2">Keuntungan</th>
                <th class="border border-gray-300 px-4 py-2">Status</th>
                <th class="border border-gray-300 px-4 py-2">Aksi</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in dataPenjualan" :key="item.id">
                <td class="border border-gray-300 px-4 py-2">{{ item.nama }}</td>
                <td class="border border-gray-300 px-4 py-2">{{ item.kategori }}</td>
                <td class="border border-gray-300 px-4 py-2">{{ item.tanggal }}</td>
                <td class="border border-gray-300 px-4 py-2">{{ item.waktu }}</td>
                <td class="border border-gray-300 px-4 py-2">{{ item.harga_jual }}</td>
                <td class="border border-gray-300 px-4 py-2">{{ item.harga_beli }}</td>
                <td class="border border-gray-300 px-4 py-2">{{ calculateProfit(item.harga_jual, item.harga_beli) }}</td>
                <td class="border border-gray-300 px-4 py-2">
                  <select v-model="item.status" @change="updateStatus(item)">
                    <option value="belum lunas">Belum Lunas</option>
                    <option value="lunas">Lunas</option>
                  </select>
                </td>
                <td class="border border-gray-300 px-4 py-2">
                  <button @click="hapusData(item.id)">Hapus</button>
                </td>
              </tr>
            </tbody>
          </table>
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
      async hapusData(id) {
        try {
          const { data, error } = await supabase.from('penjualan').delete().eq('id', id);
          if (error) {
            throw error;
          }
          console.log('Data deleted successfully:', data);
          // Refresh data setelah penghapusan
          this.fetchData();
        } catch (error) {
          console.error('Error deleting data:', error);
        }
      },
    },
    mounted() {
      this.fetchData();
    },
  };
  </script>
  
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
  </style>
  