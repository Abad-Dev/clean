<script>
    export let onClick = () => {};
    export let clicked = false;
  
</script>
  
<button on:click={() => {
    if (!clicked){onClick();}
    clicked=true;}}>
<svg viewBox="0 0 100 100" width="50" height="50">
    <circle cx="50" cy="50" r="40" fill={clicked ? 'gray' : 'lime'} />
</svg>
</button>
  
<style>
button {
    background-color: transparent;
    border: none;
    padding: 0;
}

</style>