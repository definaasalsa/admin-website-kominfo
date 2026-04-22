<script lang="ts">
  export let headers: string[] = [];
</script>

<div class="bg-white rounded-xl shadow border overflow-hidden">
  <table class="w-full text-sm">

    <thead class="bg-gray-50">
      <tr>
        {#each headers as head}
          <th class="p-4 text-left font-semibold text-gray-600">
            {head}
          </th>
        {/each}
      </tr>
    </thead>

    <tbody>
      <slot />
    </tbody>

  </table>
</div>