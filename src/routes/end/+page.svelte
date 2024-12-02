<svelte:head>
	<title>Спасибо за участие — Интерактивная игра «Угадай слово»</title>
</svelte:head>

<script>
	import qr from '$lib/images/qr.svg';
</script>

<section class="end">
	<h1 class="text-h1 end__title"><span>Спасибо за участие</span><br /> Вы молодцы!</h1>

	<img src={qr} class="end__qr" alt="" />

	<button class="btn-reset btn btn--primary end__btn">Вернуться в начало</button>
</section>

<style>
	.end {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 1;
		padding-block-start: 56px;
	}

	.end__title {
		margin-block-end: 50px;
	}

	.end__btn {
		display: flex;
		max-width: 490px;
		width: 100%;
		margin-block: auto 32px;
	}
</style>
