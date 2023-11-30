<template>
    <div class="bgik">
      <header class="bg-blue-500 text-white py-4">
        <h1 class="text-2xl font-bold text-center">Hubungi Saya</h1>
     
      </header>
  
      <main class="m-4 animate__animated animate__fadeIn">
        <section class="post bg-white p-4 border rounded-md shadow-md animate__animated animate__fadeInUp">
          <div class="user-info flex items-center animate__animated animate__fadeInUp">
            <img src="/images/icons/icon-144x144.png" alt="User Avatar" class="w-12 h-12 rounded-full mr-4">
            <h2 class="text-xl font-bold">Aditya Nugroho</h2>
          </div>
          <p class="mt-2">Rajin bermalas-malasan</p>
        </section>
        <!-- Add more posts here -->
      </main>
  
      <div class="flex justify-center mt-4">
        <a href="https://web.facebook.com/adit.lah.739/" target="_blank" class="mr-4">
          <img src="/images/facebook.png" alt="Facebook" class="w-12 h-12 transition-transform transform hover:scale-110">
        </a>
        <a href="https://twitter.com/elonmusk?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor" target="_blank" class="mr-4">
          <img src="/images/twitter.png" alt="Twitter" class="w-12 h-12 transition-transform transform hover:scale-110">
        </a>
        <a href="https://www.instagram.com/_adityngrho/" target="_blank" class="mr-4">
          <img src="/images/instagram.png" alt="Instagram" class="w-12 h-12 transition-transform transform hover:scale-110">
        </a>
        <a href="https://www.youtube.com/channel/UCiQjs5i_crzOs-6aFdkP3lg" target="_blank">
          <img src="/images/youtube.png" alt="YouTube" class="w-12 h-12 transition-transform transform hover:scale-110">
        </a>
      </div>
  
     
    </div>
  </template>
  
  <style scoped>
  /* Tambahkan gaya animasi */
  @keyframes scaleIn {
    from {
      transform: scale(0.8);
    }
    to {
      transform: scale(1);
    }
  }
  
  /* Gaya tambahan */
  body {
    margin: 0;
    padding: 0;
  }
  
  /* Gaya spesifik untuk halaman ini */
  .post:hover {
    animation: scaleIn 0.3s ease-in-out; /* Efek animasi saat hover pada post */
  }
  
  /* Gaya spesifik untuk footer */
  footer a {
    margin-right: 10px;
  }
  
  .bgik{
    background-image: url(/images/icons/icon-512x512.png);
    height: 90vh;
  }
  </style>
  