<script lang="ts">
	import Cell from '../Cell.svelte';
	import { sgrid, sdirections, ssnake } from './snake';

	let snake = ssnake;
	let directions = sdirections;
	let grid = sgrid;

	let gameOver = false;

	let score = 0;

	$: direction = directions[snake.direction];

	const handler = (e: KeyboardEvent) => {
		const d = e.key.slice(5).toLowerCase();

		if (directions[d] === undefined) return;
		if (d === 'right' && snake.direction === 'left') return;
		if (d === 'left' && snake.direction === 'right') return;
		if (d === 'up' && snake.direction === 'down') return;
		if (d === 'down' && snake.direction === 'up') return;

		snake.direction = d;
	};

	const rand = () => Math.floor(Math.random() * 19);

	let food = { x: rand(), y: rand() };
	while (grid[food.y][food.y] === 1) food = { x: rand(), y: rand() };
	grid[food.y][food.x] = 2;

	const end = () => {
		gameOver = true;
		clearInterval(interval);
	};

	const run = () => {
		// Update positions
		snake.x += direction.x;
		snake.y += direction.y;

		if (snake.x > 20 || snake.x < 0 || snake.y > 19 || snake.y < 0) {
			end();
			return;
		}

		snake.tail.forEach(({ x, y }) => {
			if (snake.x === x && snake.y === y) {
				end();
				return;
			}
		});

		if (snake.x === food.x && snake.y === food.y) {
			grid[food.y][food.x] = 0;
			snake.trail++;
			score++;
			food = { x: rand(), y: rand() };
			while (grid[food.y][food.y] === 1) food = { x: rand(), y: rand() };
			grid[food.y][food.x] = 2;
		}

		snake.tail = [{ x: snake.x, y: snake.y }, ...snake.tail];

		if (snake.tail.length > snake.trail) {
			let cell = snake.tail[snake.tail.length - 1];

			grid[cell.y][cell.x] = 0;

			snake.tail = [...snake.tail.slice(0, snake.tail.length - 1)];
		}

		// Update the grid
		snake.tail.forEach((t) => (grid[t.y][t.x] = 1));
		grid[snake.y][snake.x] = 1.5;
		// console.table(grid);
	};

	let interval = setInterval(run, 100);

	const restart = () => {
		grid = [
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
			[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
		];
		snake = {
			x: 10,
			y: 10,
			tail: [{ x: 10, y: 10 }],
			trail: 3,
			direction: 'up',
		};
		food = { x: rand(), y: rand() };
		while (grid[food.y][food.y] === 1) food = { x: rand(), y: rand() };
		grid[food.y][food.x] = 2;

		interval = setInterval(run, 100);
		gameOver = false;
	};
</script>

<svelte:window on:keydown={handler} />

<h1>Snake Game for Bio thing lol</h1>

<main>
	{#each grid as g}
		<div>
			{#each g as i}
				<Cell state={i} />
			{/each}
		</div>
	{/each}
</main>

<style lang="scss">
	h1 {
		font-family: Arial, Helvetica, sans-serif;
	}

	main {
		width: 100%;
		height: 100%;
		text-align: center;
	}

	div {
		display: flex;
	}
</style>
