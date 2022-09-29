<script lang="ts">
  import { dataset_dev } from 'svelte/internal';

  async function getCharacters(name = '', status = '', page = 1) {
    const characters = await fetch(
      `https://rickandmortyapi.com/api/character?name=${name}&status=${status}&page=${page}`
    ).then((res) => res.json());
    return characters;
  }
  let searchFilter = '';
  let statusFilter = '';
  let page = 1;
  let characters;
  $: characters = getCharacters(searchFilter, statusFilter, page);

  const handleChange = () => {
    page = 1;
  };

  const nextPage = () => {
    page++;
  };

  const prevPage = () => {
    page--;
  };
</script>

<style>
  .Alive {
    color: green;
  }

  .Dead {
    color: red;
  }
</style>

<main>
  <h2>Characters</h2>
  <input
    placeholder="Filter Database"
    bind:value={searchFilter}
    name="name-search"
    on:input={handleChange} />
  <select name="status" id="status" bind:value={statusFilter} on:change={handleChange}>
    <option value="">All</option>
    <option value="alive">Alive</option>
    <option value="dead">Dead</option>
    <option value="unknown">Unknown</option>
  </select>
  <br />
  Currently Searching: {searchFilter} {statusFilter}
  {#await characters}
    Loading
  {:then data}
    {#each data.results as c}
      <li class={c.status}>{c.name}</li>
    {/each}
    Current Page: {page}
    <br />
    <br />
    {#if data.info.prev}
      <button on:click={prevPage}>Previous Page</button>
    {/if}
    {#if data.info.next}
      <button on:click={nextPage}>Next Page</button>
    {/if}
  {/await}

</main>
