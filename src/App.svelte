<script>
	import { onMount } from 'svelte';

	const PHONE_NUMBER_KEY = 'phoneNumber';
	const REGION_NUMBER_REGEX = /^([0-9]{2})([0-9]{4})([0-9]{4,5})$/;

	export let phoneNumber = '';
	export let fullNumber = '';

	onMount(() => {
		phoneNumber = localStorage.getItem(PHONE_NUMBER_KEY);
	});

	function handleClick() {
		window.location.href = `https://wa.me/55${fullNumber}`;
	}

	function handlePhoneClick(ev) {
		ev.currentTarget.select();
	}

	$: {
		const match = REGION_NUMBER_REGEX.exec(phoneNumber)

		if(phoneNumber) {
			if (match !== null) {
				fullNumber = match[0];
			} else {
				fullNumber = '';
			}
			localStorage.setItem(PHONE_NUMBER_KEY, fullNumber);
		}
	}

	$: document.title = fullNumber ? `Começar chat com ${fullNumber}` : 'Whatsapp Direct Chat';

	$: isDisabled = phoneNumber && fullNumber ? '' : 'disabled';
</script>

<main>
	<h1>Whatsapp</h1>
	<h2>Direct Chat</h2>
	<p>mande mensagem sem precisar salvar o contato</p>
	<p>+</p>
	<input type="number" class="phone" placeholder="21 0000-0000"
		   bind:value="{phoneNumber}"
		   on:click={handlePhoneClick}
	>
	<button class="action"
			on:click={handleClick}
			disabled="{isDisabled}"
	>Iniciar Chat</button>
	
	<div>
		<p class="madeby">
			Made by <a href="https://www.linkedin.com/in/marcelobritonet/">@marcelobritonet</a>
		</p>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
		font-size: 3em;
	}

	@media (min-width: 720px) {
		main {

		}
	}

	h1 {
		color: #137730;
		text-transform: uppercase;
		font-size: 1em;
		font-weight: 100;
		margin-bottom: 0;
	}

	@media (min-width: 720px) {
		h1 {
			font-size: 2em;
		}
	}

	h2 {
		color: #198538;
		text-transform: uppercase;
		font-size: .6em;
		font-weight: 100;
		margin: 0;
	}

	@media (min-width: 720px) {
		h2 {
			font-size: 1.3em;
		}
	}

	p {
		font-size: .3em;
		color: #333333;
	}

	@media (min-width: 720px) {
		p {
			font-size: .4em;
		}
	}

	.phone {
		max-width: 100%;
		font-size: .6em;
		text-align: center;
		color: #333333;
	}

	@media (min-width: 720px) {
		.phone {
			max-width: 500px;
			font-size: 1em;
			text-align: left;
		}
	}

	.action {
		cursor: pointer;
		font-size: .5em;
	}

	@media (min-width: 720px) {
		.action {
			font-size: 1em;
		}
	}

	.madeby {
		color: #6e6e6e;
		font-style: italic;
		font-size: .2em;
	}
</style>
