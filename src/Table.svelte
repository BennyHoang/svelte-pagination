<script>
  export let tableData;

  function convertTimeStamp(unixTimestamp) {
    const date = new Date(unixTimestamp * 1000);
    return date.toLocaleDateString();
  }
</script>

<style>
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
    background-color: var(--main-color);
    color: white;
  }

  thead tr th {
    padding: 1em;
    width: 12.5%;
  }
  table tr:nth-child(even) {
    background-color: #f2f2f2;
  }
  table tbody tr:hover {
    background-color: #ddd;
  }
</style>

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
