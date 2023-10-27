<script>
    import Cowboy from './Cowboy.svelte';
    import { onMount } from 'svelte';
  
    let spawnPoints = [
      { left: 525, top: 200 }, // top left
      { left: 900, top: 200 }, // top right
      { left: 370, top: 420 }, // bottom left
      { left: 1000, top: 420 }, // bottom right
      { left: 690, top: 420 } // middle
    ];
  
    let cowboys = [];
    let score = 0;
  
    function spawnCowboy() {
      const randomSpawnPoint = spawnPoints[Math.floor(Math.random() * spawnPoints.length)];
      cowboys = [...cowboys, { left: randomSpawnPoint.left, top: randomSpawnPoint.top }];
    }
  
    function handleCowboyClick(cowboyId) {
    cowboys = cowboys.filter(cowboy => cowboy.id !== cowboyId); // Remove the clicked cowboy
    score++;
  }
    onMount(() => {
      setInterval(spawnCowboy, 2000);
    });
  </script>
  
  <div id="bg">
    <div class="score">Score: {score}</div>
    {#each cowboys as { left, top }, index}
      <Cowboy {left} {top} onCowboyClick={handleCowboyClick} />
    {/each}
  </div>
  
  <style>
    .score {
      position: fixed;
      top: 10px;
      left: 10px;
      color: red;
      font-size: 24px;
      font-weight: bold;
    }
  
    .cowboy {
      width: 100px;
      height: 100px;
      position: absolute;
      background-image: url("/src/cowboy.png");
      background-size: cover;
      cursor: pointer;
    }
  
    #bg {
      width: 100%;
      height: 100vh;
      color: #fff;
      background-image: url("/src/background.jpg");
      background-size: 100% 100%;
      position: relative;
      overflow-y: hidden;
    }
  
    /* Your other styles go here */
  </style>
  