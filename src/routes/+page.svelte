<script>
    import Timer from "../components/Timer.svelte";

    let currentTime = 0;

    
</script>

<div class="container-fluid d-flex align-items-center justify-content-center" style="min-height: 100vh; background-color: #323232">
    <div class="row text-light">
        <div class="col-12">
            <h1 class="text-center">
                <Timer bind:time={currentTime} />
            </h1>
        </div>
    </div>
</div>

