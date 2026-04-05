<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>3D Sneaker Pro</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{
    font-family: Arial;
    background:#0f172a;
    color:white;
    overflow-x:hidden;
}

/* SECTIONS */
section{
    height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    flex-direction:column;
    font-size:2rem;
}

/* CANVAS */
#canvas-container{
    position:fixed;
    top:0;
    left:0;
    width:100%;
    height:100%;
    z-index:-1;
}

/* UI */
.title{
    position:fixed;
    top:20px;
    left:50%;
    transform:translateX(-50%);
    font-size:2rem;
    z-index:10;
}
</style>
</head>

<body>

<div class="title">SneakerVerse Pro</div>
<div id="canvas-container"></div>

<section id="home">Home</section>
<section id="about">About</section>
<section id="gallery">Gallery</section>
<section id="contact">Contact</section>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>

<script>
// ===== SCENE =====
const scene = new THREE.Scene();

const camera = new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight,0.1,1000);
camera.position.set(0,2,4);

const renderer = new THREE.WebGLRenderer({antialias:true});
renderer.setSize(window.innerWidth,window.innerHeight);
document.getElementById("canvas-container").appendChild(renderer.domElement);

// ===== CONTROLS =====
const controls = new THREE.OrbitControls(camera,renderer.domElement);
controls.enableDamping = true;

// ===== LIGHT =====
const light = new THREE.DirectionalLight(0xffffff,1);
light.position.set(5,5,5);
scene.add(light);

// ===== SNEAKER (SIMPLIFIED) =====
let sneakerMesh;

function createSneaker(){
    const group = new THREE.Group();

    const body = new THREE.Mesh(
        new THREE.BoxGeometry(1.5,0.6,3),
        new THREE.MeshStandardMaterial({color:0xff4d4d})
    );
    body.position.y=0.3;

    const sole = new THREE.Mesh(
        new THREE.BoxGeometry(1.7,0.2,3.2),
        new THREE.MeshStandardMaterial({color:0x222})
    );
    sole.position.y=-0.1;

    group.add(body);
    group.add(sole);

    sneakerMesh = group;
    scene.add(group);
}

createSneaker();

// ===== ANIMATION =====
function animate(){
    requestAnimationFrame(animate);

    if(sneakerMesh){
        sneakerMesh.rotation.y += 0.01;
    }

    controls.update();
    renderer.render(scene,camera);
}
animate();

// ===== SCROLL ANIMATION =====
const sections = Array.from(document.querySelectorAll("section"));

const shiftPositions = [0,1.5,-1,1];
const rotationPositions = [0,Math.PI/4,Math.PI,-Math.PI/4];

window.addEventListener("scroll",()=>{
    const scrollY = window.scrollY;

    sections.forEach((section,index)=>{
        const offset = section.offsetTop;

        if(scrollY >= offset - 200){
            if(sneakerMesh){
                sneakerMesh.position.y = shiftPositions[index];
                sneakerMesh.rotation.y = rotationPositions[index];
            }
        }
    });
});

// ===== RESIZE =====
window.addEventListener("resize",()=>{
    camera.aspect = window.innerWidth/window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth,window.innerHeight);
});
</script>

</body>
</html>
