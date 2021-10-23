<script>
	import Tiptap from './lib/Tiptap.svelte'
	import {expression} from '../store/store.js'
	
	export let name;

	let calcView = "1+2=3"

	function zero() {
		expression.update(exps => exps + '0');
		console.log('0');
	}

</script>

<main>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<h1> {calcView} </h1>
	<!-- <Tiptap myContent= {calcView} /> -->
	<Tiptap />

	<div class="number-pad">
		<div>
			<button on:click={zero}> 
				0 
			</button>
		</div>
		<div>1</div>
		<div>2</div>
		<div>+</div>
		<div>3</div>  
		<div>4</div>
		<div>5</div>
		<div>-</div>
		<div>6</div>  
		<div>7</div>
		<div>8</div>
		<div>=</div>
		<div>9</div>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	button {
		width: 100%;
		height: 100%;
		background-color: rgba(255, 255, 255, 0);
		border: none;
		margin: 0;
		padding: 0.4em;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	.number-pad{
		display: grid;
		grid-template-columns: auto auto auto auto  ;
		grid-gap: 10px;
		background-color: #2196F3;
		padding: 10px;
	}

	.number-pad > div {
		background-color: rgba(255, 255, 255, 0.8);
		text-align: center;
		font-size: 30px;
	}
</style>