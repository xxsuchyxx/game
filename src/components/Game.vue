<template>
  <div class="gameContainer">
    <canvas id="canvas" width="728" height="512"></canvas>
    <!-- <div>this.score: {{ this.score }}</div> -->
    <button v-on:click="addScore">Add Score</button>
  </div>
</template>
<script>
/* eslint-disable */
import submarineImg from '../images/submarine.png'
import enemyImg from '../images/smallrock.png'
import rewardImg from '../images/reward.png'
import bgImg from '../images/bg.jpg'
import groundImg from '../images/ground.png'
import rockNImg from '../images/upperRock.png'
import rockSImg from '../images/rock.png'
import audio1 from '../sounds/move.mp3'
import audio2 from '../sounds/score.mp3'
export default {
  name: "Game",
  data: function(){
    return{
      gap: 95,
      score: 0,
    }
  },
  methods:{
    addScore:function(){
      this.score += 5;
    }
  },
  mounted() {
    console.log(this.this.score);
    const cvs = document.getElementById("canvas");
    const ctx = cvs.getContext("2d");
    const submarine = new Image();
    const reward = new Image();
    const bg = new Image();
    const ground = new Image();
    const upperRock = new Image();
    const rockSouth = new Image();
    const enemyRock = new Image();
    submarine.src = submarineImg;
    bg.src = bgImg;
    ground.src = groundImg;
    upperRock.src = rockNImg;
    rockSouth.src = rockSImg;
    reward.src = rewardImg;
    enemyRock.src = enemyImg;


// poczatkowe wartosci

const gap = this.gap;
let constant;
let posX = 10;
let posY = 150;
const gravity = 0.5;
let moveY = 0;
let moveX = 0;
// let this.score = 0;

// dzwieki
const move = new Audio();
const scor = new Audio();

move.src = audio1;
scor.src = audio2;

// on key down

document.addEventListener("keydown",(e)=>{
    switch(e.keyCode){
        case 40:
            console.log('down');
            moveDown();
        break;
        case 38:
            console.log('up');
            moveUp();
        break;
        case 39:
            console.log('right');
            moveRight();
        break;
        case 37:
            console.log('left');
            moveLeft();
        break;
    }
 });

function moveUp(){
    
    move.play();
    moveY -= 0.15;
}
function moveDown(){
 
    move.play();
    moveY += 0.15;
}
function moveRight(){
   move.play();
    moveX += 0.15;
}
function moveLeft(){
 move.play();
    moveX -= 0.15;
}


// rock coordinates

let rock = [];

rock[0] = {
    x : cvs.width,
    y : 0
};

 

let diamond = [];

diamond[0] = {
    x : 115,
    y : 80
};


let enemy = [];

enemy[0] = {
    x : 320,
    y : 120
};

function draw(){
    ctx.drawImage(bg,0,0);
    for(let i = 0; i < rock.length; i++){
         
        constant = upperRock.height+gap;
        ctx.drawImage(upperRock,rock[i].x,rock[i].y);
        ctx.drawImage(rockSouth,rock[i].x,rock[i].y+constant);
        rock[i].x--;
        
        if( rock[i].x == 355 ){
            rock.push({
                x : cvs.width,
                y : Math.floor(Math.random()*upperRock.height)-upperRock.height
            }); 
        }

        // detect collision
        
        if( 
          posX + submarine.width >= rock[i].x && 
          posX <= rock[i].x + upperRock.width && 
          (posY <= rock[i].y + upperRock.height || 
          posY+submarine.height >= rock[i].y+constant) || 
          posY + submarine.height >=  cvs.height - ground.height){
           location.reload(); 
         }
        
        if(rock[i].x == 5){
            this.score++;
            scor.play();
        }        
    }

  for(let i = 0; i < diamond.length; i++){

        constant = reward.height+gap;
        ctx.drawImage(reward,diamond[i].x,diamond[i].y);
            
        diamond[i].x--;
      
        if( diamond[i].x == 20 ){
            diamond.push({
                x : cvs.width,
                y : Math.floor(Math.random()*reward.height)-reward.height + 130
            }); 
        }

        // detect collision
        
        if( 
          posX + submarine.width >= diamond[i].x && 
          posY + submarine.height >= diamond[i].y && 
          posX <= diamond[i].x + reward.width && 
          posY <= diamond[i].y + reward.height)
          {
            console.log();
            diamond[i].y -= 511;
            scor.play();
            this.score+=5;
           
  
         }  
    }
    
     for(let i = 0; i < enemy.length; i++){

        constant = enemyRock.height+gap;
        ctx.drawImage(enemyRock,enemy[i].x,enemy[i].y);
            
        enemy[i].x--;
      
        if( enemy[i].x == 20 ){
            enemy.push({
                x : cvs.width,
                y : Math.floor(Math.random()*enemyRock.height)-enemyRock.height
            }); 
        }
 
        enemy[i].y += gravity;
  

        // detect collision
        
        if( 
          posX + submarine.width >= enemy[i].x && 
          posY + submarine.height >= enemy[i].y && 
          posX <= enemy[i].x + enemyRock.width && 
          posY <= enemy[i].y + enemyRock.height)
          {
            console.log();
            enemy[i].y -= 511;
            scor.play();
            this.score-=15;
            if (this.score < 0){
              location.reload(); 
            }
          
         }  
    }

    ctx.drawImage(ground,0,cvs.height - ground.height);
    
    ctx.drawImage(submarine,posX,posY);
   
    
    posY += moveY;
    posX += moveX;

    
    ctx.fillStyle = "#e9e100";
    ctx.font = "bold 20px Verdana";
    ctx.fillText("Score : "+this.score,10,cvs.height-20);
    requestAnimationFrame(draw);
}

draw();
  }
};
</script>
<style lang="scss">
  #canvas{
    border: 1px solid black;
 
        transition: width .5s, height .5s, top .5s, left .5s;
 
  }
  body {
    max-height: 100vh;
    overflow: hidden;
  }
  .gameContainer {
    height: 100%;
  }
</style>

