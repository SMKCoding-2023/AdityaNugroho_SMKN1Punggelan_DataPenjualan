<template>
  <div>
    <main class="container mx-auto p-4">
      <section>
        <h1 class="text-3xl font-bold mb-4">Form Penjualan Pulsa</h1>
        <form @submit.prevent="submitForm" class="max-w-md mx-auto">
          <!-- Nama Pelanggan -->
          <div class="mb-4">
            <label for="nama" class="block text-gray-700 text-sm font-bold mb-2">Nama Pelanggan</label>
            <input v-model="form.nama" type="text" id="nama" name="nama" class="w-full border p-2 rounded focus:outline-none focus:border-blue-500">
          </div>

          <!-- Harga Jual -->
          <div class="mb-4">
            <label for="hargaJual" class="block text-gray-700 text-sm font-bold mb-2">Harga Jual</label>
            <input v-model="form.hargaJual" type="number" id="hargaJual" name="hargaJual" class="w-full border p-2 rounded focus:outline-none focus:border-blue-500">
          </div>

          <!-- Harga Beli -->
          <div class="mb-4">
            <label for="hargaBeli" class="block text-gray-700 text-sm font-bold mb-2">Harga Beli</label>
            <input v-model="form.hargaBeli" type="number" id="hargaBeli" name="hargaBeli" class="w-full border p-2 rounded focus:outline-none focus:border-blue-500">
          </div>

          <!-- Kategori -->
          <div class="mb-4">
            <label for="kategori" class="block text-gray-700 text-sm font-bold mb-2">Kategori</label>
            <select v-model="form.kategori" id="kategori" name="kategori" class="w-full border p-2 rounded focus:outline-none focus:border-blue-500">
              <option value="pulsa">Pulsa</option>
              <option value="listrik">Listrik</option>
              <option value="uang-elektronik">Uang Elektronik</option>
              <option value="lainnya">Lainnya</option>
            </select>
          </div>

          <!-- Tanggal -->
          <div class="mb-4">
            <label for="tanggal" class="block text-gray-700 text-sm font-bold mb-2">Tanggal</label>
            <input v-model="form.tanggal" type="text" id="tanggal" name="tanggal" class="w-full border p-2 rounded focus:outline-none focus:border-blue-500" readonly>
          </div>

          <!-- Tombol Submit -->
          <button type="submit" class="bg-blue-500 text-white py-2 px-4 rounded-full text-lg transition duration-300 hover:bg-blue-700">Simpan</button>
        </form>
      </section>
    </main>
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
      form: {
        nama: '',
        hargaJual: null,
        hargaBeli: null,
        kategori: 'pulsa',
        tanggal: '',
      },
    };
  },
  methods: {
    async submitForm() {
      // Menggunakan Supabase untuk menyimpan data ke tabel 'penjualan'
      const { data, error } = await supabase.from('penjualan').upsert([
        {
          nama: this.form.nama,
          harga_jual: this.form.hargaJual,
          harga_beli: this.form.hargaBeli,
          kategori: this.form.kategori,
          tanggal: this.form.tanggal,
        },
      ]);

      if (error) {
        console.error('Error saving data to Supabase:', error);
      } else {
        console.log('Data berhasil disimpan ke Supabase:', data);
        // Setelah menyimpan, mungkin ingin mengosongkan form atau melakukan aksi lain
        this.resetForm();
      }
    },
    resetForm() {
      // Mengosongkan nilai-nilai dalam form
      this.form.nama = '';
      this.form.hargaJual = null;
      this.form.hargaBeli = null;
      this.form.kategori = 'pulsa';
      this.form.tanggal = new Date().toISOString().substr(0, 10);
    },
  },
  mounted() {
    // Setel tanggal secara otomatis saat komponen dimuat
    this.form.tanggal = new Date().toISOString().substr(0, 10);
  },
};
</script>

<style scoped>
/* Gaya khusus untuk halaman ini */
</style>
