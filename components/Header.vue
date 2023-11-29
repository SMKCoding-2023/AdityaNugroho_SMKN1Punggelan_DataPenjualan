<template>
    <header class="w-full border-b border-slate-200 py-3">
    <div class="container">
    <div class="flex justify-between items-center">
    <div>
    <NuxtLink to="/data" class="text-3xl font-bold pl-6 text-blue-600 hover:">AditCell</NuxtLink>
    </div>
    <nav class="flex items-center gap-6">
    <NuxtLink to="/" class="text-base text-blue-400 hover:text-xl text-blue-600 font-bold focus:outline-none">Home</NuxtLink>
    
    <NuxtLink to="/data" class="text-base text-blue-400 hover:text-xl font-bold  text-blue-600 focus:outline-none">data penjualan</NuxtLink>
    <NuxtLink to="/kontak" class="text-base text-blue-400 hover:text-xl font-bold text-blue-600 focus:outline-none">kontak</NuxtLink>
    </nav>
    </div>
    </div>
    </header>
   </template>

   <style scoped>
header{
    background-image: url(/images/header.png);
    background-position: center;
   
}
</style>
   