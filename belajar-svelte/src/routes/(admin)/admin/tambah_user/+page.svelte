<script lang="ts">
  import { goto } from '$app/navigation';

  let user = {
    username: "",
    nama: "",
    peran: ""
  };

  function submitForm() {
    console.log(user);
    goto('/admin');
  }
</script>

<div class="p-6 bg-gray-100 min-h-screen">

  <!-- HEADER -->
  <div class="flex justify-between items-center mb-4">
    <h1 class="text-lg font-semibold text-gray-700">
      Administrator <span class="text-gray-400 text-sm">Sunting</span>
    </h1>

    <div class="flex gap-2 text-sm">
      <button class="bg-gray-200 px-3 py-1 rounded">Daftar</button>
      <button class="bg-blue-600 text-white px-3 py-1 rounded">Lihat</button>
      <button class="bg-gray-300 px-3 py-1 rounded">Hapus</button>
    </div>
  </div>

  <!-- FORM -->
  <div class="bg-white p-6 rounded shadow-sm space-y-4">

    <!-- Username -->
    <div>
      <label class="text-sm text-gray-600">* Nama Pengguna</label>
      <input bind:value={user.username} class="border p-2 w-full mt-1" />
    </div>

    <!-- Nama -->
    <div>
      <label class="text-sm text-gray-600">* Nama</label>
      <input bind:value={user.nama} class="border p-2 w-full mt-1" />
    </div>

    <!-- Avatar -->
<div>
  <label class="text-sm text-gray-600">Avatar</label>

  <div class="border border-dashed border-gray-300 rounded p-10 text-center mt-2">

    <!-- ICON -->
    <div class="flex justify-center mb-3">
      <svg xmlns="http://www.w3.org/2000/svg" class="w-10 h-10 text-gray-400" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
        <path d="M12 16V4M12 4l-4 4M12 4l4 4"/>
        <path d="M4 16v2a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-2"/>
      </svg>
    </div>

    <!-- BUTTON -->
    <button class="bg-blue-600 text-white px-4 py-2 rounded text-sm shadow">
      Telusuri
    </button>

    <!-- TEXT -->
    <p class="text-gray-400 text-xs mt-3">
      Atau seret berkas ke sini
    </p>

  </div>
</div>

    <!-- Password -->
    <div>
      <label class="text-sm text-gray-600">Kata Sandi</label>
      <input type="password" placeholder="Masukkan Kata sandi" class="border p-2 w-full mt-1" />
    </div>

    <div>
      <label class="text-sm text-gray-600">Konfirmasi Kata Sandi</label>
      <input type="password" placeholder="Masukkan Konfirmasi kata sandi" class="border p-2 w-full mt-1" />
    </div>

    <!-- Peran -->
    <div>
      <label class="text-sm text-gray-600">Peran</label>
      <select bind:value={user.peran} class="border p-2 w-full mt-1">
        <option value="">Pilih Peran</option>
        <option value="Photographer">Photographer</option>
        <option value="Reporter">Reporter</option>
        </select>
    </div>

    <div class="flex justify-end gap-2 pt-4">
        <button
            on:click={() => goto('/admin')}
            class="px-4 py-2 border rounded">
            <span>Reset</span>
        </button>

        <button
            on:click={submitForm}
            class="px-4 py-2 bg-blue-600 text-white rounded">
            <span>kirimkan</span>    
        </button>
            
    </div>

  </div>

</div>