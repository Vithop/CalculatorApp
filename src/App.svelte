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
      output = `${input}=${math.evaluate(input)}`;
      equationHistory = [...equationHistory, output];
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
    "Enter" : evaluateExpression,
  }

  $:if (input.charAt(-1) == '=') {
    evaluateExpression()
    input = input
  }

  function handleKeydown(event) {
		// key = event.key;
		// keyCode = event.keyCode;
    // console.log(event.key);
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
    {#each equationHistory.slice(0,-1) as equation, index}
      <span on:click={getEquation(index)}>{equation}</span>
      <br />
    {/each}
    <div class="output-view">{output}</div>
  </div>
  <div class="input-view" contenteditable="true" bind:innerHTML={input}/>
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
    max-width: auto;
    margin: 0 auto;
    display: grid;
    background-image: linear-gradient(60deg, rgba(0, 255, 221, 1), rgb(89, 0, 255));
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: auto;
    }
  }
  /* Use median queries to manage css for different screen sizes this template is sorted by width */
  @media (min-width:320px)  { /* smartphones, iPhone, portrait 480x320 phones */ }
  @media (min-width:481px)  { /* portrait e-readers (Nook/Kindle), smaller tablets @ 600 or @ 640 wide. */ }
  @media (min-width:641px)  { /* portrait tablets, portrait iPad, landscape e-readers, landscape 800x480 or 854x480 phones */ }
  @media (min-width:961px)  { /* tablet, landscape iPad, lo-res laptops ands desktops */ }
  @media (min-width:1025px) { /* big landscape tablets, laptops, and desktops */ }
  @media (min-width:1281px) { /* hi-res laptops and desktops */ }
  
  /* @mixin glassProperty{
    
  } */
  .history-view{
    grid-template-columns: auto;
    text-align: left;
    padding: 0.4em;
    height: auto;
    row-gap: 1em;
    font-size: 1.75em;
    color: white;
  }
  .history-view > span{
    padding-left: 0.4em;
  }

  .output-view {
    text-align: left;
    background-color: rgba(255, 255, 255, 0.6);
    color: white;
    font-size: 1.5em;
    min-height: 1em;
    padding: 0.2em;
    border-radius: 16px;
  }
  .input-view {
    text-align: left;
    padding: 0.4em;
    margin: 0.25em;
    height: auto;
    font-size: 2em;
    color:white;

     /* From https://css.glass */
    background: rgba(255, 255, 255, 0.418);
    border-radius: 16px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(40px);
    -webkit-backdrop-filter: blur(40px);
  }

  .number-pad {
    display: grid;
    grid-template-columns: auto auto auto auto auto;
    grid-gap: 0.75em;
    background-color: rgba(255, 255, 255, 0.7);
    padding: 0.75em;
    margin: 0.25em;
    border-radius: 1em;
  }
</style>
