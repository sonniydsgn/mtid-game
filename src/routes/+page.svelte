<svelte:head>
	<title>Угадай слово — МТИД</title>
</svelte:head>

<script>
	import example_1 from '$lib/images/example_1.png';
	import example_2 from '$lib/images/example_2.png';
	import kind_cat from '$lib/images/catik.png';
	import angry_cat from '$lib/images/catik_angry.png';


	function closeOnBackDropClick({ currentTarget, target }) {
  	const dialogElement = currentTarget
  	const isClickedOnBackDrop = target === dialogElement
  	if (isClickedOnBackDrop) {
    	dialogElement.close()
  	}
	}
</script>

<section class="hero">
	<h1 class="text-h1 hero__title"><span>Интерактивная игра</span><br /> «Угадай слово»</h1>

	<div class="hero__list">
		<a class="btn-reset btn btn--primary" href="/game">Начать игру</a>
		<button class="btn-reset btn btn--secondary" type="button" onclick={window.howToPlay.showModal()}>Как играть</button>
		<button class="btn-reset btn btn--secondary" type="button" onclick={window.about.showModal()}>Об авторах</button>
	</div>

	<p class="hero__note">МТИД* — признаны самыми крутыми на территории ПНИПУ</p>
</section>



<dialog class="howToPlay" id="howToPlay" onclick={closeOnBackDropClick}>
	<div class="howToPlay__body">
		<h3 class="text-h3 howToPlay__title">Как играть</h3>

		<div class="howToPlay__info">
			<p class="howToPlay__text">Загадывается слово связанное со МТИД. Чтобы угадать у вас есть 7 попыток. Например:</p>

			<img src="{example_1}" alt="/" class="howToPlay__example">

			<p class="howToPlay__text">
				На каждой попытке игрок может ввести любое слово, которое имеет столько же букв, сколько и загаданное
			</p>
			<p class="howToPlay__text">
				Если буква есть в загаданном слове —<br>подсвечивается жёлтым
			</p>
			<p class="howToPlay__text">
				Если же она находится ещё на своём месте —<br>подсвечивается зелёным
			</p>

			<img src="{example_2}" alt="/" class="howToPlay__example">

			<p class="howToPlay__text">
				В этот раз мы отгадали слово! Теперь ваш черёд
			</p>
		</div>

		<form method="dialog" class="howToPlay__close-btn-wrapper">
			<button class="btn-reset btn btn--primary howToPlay__close-btn" type="submit">Хорошо</button>
		</form>
	</div>
</dialog>		

<dialog class="about" id="about" onclick={closeOnBackDropClick}>
	<div class="about__body">
		<h3 class="text-h3 about__title">Об авторах</h3>

		<div class="about__info">
			<p class="about__text">Игру создали ученики <span>ПНИПУ</span> группы <span>МТИД-23-1б</span></p>
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

<dialog class="win" id="win" onclick={closeOnBackDropClick}>
	<div class="win__body">
		<img src="{kind_cat}" alt="/" class="win__image" width="410" height="261">
		
		<h3 class="text-h3 win__title">Угадали</h3>
		
		<p class="win__text">Дизайн — ключевая дисциплина, включающая создание графики, интерфейсов и мультимедийного контента</p>

		<form method="dialog" class="win__close-btn-wrapper">
			<button class="btn-reset btn btn--primary win__close-btn" type="submit">Продолжить</button>
		</form>
	</div>
</dialog>	

<dialog class="lose" id="lose" onclick={closeOnBackDropClick}>
	<div class="lose__body">
		<img src="{angry_cat}" alt="/" class="lose__image" width="300" height="300">
		
		<h3 class="text-h3 lose__title">Бедолаги</h3>
		
		<p class="lose__text">Даже не знаю, что вам сказать можно...</p>

		<form method="dialog" class="lose__close-btn-wrapper">
			<button class="btn-reset btn btn--primary lose__close-btn" type="submit">Начать заново</button>
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

	.howToPlay {
		width: 700px;
		margin: auto auto;
		padding: 48px 40px;
		border: none;
		box-shadow: 0 5px 16px 0 rgba(0, 0, 0, 0.25);
		border-radius: 32px;
	}

	.howToPlay:modal {
		max-width: 100%;
		max-height: 100%;
	}

	.howToPlay::backdrop {
		background: rgba(0, 0, 0, 0.4);
	}

	.howToPlay__body {
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	.howToPlay__title {
		margin-bottom: 20px;
	}

	.howToPlay__example {
		margin-block: 16px;
	}

	.howToPlay__text + .howToPlay__text {
		margin-top: 12px;
	}

	.howToPlay__text:last-child {
		margin-bottom: 32px;
	}

	.howToPlay__close-btn {
		display: flex;
		max-width: 620px;
		width: 100%;
	}

	.about {
		width: 700px;
		margin: auto auto;
		padding: 48px 40px;
		border: none;
		box-shadow: 0 5px 16px 0 rgba(0, 0, 0, 0.25);
		border-radius: 32px;
	}

	.about:modal {
		max-width: 100%;
		max-height: 100%;
	}

	.about::backdrop {
		background: rgba(0, 0, 0, 0.4);
	}

	.about__body {
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	.about__title {
		margin-bottom: 16px;
	}

	.about__info {
		margin-bottom: 16px;
	}

	.about__text span {
		font-weight: 500;
	}

	.about__text:not(:last-child) {
		margin-bottom: 4px;
	}

	.about__list {
		margin-left: 40px;
		display: flex;
		flex-direction: column;
		gap: 16px;
		margin-bottom: 32px;
	}

	.about__close-btn {
		display: flex;
		max-width: 620px;
		width: 100%;
	}

	.win {
		width: 700px;
		margin: auto auto;
		padding: 48px 40px;
		border: none;
		box-shadow: 0 5px 16px 0 rgba(0, 0, 0, 0.25);
		border-radius: 32px;
	}

	.win:modal {
		max-width: 100%;
		max-height: 100%;
	}

	.win::backdrop {
		background: rgba(0, 0, 0, 0.4);
	}

	.win__body {
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	.win__image {
		align-self: center;
		margin-bottom: 32px;
	}

	.win__title {
		margin-bottom: 16px;
	}

	.win__text {
		margin-bottom: 32px;
	}

	.win__close-btn {
		display: flex;
		max-width: 620px;
		width: 100%;
	}

	.lose {
		width: 700px;
		margin: auto auto;
		padding: 48px 40px;
		border: none;
		box-shadow: 0 5px 16px 0 rgba(0, 0, 0, 0.25);
		border-radius: 32px;
	}

	.lose:modal {
		max-width: 100%;
		max-height: 100%;
	}

	.lose::backdrop {
		background: rgba(0, 0, 0, 0.4);
	}

	.lose__body {
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	.lose__image {
		align-self: center;
		margin-bottom: 32px;
	}

	.lose__title {
		margin-bottom: 16px;
	}

	.lose__text {
		margin-bottom: 32px;
	}

	.lose__close-btn {
		display: flex;
		max-width: 620px;
		width: 100%;
	}
</style>
