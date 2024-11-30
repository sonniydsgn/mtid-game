<script>
	import logo from '$lib/images/logo.svg';
</script>

<header class="header header--center">
	<img src={logo} class="header__logo" alt="" />
</header>

<style>
	.header {
		display: flex;
		justify-content: space-between;
		margin-block-start: 48px;
	}

	.header--center {
		align-self: center;
	}
</style>
