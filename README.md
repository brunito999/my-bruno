bruni

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Bruno game</title>

<style>
body{
  margin:0;
  font-family:Arial;
  text-align:center;
  color:white;
  background:#222;
  transition:0.4s;
}

.room{ display:none; padding:20px; }
.active{ display:block; }

/* BRUNO */
#bruno{
  width:150px;
  margin:30px auto;
  transition:0.3s;
}

.head{
  width:100px;
  height:100px;
  background:#f5c77a;
  border-radius:50%;
  margin:0 auto;
  position:relative;
}

.eye{
  width:20px;
  height:20px;
  background:black;
  border-radius:50%;
  position:absolute;
  top:35px;
}

.eye.left{ left:25px; }
.eye.right{ right:25px; }

.mouth{
  width:40px;
  height:20px;
  border-bottom:4px solid black;
  border-radius:0 0 40px 40px;
  position:absolute;
  bottom:25px;
  left:30px;
}

.hat{
  width:110px;
  height:35px;
  background:yellow;
  position:absolute;
  top:-15px;
  left:-5px;
  border-radius:20px 20px 0 0;
}

.body{
  width:110px;
  height:70px;
  background:green;
  margin:0 auto;
}

.pants{
  width:110px;
  height:50px;
  background:blue;
  margin:0 auto;
}

button{
  padding:10px;
  margin:5px;
  border:none;
  border-radius:10px;
  font-weight:bold;
  cursor:pointer;
}

@keyframes eatMove{
  0%{ transform:translateY(0px); }
  50%{ transform:translateY(10px); }
  100%{ transform:translateY(0px); }
}

.eating{
  animation:eatMove 0.3s infinite;
}
</style>
</head>

<body>

<h1>MY BRUNO 3.0</h1>
<h3>Monedas: <span id="coinText">0</span></h3>
<h3>Hambre: <span id="hungerText">50</span></h3>

<!-- SALA -->
<div id="living" class="room active">
  <div id="bruno">
    <div class="head">
      <div class="hat"></div>
      <div class="eye left"></div>
      <div class="eye right"></div>
      <div class="mouth"></div>
    </div>
    <div class="body"></div>
    <div class="pants"></div>
  </div>

  <button onclick="earn()">Jugar (+5)</button>
  <button onclick="feed()">Comer (-2)</button>
  <button onclick="goRoom('store')">Tienda</button>
</div>

<!-- TIENDA -->
<div id="store" class="room">
  <h2>Tienda</h2>

  <h3>Colores Fondo</h3>
  <button onclick="buyBackground('#1e3a8a',10)">Azul (10)</button>
  <button onclick="buyBackground('#14532d',10)">Verde (10)</button>
  <button onclick="buyBackground('#7f1d1d',10)">Rojo (10)</button>

  <h3>Ropa</h3>
  <button onclick="buyShirt('red',15)">Camisa Roja (15)</button>
  <button onclick="buyPants('black',15)">Pantalón Negro (15)</button>
  <button onclick="buyHat('orange',20)">Gorro Naranja (20)</button>

  <br><br>
  <button onclick="goRoom('living')">Volver</button>
</div>

<script>

let coins = localStorage.getItem("coins") ? parseInt(localStorage.getItem("coins")) : 0;
let hunger = localStorage.getItem("hunger") ? parseInt(localStorage.getItem("hunger")) : 50;

let coinText = document.getElementById("coinText");
let hungerText = document.getElementById("hungerText");
let bruno = document.getElementById("bruno");
let mouth = document.querySelector(".mouth");

update();

function update(){
  coinText.innerText = coins;
  hungerText.innerText = hunger;
  localStorage.setItem("coins", coins);
  localStorage.setItem("hunger", hunger);
}

function earn(){
  coins += 5;
  update();
}

function feed(){
  if(coins >= 2){
    coins -= 2;
    bruno.classList.add("eating");
    mouth.style.borderBottom = "4px solid red";
    setTimeout(()=>{
      bruno.classList.remove("eating");
      mouth.style.borderBottom = "4px solid black";
      hunger += 10;
      update();
    },1000);
  } else {
    alert("No tienes monedas!");
  }
}

function goRoom(room){
  document.querySelectorAll(".room").forEach(r=>r.classList.remove("active"));
  document.getElementById(room).classList.add("active");
}

function buyBackground(color,price){
  if(coins>=price){
    document.body.style.background=color;
    coins-=price;
    update();
  }else alert("No tienes monedas!");
}

function buyShirt(color,price){
  if(coins>=price){
    document.querySelector(".body").style.background=color;
    coins-=price;
    update();
  }else alert("No tienes monedas!");
}

function buyPants(color,price){
  if(coins>=price){
    document.querySelector(".pants").style.background=color;
    coins-=price;
    update();
  }else alert("No tienes monedas!");
}

function buyHat(color,price){
  if(coins>=price){
    document.querySelector(".hat").style.background=color;
    coins-=price;
    update();
  }else alert("No tienes monedas!");
}

</script>

</body>
</html>
