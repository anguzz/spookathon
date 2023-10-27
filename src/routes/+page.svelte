<script>
import Cowboy from './cowboy.svelte';   
import { onMount } from 'svelte';
  
let spawnPoints = [
    { left: 525, top: 200 },//top left
    { left: 900, top: 200 },//top right
    { left: 370, top: 420 },//bottom left
    { left: 1000, top: 420 },//bottom right
    { left: 690, top: 420 }//middle
  ];

  let cowboys = [];
  let score = 0;

  function spawnCowboy() {
    // Choose a random spawn point from the predefined spawnPoints array
    const randomSpawnPoint = spawnPoints[Math.floor(Math.random() * spawnPoints.length)];
    cowboys = [...cowboys, { left: randomSpawnPoint.left, top: randomSpawnPoint.top }];
  }

 
  
   
  function handleClick() {
    score++;
    alert(`You clicked a cowboy! Your score: ${score}`);
  }
  
    onMount(() => {
      setInterval(spawnCowboy, 2000);
    });
  </script>
  
 
  <div id="bg">


        <div class="score">Score: {score}</div>
        {#each cowboys as { left, top }, index}
          <Cowboy {left} {top} />
        {/each}
</div>

   <style>
    

    .score {
    position: fixed;
    top: 10px;
    left: 10px;
    color:red;
    font-size: 24px;
    font-weight: bold;
  }


	#bg {
	  width: 100%;
	height: 100vh;
	color: #fff;
	background-image: url("/src/background.jpg");	
    background-size: 100% 100%;
	position: relative;
    overflow-y: hidden;
	overflow: hidden;
	}
	* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
    scrollbar-width: thin; /* For Firefox */
    scrollbar-color: transparent transparent; /* For Firefox */
    -ms-overflow-style: none; /* For Internet Explorer and Edge */
}

  </style>
