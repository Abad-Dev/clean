<script>
// @ts-nocheck
    import { goto } from '$app/navigation';
    import Timer from "../components/Timer.svelte";
    import Circle from "../components/circle.svelte";
    let intro = 0;

    let retry = false;
    let handleRetry = (bool) => {
        retry = bool;
    }

    const setIntro = (value) => {
        intro = value
    }

    const allClean = (arr) => {
        for(let i = 1; i < arr.length; i++) {
            if(arr[i] !== arr[0]) {
                return false;
            }
        }
        return true;
    }
    let introArray = [true, true, false, false]
    $:{
        if (allClean(introArray)){
            intro = 2;
        }
    }
</script>

<div class="container d-flex align-items-center justify-content-center" style="min-height: 100vh;">
    <div class="row text-light">
        {#if intro == 0}
        <div class="col-12 d-flex align-items-center justify-content-center">
            <h1>Bienvenido a</h1>
        </div>
        <div class="col-12 d-flex justify-content-center">
            <i class="blur-in clean ms-4">CLEAN!</i>
        </div>
        <div class="col-12 d-flex justify-content-center">
            <button type="button" class="btn btn-light" on:click={() => setIntro(1)}>Empezar</button>
        </div>
        {/if}


        {#if intro == 1}
            <div class="col-12 d-flex align-items-center justify-content-center mb-5">
                <h3 class="text-center">El juego consiste en darle a los circulos verdes antes que acabe el tiempo:</h3>
            </div>

            {#if !retry}
            <div class="col-12 d-flex justify-content-center">
                <h1>Tiempo:</h1>&nbsp;<Timer initialTime={20} onEnd={()=>{handleRetry(true)}}/>
            </div>
            <div class="col-12 d-flex align-items-center justify-content-center">
                {#each introArray as clicked}
                    <Circle clicked={clicked} onClick={()=>{clicked=true;}}/>
                {/each}
            </div>
                    
            {:else}
                <div class="col-12 d-flex justify-content-center">
                    <h1>Tiempo:</h1>&nbsp;<h1>0</h1>
                </div>
                <div class="col-12 d-flex justify-content-center">
                    <button type="button" class="btn btn-light" on:click={() => {introArray = [true, true, false, false]; handleRetry(false)}}>Reintentar</button>
                </div>
            {/if}
        {/if}


        {#if intro == 2}
            <h1 class="clean text-center mb-5">CLEAN!</h1>
            <div class="col-12 d-flex align-items-center justify-content-center">
                <h3 class="text-center">Cuando consigues apagar todos los circulos obtienes un <i class="mini-clean">CLEAN</i>
                    y avanzas de nivel.  
                </h3>
            </div>
            <div class="col-12 d-flex align-items-center justify-content-center mb-5">
                <button type="button" class="btn btn-light" on:click={() => setIntro(3)}>Siguiente</button>
            </div>
        {/if}


        {#if intro == 3}
            <h1 class="clean text-center mb-5">CLEAN!</h1>
            <div class="col-12 d-flex align-items-center justify-content-center">
                <h3 class="text-center mb-5">Mientras mas niveles completes, el juego se irá volviendo mas complicado,
                    tu meta es alcanzar el máximo nivel que puedas.
                    <br />
                    <br />
                    Aceptas el reto?
                </h3>
            </div>
            <div class="col-12 d-flex align-items-center justify-content-center">
                <button type="button" class="btn btn-light" on:click={() => goto('/game')}>Acepto el reto!</button>
            </div>
        {/if}

    </div>
</div>

<style>
    .mini-clean{
        color: lime;
    }

    .clean{
        color: lime;
        font-size: 60px;
    }
    .blur-in{
        animation:blur-in 3s linear both
    } 
    @keyframes blur-in{
        0%{filter:blur(12px);opacity:0}
        100%{filter:blur(0);opacity:1}
    }
</style>
