<script>
	const commands = ['help', 'whoami', 'projects', 'contact', 'clear', 'main'];
	import CommandInput from '$lib/components/CommandInput.svelte';
	import Contact from '$lib/components/Contact.svelte';
	import Help from '$lib/components/Help.svelte';
	import Projects from '$lib/components/Projects.svelte';
	import Redirecting from '$lib/components/Redirecting.svelte';
	let command = $state('');
	let output = $state('');

	/**
	 * Handle the command input from the user.
	 * @param {CustomEvent<string>} e - The command event.
	 */
	function handleCommand(e) {
		let committedCmd = e.detail.trim();
		if (commands.includes(committedCmd.toLowerCase())) {
			output = committedCmd;
			if (committedCmd === 'main') {
				let newTab = window.open('https://longph.dev', '_blank');
				if(newTab) {
					newTab.focus();
					output = '';
				}
			}
		} else output = 'error';
	}
</script>

<main>
	<div class="container flex flex-col !overflow-hidden p-4 !text-[#b89076]">
		<div class="font-bold">Welcome to my interactive web terminal.</div>
		<div class="font-bold">For a list of available commands, type 'help'.</div>
		<div class="font-bold">To go to my personal website, type 'main'.</div>
		<CommandInput onCommit={handleCommand} bind:command />
		<div class="space-x-1">
			{#if output}
				{#if output === 'projects'}
					<Projects />
				{/if}
				{#if output === 'help'}
					<Help />
				{/if}
				{#if output === 'whoami'}
					&#8618;&nbsp;I'm Long Pham, a software engineer based in Espoo, Finland.
				{/if}
				{#if output === 'contact'}
					<Contact />
				{/if}
				{#if output === 'error'}
					&#8618;&nbsp;Command not found. Type 'help' for a list of available commands.
				{/if}
				{#if output === 'main'}
					<Redirecting />
				{/if}
			{/if}
		</div>
	</div>
</main>
