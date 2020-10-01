<script>
	import { onMount } from 'svelte';

	const PHONE_NUMBER_KEY = 'phoneNumber';
	const REGION_NUMBER_REGEX = /^([0-9]{2})([0-9]{4})([0-9]{4,5})$/;

	export let name;
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
		let m;

		if(phoneNumber) {
			if ((m = REGION_NUMBER_REGEX.exec(phoneNumber)) !== null) {
				fullNumber = m[0];
			} else {
				fullNumber = '';
			}
			localStorage.setItem(PHONE_NUMBER_KEY, fullNumber);
		}
	}

	$: document.title = `Whatsapp directly: ${fullNumber}`
</script>

<main>
	<h1>Whatsapp</h1>
	<h2>Direct Chat</h2>
	<p>mande mensagem sem precisar salvar o contato</p>
	+
	<input type="text" class="phone" placeholder="21 0000-0000"
		   bind:value="{phoneNumber}"
		   on:click={handlePhoneClick}
	>
	<button on:click={handleClick}>Start Chat</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		font-size: 3em;
	}

	h1 {
		color: #137730;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
		margin-bottom: 0;
	}

	h2 {
		color: #198538;
		text-transform: uppercase;
		font-size: 1.3em;
		font-weight: 100;
		margin: 0;
	}

	p {
		font-size: .4em;
		color: #333333;
	}

	.phone {
		max-width: 500px;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
