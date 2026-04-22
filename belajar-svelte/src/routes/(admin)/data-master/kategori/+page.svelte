<script lang="ts">
  import { goto } from '$app/navigation';
  import { onMount } from "svelte";
  import { Eye, Pencil, Trash2 } from "lucide-svelte";

  type Kategori = {
    id: number;
    nama: string;
    aktif: boolean;
  };

  let kategori: Kategori[] = [];
  let openMenuId: number | null = null;

  // ambil data dari localStorage
  onMount(() => {
    const data = localStorage.getItem("kategori");
    if (data) {
      kategori = JSON.parse(data);
    }
  });

  // ✅ SEGARKAN (RELOAD BENERAN)
  function refreshData() {
    location.reload();
  }

  function goTambah() {
    goto('/data-master/kategori/tambah_kategori');
  }

  function toggleMenu(id: number) {
    openMenuId = openMenuId === id ? null : id;
  }

  // EDIT
  function editData(item: any) {
    localStorage.setItem("edit_kategori", JSON.stringify(item));
    goto('/data-master/kategori/sunting_kategori');
  }

  // HAPUS
  function hapus(id: number) {
    const konfirmasi = confirm("Yakin mau hapus kategori ini?");
    if (!konfirmasi) return;

    let data = JSON.parse(localStorage.getItem("kategori") || "[]");

    const newData = data.filter((item: any) => item.id !== id);

    localStorage.setItem("kategori", JSON.stringify(newData));

    kategori = newData;

    openMenuId = null;
  }
</script>

<div class="p-6 bg-gray-100 min-h-screen">

  <div class="p-6">

    <!-- HEADER -->
    <div class="flex justify-between items-center mb-5">
      <h1 class="text-xl font-semibold text-gray-700">
        Kategori <span class="text-gray-400 font-normal">Daftar</span>
      </h1>

      <div class="text-sm text-gray-400">
        Beranda / Master Data / <span class="text-gray-600">Kategori</span>
      </div>
    </div>

    <!-- TOOLBAR -->
    <div class="flex justify-between items-center mb-5">

      <div class="flex items-center gap-3">

        <!-- ✅ SEGARKAN FIX -->
        <button 
          on:click={refreshData}
          class="flex items-center gap-2 px-4 py-2 text-sm border border-blue-300 text-blue-600 rounded-md bg-white hover:bg-blue-50 shadow-sm"
        >
          ⟳ Segarkan
        </button>

        <button class="flex items-center gap-2 px-4 py-2 text-sm border border-blue-300 text-blue-600 rounded-md bg-white hover:bg-blue-50 shadow-sm">
          ⏷ Saring
        </button>

        <div class="flex items-center border border-gray-200 rounded-full px-4 py-2 bg-gray-50 w-64">
          <input
            type="text"
            placeholder="Pencarian"
            class="outline-none text-sm w-full bg-transparent"
          />
        </div>

      </div>

      <div class="flex items-center gap-3">

        <button class="flex items-center gap-1 px-4 py-2 border border-blue-300 text-blue-600 rounded-md bg-white hover:bg-blue-50 shadow-sm">
          ▦ <span class="text-xs">▾</span>
        </button>

        <button
          on:click={goTambah}
          class="flex items-center justify-center px-4 py-2 border border-blue-300 text-blue-600 rounded-md bg-white hover:bg-blue-50 shadow-sm"
        >
          + Baru
        </button>

      </div>

    </div>

    <!-- TABLE -->
    <div class="border border-gray-200 rounded-xl overflow-visible">

      <table class="w-full text-sm">

        <thead class="bg-gray-50 text-gray-500">
          <tr>
            <th class="px-5 py-3 text-left font-medium">Id ↑</th>
            <th class="px-5 py-3 text-left font-medium">Kategori</th>
            <th class="px-5 py-3 text-center font-medium">Aktif</th>
            <th class="px-5 py-3 text-center font-medium">Aksi</th>
          </tr>
        </thead>

        <tbody>
          {#if kategori.length === 0}
            <tr>
              <td colspan="4" class="px-5 py-6 text-gray-400 text-center">
                Tidak ada data.
              </td>
            </tr>
          {:else}
            {#each kategori as item}
              <tr class="border-t hover:bg-gray-50 transition relative">

                <td class="px-5 py-4 text-gray-600">{item.id}</td>

                <td class="px-5 py-4 text-gray-700 font-medium">
                  {item.nama}
                </td>

                <td class="px-5 py-4 text-center text-gray-500">
                  {item.aktif ? "✓" : "×"}
                </td>

                <!-- AKSI -->
                <td class="px-5 py-4 text-center relative">

                  <button
                    on:click={() => toggleMenu(item.id)}
                    class="text-gray-500 text-lg"
                  >
                    ⋮
                  </button>

                  {#if openMenuId === item.id}
                    <div class="absolute right-0 mt-2 w-44 bg-white border rounded-lg shadow-lg z-50 text-sm">

                      <button class="flex items-center gap-3 px-4 py-2 hover:bg-gray-100 w-full text-left">
                        <Eye size={16} />
                        Tampilkan
                      </button>

                      <button
                        on:click={() => editData(item)}
                        class="flex items-center gap-3 px-4 py-2 hover:bg-gray-100 w-full text-left"
                      >
                        <Pencil size={16} />
                        Sunting Cepat
                      </button>

                      <button
                        on:click={() => hapus(item.id)}
                        class="flex items-center gap-3 px-4 py-2 hover:bg-gray-100 w-full text-left text-red-500"
                      >
                        <Trash2 size={16} />
                        Hapus
                      </button>

                    </div>
                  {/if}

                </td>

              </tr>
            {/each}
          {/if}
        </tbody>

      </table>

    </div>

  </div>

</div>