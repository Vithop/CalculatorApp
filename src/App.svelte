<script>

  import NumPadKey from "./lib/NumPadKey.svelte";
  import { create, all } from 'mathjs'

  export let name;
  
  const config = { }
  const math = create(all, config)
  
  let input = "";
  let output = "";
  let history = "";

  const keys = [
    "7","8","9","DEL","AC",
    "4","5","6","MULT","DIV",
    "1","2","3","ADD","SUB",
    "0",".","EXP","ANS","=",
  ];

  function evaluateExpression() {
	output = math.evaluate(input);
	history = input;
  }
</script>

<main>
  <h1>{name}'s PRO CALCULATOR</h1>
  <div class="history-view">
	{history}
  </div>
  <div class="current-view">
    <div class="input-view" contenteditable="true">
      {input}
    </div>
	<div class="current-view-equal">=</div>
    <div class="output-view">{output}</div>
  </div>
  <div class="number-pad">
    {#each keys as key}
      <NumPadKey symbol={key} bind:value={input} on:equal={evaluateExpression} />
    {/each}
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

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
  .current-view{
	display: grid;
	grid-template-columns: auto min-content auto;
	font: 30px;
	background-color: aquamarine;
	padding: 0.8em;
  }
  .input-view {
	text-align: left;
	background-color: rgba(255, 255, 255, 0.15);
  }
  .current-view-equal{
	background-color: rgba(255, 255, 255, 0.1);
  }
  .output-view {
	  text-align: right;
	background-color: rgba(255, 255, 255, 0.05);
  }
  .number-pad {
    display: grid;
    grid-template-columns: auto auto auto auto auto;
    grid-gap: 0.75em;
    background-color: #2196f3;
    padding: 0.75em;
  }
</style>
