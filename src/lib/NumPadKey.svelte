<script>
    import { createEventDispatcher } from 'svelte';

    export let symbol;
    export let value;

    const dispatch = createEventDispatcher();
    
    const symbolToOps = {
        "DEL":  ()=>value.slice(0, -1),
        "AC":   ()=>"",
        "MULT": ()=>value + "*",
        "DIV":  ()=>value + "/",
        "ADD":  ()=>value + "+",
        "SUB":  ()=>value + "-",
        "EXP":  ()=>value + "^",
        "=":    ()=>{
            dispatch('equal');
            return value;
        }
    }

    function emmitKeyPress() {
        console.log(symbol);
        if (symbolToOps && symbolToOps[symbol]) {
            value = symbolToOps[symbol]();
        }else{
            value = value+symbol;
        }
    }
</script>

<div class="numberPadKey">
    <button on:click={emmitKeyPress}> 
        {symbol}
    </button>
</div>

<style>
    div {
		background-color: rgba(255, 255, 255, 0.8);
		text-align: center;
		font-size: 30px;
	}
	button {
		width: 100%;
		height: 100%;
		background-color: rgba(255, 255, 255, 0);
		border: none;
		margin: 0;
		padding: 0.4em;
	}
</style>