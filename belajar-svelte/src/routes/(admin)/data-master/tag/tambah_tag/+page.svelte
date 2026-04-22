<script lang="ts">
  import { goto } from '$app/navigation';
  import { Save, RotateCcw, Pencil, List } from "lucide-svelte";

  let tag = "";

  // ✅ GENERATE ID ANGKA (10 - 99, TIDAK DUPLIKAT)
  function generateId(existingData: { id: number }[]) {
    let newId: number;

    do {
      newId = Math.floor(10 + Math.random() * 90);
    } while (existingData.some(item => item.id === newId));

    return newId;
  }

  function handleSubmit() {
    if (!tag.trim()) {
      alert("Tag tidak boleh kosong!");
      return;
    }

    const oldData = JSON.parse(localStorage.getItem("tags") || "[]");

    const newData = [
      ...oldData,
      {
        id: generateId(oldData),
        tag: tag,
        createdAt: new Date().toLocaleString(),
        updatedAt: new Date().toLocaleString()
      }
    ];

    localStorage.setItem("tags", JSON.stringify(newData));

    tag = "";
    goto('/data-master/tag');
  }

  function handleReset() {
    tag = "";
  }
</script>

<div class="p-6 bg-gray-100 min-h-screen">

  <div class="bg-white rounded-xl shadow-sm">

    <!-- HEADER -->
    <div class="flex justify-between items-center border-b px-6 py-4">
      <h1 class="text-lg font-semibold text-gray-700">
        Tag <span class="text-gray-400 font-normal">Buat</span>
      </h1>

      <div class="text-sm text-gray-400">
        Beranda / Master Data / Tag / <span class="text-gray-600">Buat</span>
      </div>
    </div>

    <div class="p-6">

      <!-- TOP ACTION -->
      <div class="flex justify-between items-center mb-6">
        <h2 class="text-gray-700 font-medium">Buat</h2>

        <button
          on:click={() => goto('/data-master/tag')}
          class="flex items-center gap-2 px-4 py-2 bg-[#4f79c7] text-white rounded-md shadow-sm"
        >
          <List size={16} />
          Daftar
        </button>
      </div>

      <!-- FORM -->
      <form on:submit|preventDefault={handleSubmit}>

        <div class="flex items-center gap-4 mb-8">
          <label class="w-24 text-gray-600">Tag</label>

          <div class="flex items-center border border-gray-200 rounded-md w-full overflow-hidden">
            <div class="px-3 bg-gray-50 border-r">
              <Pencil size={16} class="text-gray-500" />
            </div>

            <input
              type="text"
              bind:value={tag}
              placeholder="Masukkan tag"
              class="w-full px-3 py-2 outline-none"
            />
          </div>
        </div>

        <!-- BUTTON -->
        <div class="flex justify-between items-center border-t pt-4">

          <button
            type="button"
            on:click={handleReset}
            class="flex items-center gap-2 px-4 py-2 border border-gray-200 rounded-md bg-white shadow-sm"
          >
            <RotateCcw size={16} />
            Reset
          </button>

          <button
            type="submit"
            class="flex items-center gap-2 px-4 py-2 bg-[#4f79c7] text-white rounded-md shadow-sm"
          >
            <Save size={16} />
            Kirimkan
          </button>

        </div>

      </form>

    </div>
  </div>
</div>