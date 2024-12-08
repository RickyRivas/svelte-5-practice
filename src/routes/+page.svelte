<script>
	import { fly } from 'svelte/transition';
	import Header from './Header.svelte';
	let formState = $state({
		answers: {},
		step: 0,
		error: ''
	});

	// will always run when var being passed is updated
	// init
	// update
	$inspect(formState.step);

	const questions = [
		{
			question: 'What is your name?',
			id: 'name',
			type: 'text'
		},
		{
			question: 'What is your birthday',
			id: 'birthday',
			type: 'date'
		},
		{
			question: 'What is your fav color?',
			id: 'color',
			type: 'color'
		}
	];

	function nextStep(id) {
		if (formState.answers[id]) {
			formState.step += 1;
			formState.error = '';
		} else {
			formState.error = 'Please fill out the field.';
		}
	}

	// will run onMount
	$effect(() => {
		// console.log('mounted!');

		// unmount or destroyed
		// before effect re-reruns
		return () => {
			// console.log('unmounted');
		};
	});

	// will re-rerun when formState.step has changed
	$effect(() => {
		// console.log('formState', formState.step);

		// DONT create state based off other state, in effect
		// use $derived()
		return () => {
			// console.log('before formState re-runs', formState.step);
		};
	});
</script>

<Header name={formState.answers.name} />

<main>
	{#if formState.step >= questions.length}
		<p>Thank you!</p>
	{:else}
		<p>You're on step {formState.step + 1}</p>
	{/if}

	{#each questions as { question, id, type }, index (id)}
		{#if formState.step === index}
			<div
				in:fly={{ x: 200, duration: 200, opacity: 0, delay: 200 }}
				out:fly={{ x: -200, duration: 200, opacity: 0 }}
			>
				{@render formStep({ question, id, type })}
			</div>
		{/if}
	{/each}

	{#if formState.error}
		<p class="error">Error! {formState.error}</p>
	{/if}
</main>

{#snippet formStep({ question, id, type })}
	<article>
		<label for={id}>{question}</label>
		<input {type} {id} bind:value={formState.answers[id]} />
		<button
			onclick={() => {
				nextStep(id);
			}}>Next Step</button
		>
	</article>
{/snippet}

<style>
	.error {
		color: red;
	}
</style>
