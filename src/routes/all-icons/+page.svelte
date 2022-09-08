<script lang="ts">
	import { onMount } from 'svelte';
	import Icon from '$lib/Icon.svelte';
	import LazyLoad from './LazyLoad.svelte';
	import { mdiMagnify } from '@mdi/js';

	let page = 1;

	let mdiIcons: [string, string][] = [];
	let searchQuery = '';
	$: searchedIcons = mdiIcons.filter(([name]) =>
		name.toLowerCase().includes(searchQuery.toLowerCase())
	);

	function handleChange() {
		page = 1;
	}

	onMount(async () => {
		const mdi = await import('@mdi/js');

		const entries = Object.entries(mdi) as [string, string][];

		mdiIcons = entries;
	});
</script>

<div class="control has-icons-right">
	<input
		class="input is-focused"
		type="text"
		placeholder="Search icon"
		bind:value={searchQuery}
		on:keyup={handleChange}
	/>
	<span class="icon is-right">
		<Icon path={mdiMagnify} size={1.5} />
	</span>
</div>

<div class="icons">
	<LazyLoad items={searchedIcons} bind:page let:item={[name, path]} let:index>
		<!-- {index % 100 == 0  console.timeLog('mdi', index) } -->
		<div class="iconItem">
			<Icon {path} size={2.5} />
			<p>{index}: {name}</p>
		</div>
	</LazyLoad>
</div>

<style>
	.icons {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(10rem, 1fr));
		grid-gap: 1rem;
		padding: 1rem;
		max-width: 70rem;
		margin: 1rem auto;
	}

	.iconItem {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.5rem;
		font-size: 0.8rem;
		background: rgb(235, 238, 246);
		padding: 1rem 0.5rem;
		border-radius: 0.5rem;
		scale: 1;
		transition: all 0.2s;
		cursor: pointer;
	}

	.iconItem > p {
		text-align: center;
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.iconItem:hover {
		background: rgb(241, 243, 247);
		scale: 1.1;
	}

	/* Search field */
	.control {
		max-width: 70rem;
		margin: 1rem auto;
	}
</style>
