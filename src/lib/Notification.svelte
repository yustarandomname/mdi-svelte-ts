<script lang="ts">
	import { fade } from 'svelte/transition';

	let message: string;

	/**
	 * Wait x seconds before hiding the notification
	 * @param seconds
	 */
	function wait(seconds: number) {
		return new Promise((resolve) => {
			setTimeout(resolve, seconds * 1000);
		});
	}

	/**
	 * Write a message to the notification
	 */
	function hideMessage() {
		message = '';
	}

	/**
	 * Write a message to the notification
	 * @param msg
	 */
	async function writeMessage(msg: string) {
		message = msg;
		await wait(3);
		hideMessage();
	}
</script>

{#if message}
	<div
		class="fixed bottom-4 right-4 rounded bg-slate-900 p-2 z-50 cursor-pointer text-slate-100"
		on:click={hideMessage}
		transition:fade
	>
		{message}
	</div>
{/if}

<slot {writeMessage} />
