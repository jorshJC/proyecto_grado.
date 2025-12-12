<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Dulce Tentación</title>
<style>
body {
    font-family: 'Comic Sans MS', cursive, sans-serif;
    background: linear-gradient(135deg, #c7f0f9, #e0f7ff, #d6f0ff);
    margin: 0; padding: 0;
}

header {
    background: #88d1f1;
    padding: 25px;
    text-align: center;
    color: white;
    font-size: 34px;
    font-weight: bold;
    text-shadow: 2px 2px #4aa3d3;
}

nav {
    background: #b2e0f7;
    display: flex;
    justify-content: center;
    padding: 12px 0;
    flex-wrap: wrap;
    position: relative;
}

nav a {
    margin: 0 25px;
    text-decoration: none;
    font-size: 20px;
    color: #046d9d;
    font-weight: bold;
    cursor: pointer;
    position: relative;
}

nav a.active {
    text-decoration: underline;
}

nav a span.notificacion {
    background: red;
    color: white;
    border-radius: 50%;
    padding: 2px 8px;
    font-size: 14px;
    position: absolute;
    top: -10px;
    right: -15px;
    transition: transform 0.3s;
}

nav a span.notificacion.salto {
    transform: scale(1.5);
}

h2 {
    text-align: center;
    color: #046d9d;
    margin-top: 20px;
}

.contenedor-productos {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 20px;
    padding: 20px;
}

.producto {
    background: white;
    border-radius: 15px;
    padding: 15px;
    box-shadow: 0 0 12px #88d1f1;
    transition: 0.3s;
}

.producto:hover {
    transform: scale(1.05);
}

.producto img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 12px;
}

button {
    background: #88d1f1;
    border: none;
    padding: 10px;
    border-radius: 10px;
    color: white;
    cursor: pointer;
    font-size: 16px;
    width: 100%;
}

button:hover {
    background: #4aa3d3;
}

input {
    padding: 10px;
    width: 80%;
    border-radius: 10px;
    border: 2px solid #88d1f1;
}

section {
    display: none;
}

section.active {
    display: block;
}

#bienvenida {
    text-align: center;
    padding: 40px 20px;
    font-size: 20px;
    color: #046d9d;
}

#bienvenida img {
    width: 250px;
    border-radius: 15px;
    margin-top: 20px;
}

#carrito, #pago {
    padding: 20px;
    background: #e0f7ff;
    margin: 20px;
    border-radius: 15px;
    box-shadow: 0 0 10px #88d1f1;
}
</style>
</head>
<body>

<header>DULCE TENTACIÓN</header>

<nav>
    <a onclick="mostrarSeccion('inicio')" class="active">Inicio</a>
    <a onclick="mostrarSeccion('pasteles')">Pasteles</a>
    <a onclick="mostrarSeccion('postres')">Postres</a>
    <a onclick="mostrarSeccion('ofertas')">Ofertas</a>
    <a onclick="mostrarSeccion('carrito')">Carrito <span id="notificacionCarrito" class="notificacion" style="display:none">0</span></a>
</nav>

<!-- INICIO -->
<section id="inicio" class="active">
    <div id="bienvenida">
        <h2>¡Bienvenido a Dulce Tentación!</h2>
        <p>Nos alegra tenerte aquí. Disfruta de nuestros deliciosos pasteles, postres y ofertas especiales, cuidadosamente elaborados con amor y dulzura.  
        Esperamos que cada bocado te haga sonreír y endulce tu día. ¡Gracias por elegirnos para tus momentos más dulces!</p>
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRzswKWk92LDxn9JFIDeADTK4lC7uAIkB5ShQ&s.jpeg" alt="Delicioso postre">
    </div>
</section>

<!-- PASTELES -->
<section id="pasteles">
    <h2>Pasteles</h2>
    <div class="contenedor-productos" id="pasteles-lista"></div>
</section>

<!-- POSTRES -->
<section id="postres">
    <h2>Postres</h2>
    <div class="contenedor-productos" id="postres-lista"></div>
</section>

<!-- OFERTAS -->
<section id="ofertas">
    <h2>Ofertas</h2>
    <div class="contenedor-productos" id="ofertas-lista"></div>
</section>

