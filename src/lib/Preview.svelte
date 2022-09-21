<script lang="ts">
	import { onMount } from 'svelte';
	import Fuse from 'fuse.js';

	import Icon from './Icon.svelte';
	import LazyLoad from './LazyLoad.svelte';
	import Input from './Input.svelte';
	import Notification from './Notification.svelte';
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

		// TODO: debounce
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

<Notification let:writeMessage>
	<div class="container">
		<Input bind:value={searchQuery} on:keyup={handleKeyup}>
			<span class="searchBar">
				<span>{searchedIcons.length} icons</span>
				<Icon path={mdiMagnify} size={1.5} />
			</span>
		</Input>

		<div class="icons">
			<LazyLoad items={searchedIcons} bind:page let:item let:index>
				<div
					class="icon"
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

<style lang="postcss">
	.container {
		position: fixed;
		bottom: 1rem;
		right: 1rem;
		width: 30rem;
	}

	.searchBar {
		display: flex;
		gap: 0.5rem;
		align-items: center;
	}

	.icons {
		display: grid;
		gap: 1rem;
		max-height: 20rem;
		margin-inline: 1rem;
		overflow-x: hidden;
		overflow-y: auto;
		grid-template-columns: repeat(auto-fill, minmax(10rem, 1fr));
	}

	.icon {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.5rem;
		font-size: 0.8rem;
		background-color: rgb(241, 245, 249);
		padding: 0.5rem 1rem;
		border-radius: 0.25rem;
		transition: background-color 0.2s ease;
		cursor: pointer;
	}

	.icon:hover {
		background-color: rgb(248, 250, 252);
	}
</style>
