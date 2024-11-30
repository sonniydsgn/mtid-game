<svelte:head>
	<title>Угадай слово — МТИД</title>
</svelte:head>

<section class="hero">
	<h1 class="text-h1 hero__title"><span>Интерактивная игра</span><br /> «Угадай слово»</h1>

	<div class="hero__list">
		<a class="btn-reset btn btn--primary" href="/game">Начать игру</a>
		<button class="btn-reset btn btn--secondary">Как играть</button>
		<button class="btn-reset btn btn--secondary">Об авторах</button>
	</div>

	<p class="hero__note">МТИД* — признаны самыми крутыми на территории ПНИПУ</p>
</section>

<style>
	.hero {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 1;
		padding-block-start: 56px;
	}

	.hero__title {
		margin-block-end: 120px;
	}

	.hero__list {
		max-width: 490px;
		width: 100%;
		display: flex;
		flex-direction: column;
		gap: 12px;
	}

	.hero__note {
		font-size: var(--font-sm);
		margin-block: auto 32px;
	}
</style>
