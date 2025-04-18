<script>
	import example_1 from '$lib/images/example_1.png';
	import example_2 from '$lib/images/example_2.png';
	import kind_cat from '$lib/images/catik.png';
	import angry_cat from '$lib/images/catik_angry.png';

	import { words } from './data';

	import Letter from '../../components/Letter.svelte';
	import Toast from '../../components/Toast.svelte';

	import { getContext } from 'svelte';
	import { confetti } from '@neoconfetti/svelte';

	// задаем базовые значения
	const ruAlphabet = 'абвгдеёжзийклмнопрстуфхцчшщъыьэюя';
	const engAlphabet = 'abcdefghijklmnopqrstuvwxyz';
	const ruKeys = ruAlphabet + ruAlphabet.toUpperCase();
	const engKeys = engAlphabet + engAlphabet.toUpperCase();
	let wrongkeys = $state(false);

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
	const level = getContext('level');

	$effect(() => {
		level.current = wordIndex + 1;
		level.all = words.length;

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

	function next() {
		reset();

		wordIndex += 1;

		let finished = words.length === wordIndex;
		if (finished) window.location.href = '/end';
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

		wrongkeys = false;
		if (key === 'Backspace') {
			guesses[guessIndex] = guesses[guessIndex].slice(0, -1);
		} else if (ruKeys.includes(key) && currentGuess.length < wordLength) {
			guesses[guessIndex] += key;
		} else if (engKeys.includes(key)) {
			wrongkeys = true;
		}
	}
</script>

<svelte:head>
	<title>{level.current} уровень — Интерактивная игра «Угадай слово»</title>
</svelte:head>

<svelte:window onkeydown={keydown} />

{#if wrongkeys}
	<Toast text="Переключите раскладку клавиатуры на русский язык" icon="ℹ️" />
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
		{#if won}
			<div
				style="position: fixed; left: 50%; top: 30%;"
				use:confetti={{
					force: 0.7,
					stageWidth: window.innerWidth,
					stageHeight: window.innerHeight,
					colors: ['#ff3e00', '#40b3ff', '#676778']
				}}
			></div>
		{/if}
		<div class="win__body">
			<img
				src={words[wordIndex].image || kind_cat}
				alt="/"
				class="win__image"
				width="400"
				height="260"
			/>

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

	.win__image {
		border-radius: 16px;
	}
</style>
