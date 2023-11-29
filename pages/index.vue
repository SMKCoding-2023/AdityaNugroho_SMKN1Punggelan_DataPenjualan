<template>
    <div>
      <main class="container mx-auto p-4">
        <section class="text-center">
          <h1 class="text-4xl font-bold mb-4 animate__animated animate__fadeIn">
            Selamat Datang di Aplikasi Penjualan Pulsa!
          </h1>
          <p class="text-lg mb-8 animate__animated animate__fadeInUp">
            Kami memberikan kemudahan dalam mencatat dan mengelola penjualan pulsa Anda.
          </p>
          <NuxtLink to ="/penjualan" class="bg-blue-500 text-white py-2 px-4 rounded-full text-lg transition duration-300 hover:bg-blue-700">
            Lihat Penjualan Pulsa
          </NuxtLink>
          <br>
        
        
        </section>
  
        <!-- Animasi untuk elemen berikutnya -->
        <section id="penjualan" class="mt-10">
          <h2 class="text-3xl font-bold mb-4 animate__animated animate__fadeIn">Penjualan Pulsa</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
            <!-- Kartu Penjualan 1 -->
            <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp">
              <h3 class="text-xl font-bold mb-2">Paket A</h3>
              <p class="text-gray-600">Penjualan hari ini: 50</p>
              <p class="text-gray-600">Total pendapatan: $500</p>
            </div>
  
            <!-- Kartu Penjualan 2 -->
            <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp" style="animation-delay: 0.2s;">
              <h3 class="text-xl font-bold mb-2">Paket B</h3>
              <p class="text-gray-600">Penjualan hari ini: 30</p>
              <p class="text-gray-600">Total pendapatan: $300</p>
            </div>
  
            <!-- Kartu Penjualan 3 -->
            <div class="bg-white p-4 rounded-lg shadow-md animate__animated animate__fadeInUp" style="animation-delay: 0.4s;">
              <h3 class="text-xl font-bold mb-2">Paket C</h3>
              <p class="text-gray-600">Penjualan hari ini: 20</p>
              <p class="text-gray-600">Total pendapatan: $200</p>
            </div>
          </div>
        </section>
      </main>
      <Footer />
    </div>
  </template>
  
  
  
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
  