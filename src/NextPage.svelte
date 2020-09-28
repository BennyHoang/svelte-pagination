<script>
  import { currentPages } from "./stores.js";
  import { onDestroy } from "svelte";
  import { createEventDispatcher } from "svelte";
  let page_value;
  export let totalPages;
  const dispatch = createEventDispatcher();

  const unsubscribe = currentPages.subscribe(value => {
    page_value = value;
  });
  function nextPage() {
    if (page_value < totalPages - 1) {
      currentPages.update(n => n + 1);
      dispatch('showPage');
    }
  }
  onDestroy(unsubscribe);
</script>

<button on:click={nextPage}>next</button>
