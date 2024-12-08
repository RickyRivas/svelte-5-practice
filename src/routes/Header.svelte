<script>
	let { name } = $props();
	import { createState, RickyState } from './state.svelte';

	const myState = createState();
	const myStateTwo = new RickyState();
</script>

<h1>{name ? name : 'User'}'s Form</h1>
<button onclick={myState.up}>{myState.value}</button>
<button
	onclick={() => {
		myStateTwo.up();
	}}>{myStateTwo.value}</button
>
