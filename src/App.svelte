<script>
  import { onMount } from "svelte";
  import PreviousPage from "./PreviousPage.svelte";
  import NextPage from "./NextPage.svelte";
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
  function convertTimeStamp(unixTimestamp) {
    const date = new Date(unixTimestamp * 1000);
    return date.toLocaleDateString();
  }
  function changePage(pageNumber) {
    currentPages.set(pageNumber);
    showPage();
  }
  function showPage() {
    console.log("showing page:");
    startPosition = currentPage_value * numberPerPage;
    endPosition = startPosition + numberPerPage;

    tableData = bitcoinData.filter(
      (data, i) => i >= startPosition && i < endPosition
    );
  }
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  table {
    margin: auto;
    width: 70%;
    border-collapse: collapse;
  }

  table td,
  table th {
    border: 1px solid #ddd;
  }
  thead {
    background-color: green;
    color: white;
  }

  thead tr th {
    padding: 1%;
    width: 12.5%;
  }

  table tr:nth-child(even) {
    background-color: #f2f2f2;
  }
  table tbody tr:hover {
    background-color: #ddd;
  }

  .active {
    background-color: blue;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h2>Bitcoin Data table</h2>
  <table>
    <thead>
      <tr>
        <th>Time</th>
        <th>High</th>
        <th>Low</th>
        <th>Open</th>
        <th>VolumeFrom</th>
        <th>VolumeTo</th>
        <th>Close</th>
        <th>ConversionType</th>
      </tr>
    </thead>
    <tbody id="data">
      {#each tableData as data}
        <tr>
          <td>
            {@html convertTimeStamp(data.time)}
          </td>
          <td>{data.high}</td>
          <td>{data.low}</td>
          <td>{data.open}</td>
          <td>{data.volumefrom}</td>
          <td>{data.volumeto}</td>
          <td>{data.close}</td>
          <td>{data.conversionType}</td>
        </tr>
      {:else}
        <p>loading...</p>
      {/each}

    </tbody>
  </table>
  <PreviousPage on:showPage={() => showPage()} />
  {#each { length: totalPages } as _, i}
    {#if i === $currentPages}
      <button class="active">{i + 1}</button>
    {:else}
      <button on:click={() => changePage(i)}>{i + 1}</button>
    {/if}
  {/each}
  <NextPage on:showPage={() => showPage()} {totalPages} />

</main>
