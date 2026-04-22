<script lang="ts">
  import { goto } from '$app/navigation';
  import { onMount } from "svelte";

  let id: number;
  let nama = "";
  let aktif = true;

  onMount(() => {
    const data = JSON.parse(localStorage.getItem("edit_kategori") || "{}");

    if (data) {
      id = data.id;
      nama = data.nama;
      aktif = data.aktif;
    }
  });

  function simpan() {
    if (!nama.trim()) {
      alert("Nama kategori tidak boleh kosong!");
      return;
    }

    let semuaData = JSON.parse(localStorage.getItem("kategori") || "[]");

    semuaData = semuaData.map((item: any) =>
      item.id === id
        ? { ...item, nama, aktif }
        : item
    );

    localStorage.setItem("kategori", JSON.stringify(semuaData));

    localStorage.removeItem("edit_kategori");

    goto('/data-master/kategori');
  }
</script>

<div class="p-6 bg-gray-100 min-h-screen">

  <div class="bg-white rounded-xl shadow-sm p-6">

    <h1 class="text-lg font-semibold mb-6">
      Kategori <span class="text-gray-400">Sunting</span>
    </h1>

    <form on:submit|preventDefault={simpan}>

      <div class="mb-4">
        <label class="block text-sm mb-1">Nama Kategori</label>
        <input
          type="text"
          bind:value={nama}
          class="w-full border px-3 py-2 rounded"
        />
      </div>

      <div class="mb-6">
        <label class="flex items-center gap-2 text-sm">
          <input type="checkbox" bind:checked={aktif} />
          Aktif
        </label>
      </div>

      <div class="flex justify-end gap-2">
        <button
          type="button"
          on:click={() => goto('/data-master/kategori')}
          class="px-3 py-2 border rounded"
        >
          Batal
        </button>

        <button
          type="submit"
          class="px-4 py-2 bg-blue-600 text-white rounded"
        >
          Simpan Perubahan
        </button>
      </div>

    </form>

  </div>

</div>