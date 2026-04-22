<script lang="ts">
  import { goto } from '$app/navigation';

  let nama = "";
  let aktif = true;

  function simpan() {
    if (!nama.trim()) {
      alert("Nama kategori tidak boleh kosong!");
      return;
    }

    let oldData = JSON.parse(localStorage.getItem("kategori") || "[]");

    // bersihin data kalau ada yang aneh
    oldData = oldData.filter((item: any) => typeof item.id === "number");

    // id random biar aman
    const newId = Math.floor(Math.random() * 1000) + 1;

    const newData = [
      {
        id: newId,
        nama,
        aktif
      },
      ...oldData
    ];

    localStorage.setItem("kategori", JSON.stringify(newData));

    goto('/data-master/kategori');
  }
</script>

<div class="p-6 bg-[#f5f7fb] min-h-screen">

  <div class="bg-white rounded-2xl shadow-sm p-8 w-full">

    <!-- HEADER -->
    <div class="flex justify-between items-center mb-10">
      <h1 class="text-xl font-semibold text-gray-800">
        Kategori <span class="text-gray-400 font-normal">Buat</span>
      </h1>

      <div class="text-sm text-gray-400">
        Beranda / Master Data / <span class="text-gray-600">Kategori</span>
      </div>
    </div>

    <!-- FORM -->
    <div class="w-full ml-6"></div>

      <form on:submit|preventDefault={simpan}>

        <!-- INPUT -->
        <div class="mb-7">
          <label class="block text-sm text-gray-600 mb-2">
            Nama Kategori
          </label>

          <input
            type="text"
            bind:value={nama}
            placeholder="Masukkan nama kategori"
            class="w-full border border-gray-200 px-4 py-3 rounded-xl
                   focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-400 transition"
          />
        </div>

        <!-- AKTIF -->
        <div class="mb-10">
          <label class="flex items-center gap-3 text-sm text-gray-600">
            <input
              type="checkbox"
              bind:checked={aktif}
              class="w-4 h-4 bg-[#4f79c7]"
            />
            Aktif
          </label>
        </div>

        <!-- BUTTON -->
        <div class="border-t border-gray-100 pt-6 flex justify-end gap-3">

          <button
            type="button"
            on:click={() => goto('/data-master/kategori')}
            class="px-5 py-2.5 border border-gray-200 text-gray-600 rounded-lg hover:bg-gray-50"
          >
            Batal
          </button>

          <button
            type="submit"
            class="px-6 py-2.5 bg-[#4f79c7] text-white rounded-lg hover:bg-blue-700"
          >
            Simpan
          </button>

        </div>

      </form>

    </div>

  </div>