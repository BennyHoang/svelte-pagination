<script>
  import { currentPages } from './stores.js';
  import { onDestroy } from 'svelte';
  import {createEventDispatcher} from 'svelte';
  let page_value;
  const dispatch = createEventDispatcher();

  const unsubscribe = currentPages.subscribe(value => {
    page_value = value;
  });
  function prevPage() {
    if (page_value >= 1) {
      currentPages.update(n => n - 1);
      dispatch('showPage');
    }
  }

  onDestroy(unsubscribe);
</script>

<button on:click={prevPage}>prev (component)</button>
