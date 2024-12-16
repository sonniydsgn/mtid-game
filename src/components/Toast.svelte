<script>
	import { onDestroy, onMount } from 'svelte';

	let { text, icon } = $props();

	let show = $state(false);

	onMount(() => {
		setTimeout(() => {
			show = true;
		}, 0);
	});
</script>

<div class="toast" class:show>
	<span>{icon}</span>
	<span>{text}</span>
</div>

<style>
	.toast {
		position: fixed;
		bottom: 16px;
		display: flex;
		align-items: center;
		gap: 8px;
		align-self: center;
		max-width: 600px;
		border-radius: 16px;
		padding-block: 16px;
		padding-inline-start: 16px;
		padding-inline-end: 24px;
		font-size: var(--font-sm);
		background-color: #3c3c3c;
		z-index: 9999;
		color: #fff;
		transform: translateY(calc(100% + 16px));
		transition: transform 0.15s ease;
	}

	.toast.show {
		transform: translateY(0);
	}
</style>
