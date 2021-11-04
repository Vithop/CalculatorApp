<script>
  import NumPadKey from "./lib/NumPadKey.svelte";
  import { standardKeys } from "./lib/NumPadTemplates.svelte";
  import { create, all } from "mathjs";

  const testHistory = ["1+1=2", "2+5=7", "10*45=450"];

  const config = {};
  const math = create(all, config);

  let input = "";
  let output = "";
  let equationHistory = [];
  let clearFlag = false; 

  function evaluateExpression() {
    try {
      output = math.evaluate(input);
      equationHistory = [...equationHistory, `${input}=${output}`];
      input = "";
    } catch (error) {
      input = error.name;
      output = "";
      clearFlag = true;
    }
  }

  const getEquation = (index) => () => {
    input = equationHistory[index];
  };

  const keyBoardToOps = {
    "Enter": evaluateExpression,
  }

  function handleKeydown(event) {
		// key = event.key;
		// keyCode = event.keyCode;
    if (clearFlag == true) {
      input = "";
      clearFlag = false;
    }
    if (keyBoardToOps && keyBoardToOps[event.key]) {
      if (event.keyCode == 13 && !event.shiftKey){
      // prevent default behavior
        event.preventDefault();
        keyBoardToOps[event.key]();
      }
    }
	}
</script>

<svelte:window on:keydown={handleKeydown}/>

<main>
  <div class="history-view">
    <br />
    {#each equationHistory as equation, index}
      <span on:click={getEquation(index)}>{equation}</span>
      <br />
    {/each}
  </div>
  <div class="current-view">
    <div class="input-view" contenteditable="true" bind:innerHTML={input}/>
    <div class="current-view-equal">=</div>
    <div class="output-view">{output}</div>
  </div>
  <div class="number-pad">
    {#each standardKeys as key, index}
      <NumPadKey
        symbol={key}
        bind:value={input}
        bind:clearFlag={clearFlag}
        on:equal={evaluateExpression}
      />
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
  /* Use median queries to manage css for different screen sizes this template is sorted by width */
  @media (min-width:320px)  { /* smartphones, iPhone, portrait 480x320 phones */ }
  @media (min-width:481px)  { /* portrait e-readers (Nook/Kindle), smaller tablets @ 600 or @ 640 wide. */ }
  @media (min-width:641px)  { /* portrait tablets, portrait iPad, landscape e-readers, landscape 800x480 or 854x480 phones */ }
  @media (min-width:961px)  { /* tablet, landscape iPad, lo-res laptops ands desktops */ }
  @media (min-width:1025px) { /* big landscape tablets, laptops, and desktops */ }
  @media (min-width:1281px) { /* hi-res laptops and desktops */ }
  .current-view {
    display: grid;
    grid-template-columns: auto min-content auto;
    font: 30px;
    background-color: aquamarine;
    padding: 0.8em;
    height: auto;
  }
  .input-view {
    text-align: left;
    background-color: rgba(255, 255, 255, 0.15);
  }
  .current-view-equal {
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
