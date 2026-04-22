<script lang="ts">
  export let className: string = "";
</script>

<div class={`rounded-xl border p-4 shadow-sm bg-white ${className}`}>
  <slot />
</div>