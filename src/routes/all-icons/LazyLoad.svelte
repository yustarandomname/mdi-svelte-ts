<script lang="ts">
	type T = $$Generic;

	export let items: T[] = [];
	export let page = 1;
	export let size = 100;

	$: pageItems = page ? items.slice(0, size * page) : [];

	function handleScroll() {
		if (pageItems.length >= items.length) return;

		const { scrollTop, scrollHeight, clientHeight } = document.documentElement;

		// Add page when scroll bottom is nearly reached
		if (scrollTop + clientHeight >= scrollHeight) {
			page += 1;
		}
	}
</script>

<div class="pageCount">
	{page}: {pageItems.length}/{items.length} loaded
</div>

<svelte:window on:scroll={handleScroll} />

{#each pageItems as item, index}
	<slot {item} {index} />
{:else}
	<slot name="not-found">
		<p>Nothing found</p>
	</slot>
{/each}

{#if pageItems.length < items.length}
	<button on:click={() => page++}>Load more</button>
{/if}

<style>
	.pageCount {
		position: fixed;
		bottom: 1rem;
		right: 1rem;
		background: black;
		padding: 1rem;
		color: white;
		z-index: 100;
		border-radius: 0.5rem;
	}
</style>
