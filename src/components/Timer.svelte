<script>
    export let onEnd = () => {};
    export let initialTime = 10;

    export let time = initialTime;
    
    const interval = setInterval(() => {time--}, 1000)

    $: {
        if (time <= 0 ){
            onEnd();
            clearInterval(interval)
        }
    }
</script>

<h1 class="d-flex">{time}</h1>