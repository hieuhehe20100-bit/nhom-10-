<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Bài của nhóm 10 - 3D</title>
<style>
body { margin: 0; overflow: hidden; font-family: Arial; }
#menu {
position:absolute;
width:100%;
height:100%;
background:#111;
color:white;
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
z-index:10;
}
button{padding:10px 20px;margin:10px;font-size:16px;cursor:pointer;}
#questionBox{
position:absolute;
bottom:10px;
left:50%;
transform:translateX(-50%);
background:white;
padding:15px;
border-radius:10px;
width:60%;
display:none;
}
</style>
</head>
<body>

<div id="menu">
<h1>Bài của nhóm 10</h1>
<h3>Thành viên:</h3>
<p>Đạt - Hiếu - Huy - Duy</p>
<button onclick="startGame()">Bắt đầu</button>
</div>

<div id="questionBox"></div>

<script src="https://cdn.jsdelivr.net/npm/three@0.150.1/build/three.min.js"></script>

<script>
let scene, camera, renderer, cube;
let gravity = -0.02;
let velocity = 0;
let dragging = false;

let questions = [
{q:"1. Công thức động năng?",a:["mgh","1/2mv^2","Fs"],c:1},
{q:"2. Vật 2kg v=3m/s. Động năng?",a:["9J","6J","18J"],c:0},
{q:"3. Thế năng trọng trường?",a:["mgh","mv","Pt"],c:0},
{q:"4. Cơ năng bằng?",a:["Wđ+Wt","Wt-Wđ","m/v"],c:0},
{q:"5. 1kg cao 10m (g=10). Thế năng?",a:["100J","10J","50J"],c:0},
{q:"6. Khi rơi tự do, động năng?",a:["Giảm","Tăng","Không đổi"],c:1},
{q:"7. Không ma sát, cơ năng?",a:["Không đổi","Giảm","Tăng"],c:0},
{q:"8. 3kg v=4m/s. Động năng?",a:["24J","12J","48J"],c:0},
{q:"9. Đơn vị cơ năng?",a:["J","N","m/s"],c:0},
{q:"10. Động năng phụ thuộc?",a:["Khối lượng và vận tốc","Độ cao","Thời gian"],c:0}
];

let currentQ = 0;

function startGame(){
document.getElementById("menu").style.display="none";
init();
showQuestion();
}

function init(){
scene = new THREE.Scene();
camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

let geometry = new THREE.BoxGeometry();
let material = new THREE.MeshStandardMaterial({color:0x00ffcc});
cube = new THREE.Mesh(geometry, material);
cube.position.y = 5;
scene.add(cube);

let planeGeo = new THREE.PlaneGeometry(20,20);
let planeMat = new THREE.MeshStandardMaterial({color:0x555555});
let plane = new THREE.Mesh(planeGeo, planeMat);
plane.rotation.x = -Math.PI/2;
scene.add(plane);

let light = new THREE.PointLight(0xffffff,1);
light.position.set(10,10,10);
scene.add(light);

camera.position.z = 10;
animate();
}

function animate(){
requestAnimationFrame(animate);

if(!dragging){
velocity += gravity;
cube.position.y += velocity;
if(cube.position.y < 0.5){
cube.position.y = 0.5;
velocity = 0;
}
}
renderer.render(scene,camera);
}

window.addEventListener("mousedown",()=> dragging=true);
window.addEventListener("mouseup",()=> dragging=false);

window.addEventListener("mousemove",(e)=>{
if(dragging){
cube.position.x = (e.clientX/window.innerWidth)*10-5;
cube.position.y = (1-(e.clientY/window.innerHeight))*10;
velocity = 0;
}
});

document.addEventListener("keydown",(e)=>{
if(e.key==="Escape"){
alert("Thoát game!");
location.reload();
}
});

function showQuestion(){
let box = document.getElementById("questionBox");
box.style.display="block";
let q = questions[currentQ];
box.innerHTML="<h3>"+q.q+"</h3>";
q.a.forEach((opt,i)=>{
box.innerHTML+=`<button onclick="check(${i})">${opt}</button><br>`;
});
}

function check(i){
if(i===questions[currentQ].c){
alert("Đúng!");
}else{
alert("Sai!");
}
currentQ++;
if(currentQ<questions.length){
showQuestion();
}else{
alert("Hoàn thành game!");
}
}
</script>

</body>
</html>
