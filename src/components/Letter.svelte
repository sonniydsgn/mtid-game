<script>
	let { children, exact, close, missing, selected } = $props();
</script>

<div class="letter" class:exact class:close class:missing class:selected>{@render children()}</div>

<style lang="postcss">
	.letter {
		--size: 90px;

		display: flex;
		justify-content: center;
		align-items: center;
		padding: 0px 0px 8px;
		width: var(--size);
		height: var(--size);
		background: #ececec;
		box-shadow: inset 0 0 0 1px #b4b4b4;
		border-radius: 16px;
		font-weight: 400;
		font-size: 72px;
		line-height: 1;
		text-align: center;
		text-transform: lowercase;
		color: #3c3c3c;

		&.selected {
			box-shadow: inset 0 0 0 4px #000;
		}

		&.exact {
			font-weight: 600;
			background-color: #6aaa64;
			color: #fff;
			box-shadow: transparents;
		}

		&.close {
			font-weight: 600;
			background-color: #d1b036;
			color: #fff;
			box-shadow: transparents;
		}

		&.missing {
			opacity: 0.35;
		}
	}
</style>
