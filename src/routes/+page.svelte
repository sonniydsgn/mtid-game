<script>
	import example_1 from '$lib/images/example_1.png';
	import example_2 from '$lib/images/example_2.png';
</script>

<svelte:head>
	<title>Угадай слово — МТИД</title>
</svelte:head>

<section class="hero">
	<h1 class="text-h1 hero__title"><span>Интерактивная игра</span><br /> «Угадай слово»</h1>

	<div class="hero__list">
		<a class="btn-reset btn btn--primary" href="/game">Начать игру</a>
		<button
			class="btn-reset btn btn--secondary"
			type="button"
			onclick={() => window.howToPlay.showModal()}>Как играть</button
		>
		<button
			class="btn-reset btn btn--secondary"
			type="button"
			onclick={() => window.about.showModal()}>Об авторах</button
		>
	</div>

	<p class="hero__note">МТИД* — признаны самыми крутыми на территории ПНИПУ</p>
</section>

<dialog class="about" id="about">
	<div class="about__body">
		<h3 class="text-h3 about__title">Об авторах</h3>

		<div class="about__info">
			<p class="about__text">
				Игру создали ученики <span>ПНИПУ</span> группы <span>МТИД-23-1б</span>
			</p>
			<p class="about__text">А именно такие умники и умнички, как:</p>
		</div>

		<ul class="about__list">
			<li class="about__item">Алексей Калинин (писька вредная)</li>
			<li class="about__item">Анатолий Шредер (самый крутой, после верхнего лоха)</li>
			<li class="about__item">Парфирьев Тимофей (лысая в откисе)</li>
			<li class="about__item">Воскресенская Евгения</li>
			<li class="about__item">Толстенко Анастасия</li>
			<li class="about__item">Семенова Софья</li>
		</ul>

		<form method="dialog" class="about__close-btn-wrapper">
			<button class="btn-reset btn btn--primary about__close-btn" type="submit">Окей</button>
		</form>
	</div>
</dialog>

<dialog class="howToPlay" id="howToPlay">
	<div class="howToPlay__body">
		<h3 class="text-h3 howToPlay__title">Как играть</h3>

		<div class="howToPlay__info">
			<p class="howToPlay__text">
				Загадывается слово связанное со МТИД. Чтобы угадать у вас есть 7 попыток. Например:
			</p>

			<img src={example_1} alt="/" class="howToPlay__example" />

			<p class="howToPlay__text">
				На каждой попытке игрок может ввести любое слово, которое имеет столько же букв, сколько
				и загаданное
			</p>
			<p class="howToPlay__text">
				Если буква есть в загаданном слове —<br />подсвечивается жёлтым
			</p>
			<p class="howToPlay__text">
				Если же она находится ещё на своём месте —<br />подсвечивается зелёным
			</p>

			<img src={example_2} alt="/" class="howToPlay__example" />

			<p class="howToPlay__text">В этот раз мы отгадали слово! Теперь ваш черёд</p>
		</div>

		<form method="dialog" class="howToPlay__close-btn-wrapper">
			<button class="btn-reset btn btn--primary howToPlay__close-btn" type="submit">Хорошо</button>
		</form>
	</div>
</dialog>

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
