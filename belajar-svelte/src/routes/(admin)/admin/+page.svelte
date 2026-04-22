<script lang="ts">
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte'; 
  import { RefreshCw } from 'lucide-svelte';

  type User = {
    id: number;
    username: string;
    nama: string;
    peran: string;
    dibuat: string;
    update: string;
  };

  let users: User[] = [];

  let search = "";

  let activeMenu: number | null = null;
  let showEditModal = false;

  let selectedUser = {
    id:0,
    username:"",
    nama:"",
    peran:"",
    dibuat:"",
    update:"",
  };

  // 🔥 ambil data dari localStorage
  onMount(() => {
    const data = localStorage.getItem("users");
    if (data) {
      users = JSON.parse(data);
    }
  });

  // 🔥 DELETE USER (SUDAH BENAR)
  function deleteUser(id: number) {
    users = users.filter(u => u.id !== id);
    localStorage.setItem("users", JSON.stringify(users));
    activeMenu = null;
  }

  let showAddForm = false;

  let newUser = {
    username: "",
    nama: "",
    peran: "",
    password: "",
    confirmPassword: ""
  };

  function addUser() {
    const now = new Date().toISOString().slice(0, 19).replace("T", " ");

    users = [
      ...users,
      {
        id: Date.now(),
        username: newUser.username,
        nama: newUser.nama,
        peran: newUser.peran,
        dibuat: now,
        update: now
      }
    ];

    showAddForm = false;
  }

  $: filteredUsers = users.filter(u =>
    u.username.toLowerCase().includes(search.toLowerCase()) ||
    u.nama.toLowerCase().includes(search.toLowerCase())
  );

  function openMenu(id: number) {
    activeMenu = activeMenu === id ? null : id;
  }

  function editUser(user: any) {
    selectedUser = { ...user };
    showEditModal = true;
    activeMenu = null;
  }

  function saveEdit() {
    users = users.map(u =>
      u.id === selectedUser.id ? selectedUser : u
    );
    showEditModal = false;
  }
</script> 

  
<div class="p-8 bg-gray-100 min-h-screen space-y-8">

  <!-- HEADER -->
  <div class="flex justify-between items-center">
    <h1 class="text-xl font-semibold text-gray-700">
      Administrator <span class="text-gray-400 text-sm">Daftar</span>
    </h1>

    <div class="text-sm text-gray-500">
      Beranda / Auth / Users
    </div>
  </div>

  <!-- TOOLBAR -->
  <div class="flex justify-between items-center">

    <div class="flex items-center gap-3">

      <button class="flex items-center gap-2 bg-gray-200 px-3 py-2 rounded text-sm">
        <RefreshCw size={16} />
        Segarkan
      </button>
      
      <input 
        bind:value={search}
        placeholder="Pencarian"
        class="border px-4 py-2 rounded text-sm w-56 focus:outline-none"
      />

    </div>

    <button 
      on:click={() => goto('/admin/tambah_user')}
      class="bg-blue-600 text-white px-4 py-2 rounded text-sm">
      + Baru
    </button>

  </div>

  <!-- TABLE -->
  <div class="bg-white rounded shadow-sm overflow-visible">

    <table class="w-full text-sm">

      <thead class="bg-gray-100 text-gray-600">
        <tr>
          <th class="p-4 text-left">ID</th>
          <th class="p-4 text-left">Username</th>
          <th class="p-4 text-left">Nama</th>
          <th class="p-4 text-left">Peran</th>
          <th class="p-4 text-left">Izin</th>
          <th class="p-4 text-left">Dibuat Pada</th>
          <th class="p-4 text-left">Diperbarui Pada</th>
          <th class="p-4 text-left">Aksi</th>
        </tr>
      </thead>

      <tbody>
        {#each filteredUsers as user}
        <tr class="border-t hover:bg-gray-50">

          <td class="p-4">{user.id}</td>
          <td class="p-4">{user.username}</td>
          <td class="p-4">{user.nama}</td>

          <td class="p-4">
            <span class="bg-blue-500 text-white px-3 py-1 rounded text-xs">
              {user.peran}
            </span>
          </td>

          <td class="p-4 text-blue-500 cursor-pointer"
            on:click={() => window.location.href = `/admin/izin?id=${user.id}`}
          >
            Lihat
          </td>

          <td class="p-4 text-gray-500">{user.dibuat}</td>
          <td class="p-4 text-gray-500">{user.update}</td>

          <!-- AKSI -->
          <td class="p-4 relative">

            <button on:click={() => openMenu(user.id)}>⋮</button>

            {#if activeMenu === user.id}
            <div class="absolute right-4 mt-2 bg-white border rounded shadow-md w-48 z-10">

              <button class="flex items-center gap-3 w-full px-4 py-2 hover:bg-gray-100 text-gray-700">
                <span>Tampilkan</span>
              </button>

              <button 
                on:click={() => window.location.href =`/admin/sunting_admin?id=${user.id}`}
                class="flex items-center gap-3 w-full px-4 py-2 hover:bg-gray-100 text-gray-700"
              >
                <span>Sunting Cepat</span>
              </button>

              <button 
                  on:click={() => deleteUser(user.id)}
                  class="flex items-center gap-3 w-full px-4 py-2 hover:bg-gray-100 text-red-500">
                  <span>Hapus</span>
              </button>

            </div>
            {/if}

          </td>

        </tr>
        {/each}
      </tbody>

    </table>

  </div>

  <!-- FOOTER -->
  <div class="flex justify-between items-center text-sm text-gray-500">

    <span>
      Menampilkan {filteredUsers.length} data
    </span>

    <div class="flex items-center gap-2">

      <button class="px-3 py-1 border rounded hover:bg-gray-100">‹</button>

      <button class="px-3 py-1 bg-blue-600 text-white rounded">
        1
      </button>

      <button class="px-3 py-1 border rounded hover:bg-gray-100">
        2
      </button>

      <button class="px-3 py-1 border rounded hover:bg-gray-100">›</button>

    </div>

  </div>

</div>