<!-- CARRITO -->
<section id="carrito">
    <h2>Tu Carrito de Compras</h2>
    <div id="carrito-lista"></div>
    <h3>Total: $<span id="total">0</span></h3>

    <h2>Pago con tarjeta</h2>
    <div id="pago">
        <label>Nombre completo:</label><br>
        <input type="text" id="nombre"><br><br>
        <label>Número de tarjeta:</label><br>
        <input type="number" id="tarjeta"><br><br>
        <button onclick="pagar()">Pagar</button>
    </div>
</section>

<script>
let carrito = [];

// Datos de productos
const pasteles = [
    {nombre:"Pastel de Chocolate", precio:25, img:"https://images.pexels.com/photos/4109991/pexels-photo-4109991.jpeg"},
    {nombre:"Pastel de Vainilla", precio:22, img:"https://images.pexels.com/photos/3026806/pexels-photo-3026806.jpeg"},
    {nombre:"Pastel de Fresas", precio:24, img:"https://images.pexels.com/photos/291528/pexels-photo-291528.jpeg"},
    {nombre:"Pastel Tres Leches", precio:23, img:"https://images.pexels.com/photos/1026122/pexels-photo-1026122.jpeg"},
    {nombre:"Pastel Arcoíris", precio:28, img:"https://images.pexels.com/photos/3776936/pexels-photo-3776936.jpeg"},
    {nombre:"Red Velvet", precio:27, img:"https://images.pexels.com/photos/1998638/pexels-photo-1998638.jpeg"},
    {nombre:"Pastel de Limón", precio:24, img:"https://images.pexels.com/photos/2337812/pexels-photo-2337812.jpeg"},
    {nombre:"Pastel de Zanahoria", precio:25, img:"https://images.pexels.com/photos/1111469/pexels-photo-1111469.jpeg"},
    {nombre:"Pastel de Mora", precio:26, img:"https://images.pexels.com/photos/4109992/pexels-photo-4109992.jpeg"},
    {nombre:"Pastel Sorpresa", precio:30, img:"https://images.pexels.com/photos/1026122/pexels-photo-1026122.jpeg"},
];

const postres = [
    {nombre:"Flan Casero", precio:8, img:"https://images.pexels.com/photos/4109990/pexels-photo-4109990.jpeg"},
    {nombre:"Tiramisú", precio:10, img:"https://images.pexels.com/photos/1352278/pexels-photo-1352278.jpeg"},
    {nombre:"Mousse de Chocolate", precio:9, img:"https://images.pexels.com/photos/3026804/pexels-photo-3026804.jpeg"},
    {nombre:"Gelatina de Colores", precio:6, img:"https://images.pexels.com/photos/4109992/pexels-photo-4109992.jpeg"},
    {nombre:"Pay de Limón", precio:10, img:"https://images.pexels.com/photos/691152/pexels-photo-691152.jpeg"},
    {nombre:"Cheesecake de Fresa", precio:12, img:"https://images.pexels.com/photos/291528/pexels-photo-291528.jpeg"},
    {nombre:"Brownie", precio:7, img:"https://images.pexels.com/photos/669744/pexels-photo-669744.jpeg"},
    {nombre:"Helado Artesanal", precio:5, img:"https://images.pexels.com/photos/246337/pexels-photo-246337.jpeg"},
    {nombre:"Churros con Canela", precio:6, img:"https://images.pexels.com/photos/236337/pexels-photo-236337.jpeg"},
    {nombre:"Crepas Dulces", precio:7, img:"https://images.pexels.com/photos/3026803/pexels-photo-3026803.jpeg"},
];

const ofertas = [
    {nombre:"Cheesecake de Mora", precio:10, img:"https://images.pexels.com/photos/291528/pexels-photo-291528.jpeg"},
    {nombre:"Pastel de Fresa Mini", precio:12, img:"https://images.pexels.com/photos/1026122/pexels-photo-1026122.jpeg"},
    {nombre:"Cupcakes Surtidos", precio:8, img:"https://images.pexels.com/photos/291528/pexels-photo-291528.jpeg"},
    {nombre:"Galletas con Chispas", precio:5, img:"https://images.pexels.com/photos/4109991/pexels-photo-4109991.jpeg"},
    {nombre:"Donas Surtidas", precio:6, img:"https://images.pexels.com/photos/235127/pexels-photo-235127.jpeg"},
    {nombre:"Rollos de Canela", precio:7, img:"https://images.pexels.com/photos/213780/pexels-photo-213780.jpeg"},
    {nombre:"Mini Tartas", precio:9, img:"https://images.pexels.com/photos/461430/pexels-photo-461430.jpeg"},
    {nombre:"Helado Doble", precio:8, img:"https://images.pexels.com/photos/403364/pexels-photo-403364.jpeg"},
    {nombre:"Pay de Queso Mini", precio:7, img:"https://images.pexels.com/photos/4110003/pexels-photo-4110003.jpeg"},
    {nombre:"Trufas de Chocolate", precio:6, img:"https://images.pexels.com/photos/3923551/pexels-photo-3923551.jpeg"},
];

