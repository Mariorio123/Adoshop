<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Adoshop</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(to right, #111, #333);
    color: white;
}

header {
    text-align: center;
    padding: 20px;
    background: black;
}

h1 {
    color: gold;
}

.container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
    padding: 20px;
}

.card {
    background: #222;
    padding: 15px;
    border-radius: 10px;
    text-align: center;
}

.card img {
    width: 100%;
    border-radius: 10px;
}

button {
    margin-top: 10px;
    padding: 10px;
    background: gold;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background: orange;
}
</style>

</head>

<body>

<header>
    <h1>Adoshop 🛍️</h1>
    <p>Vente de sacs tendance</p>
</header>

<div class="container">

    <div class="card">
        <img src="https://via.placeholder.com/200" alt="Sac">
        <h3>Sac Luxe</h3>
        <p>15 000 FCFA</p>
        <button onclick="contact()">Commander</button>
    </div>

    <div class="card">
        <img src="https://via.placeholder.com/200" alt="Sac">
        <h3>Sac Classique</h3>
        <p>10 000 FCFA</p>
        <button onclick="contact()">Commander</button>
    </div>

    <div class="card">
        <img src="https://via.placeholder.com/200" alt="Sac">
        <h3>Sac Premium</h3>
        <p>20 000 FCFA</p>
        <button onclick="contact()">Commander</button>
    </div>

</div>

<script>
function contact() {
    window.open("https://wa.me/229XXXXXXXX", "_blank");
}
</script>

</body>
</html>
