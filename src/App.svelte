<script>
	import { setContext, afterUpdate } from 'svelte'

	import Navbar from './Navbar.svelte'
	import ExpenseList from './ExpensesList.svelte'
	import Totals from './Totals.svelte'
	import ExpenseForm from './ExpenseForm.svelte'
	import Modal from './Modal.svelte'

	import expensesData from './expenses'

	let expenses = JSON.parse(localStorage.getItem('expenses')) || []
	$: total = expenses.reduce((acc, { amount }) => acc + amount, 0)

	let setName = ''
	let setAmount = null
	let setId = null
	$: isEditing = Boolean(setId)

	let isFormOpen = false

	const setLocalStorage = () => {
		localStorage.setItem('expenses', JSON.stringify(expenses))
	}

	const clearEditFields = () => {
		setName = ''
		setAmount = null
		setId = null
	}

	const showForm = () => isFormOpen = true
	const hideForm = () => {
		isFormOpen = false
		clearEditFields()
	}

	const removeExpense = id => {
		expenses = expenses.filter(expense => expense.id !== id)
	}

	const clearExpenses = () => expenses = []
	
	const addExpense = ({ name, amount }) => {
		const expense = { id: Math.random() * Date.now(), name, amount }
		expenses = [expense, ...expenses]
	}

	const setModifiedExpense = (id) => {
		let expense = expenses.find(expense => expense.id === id)
		setId = expense.id
		setName = expense.name
		setAmount = expense.amount
		showForm()
	}

	const editExpense = ({ name, amount }) => {
		const modifiedExpense = { id: setId, name, amount }
		expenses =  expenses.map(expense => {
			return (expense.id === modifiedExpense.id ? modifiedExpense : expense)
		})
		clearEditFields()
	}

	setContext('remove', removeExpense)
	setContext('add', addExpense)
	setContext('modify', setModifiedExpense)
	setContext('edit', editExpense)
	setContext('hide', hideForm)

	afterUpdate(() => {
		setLocalStorage()
	})
</script>

<Navbar {showForm} />
<main class="content">
	{#if isFormOpen}
		<Modal>
			<ExpenseForm {isEditing} name={setName} amount={setAmount} />
		</Modal>
	{/if}

	<Totals title="Total expenses" {total} />

	<ExpenseList {expenses} />
	<button 
		type="button"
		class="btn btn-primary btn-block"
		on:click={clearExpenses}>
		Clear expenses
	</button>
</main>