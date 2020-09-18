<script>
  export let bitcoinData = [];
  let currentPage = 0;
  $: totalPages = Math.round(bitcoinData.length / 20);

  async function getBitcoinData() {
    let response = await fetch(
      `https://min-api.cryptocompare.com/data/v2/histoday?fsym=BTC&tsym=USD&limit=100&api_key=8ae55d463e1bf8d38b4a502ca47512f9b1dec21533ad9af7acb993e8ba952bc`
    );
    let data = await response.json();
	bitcoinData = data.Data.Data;
  }
  function prevPage() {
    if (currentPage >= 1) {
      currentPage--;
      console.log("current page: ", currentPage);
    }
  }
  function nextPage() {
    if (currentPage < totalPages-1) {
      currentPage++;
      console.log("current page: ", currentPage);
      console.log("totalPages: ", totalPages);
    }
  }
  function changePage(pageNumber) {
	  currentPage = pageNumber;
	  console.log("current page: ", pageNumber);
  }
  function showBitcoinData(){

  }
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
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
  <button on:click={getBitcoinData}>Click me</button>
  <button on:click={prevPage}>prev</button>
  {totalPages}
  {#each { length: totalPages } as _, i}
    {#if i === currentPage}
      <button class="active">{i + 1}</button>
    {:else}
      <button on:click={() => changePage(i)}>{i + 1}</button>
    {/if}
  {/each}
  <button on:click={nextPage}>next</button>

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
      {#each bitcoinData as bitcoinData}
        <tr>
          <td>{bitcoinData.time}</td>
          <td>{bitcoinData.high}</td>
          <td>{bitcoinData.low}</td>
          <td>{bitcoinData.open}</td>
          <td>{bitcoinData.volumefrom}</td>
          <td>{bitcoinData.volumeto}</td>
          <td>{bitcoinData.close}</td>
          <td>{bitcoinData.conversionType}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</main>
