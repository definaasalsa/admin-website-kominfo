<script lang="ts">
  import { goto } from '$app/navigation';
  import { onMount } from "svelte";

  type Tag = {
    id: number;
    tag: string;
    createdAt: string;
    updatedAt: string;
  };

  let tags: Tag[] = [];

  // ambil data
  onMount(() => {
    const data = localStorage.getItem("tags");
    if (data) {
      tags = JSON.parse(data);
    }
  });

  // SEGARKAN
  function resetData() {
    location.reload();
  }
</script>

<div class="p-6 bg-gray-100 min-h-screen">

  <div class="p-4">

    <!-- HEADER -->
    <div class="flex justify-between items-center mb-4">
      <h1 class="text-lg font-semibold text-gray-700">
        Tag <span class="text-gray-400 font-normal">Daftar</span>
      </h1>

      <div class="text-sm text-gray-400">
        Beranda / Master Data / <span class="text-gray-600">Tag</span>
      </div>
    </div>

    <!-- TOOLBAR -->
    <div class="flex justify-between items-center mb-4">

      <!-- LEFT -->
      <div class="flex items-center gap-2">

        <!-- SEGARKAN -->
        <button 
  on:click={refresh}
  class="flex items-center gap-2 px-4 py-2 text-sm border border-blue-300 text-blue-600 rounded-md bg-white hover:bg-blue-50 shadow-sm"
>
  <RefreshCw size={16} />
  Segarkan
</button>

        <button class="px-3 py-2 text-sm border border-blue-300 text-blue-600 rounded-md bg-white hover:bg-blue-50 shadow-sm">
          Saring
        </button>

      </div>

      <!-- RIGHT -->
      <div class="flex items-center gap-2">

        <button class="flex items-center gap-1 px-3 py-2 border border-blue-300 text-blue-600 rounded-md bg-white hover:bg-blue-50 shadow-sm">
          ▦ <span class="text-xs">▾</span>
        </button>

        <button
          on:click={() => goto('/data-master/tag/tambah_tag')}
          class="flex items-center justify-center px-4 py-2 border border-blue-300 text-blue-600 rounded-md bg-white hover:bg-blue-50 shadow-sm"
        >
          + Baru
        </button>

      </div>

    </div>

    <!-- TABLE -->
    <div class="border rounded overflow-hidden">

      <table class="w-full text-sm">
        <thead class="bg-gray-50 text-gray-600">
          <tr>
            <th class="px-4 py-3 text-left">Id ↑</th>
            <th class="px-4 py-3 text-left">Tag</th>
            <th class="px-4 py-3 text-left">Dibuat Pada</th>
            <th class="px-4 py-3 text-left">Diperbarui Pada ↑</th>
            <th class="px-4 py-3 text-center">Aksi</th>
          </tr>
        </thead>

        <tbody>
          {#if tags.length === 0}
            <tr>
              <td colspan="5" class="px-4 py-6 text-gray-400">
                ⓘ Tidak ada data.
              </td>
            </tr>
          {:else}
            {#each tags as item}
              <tr class="border-t hover:bg-gray-50">
                <td class="px-4 py-3">{item.id}</td>
                <td class="px-4 py-3">{item.tag}</td>
                <td class="px-4 py-3">{item.createdAt}</td>
                <td class="px-4 py-3">{item.updatedAt}</td>
                <td class="px-4 py-3 text-center">⋮</td>
              </tr>
            {/each}
          {/if}
        </tbody>
      </table>

    </div>

    <!-- FOOTER -->
    <div class="flex justify-between items-center mt-4 text-sm text-gray-500">

      <span>Menampilkan sampai dari {tags.length} entri</span>

      <div class="flex items-center gap-2">

        <select class="border border-gray-300 rounded px-2 py-1 text-sm bg-white">
          <option>10</option>
          <option>25</option>
          <option>50</option>
        </select>

        <div class="flex items-center gap-1">
          <button class="px-2 py-1 border border-gray-300 rounded bg-white hover:bg-gray-100">
            ‹
          </button>

          <button class="px-3 py-1 rounded bg-[#4f79c7] text-white">
            1
          </button>

          <button class="px-2 py-1 border border-gray-300 rounded bg-white hover:bg-gray-100">
            ›
          </button>
        </div>

      </div>

    </div>

  </div>

</div>