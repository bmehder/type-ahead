<script>
  const endpoint =
    'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json'
  const formatNumber = new Intl.NumberFormat('en-US')

  let inputEl = null
  let value = ''
  let allCities = null
  let filteredPlaces = []

  $: inputEl?.focus()

  const handlePress = () => {
    filteredPlaces = allCities.filter(place => {
      // return (
      //   place.city.toLowerCase().startsWith(value.toLowerCase()) ||
      //   place.state.toLowerCase().startsWith(value.toLowerCase())
      // )
      return place.city.toLowerCase().startsWith(value.toLowerCase())
    })
  }

  const getData = async (node, endpoint) => {
    const res = await fetch(endpoint)
    const data = await res.json()

    allCities = data
  }
</script>

<form use:getData={endpoint}>
  <input
    bind:this={inputEl}
    bind:value
    on:keyup={handlePress}
    placeholder="Search City"
  />
  <ul>
    {#if value !== ''}
      {#each filteredPlaces as { city, state, population, rank }}
        <li>
          <span
            >{@html `<span class="highlight">${value}</span>` +
              city.toLowerCase().replace(value, '')}, {state}</span
          >
          <span>{formatNumber.format(population)}</span>
          <span>Rank: {rank}</span>
        </li>
      {/each}
    {/if}
  </ul>
</form>

<style>
  form {
    max-width: 400px;
    margin: 50px auto;
  }
  input {
    margin: 0;
    padding: 20px;
    text-align: center;
    outline: 0;
    border: 10px solid #f7f7f7;
    width: 120%;
    left: -10%;
    position: relative;
    top: 10px;
    z-index: 2;
    border-radius: 5px;
    font-size: 40px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.12), inset 0 0 2px rgba(0, 0, 0, 0.19);
  }
  li {
    background: white;
    list-style: none;
    border-bottom: 1px solid #d8d8d8;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.14);
    margin: 0;
    padding: 20px;
    transition: background 0.2s;
    display: flex;
    /* justify-content: space-between; */
    text-transform: uppercase;
  }
  li:nth-child(even) {
    transform: perspective(100px) rotateX(3deg) translateY(2px) scale(1.001);
    background: linear-gradient(to bottom, #ffffff 0%, #efefef 100%);
  }
  li:nth-child(odd) {
    transform: perspective(100px) rotateX(-3deg) translateY(3px);
    background: linear-gradient(to top, #ffffff 0%, #efefef 100%);
  }
  span {
    flex: 1;
  }
  span:first-child {
    flex: 3;
  }
  span:nth-child(2),
  span:nth-child(3) {
    font-size: 15px;
  }
  :global(.highlight) {
    background: #ffc600;
  }
</style>
