<script>
  import { fly } from 'svelte/transition'
  import { flip } from 'svelte/animate'
  import Title from './Title.svelte'
  import Expense from './Expense.svelte'

  export let expenses = []
</script>

<section>
  <Title title="Expense List" />
  <!-- The (key) is important for Svelte to know which element to transition -->
  <!-- Animate is used for the elements that are already in the DOM -->
  <!-- Transition is used for elements that are created or removed from the DOM -->

  <!-- Also, animate can only be used with direct childs of EACH BLOCKS -->
  {#each expenses as expense, index (expense.id)}
    <div transition:fly={{ x: 100 }} animate:flip>
      <Expense {...expense}/>
    </div>
  {:else}
    <h2>No expenses added to the list</h2>
  {/each}
</section>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>