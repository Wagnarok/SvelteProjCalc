<script>
  import { prevent_default } from 'svelte/internal';
  import materialStore from './material-store.js';
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  let materials = [];
  $: totalCost = materials.reduce((prev, next) => {
    prev += next.price;
    return prev;
  }, 0);

  materialStore.subscribe((items) => {
    materials = items;
  });

  const formatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
  });

  function edit(id, name, price) {
    dispatch('edit', { id, name, price });
  }
</script>

<style>
  table {
    width: 100%;
  }
  tr {
    cursor: pointer;
  }
</style>

<table class="primary">
  <thead>
    <tr>
      <th>Material</th>
      <th>Price</th>
      <th />
    </tr>
  </thead>
  <tbody>
    {#each materials as material (material.id)}
      <tr on:click={edit(material.id, material.name, material.price)}>
        <td>{material.name}</td>
        <td>{formatter.format(material.price)}</td>
        <td><i class="far fa-trash-alt" /></td>
      </tr>
    {/each}
    <tr>
      <td>Total</td>
      <td colspan="2">{formatter.format(totalCost)}</td>
    </tr>
  </tbody>
</table>
