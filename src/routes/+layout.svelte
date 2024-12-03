<script>
	import { page } from '$app/stores';
	import { setContext } from 'svelte';

	import Header from '../components/Header.svelte';
	import '../app.css';

	const level = $state({ current: 0, all: 0 });
	setContext('level', level);

	function closeOnBackDropClick({ target }) {
		const dialogElement = target.closest('dialog');
		const isClickedOnBackDrop = target === dialogElement;
		if (dialogElement && isClickedOnBackDrop) {
			dialogElement.close();
		}
	}

	let { children } = $props();
</script>

<svelte:window onclick={closeOnBackDropClick} />

<div class="app">
	<Header centered={$page.url.pathname === '/game' ? false : true} />

	<main>
		{@render children()}
	</main>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		width: 100%;
		margin: 0 auto;
		padding-block-end: 32px;
	}
</style>
