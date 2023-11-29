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
      submitForm() {
        // Implementasi logika untuk menyimpan data ke basis data (misalnya, Supabase)
        // Di sini, Anda dapat menggunakan Supabase atau API lain yang Anda pilih
        // Setelah data berhasil disimpan, mungkin ingin mengosongkan form atau melakukan aksi lain
        console.log('Data yang akan disimpan:', this.form);
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
  