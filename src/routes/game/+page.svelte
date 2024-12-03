<script>
	import example_1 from '$lib/images/example_1.png';
	import example_2 from '$lib/images/example_2.png';
	import kind_cat from '$lib/images/catik.png';
	import angry_cat from '$lib/images/catik_angry.png';

	import Letter from '../../components/Letter.svelte';
	import { confetti } from '@neoconfetti/svelte';
	import { onMount } from 'svelte';

	// задаем базовые значения
	const keys = 'абвгдеёжзийклмнопрстуфхцчшщъыьэюя';

	const words = [
		{
			word: 'БАллы',
			desc: 'Проходной балл на бюджетную основу специальности МТИД в 2024 году составил 225 баллов'
		},
		{
			word: 'дизайн',
			desc: 'Дизайн - ключевая дисциплина, включающая создание графики, интерфейсов и мультимедийного контента'
		},
		{
			word: 'сессия',
			desc: 'Сессия - это период сдачи экзаменов. В ПНИПУ сессия проходит в январе и июне. Успешная сдача экзаменов влияет на стипендию'
		},
		{
			word: 'куратор',
			desc: 'Куратор - преподаватель, который помогает студентам в учебном процессе и адаптации'
		},
		{
			word: 'практика',
			desc: 'Практика - это форма учебной деятельности, которая позволяет студентам получать опыт работы в реальных условиях и развивать профессиональные навыки. Студенты специальности МТИД могут пройти практику в таких организациях как GreenData, RIO Soft, Parma Technologies Group и других ведущих IT-компаниях'
		},
		{
			word: 'стипендия',
			desc: 'Стипендия - это денежное пособие учащимся высшего учебного заведения. Академическая стипендия в ПНИПУ составляет 3 036  рублей, но может повышаться за отличную учебу. Губернаторская стипендия составляет 5 750 рублей'
		}
	];

	const attempts = 7;

	// смотрю какие ответы даны и сколько попыток использована
	let answers = $state([]);
	let guesses = $state(Array(attempts).fill(''));

	// смотрю текущее слово
	let wordIndex = $state(0);
	let currentWord = $derived(words[wordIndex].word.toLowerCase());
	let wordLength = $derived(currentWord.length);

	// проверяю статус игры
	let won = $derived(answers[answers.length - 1] === Array(wordLength).fill('x').join(''));
	let lose = $derived(answers.length === attempts && !won);

	$effect(() => {
		if (submittable) check();

		if (lose) {
			window.lose.showModal();
		}

		if (won) {
			setTimeout(() => {
				window.win.showModal();
			}, 100);
		}
	});

	// смотрю индекс текущих слов
	let guessIndex = $derived(won ? -1 : answers.length);
	let currentGuess = $derived(guesses[guessIndex] || '');

	// проверяю можно ли принять слово
	let submittable = $derived(currentGuess.length === wordLength);

	function setlevel() {
		window.localStorage.setItem('level', wordIndex + 1 + '/' + words.length);
	}

	function next() {
		reset();

		wordIndex += 1;

		let finished = words.length === wordIndex;
		if (finished) window.location.href = '/end';

		setlevel()
	}

	function reset() {
		guesses = Array(attempts).fill('');
		answers = [];
	}

	function check() {
		const letters = currentGuess.toLowerCase().split('');

		const available = Array.from(currentWord);
		const answer = Array(wordLength).fill('_');

		// first, find exact matches
		for (let i = 0; i < wordLength; i += 1) {
			if (letters[i] === available[i]) {
				answer[i] = 'x';
				available[i] = ' ';
			}
		}

		// then find close matches (this has to happen
		// in a second step, otherwise an early close
		// match can prevent a later exact match)
		for (let i = 0; i < wordLength; i += 1) {
			if (answer[i] === '_') {
				const index = available.indexOf(letters[i]);
				if (index !== -1) {
					answer[i] = 'c';
					available[index] = ' ';
				}
			}
		}

		answers.push(answer.join(''));

		return true;
	}

	function keydown(event) {
		const key = event.key;

		if (event.metaKey) return;

		if (key === 'Backspace') {
			guesses[guessIndex] = guesses[guessIndex].slice(0, -1);
		} else if ((keys.includes(key) || keys.toUpperCase().includes(key))  & (currentGuess.length < wordLength)) {
			guesses[guessIndex] += key;
		}
	}

	onMount(() => {
		setlevel()
	})
</script>

<svelte:head>
	<title>Первое слово — Интерактивная игра «Угадай слово»</title>
</svelte:head>

<svelte:window onkeydown={keydown} />

{#if won}
	<div
		style="position: absolute; left: 50%; top: 30%"
		use:confetti={{
			force: 0.7,
			stageWidth: window.innerWidth,
			stageHeight: window.innerHeight,
			colors: ['#ff3e00', '#40b3ff', '#676778']
		}}
	></div>
{/if}

<section class="game">
	<h1 class="visually-hidden">Интерактивная игра «Угадай слово»</h1>
	<div class="game__grid">
		{#each Array.from(Array(attempts).keys()) as row (row)}
			{@const current = row === guessIndex}
			<div class="game__row" class:current>
				{#each Array.from(Array(wordLength).keys()) as column (column)}
					{@const guess = current ? currentGuess : guesses[row]}
					{@const answer = answers[row]?.[column]}
					{@const value = guess?.[column] ?? ''}
					{@const selected = current && column === guess.length}
					{@const exact = answer === 'x'}
					{@const close = answer === 'c'}
					{@const missing = answer === '_'}
					<Letter {selected} {exact} {close} {missing}>{value}</Letter>
				{/each}
			</div>
		{/each}
	</div>
</section>

<div class="dialog__list">
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
				<button class="btn-reset btn btn--primary howToPlay__close-btn" type="submit">Хорошо</button
				>
			</form>
		</div>
	</dialog>

	<dialog class="win" id="win" onclose={next}>
		<div class="win__body">
			<img src={kind_cat} alt="/" class="win__image" width="410" height="261" />

			<h3 class="text-h3 win__title">Угадали</h3>

			<p class="win__text">{words[wordIndex].desc}</p>

			<form method="dialog" class="win__close-btn-wrapper">
				<button class="btn-reset btn btn--primary win__close-btn" type="submit">Продолжить</button>
			</form>
		</div>
	</dialog>

	<dialog class="lose" id="lose" onclose={reset}>
		<div class="lose__body">
			<img src={angry_cat} alt="/" class="lose__image" width="300" height="300" />

			<h3 class="text-h3 lose__title">Бедолаги</h3>

			<p class="lose__text">Даже не знаю, что вам сказать можно...</p>

			<form method="dialog" class="lose__close-btn-wrapper">
				<button class="btn-reset btn btn--primary lose__close-btn" type="submit"
					>Начать заново</button
				>
			</form>
		</div>
	</dialog>
</div>

<style>
	.game {
		display: flex;
		flex-direction: column;
		place-items: center;
		padding-block-start: 32px;
	}

	.game__grid {
		--gap: 8px;

		gap: var(--gap);
		align-self: center;
		justify-self: center;
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
	}

	.game__row {
		display: flex;
		gap: var(--gap);
	}
</style>
