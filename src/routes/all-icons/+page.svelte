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

	function writeToClipboard(name: string) {
		const str = `<Icon path={${name}} />`;
		navigator.clipboard.writeText(str);
	}

	onMount(async () => {
		const mdi = await import('@mdi/js');

		mdiIcons = Object.entries(mdi) as [string, string][];
	});
</script>

<div class="container mx-auto p-4 my-12 max-w-6xl">
	<Input bind:value={searchQuery} on:keyup={() => (page = 1)} />

	<div class="icons grid gap-4 my-4">
		<LazyLoad items={searchedIcons} bind:page let:item={[name, path]} let:index>
			<div
				on:click={() => writeToClipboard(name)}
				class="flex flex-col items-center gap-2 text-sm bg-slate-100 hover:bg-slate-50 px-2 py-4 rounded transition-colors cursor-pointer truncate"
			>
				<Icon {path} size={2.5} />
				<p class="w-full py-2 text-center">{index}: {name}</p>
			</div>
		</LazyLoad>
	</div>
</div>

<style>
	.icons {
		grid-template-columns: repeat(auto-fill, minmax(10rem, 1fr));
	}
</style>
