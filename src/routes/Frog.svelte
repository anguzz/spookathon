<script>
    export let id;
    export let left;
    export let top;
    export let onFrogClick;
  
    function handleClick() {
      onFrogClick(id);
    }
  </script>
  
  <div class="frog" style="left: {left}px; top: {top}px" on:click={handleClick}></div>
  
  <style>
    .frog {
      width: 100px;
      height: 100px;
      position: absolute;
      background-image: url("/src/frog.jpg"); /* Replace with the path to your bartender image */
      background-size: cover;
      cursor: pointer;
    }
  </style>
  