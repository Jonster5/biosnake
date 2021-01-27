<script lang="ts">
	export let state: number;

	$: cls =
		state === 0
			? 'empty'
			: state === 1
			? 'snake'
			: state === 2
			? 'food'
			: 'empty';
</script>

<main class={cls} />

<style lang="scss">
	main {
		width: 2vw;
		height: 2vw;
	}

	.empty {
		background: black;
	}

	.snake {
		background: lime;
	}

	.food {
		background: red;
	}
</style>
