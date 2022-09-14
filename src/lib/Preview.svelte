<script lang="ts">
	import { onMount } from 'svelte';
	import Icon from '$lib/Icon.svelte';
	import LazyLoad from '$lib/LazyLoad.svelte';
	import Input from '$lib/Input.svelte';

	let page = 1;
	let mdiIcons: [string, string][] = [];
	let searchQuery = '';

	$: searchedIcons = mdiIcons.filter(([name]) =>
		name.toLowerCase().includes(searchQuery.toLowerCase())
	);

	onMount(async () => {
		const mdi = await import('@mdi/js');

		mdiIcons = Object.entries(mdi) as [string, string][];
	});
</script>

<div class="fixed bottom-4 right-4 w-[30rem]">
	<Input bind:value={searchQuery} on:keyup={() => (page = 1)} />

	<div class="icons grid gap-4 max-h-80 my-4 overflow-x-auto">
		<LazyLoad items={searchedIcons} bind:page let:item={[name, path]} let:index>
			<div
				class="flex flex-col items-center gap-2 text-sm bg-slate-100 hover:bg-slate-50 px-2 py-4 rounded transition-colors cursor-pointer"
			>
				<Icon {path} size={2.5} />
				<p>{index}: {name}</p>
			</div>
		</LazyLoad>
	</div>
</div>

<style>
	.icons {
		grid-template-columns: repeat(auto-fill, minmax(10rem, 1fr));
	}
</style>
