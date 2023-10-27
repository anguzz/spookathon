<script>
    import Cowboy from './Cowboy.svelte';
    import Frog from './Frog.svelte';
    import { onMount } from 'svelte';
  
    let spawnPoints = [
      { left: 525, top: 200 }, // top left
      { left: 900, top: 200 }, // top right
      { left: 370, top: 420 }, // bottom left
      { left: 1000, top: 420 }, // bottom right
      { left: 690, top: 420 } // middle
    ];
  
    let cowboys = [];
    let frogs = [];
    let score = 0;

  
  
    function isTooClose(newSpawnPoint, existingEntities) {
      const minDistance = 5; // Minimum distance between entities
      for (const entity of existingEntities) {
        const distance = Math.sqrt(Math.pow(newSpawnPoint.left - entity.left, 2) + Math.pow(newSpawnPoint.top - entity.top, 2));
        if (distance < minDistance) {
          return true; // The new spawn point is too close to an existing entity
        }
      }
      return false; // The new spawn point is not too close to any existing entity
    }
  
 
    function spawnCowboy() {
    const randomSpawnPoint = spawnPoints[Math.floor(Math.random() * spawnPoints.length)];
    if (!isTooClose(randomSpawnPoint, [...frogs, ...cowboys])) {
      const cowboy = { id: Date.now(), left: randomSpawnPoint.left, top: randomSpawnPoint.top };
      cowboys = [...cowboys, cowboy];

      // Set a timer to remove the cowboy after 2 seconds
      setTimeout(() => {
        cowboys = cowboys.filter(cowboy => cowboy.id !== cowboy.id);
      }, 2000);
    }
  }

  function spawnFrog() {
    const frogNames = ['Hoppy', 'Ribbit', 'Croaky', 'Jumpy', 'Lily']; // Frog names
    const randomSpawnPoint = spawnPoints[Math.floor(Math.random() * spawnPoints.length)];
    const newFrog = {
      id: Date.now(),
      name: frogNames[Math.floor(Math.random() * frogNames.length)], // Random frog name
      left: randomSpawnPoint.left,
      top: randomSpawnPoint.top
    };

    if (!isTooClose(randomSpawnPoint, [...frogs, ...cowboys])) {
      frogs = [...frogs, newFrog];

      // Set a timer to remove the frog after 2 seconds
      setTimeout(() => {
        frogs = frogs.filter(frog => frog.id !== newFrog.id);
      }, 2000);
    }
  }
    function handleCowboyClick(clickedId) {
      cowboys = cowboys.filter(cowboy => cowboy.id !== clickedId);
      score++;
    }
  
    function handleFrogClick(clickedId) {
  frogs = frogs.filter(frog => frog.id !== clickedId);
  score--; // Decrement the score when a frog is clicked
}

  
    onMount(() => {
      setInterval(spawnCowboy, 2000);
      setInterval(spawnFrog, 3000);
    });


    let timer = 30; // Set the initial timer value in seconds

    const cowboyInterval = setInterval(spawnCowboy, 2000);
  const frogInterval = setInterval(spawnFrog, 3000);

function updateTimer() {
  if (timer > 0) {
    timer--; // Decrement the timer every second
  } else {
    clearInterval(timerInterval); // Stop the timer when it reaches zero
    clearInterval(cowboyInterval); // Stop spawning cowboys
    clearInterval(frogInterval); // Stop spawning frogs
    //alert("Game Over! Your final score: " + score);
    timer=0;

  }
}




// Start the timer when the component is mounted
const timerInterval = setInterval(updateTimer, 1000);
  </script>
  
  <div id="bg">
    <div class="score">Score: {score}</div>
    <div class="timer">Time Left: {timer} seconds</div>
    {#each cowboys as { id, left, top }, index}
      <Cowboy {id} {left} {top} onCowboyClick={handleCowboyClick} />
    {/each}
    {#each frogs as { id, name, left, top }, index}
      <Frog {id} {name} {left} {top} onFrogClick={handleFrogClick} />
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
  
    .cowboy, .frog {
      width: 100px;
      height: 100px;
      position: absolute;
      cursor: pointer;
    }
  
    .frog img {
      width: 100px;
      height: auto;
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

    .timer {
    position: fixed;
    top: 40px;
    left: 10px;
    color: blue;
    font-size: 24px;
    font-weight: bold;
  }
  </style>
  