<script lang="ts">
    import {goto} from '$app/navigation';
    import { ChevronDown, ChevronRight, Pencil, Trash } from 'lucide-svelte';


  let openGroup: string[] = [];

  function toggle(group: string) {
    if (openGroup.includes(group)) {
      openGroup = openGroup.filter(g => g !== group);
    } else {
      openGroup = [...openGroup, group];
    }
  }

  function openAll() {
    openGroup = permissions.map(p => p.key);
  }

  function closeAll() {
    openGroup = [];
  }

  let permissions = [
    {
      group: "Auth management",
      key: "auth",
      items: [
        { name: "Users", method: "ANY", path: "admin/auth/users*" },
        { name: "Roles", method: "ANY", path: "admin/auth/roles*" },
        { name: "Permissions", method: "ANY", path: "admin/auth/permissions*" },
        { name: "Menu", method: "ANY", path: "admin/auth/menu*" },
        { name: "Extension", method: "ANY", path: "admin/auth/extensions*" },
      ]
    },
    {
      group: "Helpers",
      key: "helpers",
      items: [
        { name: "Scaffold", method: "ANY", path: "admin/helpers/scaffold*" },
        { name: "Icons", method: "ANY", path: "admin/helpers/icons*" },
      ]
    },
    {
      group: "Master Data",
      key: "master",
      items: [
        { name: "Tag List", method: "GET", path: "admin/master-data/tag*" },
        { name: "Tag Create", method: "POST", path: "admin/master-data/tag" },
      ]
    }
  ];

</script>

<div class="p-8 bg-gray-100 min-h-screen space-y-6">

  <!-- header -->
  <div class="flex justify-between items-center">
    <h1 class="text-xl font-semibold text-gray-700">
      Izin <span class="text-gray-400 text-sm">Daftar</span>
    </h1>

    <div class="text-sm text-gray-500">
      Beranda / Auth / Permissions
    </div>
  </div>

  <!-- toolbar -->
  <div class="flex justify-between items-center">

    <div class="flex gap-2">

      <button on:click={openAll} class="bg-[#4f79c7] text-white px-3 py-2 rounded text-sm">
        Bentangkan
      </button>

      <button on:click={closeAll} class="bg-[#4f79c7] text-white px-3 py-2 rounded text-sm">
        Ciutkan
      </button>

      <button class="bg-gray-200 px-3 py-2 rounded text-sm">
        Simpan
      </button>

      <button class="bg-gray-200 px-3 py-2 rounded text-sm">
        Segarkan
      </button>

    </div>

    <button class="bg-[#4f79c7] text-white px-4 py-2 rounded text-sm">
      + Baru
    </button>

  </div>

  <!-- list -->
  <div class="bg-white rounded shadow-sm divide-y">

    {#each permissions as group}

      <div>

        <!-- header group -->
<button
  class="flex justify-between items-center p-4 w-full cursor-pointer hover:bg-gray-50"
  on:click={() => toggle(group.key)}
>

  <!-- kiri -->
  <div class="flex items-center gap-3 font-semibold text-gray-700">
    
    {#if openGroup.includes(group.key)}
      <ChevronDown size={16} />
    {:else}
      <ChevronRight size={16} />
    {/if}

    <span>{group.group}</span>

  </div>

  <!-- kanan -->
  <div class="flex items-center gap-3 text-gray-400">
    <Pencil size={16} />
    <Trash size={16} />
  </div>

</button>

        <!-- content -->
{#if openGroup.includes(group.key)}
<div class="px-6 pb-4 space-y-2">

  {#each group.items as item}

  <div class="flex justify-between items-center border rounded px-4 py-2 hover:bg-gray-50">

    <!-- kiri -->
    <div class="flex items-center gap-4">

      <span class="text-gray-700">{item.name}</span>

      <span class="bg-[#4f79c7] text-white text-xs px-2 py-1 rounded">
        {item.method}
      </span>

      <span class="text-gray-500 text-sm">
        {item.path}
      </span>

    </div>

    <!-- icon kanan -->
    <div class="flex items-center gap-3 text-gray-400">

      <!-- edit -->
      <button class="hover:text-blue-600">
        <Pencil size={16} />
      </button>

      <!-- delete -->
      <button class="hover:text-red-500">
        <Trash size={16} />
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