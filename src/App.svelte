<script>
  import { onMount } from "svelte";
  import PreviousPage from "./PreviousPage.svelte";
  import NextPage from "./NextPage.svelte";
  import PageControl from "./PageControl.svelte";
  import Table from "./Table.svelte";
  import { currentPages } from "./stores.js";
  let bitcoinData = [];
  let startPosition = 0;
  let numberPerPage = 20;
  let endPosition = 20;
  const apiUrl = `https://min-api.cryptocompare.com/data/v2/histoday?fsym=BTC&tsym=USD&limit=100&api_key=8ae55d463e1bf8d38b4a502ca47512f9b1dec21533ad9af7acb993e8ba952bc`;
  export let tableData = [];
  let currentPage_value;

  $: totalPages = Math.round(bitcoinData.length / 20);
  const unsubscribeCurrentPage = currentPages.subscribe(value => {
    currentPage_value = value;
  });

  onMount(async () => {
    let response = await fetch(apiUrl);
    let data = await response.json();
    bitcoinData = data.Data.Data.reverse();
    showPage();
  });

  function showPage() {
    startPosition = currentPage_value * numberPerPage;
    endPosition = startPosition + numberPerPage;

    tableData = bitcoinData.filter(
      (data, i) => i >= startPosition && i < endPosition
    );
  }
</script>

<style>
  :root {
    --main-color: #1db954;
  }
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
  .pagination-controls-wrapper {
    margin-top: 1.5em;
  }
  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h2>Bitcoin Data table</h2>
  <Table tableData={tableData}></Table>
  <div class="pagination-controls-wrapper">
    <PreviousPage on:showPage={() => showPage()} />
    <PageControl on:showPage={()=> showPage()} totalPages={totalPages}></PageControl> 
    <NextPage on:showPage={() => showPage()} totalPages={totalPages} />
  </div>

</main>
