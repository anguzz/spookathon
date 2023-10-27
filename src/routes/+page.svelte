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
  
    function isTooClose(newSpawnPoint, existingCowboys) {
      const minDistance = 10; // Minimum distance between cowboys
      for (const cowboy of existingCowboys) {
        const distance = Math.sqrt(Math.pow(newSpawnPoint.left - cowboy.left, 2) + Math.pow(newSpawnPoint.top - cowboy.top, 2));
        if (distance < minDistance) {
          return true; // The new spawn point is too close to an existing cowboy
        }
      }
      return false; // The new spawn point is not too close to any existing cowboy
    }
  
    function spawnCowboy() {
      const randomSpawnPoint = spawnPoints[Math.floor(Math.random() * spawnPoints.length)];
      if (!isTooClose(randomSpawnPoint, cowboys)) {
        const cowboy = { id: Date.now(), left: randomSpawnPoint.left, top: randomSpawnPoint.top };
        cowboys = [...cowboys, cowboy];
      }
    }
  
    function handleCowboyClick(clickedId) {
      cowboys = cowboys.filter(cowboy => cowboy.id !== clickedId); // Remove the clicked cowboy
      score++;
    }
  
    onMount(() => {
      setInterval(spawnCowboy, 2000);
    });
  </script>
  
  <div id="bg">
    <div class="score">Score: {score}</div>
    {#each cowboys as { id, left, top }, index}
      <Cowboy {id} {left} {top} onCowboyClick={handleCowboyClick} />
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
      background-image: url("/src/cowboy.jpg");
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
  </style>
  