<script>
    export let limitTime = 1;
    export let initialTime = 0;

    export let time = initialTime;
    
    const interval = setInterval(() => {time++}, 1000)

    $: {
        if (time >= limitTime ){
            clearInterval(interval)
        }
    }
</script>

<h1 class="text-center">{time}</h1>