<script>
  export let id;
  export let name = '';
  export let price;

  $: mode = id ? 'edit' : 'add';
  $: canSubmit = price >= 0 && name !== '';

  function submit() {
    if (!canSubmit) return;

    price = '';
    name = '';
    id = undefined;
  }

  function cancel() {
    price = '';
    name = '';
    id = undefined;
  }
</script>

<style>
  button {
    margin-left: 20px;
  }
  button:disabled {
    cursor: not-allowed;
  }
</style>

<form on:submit|preventDefault={submit}>
  <fieldset>
    <label for="nameField">Material</label>
    <input
      bind:value={name}
      type="text"
      placeholder="Wood, Glue, Etc"
      id="nameField" />
    <label for="priceField">Cost</label>
    <input
      bind:value={price}
      min="0"
      step="any"
      type="number"
      placeholder="Price"
      id="priceField" />
  </fieldset>
  <button
    disabled={!canSubmit}
    class="float-right"
    type="submit">{mode}</button>
  {#if mode === 'edit'}
    <button on:click={cancel} class="float-right" type="button">CANCEL</button>
  {/if}
</form>
