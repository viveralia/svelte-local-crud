<script>
  import { getContext } from 'svelte'
  import Title from './Title.svelte'
  export let name = ''
  export let amount = null
  export let isEditing = false

  $: isEmpty = !name || !amount

  const add = getContext('add')
  const edit = getContext('edit')
  const hideForm = getContext('hide')

  const resetForm = () => {
    name = ''
    amount = null
  }

  const handleSubmit = () => {
    if (isEditing) {
      edit({ name, amount: Number(amount) })
    } else {
      add({ name, amount: Number(amount) })
    }
    resetForm()
    hideForm()
  }
</script>

<section class="form">
  <Title title={isEditing ? 'Edit expense' : 'Add expense'} />

  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={name}>
    </div>

    <div class="form-control">
      <label for="amount">Amount</label>
      <input type="number" id="amount" bind:value={amount}>
    </div>

    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}

    <!-- Will add the class disabled only if isEmpty is true -->
    <button 
      type="submit" 
      class="btn btn-block"
      class:disabled={isEmpty} 
      disabled={isEmpty}>
        {isEditing ? 'Edit expense' : 'Add expense'}
      </button>

    <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>