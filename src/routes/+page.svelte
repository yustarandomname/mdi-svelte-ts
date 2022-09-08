<script lang="ts">
	import Icon from '$lib/Icon.svelte';

	import { mdiHeart } from '@mdi/js';

	onMount(async () => {
		const mdi = await import('@mdi/js');
		const entries = Object.entries(mdi);
		console.log(entries.length);
	});

	import { onMount } from 'svelte';
</script>

<div class="control">
	<input class="input is-focused" type="text" placeholder="Focused input" />
</div>

<h1>Welcome to your library project</h1>
<p>Create your package using @sveltejs/package and preview/showcase your work with SvelteKit</p>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<Icon path={mdiHeart} />
