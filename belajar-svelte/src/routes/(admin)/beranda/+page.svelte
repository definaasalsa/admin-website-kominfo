<script>
  import { onMount } from 'svelte';

  let chart;

  onMount(async () => {
    const { Chart } = await import('chart.js/auto');

    const ctx = document.getElementById('beritaChart');

    chart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: ['Januari 2026', 'Februari 2026'],
        datasets: [{
          label: 'Jumlah Berita',
          data: [150, 130],
          backgroundColor: '#5c6ea8'
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: {
            display: false
          }
        }
      }
    });
  });
</script>

<div class="space-y-6">

  <div class="flex justify-between items-center mb-4">
    <h1 class="text-gray-700 font-semibold">
      <span>Dashboard Web Profile Kominfo Jatim</span>
    </h1>

    <div class="text-sm text-blue-600 flex items-center gap-1 cursor-pointer">
      Beranda
    </div>
  </div>
  
  <div class="grid grid-cols-4 gap-8">

    <div class="col-span-2 bg-[#4f79c7] text-white rounded-none flex flex-col items-center justify-center py-14 shadow-sm font-semibold">
      <img 
        src="https://pertanian.jatimprov.go.id/storage/2019/10/logo-provinsi-jawa-timur.png" 
        class="w-16 h-20 mb-3"
      />
      <p class="text-lg">
        <span>Dinas Kominfo Jatim</span>
      </p>
    </div>

    <div class="bg-[#d1a25a] text-white h-32 relative rounded-none shadow-sm">
      <p class="absolute top-3 left-4 text-sm font-semibold">
        Jumlah Redaktur Terdaftar Sistem
      </p>
      <div class="flex items-center justify-center h-full">
        <h2 class="text-3xl font-semibold">5</h2>
      </div>
    </div>

    <div class="bg-[#d1a25a] text-white h-32 relative rounded-none shadow-sm">
      <p class="absolute top-3 left-4 text-sm font-semibold">
        Jumlah Reporter Terdaftar Sistem
      </p>
      <div class="flex items-center justify-center h-full">
        <h2 class="text-3xl font-semibold">5</h2>
      </div>
    </div>

  </div>

  <div class="grid grid-cols-2 gap-6">

    <div class="bg-[#5fb07d] text-white h-40 relative rounded-none shadow-sm">
      <p class="absolute top-4 left-5 font-semibold">
        Total Berita Terbuat
      </p>
      <div class="flex items-center justify-center h-full">
        <h2 class="text-4xl font-semibold">300</h2>
      </div>
    </div>

    <div class="bg-[#d65b55] text-white h-40 relative rounded-none shadow-sm">
      <p class="absolute top-4 left-5 font-semibold">
        Total Berita Tahun Ini
      </p>
      <div class="flex items-center justify-center h-full">
        <h2 class="text-4xl font-semibold">300</h2>
      </div>
    </div>

  </div>

  <div class="grid grid-cols-4 gap-6">

    <div class="bg-[#5c6ea8] text-white h-32 relative rounded-none shadow-sm">
      <p class="absolute top-3 left-4 text-sm font-semibold">
        <span>Total Galeri dan Gambar Terbuat</span>
      </p>
      <div class="flex items-center justify-center h-full">
        <h2 class="text-2xl font-semibold">1198</h2>
      </div>
    </div>

    <div class="bg-[#5fb07d] text-white h-32 relative rounded-none shadow-sm">
      <p class="absolute top-3 left-4 text-sm font-semibold">
        <span>Total Galeri dan Gambar Tahun Ini</span>
      </p>
      <div class="flex items-center justify-center h-full">
        <h2 class="text-2xl font-semibold">1198</h2>
      </div>
    </div>

    <div class="bg-[#5c6ea8] text-white h-32 relative rounded-none shadow-sm">
      <p class="absolute top-3 left-4 text-sm font-semibold">
        <span>Total Video Terbuat</span>
      </p>
      <div class="flex items-center justify-center h-full">
        <h2 class="text-2xl font-semibold">8</h2>
      </div>
    </div>

    <div class="bg-[#d1a25a] text-white h-32 relative rounded-none shadow-sm">
      <p class="absolute top-3 left-4 text-sm font-semibold">
        <span>Total Video Tahun Ini</span>
      </p>
      <div class="flex items-center justify-center h-full">
        <h2 class="text-2xl font-semibold">8</h2>
      </div>
    </div>

  </div>

  
  <div class="bg-white p-6 shadow-sm mt-6">
    <h2 class="text-gray-700 font-semibold mb-4">
      Grafik Jumlah Berita
    </h2>
    <canvas id="beritaChart"></canvas>
  </div>

</div>