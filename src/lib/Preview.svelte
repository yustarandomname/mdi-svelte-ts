<script lang="ts">
	import { onMount } from 'svelte';
	import Fuse from 'fuse.js';

	import Icon from '$lib/Icon.svelte';
	import LazyLoad from '$lib/LazyLoad.svelte';
	import Input from '$lib/Input.svelte';
	import Notification from '$lib/Notification.svelte';
	import { mdiMagnify } from '@mdi/js';

	interface NamePath {
		name: string;
		path: string;
	}

	let page = 1;
	let mdiIcons: NamePath[] = [];
	let searchQuery = '';
	let searchedIcons: Fuse.FuseResult<NamePath>[] = [];

	const fuseOptions = {
		includeScore: true,
		keys: ['name', 'path']
	};

	$: fuse = new Fuse(mdiIcons, fuseOptions);

	function writeToClipboard(name: string) {
		const str = `<Icon path={${name}} />`;
		navigator.clipboard.writeText(str);
	}

	function defaultSearch() {
		const f = new Fuse(mdiIcons, fuseOptions);
		searchedIcons = f.search('mdi');
	}

	function handleKeyup(e: KeyboardEvent) {
		const key = e.key;
		if (key in new Set(['Meta', 'Control', 'Shift', 'Alt', 'Space'])) return;

		page = 1;

		if (searchQuery == '') {
			defaultSearch();
		} else {
			searchedIcons = fuse.search(searchQuery);
		}
	}

	onMount(async () => {
		const mdi = await import('@mdi/js');

		const mdiArray = Object.entries(mdi) as [string, string][];
		mdiIcons = mdiArray.map(([name, path]) => ({ name, path }));

		defaultSearch();
	});
</script>

{page}
<Notification let:writeMessage>
	<div class="fixed bottom-4 right-4 w-[30rem]">
		<Input bind:value={searchQuery} on:keyup={handleKeyup}>
			<span class="flex gap-2 items-center">
				<span>{searchedIcons.length} icons</span>
				<Icon path={mdiMagnify} size={1.5} />
			</span>
		</Input>

		<div class="icons grid gap-4 max-h-80 my-4 overflow-x-auto">
			<LazyLoad items={searchedIcons} bind:page let:item let:index>
				<div
					class="flex flex-col items-center gap-2 text-sm bg-slate-100 hover:bg-slate-50 px-2 py-4 rounded transition-colors cursor-pointer"
					on:click={() => {
						writeToClipboard(item.item.name);
						writeMessage(`Copied ${item.item.name} to clipboard!`);
					}}
				>
					<Icon path={item.item.path} size={2.5} />
					<p>{index}: {item.item.name}</p>
				</div>
			</LazyLoad>
		</div>
	</div>
</Notification>

<style>
	.icons {
		grid-template-columns: repeat(auto-fill, minmax(10rem, 1fr));
	}
</style>
