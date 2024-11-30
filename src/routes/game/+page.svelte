<script>
	function keydown(event) {
		if (event.metaKey) return;

		if (event.key === 'Enter' && !submittable) return;

		console.log(event.key);
	}
</script>

<svelte:head>
	<title>Первое слово — Интерактивная игра «Угадай слово»</title>
</svelte:head>

<svelte:window onkeydown={keydown} />

<section class="game">
	<h1 class="visually-hidden">Интерактивная игра «Угадай слово»</h1>
	пипец
</section>

<style>
	.game {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 1;
		padding-block-start: 56px;
	}
</style>