// Funciones
function mostrarSeccion(id){
    document.querySelectorAll("section").forEach(sec=>{
        sec.classList.remove("active");
    });
    document.getElementById(id).classList.add("active");

    document.querySelectorAll("nav a").forEach(a=>{
        a.classList.remove("active");
    });
    event.target.classList.add("active");
}

function agregarAlCarrito(nombre, precio){
    const cantidad = parseInt(prompt("¿Cuántos deseas llevar?"));
    if(!cantidad || cantidad<=0) return;
    carrito.push({nombre, precio, cantidad});
    alert(`Producto agregado al carrito: ${nombre} x${cantidad}`);
    mostrarCarrito();
    actualizarNotificacion();
}

function mostrarCarrito(){
    const div = document.getElementById("carrito-lista");
    div.innerHTML = "";
    let total = 0;
    carrito.forEach(p=>{
        const item = document.createElement("p");
        item.textContent = `${p.nombre} x${p.cantidad} = $${p.precio*p.cantidad}`;
        div.appendChild(item);
        total += p.precio*p.cantidad;
    });
    document.getElementById("total").textContent = total;
}

function actualizarNotificacion(){
    const span = document.getElementById("notificacionCarrito");
    if(carrito.length>0){
        span.style.display="inline-block";
        span.textContent = carrito.length;
        span.classList.add("salto");
        setTimeout(()=>{ span.classList.remove("salto"); }, 300);
    } else {
        span.style.display="none";
    }
}

function pagar(){
    if(carrito.length===0){
        alert("El carrito está vacío");
        return;
    }
    const nombre = document.getElementById("nombre").value;
    const tarjeta = document.getElementById("tarjeta").value;
    if(!nombre || !tarjeta){
        alert("Ingresa todos los datos de pago");
        return;
    }
    let mensaje = `Gracias por tu compra, ${nombre}!\nFactura:\n`;
    let total = 0;
    carrito.forEach(p=>{
        mensaje += `${p.nombre} x${p.cantidad} = $${p.precio*p.cantidad}\n`;
        total += p.precio*p.cantidad;
    });
    mensaje += `Total: $${total}\nPago realizado con tarjeta terminada en ${tarjeta.slice(-4)}`;
    alert(mensaje);
    carrito = [];
    mostrarCarrito();
    actualizarNotificacion();
}

// Mostrar productos
function cargarProductos(){
    const contPasteles = document.getElementById("pasteles-lista");
    pasteles.forEach(p=>{
        const div = document.createElement("div");
        div.classList.add("producto");
        div.innerHTML = `<img src="${p.img}" alt="${p.nombre}">
                         <h3>${p.nombre}</h3>
                         <p>Precio: $${p.precio}</p>
                         <button onclick="agregarAlCarrito('${p.nombre}',${p.precio})">Comprar</button>`;
        contPasteles.appendChild(div);
    });

    const contPostres = document.getElementById("postres-lista");
    postres.forEach(p=>{
        const div = document.createElement("div");
        div.classList.add("producto");
        div.innerHTML = `<img src="${p.img}" alt="${p.nombre}">
                         <h3>${p.nombre}</h3>
                         <p>Precio: $${p.precio}</p>
                         <button onclick="agregarAlCarrito('${p.nombre}',${p.precio})">Comprar</button>`;
        contPostres.appendChild(div);
    });

    const contOfertas = document.getElementById("ofertas-lista");
    ofertas.forEach(p=>{
        const div = document.createElement("div");
        div.classList.add("producto");
        div.innerHTML = `<img src="${p.img}" alt="${p.nombre}">
                         <h3>${p.nombre}</h3>
                         <p>Precio: $${p.precio}</p>
                         <button onclick="agregarAlCarrito('${p.nombre}',${p.precio})">Comprar</button>`;
        contOfertas.appendChild(div);
    });
}

// Inicializar
cargarProductos();
</script>

</body>
</html>
