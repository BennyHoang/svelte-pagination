<script>
  import { createEventDispatcher } from "svelte";
  export let totalPages;
  import { currentPages } from "./stores.js";
  const dispatch = createEventDispatcher();

  function changePage(pageNumber) {
    currentPages.set(pageNumber);
    dispatch("showPage");
  }
</script>

<style>
  .active {
    background-color: var(--main-color);
  }
</style>

{#each { length: totalPages } as _, i}
  {#if i === $currentPages}
    <button class="active">{i + 1}</button>
  {:else}
    <button on:click={() => changePage(i)}>{i + 1}</button>
  {/if}
{/each}
