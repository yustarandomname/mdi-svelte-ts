<script lang="ts">
	import { Icon } from '$lib';
	import { mdiMagnify } from '@mdi/js';

	export let value: string;
</script>

<div class="w-full relative grid">
	<input class="w-full p-2 rounded" type="text" placeholder="Search icon" bind:value on:keyup />
	<span class="justify-self-end self-center py-2">
		<Icon path={mdiMagnify} size={1.5} />
	</span>
</div>

<style>
	.grid {
		grid-template: 'container';
	}

	input,
	span {
		grid-area: container;
	}
</style>
