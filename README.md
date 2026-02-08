# sayanggg<!DOCTYPE html>
<html>
<head>
<title>Anniversary Love 3D</title>
<style>
body{
margin:0;
overflow:hidden;
background:radial-gradient(circle,#20002c,#000);
font-family:Georgia,serif;
}

canvas{display:block;}

#prompter{
position:absolute;
bottom:-100%;
width:100%;
text-align:center;
font-size:2.3rem;
color:white;
letter-spacing:4px;
text-shadow:0 0 15px #ff4da6,0 0 35px #ff0077;
animation:scroll 25s linear infinite, float 3s ease-in-out infinite;
pointer-events:none;
}

@keyframes scroll{
from{bottom:-100%}
to{bottom:110%}
}

@keyframes float{
0%{transform:translateY(0)}
50%{transform:translateY(-15px)}
100%{transform:translateY(0)}
}
</style>
</head>

<body>

<div id="prompter"></div>

<audio autoplay loop>
<source src="https://cdn.pixabay.com/download/audio/2023/03/27/audio_7d9bfa38db.mp3" type="audio/mpeg">
</audio>

<script type="importmap">
{
 "imports":{
  "three":"https://unpkg.com/three@0.160.0/build/three.module.js"
 }
}
</script>

<script type="module">
import * as THREE from 'three';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75,innerWidth/innerHeight,0.1,1000);
const renderer = new THREE.WebGLRenderer({alpha:true,antialias:true});
renderer.setSize(innerWidth,innerHeight);
document.body.appendChild(renderer.domElement);

/* STARFIELD */
const starsGeo=new THREE.BufferGeometry();
const count=2000;
const arr=new Float32Array(count*3);
for(let i=0;i<arr.length;i++) arr[i]=(Math.random()-0.5)*1000;
starsGeo.setAttribute("position",new THREE.BufferAttribute(arr,3));
scene.add(new THREE.Points(starsGeo,new THREE.PointsMaterial({color:0xffffff,size:0.7})));

/* HEART */
const heartShape=new THREE.Shape();
heartShape.moveTo(5,5);
heartShape.bezierCurveTo(5,5,4,0,0,0);
heartShape.bezierCurveTo(-6,0,-6,7,-6,7);
heartShape.bezierCurveTo(-6,11,-3,15,5,19);
heartShape.bezierCurveTo(12,15,16,11,16,7);
heartShape.bezierCurveTo(16,7,16,0,10,0);
heartShape.bezierCurveTo(7,0,5,5,5,5);

const heart=new THREE.Mesh(
 new THREE.ExtrudeGeometry(heartShape,{depth:2,bevelEnabled:true}),
 new THREE.MeshPhongMaterial({
  color:0xff0055,
  shininess:120,
  emissive:0x220000
 })
);

heart.rotation.x=Math.PI;
heart.scale.set(.15,.15,.15);
scene.add(heart);

/* HEART TEXT (FOLLOW SHAPE) */
const loader = new THREE.FontLoader();
loader.load('https://threejs.org/examples/fonts/helvetiker_regular.typeface.json', font=>{
const textGeo=new THREE.TextGeometry("I LOVE YOU FOREVER",{
font:font,size:1,height:.2
});
const text=new THREE.Mesh(textGeo,new THREE.MeshBasicMaterial({color:0xff66cc}));
text.position.set(-6,4,2);
scene.add(text);
});

/* FIREWORKS */
const sparks=[];
const sparkGeo=new THREE.SphereGeometry(0.07,6,6);
const sparkMat=new THREE.MeshBasicMaterial({color:0xffcc00});

for(let i=0;i<120;i++){
const s=new THREE.Mesh(sparkGeo,sparkMat);
s.position.set((Math.random()-0.5)*20,Math.random()*15,(Math.random()-0.5)*20);
scene.add(s);
sparks.push(s);
}

/* LIGHT */
scene.add(new THREE.PointLight(0xffffff,800).position.set(10,15,10));
scene.add(new THREE.AmbientLight(0xffffff,.6));

camera.position.z=20;

/* PROMPTER TEXT */
const lines=[
"Happy Anniversary My Love ❤️",
"You Are My Forever 🌹",
"Thank You For Loving Me 💖",
"Every Moment With You Is Magic ✨",
"I Promise To Love You Always 💍"
];
const prompter=document.getElementById("prompter");
let i=0;
function update(){
prompter.innerHTML=lines[i];
i=(i+1)%lines.length;
}
update();
setInterval(update,4000);

/* ANIMATE */
let t=0;
function animate(){
requestAnimationFrame(animate);
t+=0.02;

heart.rotation.y+=0.01;
const pulse=1+Math.sin(t*2)*.1;
heart.scale.set(.15*pulse,.15*pulse,.15*pulse);

sparks.forEach(s=>{
s.position.y+=0.15;
if(s.position.y>15){
s.position.y=0;
s.position.x=(Math.random()-0.5)*20;
s.position.z=(Math.random()-0.5)*20;
}
});

renderer.render(scene,camera);
}
animate();

addEventListener("resize",()=>{
camera.aspect=innerWidth/innerHeight;
camera.updateProjectionMatrix();
renderer.setSize(innerWidth,innerHeight);
});
</script>

</body>
</html>
