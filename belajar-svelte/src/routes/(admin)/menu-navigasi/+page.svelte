<script lang="ts">
  import { Copy } from "lucide-svelte";
  import { onMount } from "svelte";
  import { Pencil, Trash2, ChevronDown, ChevronRight } from "lucide-svelte";
  import { goto } from "$app/navigation";

  type Menu = {
    id: number;
    nama: string;
    link: string;
    aktif: boolean;
    children?: Menu[];
  };

  let menu: Menu[] = [];
  let openIds: number[] = [];

  onMount(() => {
    const data = localStorage.getItem("menu_tree");

    if (data) {
      menu = JSON.parse(data);
    } else {
      menu = [
        {
          id: 1,
          nama: "Home",
          link: "/",
          aktif: true
        },
        {
          id: 2,
          nama: "Profil",
          link: "#",
          aktif: true,
          children: [
            { id: 3, nama: "Kedudukan dan Alamat", link: "#", aktif: true },
            { id: 4, nama: "Tugas dan Fungsi", link: "#", aktif: true },
            { id: 5, nama: "Unit Kerja", link: "#", aktif: true },
            { id: 6, nama: "Struktur Organisasi", link: "#", aktif: true },
            { id: 7, nama: "Pejabat Struktural", link: "#", aktif: true },
            { id: 8, nama: "Visi Misi", link: "#", aktif: true }
          ]
        },
        {
          id: 9,
          nama: "Program",
          link: "#",
          aktif: true,
          children: [
            {
              id: 10,
              nama: "Rencana Strategis",
              link: "#",
              aktif: true,
              children: [
                { id: 11, nama: "Renstra 2014-2019", link: "#", aktif: true },
                { id: 12, nama: "Renstra 2019-2024", link: "#", aktif: true }
              ]
            },
            {
              id: 13,
              nama: "Indikator Kinerja Utama",
              link: "#",
              aktif: true
            }
          ]
        }
      ];

      localStorage.setItem("menu_tree", JSON.stringify(menu));
    }
  });

  function toggle(id: number) {
    openIds = openIds.includes(id)
      ? openIds.filter(i => i !== id)
      : [...openIds, id];
  }

  function hapus(id: number) {
    if (!confirm("Hapus menu ini?")) return;

    function remove(items: Menu[]): Menu[] {
      return items
        .filter(item => item.id !== id)
        .map(item => ({
          ...item,
          children: item.children ? remove(item.children) : []
        }));
    }

    menu = remove(menu);
    localStorage.setItem("menu_tree", JSON.stringify(menu));
  }

  function expandAll() {
    const getAllIds = (items: Menu[]): number[] =>
      items.flatMap(item => [
        item.id,
        ...(item.children ? getAllIds(item.children) : [])
      ]);

    openIds = getAllIds(menu);
  }

  function collapseAll() {
    openIds = [];
  }

  function simpan() {
    localStorage.setItem("menu_tree", JSON.stringify(menu));
    alert("Tersimpan!");
  }

  function refresh() {
    location.reload(); // ✅ reload tanpa hapus data
  }

  // ✅ FIX ERROR DI SINI
  function tambahMenu() {
    goto("/menu-navigasi/tambah_menu");
  }
</script>

<div class="p-6 bg-gray-100 min-h-screen">

  <div class="bg-white rounded-xl shadow-sm p-6">

    <!-- HEADER -->
    <div class="flex justify-between items-center mb-4">
      <h1 class="text-lg font-semibold text-gray-700">
        Menu Navigasi <span class="text-gray-400 font-normal">Daftar</span>
      </h1>

      <div class="text-sm text-gray-400">
        Beranda / Master Data / <span class="text-gray-600">Menu Navigasi</span>
      </div>
    </div>

    <!-- TOOLBAR -->
    <div class="flex justify-between items-center mb-4">

      <div class="flex gap-2">
        <button on:click={expandAll}
          class="px-3 py-2 text-sm bg-[#4f79c7] text-white rounded">
          Bentangkan
        </button>

        <button on:click={collapseAll}
          class="px-3 py-2 text-sm bg-[#4f79c7] text-white rounded">
          Ciutkan
        </button>

        <button on:click={simpan}
          class="px-3 py-2 text-sm bg-[#4f79c7] text-white rounded">
          Simpan
        </button>

        <button on:click={refresh}
          class="px-3 py-2 text-sm border rounded bg-white hover:bg-gray-100">
          Segarkan
        </button>
      </div>

      <!-- ✅ TOMBOL BARU SUDAH AKTIF -->
      <button
        on:click={tambahMenu}
        class="flex items-center gap-3 px-4 py-2 bg-[#4f79c7] text-white rounded-md shadow-sm transition"
      >
        <span class="text-sm font-medium">+ Baru</span>
        <span class="border-l border-white/40 h-4"></span>
        <Copy size={16} class="opacity-90" />
      </button>

    </div>

    <!-- TREE -->
    <div class="space-y-2">

      {#each menu as item}
        <div class="border rounded p-2">

          <div class="flex justify-between items-center">

            <div class="flex items-center gap-2">

              {#if item.children}
                <button on:click={() => toggle(item.id)}>
                  {#if openIds.includes(item.id)}
                    <ChevronDown size={16} />
                  {:else}
                    <ChevronRight size={16} />
                  {/if}
                </button>
              {/if}

              <span>{item.nama}</span>

              {#if item.aktif}
                <span class="text-xs bg-[#4f79c7] text-white px-2 rounded">
                  aktif
                </span>
              {/if}

            </div>

            <div class="flex gap-3 text-gray-500">
              <Pencil size={16} />
              <button on:click={() => hapus(item.id)}>
                <Trash2 size={16} class="text-red-500" />
              </button>
            </div>

          </div>

          {#if openIds.includes(item.id) && item.children}
            <div class="mt-2 space-y-2 ml-6">

              {#each item.children as child}
                <div class="border rounded p-2 flex justify-between items-center">

                  <div class="flex items-center gap-2">
                    <span>{child.nama}</span>

                    {#if child.aktif}
                      <span class="text-xs bg-[#4f79c7] text-white px-2 rounded">
                        aktif
                      </span>
                    {/if}
                  </div>

                  <div class="flex gap-3 text-gray-500">
                    <Pencil size={16} />
                    <button on:click={() => hapus(child.id)}>
                      <Trash2 size={16} class="text-red-500" />
                    </button>
                  </div>

                </div>
              {/each}

            </div>
          {/if}

        </div>
      {/each}

    </div>

  </div>

</div>