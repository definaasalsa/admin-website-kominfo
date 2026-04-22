<script lang="ts">
  import { goto } from "$app/navigation";

  let form = {
    judul: "",
    url: "",
    parent: "",
    aktif: false
  };

  function submit() {
    const data = localStorage.getItem("menu_tree");
    let menu = data ? JSON.parse(data) : [];

    const newMenu = {
      id: Date.now(),
      nama: form.judul,
      link: form.url,
      aktif: form.aktif,
      children: []
    };

    menu.push(newMenu);

    localStorage.setItem("menu_tree", JSON.stringify(menu));

    alert("Menu berhasil ditambahkan!");
    goto("/menu-navigasi");
  }

  function reset() {
    form = {
      judul: "",
      url: "",
      parent: "",
      aktif: false
    };
  }
</script>

<div class="p-6 bg-gray-100 min-h-screen">

  <!-- 🔥 BREADCRUMB (LUAR PUTIH) -->
  <div class="flex justify-end mb-4">
    <div class="text-sm text-gray-400">
      Beranda / Menu Navigasi / <span class="text-gray-600">Buat</span>
    </div>
  </div>

  <!-- 🔥 CARD -->
  <div class="bg-white rounded-xl shadow-sm p-6">

    <!-- HEADER DALAM CARD -->
    <div class="flex justify-between items-center mb-6">

      <h1 class="text-lg font-semibold text-gray-700">
        Menu Navigasi <span class="text-gray-400 font-normal">Buat</span>
      </h1>

      <!-- 🔥 DAFTAR (DALAM PUTIH) -->
      <button
        on:click={() => goto('/menu-navigasi')}
        class="bg-[#4f79c7] text-white px-4 py-2 rounded text-sm shadow"
      >
        ≡ Daftar
      </button>

    </div>

    <!-- FORM -->
    <div class="space-y-6">

      <!-- JUDUL -->
      <div class="flex items-center gap-4">
        <label class="w-40 text-gray-600 text-sm">
          <span class="text-red-500">*</span> Judul
        </label>

        <input
          bind:value={form.judul}
          placeholder="Masukkan judul"
          class="flex-1 border rounded px-3 py-2 text-sm focus:outline-none"
        />
      </div>

      <!-- URL -->
      <div class="flex items-center gap-4">
        <label class="w-40 text-gray-600 text-sm">
          Url / Path
        </label>

        <input
          bind:value={form.url}
          placeholder="Masukkan url / path"
          class="flex-1 border rounded px-3 py-2 text-sm focus:outline-none"
        />
      </div>

      <!-- PARENT -->
      <div class="flex items-center gap-4">
        <label class="w-40 text-gray-600 text-sm">
          Orang Tua
        </label>

        <select
          bind:value={form.parent}
          class="flex-1 border rounded px-3 py-2 text-sm bg-white"
        >
          <option value="">orang tua</option>
          <option>Home</option>
          <option>Profil</option>
          <option>Program</option>
        </select>
      </div>

      <!-- AKTIF -->
      <div class="flex items-center gap-4">
        <label class="w-40 text-gray-600 text-sm">
          Aktif
        </label>

        <label class="relative inline-flex items-center cursor-pointer">
          <input
            type="checkbox"
            bind:checked={form.aktif}
            class="sr-only peer"
          />

          <div class="w-11 h-6 bg-gray-300 rounded-full transition peer-checked:bg-[#4f79c7]"></div>

          <div class="absolute left-1 top-1 w-4 h-4 bg-white rounded-full transition peer-checked:translate-x-5"></div>
        </label>
      </div>

    </div>

    <!-- FOOTER -->
    <div class="flex justify-between items-center mt-10 border-t pt-6">

      <button
        on:click={reset}
        class="px-4 py-2 text-sm bg-gray-100 rounded hover:bg-gray-200"
      >
        ⟲ Reset
      </button>

      <button
        on:click={submit}
        class="px-5 py-2 text-sm bg-[#4f79c7] text-white rounded shadow"
      >
        Kirimkan
      </button>

    </div>

  </div>

</div>