<script>
  export let bitcoinData = [];
  async function getBitcoinData() {
    let response = await fetch(
      `https://min-api.cryptocompare.com/data/v2/histoday?fsym=BTC&tsym=USD&limit=100&api_key=8ae55d463e1bf8d38b4a502ca47512f9b1dec21533ad9af7acb993e8ba952bc`
    );
	let data = await response.json();
	bitcoinData = data.Data.Data;
    return data.Data.Data;
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

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Hello {name}!</h1>
  <button on:click={getBitcoinData}>Click me</button>
  <p>
    Visit the
    <a href="https://svelte.dev/tutorial">Svelte tutorial</a>
    to learn how to build Svelte apps.
  </p>

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
	<tbody>
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
