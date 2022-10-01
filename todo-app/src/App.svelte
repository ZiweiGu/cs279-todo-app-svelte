<!-- adapted from https://eugenkiss.github.io/7guis/tasks#crud -->

<script>
  // Pre-define some todos
	let tasks = [
		{ title: 'cook dinner' },
		{ title: 'walk the dog' },
		{ title: 'wash dishes' }
	];

	let title = ''; // title of a todo
	let i = 0; // pointer to the selected todo

	$: selected = tasks[i]; // $: marks this statement as reactive (will update itself when i gets a new assigment)

	$: reset_inputs(selected); // $: marks this statement as reactive, will execute when selected gets a new assignment

	function create() {
		tasks = tasks.concat({ title });
		i = tasks.length - 1; // move pointer to the newly created todo
		title = '';
	}

	function update() {
		selected.title = title; // update todo title
		tasks = tasks; // re-render todos, with the updated title
	}

	function remove() {
		// Remove selected task from the source array (tasks)
		const index = tasks.indexOf(selected);
		tasks = [...tasks.slice(0, index), ...tasks.slice(index + 1)];
		title = '';
		i = Math.min(i, tasks.length - 2); // move pointer to either the next item, or the previous item if last todo is deleted
	}

	function reset_inputs(task) {
		title = task ? task.title : ''; // make sure the update field reflects currently selected todo
	}
</script>

<h1>CS 279 Todo App - Built with Svelte</h1>

<!-- List all the todos in a select element -->
<select bind:value={i} size={5}>
	{#each tasks as task, i}
		<option value={i}>{task.title}</option>
	{/each}
</select>

<!-- an input for todo titles -->
<label><input bind:value={title} placeholder="title"></label>

<div class='buttons'>
	<button on:click={create} disabled="{!title}">create</button> <!-- not allowing creating a todo if title is empty -->
	<button on:click={update} disabled="{!title || !selected}">update</button> <!-- not allowing updating if title is empty or no todo is selected -->
	<button on:click={remove} disabled="{!selected}">delete</button> <!-- not allowing deleting if no todo is selected -->
</div>

<style>
	* {
		font-family: inherit;
		font-size: inherit;
	}

	input {
		display: block;
		margin: 0 0 0.5em 0;
	}

	select {
		float: left;
		margin: 0 1em 1em 0;
		width: 14em;
	}

	.buttons {
		clear: both;
	}
</style>