<script>
	import Letter from '../../components/Letter.svelte';

	// let won = $derived(data.answers.at(-1) === 'xxxxx');
	// let i = $derived(won ? -1 : data.answers.length);

	// задаем базовые значения
	const keys = 'абвгдеёжзийклмнопрстуфхцчшщъыьэюя';

	const words = [
		{
			word: 'баллы',
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

	// динамические переменные
	let value = $state('');

	// смотрю текущее слово
	let wordIndex = $state(0); // буду добавлять после клика модального окна
	let currentWord = $derived(words[wordIndex].word);
	let wordLength = $derived(currentWord.length);

	// проверяю победу
	let won = $derived(answers[answers.length - 1] === Array(wordLength).fill('x').join(''));

	$effect(() => {
		if (won) reset();
	});

	// смотрю индекс текущих слов
	let guessIndex = $derived(won ? -1 : answers.length);
	let currentGuess = $derived(guesses[guessIndex] || '');

	// проверяю можно ли принять слово
	let submittable = $derived(currentGuess.length - 1 === wordLength);

	function reset() {
		guesses = Array(attempts).fill('');
		answers = [];

		wordIndex += 1;
		console.log('победа!!');
	}

	function check() {
		const letters = currentGuess.split('');

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

		if (submittable) check();

		if (key === 'Backspace') {
			guesses[guessIndex] = guesses[guessIndex].slice(0, -1);
		} else if (keys.includes(key) & (currentGuess.length < wordLength)) {
			guesses[guessIndex] += key;
		}
	}

	$inspect(currentGuess.length);
</script>

<svelte:head>
	<title>Первое слово — Интерактивная игра «Угадай слово»</title>
</svelte:head>

<svelte:window onkeydown={keydown} />

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

<style>
	.game {
		display: flex;
		flex-direction: column;
		place-items: center;
		padding-block-start: 56px;
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
