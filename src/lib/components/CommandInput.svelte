<script>
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';
	/**
	 * @typedef Props
	 * @property {string} command - The command input.
	 * @property {(command: CustomEvent<string>) => void} onCommit - The function to commit the command.
	 */

	/** @type {Props} */
	let { command = $bindable(''), onCommit } = $props();
	let commandHistory = $state(
		browser ? JSON.parse(localStorage.getItem('commandHistory') ?? '[]') : []
	);
	let lastCommandIdx = commandHistory.length;

	onMount(() => {
		if (document) document.getElementById('command')?.focus();
	});
</script>

<div class="flex !flex-row space-x-2">
	<label for="command" class="font-bold !text-[#1ECC21]">visitor@longph.com:~$</label><input
		class="w-full overflow-scroll border-none bg-[#151515] font-bold text-[#73abad] outline-none"
		name="command"
		id="command"
		autocomplete="off"
		type="text"
		bind:value={command}
		onkeydown={(e) => {
			switch (e.key) {
				case 'ArrowUp':
					e.preventDefault();
					if (lastCommandIdx > 0) {
						lastCommandIdx--;
					}
					command = commandHistory[lastCommandIdx];
					break;
				case 'ArrowDown':
					e.preventDefault();
					if (lastCommandIdx < commandHistory.length - 1) {
						lastCommandIdx++;
					}
					command = commandHistory[lastCommandIdx];
					break;
				case 'Enter':
					commandHistory.push(command.trim());
					localStorage.setItem('commandHistory', JSON.stringify(commandHistory));
					lastCommandIdx = commandHistory.length;
					onCommit(new CustomEvent('commit', { detail: command }));
					command = '';
					break;
				default:
					// FIXME: Any key will trigger this, we only want to do this when text changed. For some reason, on:change doesn't work for text deletion
					lastCommandIdx = commandHistory.length;
					break;
			}
		}}
	/>
</div>
