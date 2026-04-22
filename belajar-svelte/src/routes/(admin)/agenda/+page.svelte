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
  let tanggal = "";
  let lokasi = "";
  let status = "Dipublikasikan";
  let showForm = false;

  let agendaList: any[] = [];

  function tambahAgenda() {
    agendaList = [
      ...agendaList,
      {
        judul,
        tanggal,
        lokasi,
        status
      }
    ];

    judul = "";
    tanggal = "";
    lokasi = "";
    status = "Dipublikasikan";
    showForm = false;
  }
</script>

<div class="space-y-8">

  <div class="bg-blue-500 text-white rounded-xl py-10 px-20 shadow text-center">
    <h1 class="text-2xl font-semibold">
      <span>MANAJEMEN AGENDA</span>
    </h1>
  </div>

  <button
    class="bg-blue-500 text-white px-4 py-2 rounded-lg"
    on:click={() => (showForm = !showForm)}
  >
    <span>MANAJEMEN AGENDA</span>
  </button>

  {#if showForm}
  <div class="bg-white p-6 rounded-xl shadow space-y-4">

    <input
      class="border p-2 w-full rounded"
      placeholder="Judul Agenda"
      bind:value={judul}
    />

    <input
      type="date"
      class="border p-2 w-full rounded"
      bind:value={tanggal}
    />

    <input
      class="border p-2 w-full rounded"
      placeholder="Lokasi"
      bind:value={lokasi}
    />

    <select class="border p-2 w-full rounded" bind:value={status}>
      <option>Draft</option>
      <option>Dipublikasikan</option>
    </select>

    <button
      class="bg-green-500 text-white px-4 py-2 rounded"
      on:click={tambahAgenda}
    >
      <span>Simpan</span>
    </button>

  </div>
  {/if}

  <Table>
    <TableHeader>
      <TableRow>
        <TableHead>No</TableHead>
        <TableHead>Judul Agenda</TableHead>
        <TableHead>Tanggal</TableHead>
        <TableHead>Lokasi</TableHead>
        <TableHead>Status</TableHead>
        <TableHead>Aksi</TableHead>
      </TableRow>
    </TableHeader>

    <TableBody>
      {#each agendaList as agenda, i}
      <TableRow>
        <TableCell>{i + 1}</TableCell>
        <TableCell>{agenda.judul}</TableCell>
        <TableCell>{agenda.tanggal}</TableCell>
        <TableCell>{agenda.lokasi}</TableCell>
        <TableCell>{agenda.status}</TableCell>
        <TableCell>
          <button class="text-blue-500">Edit</button>
          <button class="text-red-500 ml-2">Hapus</button>
        </TableCell>
      </TableRow>
      {/each}
    </TableBody>
  </Table>

</div>