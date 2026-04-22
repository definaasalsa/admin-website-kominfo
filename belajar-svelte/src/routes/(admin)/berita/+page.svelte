<script lang="ts">
  import {
    Table,
    TableHeader,
    TableBody,
    TableRow,
    TableHead,
    TableCell
  } from "$lib/components/ui/table";

  let judul = "";
  let kategori = "";
  let status = "Dipublikasikan";
  let previewGambar = "";
  let search = "";
  let editingIndex: number | null = null;
  let showForm = false;

  const kategoriOptions = ["UMUM", "PEMERINTAH", "TEKNOLOGI", "PENDIDIKAN"];
  const statusOptions = ["Draft", "Dipublikasikan"];

  let beritaList: any[] = [];

  function uploadGambar(e: any) {
    const file = e.target.files[0];
    if (file) {
      previewGambar = URL.createObjectURL(file);
    }
  }

  function tambahAtauEdit() {

    if (editingIndex !== null) {
      beritaList[editingIndex] = {
        ...beritaList[editingIndex],
        judul,
        kategori,
        status,
        gambar: previewGambar
      };
      editingIndex = null;
    } else {
      beritaList = [
        ...beritaList,
        {
          gambar: previewGambar,
          judul,
          kategori,
          tanggal: new Date().toLocaleString(),
          status
        }
      ];
    }

    judul = "";
    kategori = "umum";
    status = "Dipublikasikan";
    previewGambar = "";
    showForm = false;
  }

  function editBerita(index: number) {
    const data = beritaList[index];
    judul = data.judul;
    kategori = data.kategori;
    status = data.status;
    previewGambar = data.gambar;
    editingIndex = index;
    showForm = true;
  }

  function hapusBerita(index: number) {
    beritaList = beritaList.filter((_, i) => i !== index);
  }

  function segarkan() {
    search = "";
  }

  $: filteredBerita = beritaList.filter((b) =>
    b.judul.toLowerCase().includes(search.toLowerCase())
  );
</script>

<div class="space-y-6">

  <div class="flex justify-between items-center">
    <h1 class="text-2xl font-bold">
      <span>Berita</span>
      <span class="text-muted-foreground font-normal">Daftar</span>
    </h1>

    <div class="flex gap-2">
      <button on:click={segarkan} class="border px-4 py-2 rounded-md text-sm">
        <span>Segarkan</span>
      </button>

      <button
        on:click={() => {
          showForm = true;
          editingIndex = null;
          judul = "";
          kategori = "UMUM";
          status = "";
          previewGambar = "";
        }}
        class="bg-blue-600 text-white px-4 py-2 rounded-md text-sm"
      >
        <span>+ Baru</span>
      </button>
    </div>
  </div>

  {#if showForm}
  <div class="bg-white p-4 rounded-lg border shadow-sm space-y-4">

    <input
      type="text"
      placeholder="Judul berita"
      bind:value={judul}
      class="border rounded-md px-3 py-2 w-full"
    />

    <select
      bind:value={kategori}
      class="border rounded-md px-3 py-2 w-full"
    >
      <option value="">Pilih Kategori</option>
      {#each kategoriOptions as option}
        <option value={option}>{option}</option>
      {/each}
    </select>

    <select
      bind:value={status}
      class="border rounded-md px-3 py-2 w-full"
    >
      {#each statusOptions as option}
        <option value={option}>{option}</option>
      {/each}
    </select>

    <input
      type="file"
      accept="image/*"
      on:change={uploadGambar}
      class="border rounded-md px-3 py-2 w-full"
    />

    {#if previewGambar}
      <img
        src={previewGambar}
        alt="preview gambar berita"
        class="w-24 h-24 rounded object-cover"
      />
    {/if}

    <div class="flex gap-2">
      <button
        on:click={tambahAtauEdit}
        class="bg-green-600 text-white px-4 py-2 rounded-md text-sm"
      >
        {editingIndex !== null ? "Update" : "Simpan"}
      </button>

      <button
        on:click={() => (showForm = false)}
        class="border px-4 py-2 rounded-md text-sm"
      >
        <span>Batal</span>
      </button>
    </div>

  </div>
  {/if}

  <input
    type="text"
    placeholder="Pencarian..."
    bind:value={search}
    class="border rounded-md px-3 py-2 w-64"
  />

  <div class="rounded-xl border bg-white shadow-sm overflow-hidden">

    <Table>

      <TableHeader>
        <TableRow>
          <TableHead>Gambar</TableHead>
          <TableHead>Judul</TableHead>
          <TableHead>Kategori</TableHead>
          <TableHead>Tanggal</TableHead>
          <TableHead>Status</TableHead>
          <TableHead>Aksi</TableHead>
        </TableRow>
      </TableHeader>

      <TableBody>

        {#if filteredBerita.length === 0}
          <TableRow>
            <TableCell colspan={6} class="text-center py-6 text-muted-foreground">
              Belum ada data
            </TableCell>
          </TableRow>
        {:else}

        {#each filteredBerita as berita, index}

        <TableRow>

          <TableCell>
            {#if berita.gambar}
              <img
                src={berita.gambar}
                alt={berita.judul}
                class="w-20 h-20 rounded-md object-cover"
              />
            {/if}
          </TableCell>

          <TableCell class="font-medium">
            {berita.judul}
          </TableCell>

          <TableCell>
            <span class="bg-blue-100 text-blue-700 px-2 py-1 rounded text-xs">
              {berita.kategori}
            </span>
          </TableCell>

          <TableCell class="text-sm text-muted-foreground">
            {berita.tanggal}
          </TableCell>

          <TableCell>
            <span class="px-2 py-1 rounded text-xs
              {berita.status === 'Draft'
                ? 'bg-yellow-100 text-yellow-700'
                : 'bg-green-100 text-green-700'}">
              {berita.status}
            </span>
          </TableCell>

          <TableCell>
            <div class="flex gap-2">
              <button
                on:click={() => editBerita(index)}
                class="bg-indigo-500 text-white px-3 py-1 rounded text-xs"
              >
                Edit
              </button>

              <button
                on:click={() => hapusBerita(index)}
                class="bg-red-500 text-white px-3 py-1 rounded text-xs"
              >
                Hapus
              </button>
            </div>
          </TableCell>

        </TableRow>

        {/each}
        {/if}

      </TableBody>

    </Table>

  </div>

</div>