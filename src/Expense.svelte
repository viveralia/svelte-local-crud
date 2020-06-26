<script>
  import { slide } from 'svelte/transition'
  import { getContext, createEventDispatcher } from 'svelte'
  export let id
  export let name = ""
  export let amount = 0

  let isDisplayAmount = false

  const toggleAmount = () => {
    isDisplayAmount = !isDisplayAmount
  }

  const removeExpense = getContext('remove')
  const setModifiedExpense = getContext('modify')
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
    {name} 
    <button class="amount-btn" on:click={toggleAmount}>
      <i class="fas fa-caret-down" />
    </button>
    </h2>

    {#if isDisplayAmount}
      <h4 transition:slide>Amount: ${amount}</h4>
    {/if}

  </div>

  <div class="expense-buttons">
    <button class="expense-btn edit-btn" on:click={setModifiedExpense(id)}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>