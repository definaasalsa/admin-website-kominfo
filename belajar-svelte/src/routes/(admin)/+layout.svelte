<script>
  import '../../app.css'
  import { page } from '$app/stores';
  import { goto } from '$app/navigation';

  let sidebarOpen = true;
  let openDataMaster = false;
</script>

{#if $page.url.pathname === '/login'}
  <slot />
{:else}

<div>

  <!-- SIDEBAR -->
  <div class={`fixed top-0 left-0 h-screen bg-white border-r transition-all duration-300 
    ${sidebarOpen ? 'w-64' : 'w-0'} overflow-y-auto`}>

    <div class="p-4">

      <div class="flex items-center gap-2 mb-8">
        <img src="https://pertanian.jatimprov.go.id/storage/2019/10/logo-provinsi-jawa-timur.png" class="w-8 h-10" />
        <span class="font-semibold text-gray-700">
          Dinas Kominfo Jatim
        </span>
      </div>

      <ul class="space-y-2 text-sm text-gray-700">

        <li>
          <a href="/beranda" class="p-2 block hover:bg-gray-100 rounded font-semibold">
            Beranda
          </a>
        </li>

        <li>
          <a href="/admin" class="flex justify-between p-2 rounded font-semibold hover:bg-gray-100">
            <span>Admin</span>
            <span>›</span>
          </a>
        </li>

        <!-- DATA MASTER -->
        <li>
          <button
            on:click={() => openDataMaster = !openDataMaster}
            class="w-full flex justify-between items-center p-2 rounded font-semibold hover:bg-gray-100"
          >
            <span>Data Master</span>
            <span class={`transition-transform duration-200 ${openDataMaster ? 'rotate-90' : ''}`}>
              ›
            </span>
          </button>

          {#if openDataMaster}
            <ul class="ml-6 mt-1 space-y-1 text-sm text-gray-600">

              <li>
                <button
                  on:click={() => goto('/data-master/tag')}
                  class="w-full text-left p-2 hover:bg-gray-100 rounded"
                >
                  Tag
                </button>
              </li>

              <li>
                <button
                  on:click={() => goto('/data-master/kategori')}
                  class="w-full text-left p-2 hover:bg-gray-100 rounded"
                >
                  Kategori
                </button>
              </li>

            </ul>
          {/if}
        </li>

        <!-- MENU LAIN -->
        <li><a href="/menu-navigasi" class="p-2 block hover:bg-gray-100 rounded font-semibold">Menu Navigasi</a></li>
        <li><a href="/agenda" class="p-2 block hover:bg-gray-100 rounded font-semibold">Agenda</a></li>
        <li><a href="/banner" class="p-2 block hover:bg-gray-100 rounded font-semibold">Banner</a></li>
        <li><a href="/berita" class="p-2 block hover:bg-gray-100 rounded font-semibold">Berita</a></li>
        <li><a href="/galeri" class="p-2 block hover:bg-gray-100 rounded font-semibold">Galeri</a></li>
        <li><a href="/halaman" class="p-2 block hover:bg-gray-100 rounded font-semibold">Halaman</a></li>
        <li><a href="/video" class="p-2 block hover:bg-gray-100 rounded font-semibold">Video</a></li>
        <li><a href="/kuesioner" class="p-2 block hover:bg-gray-100 rounded font-semibold">Kuesioner</a></li>
        <li><a href="/running-text" class="p-2 block hover:bg-gray-100 rounded font-semibold">Running Text</a></li>
        <li><a href="/download" class="p-2 block hover:bg-gray-100 rounded font-semibold">Download</a></li>
        <li><a href="/majalah" class="p-2 block hover:bg-gray-100 rounded font-semibold">Majalah</a></li>

      </ul>

    </div>
  </div>

  <!-- MAIN -->
  <!-- 🔥 FIX DISINI: HAPUS p-6 -->
  <div class={`bg-gray-100 min-h-screen transition-all duration-300 
    ${sidebarOpen ? 'ml-64' : 'ml-0'}`}>

    <!-- TOPBAR -->
    <div class="flex items-center gap-4 p-4">
      <button 
        on:click={() => sidebarOpen = !sidebarOpen}
        class="text-xl p-2 bg-white rounded shadow-sm"
      >
        ☰
      </button>
    </div>

    <!-- CONTENT -->
    <div class="px-6 pb-6">
      <slot />
    </div>

  </div>

</div>

{/if}