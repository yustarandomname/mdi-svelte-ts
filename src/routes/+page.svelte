<script lang="ts">
	import Icon from '$lib/Icon.svelte';
	import Preview from '$lib/Preview.svelte';

	import { mdiHeart } from '@mdi/js';
</script>

<Preview />

<div class="control">
	<input class="input is-focused" type="text" placeholder="Focused input" />
</div>

<h1 class="text-3xl font-bold underline">Hello world!</h1>
<p>Create your package using @sveltejs/package and preview/showcase your work with SvelteKit</p>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<Icon path={mdiHeart} />
