<script>
  import { currentPages } from "./stores.js";
  import { onDestroy } from "svelte";
  let page_value;
  export let totalPages;

  const unsubscribe = currentPages.subscribe(value => {
    page_value = value;
  });
  function prevPage() {
    if (page_value >= 1) {
      currentPages.update(n => n - 1);
    }
  }

  onDestroy(unsubscribe);
</script>

<button on:click={prevPage}>prev (component)</button>
