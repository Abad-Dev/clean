<script>
// @ts-nocheck
    import Timer from "../../components/Timer.svelte";
    import Circle from "../../components/circle.svelte";

    let start = false;
    let cleaned = false;
    let currentLevel = 0;
    let currentArr = [];
    let clickeds = 0;
    let lose = false;


    let levelsConfig = [
        {
            columns: 3,
            rows: 3,
            notClicked: 4,
            time: 30,
            gap: 2
        },
        {
            columns: 4,
            rows: 4,
            notClicked: 7,
            time: 20,
            gap: 5
        },
        {
            columns: 5,
            rows: 5,
            notClicked: 10,
            time: 20,
            gap: 10
        },
        {
            columns: 5,
            rows: 6,
            notClicked: 13,
            time: 20,
            gap: 15
        },
        {
            columns: 5,
            rows: 8,
            notClicked: 16,
            time: 15,
            gap: 20
        }
    ]

    let selectedLevel = {};

    const levelUp = () => {
        currentLevel++; // Pasa de ser lvl 0 a lvl 1 al principio
        if (currentLevel > 5){ // Este es el bucle infinito
            selectedLevel = {
                columns: 5,
                rows: 8,
                notClicked: 16,
                time: 20 - currentLevel,
                gap: 20
            }
            let notClickeds = selectedLevel.notClicked;
            let arr = Array.from({length: (selectedLevel.columns * selectedLevel.rows)}, (_, i) => {
                if (notClickeds != 0){ // If there is notClickeds
                    notClickeds--;
                    return false // return clicked = false
                } else{
                    return true
                }
            })
            arr.sort(() => Math.random() - 0.5);
            currentArr = arr;
        }
        else {
            selectedLevel = levelsConfig[currentLevel-1]
            let notClickeds = selectedLevel.notClicked;
            let arr = Array.from({length: (selectedLevel.columns * selectedLevel.rows)}, (_, i) => {
                if (notClickeds != 0){ // If there is notClickeds
                    notClickeds--;
                    return false // return clicked = false
                } else{
                    return true
                }
                
            })
            arr.sort(() => Math.random() - 0.5);
            currentArr = arr;
        }
    }
    levelUp()

    const restartGame = () => {
        currentLevel = 0;
        clickeds = 0;
        levelUp();
        lose = false;
        start = false;
    }

    $: {
        if (selectedLevel.notClicked === clickeds){
            cleaned=true;
            setTimeout(() => {cleaned=false;start=false;}, 2000)
            
            clickeds=0;
            levelUp()
        }
    }
</script>


<div class="container d-flex align-items-center justify-content-center text-light" style="min-height: 100vh;">
    {#if start==false}
    <div class="row">
        <div class="col-12 cover">
            <h1 class="text-center">Nivel:</h1>
        </div>
        <div class="col-12">
            <h1 class="text-center level">{currentLevel}</h1>
        </div>
        <div class="col-12 d-flex align-items-center justify-content-center">
            <button class="btn btn-light" on:click={() => {start=true;}}>Empezar</button>
        </div>
    </div>
    {:else if cleaned == true}
    <div class="row">
        <div class="col-12 d-flex flex-column align-items-center">
            <h1 class="clean">CLEAN!</h1>
            <small>
            {#if currentLevel-1 === 1}
                Eso estuvo facil, verdad? Vamos con algo mas complicado...
            {:else if currentLevel-1 === 2}
                Parece que se te da bien, subamos un poco el nivel
            {:else if currentLevel-1 === 3}
                A partir de acá las cosas se pondrán complicadas
            {:else if currentLevel-1 === 4}
                Sigue así intenta obtener todos los CLEAN que puedas
            {:else if currentLevel-1 === 10}
                Has llegado lejos, sigue así
            {:else if currentLevel-1 === 15}
                La mayoría no pasan de acá, veamos si tu puedes...
            {/if}
            </small>
        </div>
    </div>
    {:else if lose == true}
    <div class="row">
        <div class="col-12 d-flex flex-column align-items-center mb-3">
            <h1 class="lose">Perdiste!</h1>
            <small>Lo has hecho muy bien, felicitaciones por haber llegado al nivel: </small><h1 style="font-size: 40px; color: lime;">{currentLevel}</h1>
        </div>
        <div class="col-12 d-flex align-items-center justify-content-center">
            <button class="btn btn-light" on:click={() => restartGame()}>Reintentar</button>
        </div>
    </div>
    {:else} <!--Here the game happens-->
    <div class="row">
        <div class="col-12 d-flex justify-content-center align-items-center">
            <h1>Tiempo:</h1> <Timer initialTime={selectedLevel.time} onEnd={() => {lose=true;}}/>
        </div>
        <div class="col-12 d-flex justify-content-center align-items-center">
            <div style={
            `display: grid;

            grid-template-columns: repeat(${selectedLevel.columns}, 50px);
            grid-template-rows: repeat(${selectedLevel.rows}, 50px);
            gap: ${selectedLevel.gap}px;
            `}>
                {#each currentArr as clicked}
                    <Circle clicked={clicked} onClick={() => {clickeds++;}} />
                {/each}
            </div>
        </div>
    </div>
    {/if}


</div>


<style>
    .lose{
        color: indianred;
        text-decoration: underline;
        font-size: 70px;
    }
    .clean{
        color: lime;
        font-size: 80px;
    }
    .cover{
        background-color: #323232;
        z-index: 1;
    }
    .level{
        font-size: 40px;
        animation:level .6s linear both;
        color: lime;
    }

    @keyframes level{
        0%{letter-spacing:-.2em;transform:translateY(-49px);opacity:0}
        40%{opacity:.6;}
        100%{transform:translateY(0);opacity:1}
    }
</style>