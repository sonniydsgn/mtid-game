<script>
	import { onMount } from 'svelte';
	import { getContext } from 'svelte';

	import logo from '$lib/images/logo.svg';

	const { centered = false } = $props();
	const level = getContext('level');

	let isFullscreen = $state(false);

	const changeFullscreen = () => {
		if (isFullscreen) {
			document.exitFullscreen();
		} else {
			document.documentElement.requestFullscreen();
		}
	};
</script>

<svelte:document on:fullscreenchange={() => (isFullscreen = document.fullscreenElement !== null)} />

<header class="header" class:header--center={centered}>
	<div class="header__title">
		<a href="/" class="header__link" title="На главную">
			<img src={logo} class="header__logo" width="213" height="25" alt="" />
		</a>

		{#if level && !centered}
			<span class="header__level">{level.current + '/' + level.all + ' уровень'}</span>
		{/if}
	</div>

	{#if !centered}
		<button
			class="btn-reset btn btn--icon btn--tertiary"
			onclick={() => window.howToPlay.showModal()}
		>
			Правила игры

			<svg
				class="icon header__icon"
				width="24"
				height="24"
				viewBox="0 0 24 24"
				fill="none"
				xmlns="http://www.w3.org/2000/svg"
			>
				<path
					d="M11 17H13V11H11V17ZM12 9C12.2833 9 12.521 8.904 12.713 8.712C12.905 8.52 13.0007 8.28267 13 8C12.9993 7.71733 12.9033 7.48 12.712 7.288C12.5207 7.096 12.2833 7 12 7C11.7167 7 11.4793 7.096 11.288 7.288C11.0967 7.48 11.0007 7.71733 11 8C10.9993 8.28267 11.0953 8.52033 11.288 8.713C11.4807 8.90567 11.718 9.00133 12 9ZM12 22C10.6167 22 9.31667 21.7373 8.1 21.212C6.88334 20.6867 5.825 19.9743 4.925 19.075C4.025 18.1757 3.31267 17.1173 2.788 15.9C2.26333 14.6827 2.00067 13.3827 2 12C1.99933 10.6173 2.262 9.31733 2.788 8.1C3.314 6.88267 4.02633 5.82433 4.925 4.925C5.82367 4.02567 6.882 3.31333 8.1 2.788C9.318 2.26267 10.618 2 12 2C13.382 2 14.682 2.26267 15.9 2.788C17.118 3.31333 18.1763 4.02567 19.075 4.925C19.9737 5.82433 20.6863 6.88267 21.213 8.1C21.7397 9.31733 22.002 10.6173 22 12C21.998 13.3827 21.7353 14.6827 21.212 15.9C20.6887 17.1173 19.9763 18.1757 19.075 19.075C18.1737 19.9743 17.1153 20.687 15.9 21.213C14.6847 21.739 13.3847 22.0013 12 22ZM12 20C14.2333 20 16.125 19.225 17.675 17.675C19.225 16.125 20 14.2333 20 12C20 9.76667 19.225 7.875 17.675 6.325C16.125 4.775 14.2333 4 12 4C9.76667 4 7.875 4.775 6.325 6.325C4.775 7.875 4 9.76667 4 12C4 14.2333 4.775 16.125 6.325 17.675C7.875 19.225 9.76667 20 12 20Z"
					fill="#5D5D5D"
				/>
			</svg>
		</button>
	{/if}

	<button
		class="btn-reset btn btn--icon btn--secondary header__fs-btn"
		aria-label={isFullscreen ? 'Выйти из полного экрана' : 'Открыть в полный экран'}
		onclick={changeFullscreen}
	>
		{#if isFullscreen}
			<svg
				class="icon"
				xmlns="http://www.w3.org/2000/svg"
				width="24"
				height="24"
				viewBox="0 0 24 24"
				><path
					fill="currentColor"
					d="M6 21v-3H3v-2h5v5zm10 0v-5h5v2h-3v3zM3 8V6h3V3h2v5zm13 0V3h2v3h3v2z"
				/></svg
			>
		{:else}
			<svg
				class="icon"
				xmlns="http://www.w3.org/2000/svg"
				width="24"
				height="24"
				viewBox="0 0 24 24"
				><path
					fill="currentColor"
					d="M3 21v-5h2v3h3v2zm13 0v-2h3v-3h2v5zM3 8V3h5v2H5v3zm16 0V5h-3V3h5v5z"
				/></svg
			>
		{/if}
	</button>
</header>

<style>
	.header {
		display: flex;
		padding-inline: 64px;
		margin-block-start: 32px;
		align-items: center;
	}

	.header__title {
		display: flex;
		align-items: center;
		margin-right: auto;
	}

	.header__level {
		font-weight: 500;
		margin-left: 14px;
		padding-left: 16px;
		font-size: 18px;
		font-weight: 400;
		border-left: 1px solid #e0e0e0;
	}

	.header__logo {
		object-fit: none;
	}

	.header__fs-btn {
		padding: 12px;
		margin-left: 24px;
	}
</style>
