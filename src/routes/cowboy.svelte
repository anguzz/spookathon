<script>
  export let left;
  export let top;

  function handleClick() {
    alert('You clicked a cowboy!');
  }
</script>

<div class="cowboy" style="left: {left}px; top: {top}px" on:click={handleClick}></div>

<style>
  .cowboy {
    width: 100px;
    height: 100px;
    position: absolute;
    background-image: url("/src/cowboy.jpg");
    background-size: cover;
    cursor: pointer;
  }
</style>
