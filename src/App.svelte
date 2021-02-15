<script lang="ts">
	import Puzzle from "./components/Puzzle.svelte"

	function randomInt(range:number) {
		return Math.floor(Math.random() * range)
	}

	const layers = [
		randomInt(6),
		randomInt(6),
		randomInt(8),
		randomInt(8),
	]
	const palette = randomInt(12)
	const quantity = randomInt(9) + 3

</script>

<main>
	<div class="row"><Puzzle {layers} {palette} {quantity}/><Puzzle {layers} {palette} {quantity}/><Puzzle {layers} {palette} {quantity}/></div>
	<div class="row"><Puzzle {layers} {palette} {quantity}/><Puzzle {layers} {palette} {quantity}/><Puzzle {layers} {palette} {quantity}/></div>
	<div class="row"><Puzzle {layers} {palette} {quantity}/><Puzzle {layers} {palette} {quantity}/><Puzzle {layers} {palette} {quantity}/></div>
</main>

<style>
	main {
		width: 100%;
		height: 100%;
		display: block;
	}
	.row {
		width: 300px;
		height: 100px;
	}
</style>