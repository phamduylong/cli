<script>
	import { afterNavigate } from '$app/navigation';
	const commands = ['help', 'whoami', 'projects', 'contact', 'clear', 'main'];
	import CommandInput from '$lib/components/CommandInput.svelte';
	import { onMount } from 'svelte';
	let command = $state('');
	let output = $state('');
	afterNavigate(() => {
		command = '';
		output = '';
	});
	/**
	 * Handle the command input from the user.
	 * @param {CustomEvent<string>} e - The command event.
	 */
	function handleCommand(e) {
		switch (e.detail.trim().toLowerCase()) {
			case 'help':
				output = `<b>Available commands:</b><div><ul>${commands.map((command) => `<li>&nbsp;&nbsp;${command}</li>`).join('')}</ul></div>`;
				break;
			case 'whoami':
				output = "I'm Long Pham, a software engineer based in Espoo, Finland.";
				break;
			case 'projects':
				output = `You can find my projects&nbsp;<a class="underline" href="https://longph.com/projects">here</a>.`;
				break;
			case 'contact':
				output = `You can contact me via email at <a class="underline" href="mailto:longphamduy2002@gmail.com">longphamduy2002@gmail.com</a> 
          or <a class="underline" href="https://longph.com/contact" target="_blank"> write to me.</a>`;
				break;
			case 'clear':
				output = '';
				break;
			case 'main':
				/**
				 * @see https://tw-elements.com/docs/standard/components/spinners/
				 */
				output = `Redirecting to my website
				<div
					class="text-primary inline-block h-4 w-4 animate-spin rounded-full 
					border-4 border-solid border-current border-e-transparent 
					align-[-0.125em] motion-reduce:animate-[spin_1.5s_linear_infinite]"
					role="status"
				>
					<span
						class="!absolute !-m-px !h-px !w-px !overflow-hidden !whitespace-nowrap !border-0 !p-0 ![clip:rect(0,0,0,0)]"
						>Loading...</span
					>
				</div>
				`;
				window.location.href = 'https://longph.com';
				break;
			case '':
				break;
			default:
				output = "Command not found. Type 'help' for a list of available commands.";
				break;
		}
	}
</script>

<main>
	<div class="container flex flex-col !overflow-hidden p-4 !text-[#b89076]">
		<div class="font-bold">Welcome to my interactive web terminal.</div>
		<div class="font-bold">For a list of available commands, type 'help'.</div>
		<div class="font-bold">To go to my personal website, type 'main'.</div>
		<CommandInput commit={handleCommand} bind:command />
		{#if output}
			<div class="space-x-1">&#8618;&nbsp;{@html output}</div>
		{/if}
	</div>
</main>
