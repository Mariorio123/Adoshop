# Adoshop
Ventes de sacs et tous 
<?php
// index.php
session_start();

if (!isset($_SESSION['cart'])) {
    $_SESSION['cart'] = [];
}

$whatsapp_number = "+22940643268"; // CHANGE CE NUMÉRO par le tien !

// Produits
$products = [
    1 => ['id'=>1, 'name'=>'Sac à Main Élégant Cuir Noir', 'price'=>25000, 'image'=>'https://picsum.photos/id/1015/600/600', 'desc'=>'Cuir véritable - Design moderne'],
    2 => ['id'=>2, 'name'=>'Sac Bandoulière Marron Chic', 'price'=>18000, 'image'=>'https://picsum.photos/id/133/600/600', 'desc'=>'Parfait pour tous les jours'],
    3 => ['id'=>3, 'name'=>'Sac à Dos Urbain Sport', 'price'=>22000, 'image'=>'https://picsum.photos/id/201/600/600', 'desc'=>'Confortable et résistant'],
    4 => ['id'=>4, 'name'=>'Sac de Voyage Toile Premium', 'price'=>35000, 'image'=>'https://picsum.photos/id/1005/600/600', 'desc'=>'Grande capacité - Idéal voyage'],
    5 => ['id'=>5, 'name'=>'Mini Sac Luxe Doré', 'price'=>30000, 'image'=>'https://picsum.photos/id/1009/600/600', 'desc'=>'Style soirée élégant'],
    6 => ['id'=>6, 'name'=>'Sac Cabas Shopping Écru', 'price'=>15000, 'image'=>'https://picsum.photos/id/160/600/600', 'desc'=>'Tendance et spacieux']
];

// Traitement des formulaires
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    if (isset($_POST['add_to_cart'])) {
        $id = (int)$_POST['product_id'];
        if (isset($products[$id])) {
            $found = false;
            foreach ($_SESSION['cart'] as &$item) {
                if ($item['id'] === $id) {
                    $item['quantity']++;
                    $found = true;
                    break;
                }
            }
            if (!$found) {
                $_SESSION['cart'][] = ['id'=>$id, 'name'=>$products[$id]['name'], 'price'=>$products[$id]['price'], 'quantity'=>1];
            }
        }
        header("Location: index.php?added=1");
        exit;
    }

    if (isset($_POST['remove_from_cart'])) {
        $index = (int)$_POST['item_index'];
        if (isset($_SESSION['cart'][$index])) {
            unset($_SESSION['cart'][$index]);
            $_SESSION['cart'] = array_values($_SESSION['cart']);
        }
        header("Location: index.php");
        exit;
    }

    if (isset($_POST['checkout'])) {
        $total = 0;
        foreach ($_SESSION['cart'] as $item) {
            $total += $item['price'] * $item['quantity'];
        }
        $success_message = "✅ Commande enregistrée ! Total : " . number_format($total, 0, ',', ' ') . " FCFA.<br>Nous vous contacterons sur WhatsApp pour le paiement et la livraison.";
        $_SESSION['cart'] = [];
    }
}

// Calcul du panier
$cart_total = 0;
$cart_count = 0;
foreach ($_SESSION['cart'] as $item) {
    $cart_total += $item['price'] * $item['quantity'];
    $cart_count += $item['quantity'];
}
?>

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ADO SHOP - Sacs de qualité</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap');
        * { font-family: 'Poppins', sans-serif; }
        
        .splash {
            position: fixed;
            inset: 0;
            background: linear-gradient(to bottom right, #0f766e, #115e59);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 50;
            transition: opacity 0.8s ease-out;
        }
        
        .product-card {
            transition: all 0.3s ease;
        }
        .product-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 25px 50px -12px rgb(0 0 0 / 0.25);
        }
    </style>
</head>
<body class="bg-gray-50">

    <!-- SPLASH SCREEN -->
    <div id="splash" class="splash">
        <div class="text-center text-white px-6">
            <div class="mx-auto mb-8 w-28 h-28 bg-white/20 backdrop-blur-xl rounded-3xl flex items-center justify-center border border-white/30">
                <i class="fa-solid fa-bag-shopping text-7xl text-white"></i>
            </div>
            <h1 class="text-6xl font-bold tracking-tighter mb-4">BIENVENUE À</h1>
            <h2 class="text-6xl font-bold tracking-tighter">ADO SHOP</h2>
            <p class="mt-8 text-xl">Chargement complet de votre boutique...</p>
            <div class="mt-12 w-8 h-8 border-4 border-white border-t-transparent rounded-full animate-spin mx-auto"></div>
        </div>
    </div>

    <!-- CONTENU PRINCIPAL -->
    <div id="main-content" class="hidden">

        <!-- Navbar -->
        <nav class="bg-white border-b shadow-sm sticky top-0 z-40">
            <div class="max-w-7xl mx-auto px-6 py-5 flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <i class="fa-solid fa-bag-shopping text-3xl text-emerald-600"></i>
                    <h1 class="text-3xl font-bold text-gray-900">ADO SHOP</h1>
                </div>
                <div class="flex items-center gap-8 text-gray-700">
                    <a href="#" onclick="showHome()" class="flex items-center gap-2 hover:text-emerald-600 font-medium">
                        <i class="fa-solid fa-house"></i> Accueil
                    </a>
                    <a onclick="showCart()" class="flex items-center gap-2 hover:text-emerald-600 font-medium cursor-pointer">
                        <i class="fa-solid fa-cart-shopping"></i> Panier
                        <?php if ($cart_count > 0): ?>
                            <span class="bg-emerald-600 text-white text-xs font-bold px-2 py-0.5 rounded-full"><?= $cart_count ?></span>
                        <?php endif; ?>
                    </a>
                    <a href="https://wa.me/<?= $whatsapp_number ?>" target="_blank" class="bg-emerald-600 text-white px-6 py-3 rounded-2xl flex items-center gap-3 hover:bg-emerald-700">
                        <i class="fa-brands fa-whatsapp text-xl"></i> WhatsApp
                    </a>
                </div>
            </div>
        </nav>

        <?php if (isset($success_message)): ?>
        <div class="max-w-7xl mx-auto px-6 pt-6">
            <div class="bg-emerald-100 border border-emerald-300 text-emerald-800 rounded-3xl p-6">
                <?= $success_message ?>
            </div>
        </div>
        <?php endif; ?>

        <!-- Hero -->
        <div class="bg-gradient-to-r from-emerald-600 to-teal-600 text-white py-16">
            <div class="max-w-7xl mx-auto px-6 text-center">
                <h2 class="text-5xl font-bold mb-4">Découvrez nos sacs tendance</h2>
                <p class="text-xl">Qualité premium • Livraison rapide • Paiement Mobile Money ou WhatsApp</p>
            </div>
        </div>

        <!-- Produits -->
        <div class="max-w-7xl mx-auto px-6 py-12">
            <h2 class="text-4xl font-bold mb-8">Nos Sacs</h2>
            <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8">
                <?php foreach ($products as $product): ?>
                <div class="product-card bg-white rounded-3xl overflow-hidden border">
                    <a href="https://wa.me/<?= $whatsapp_number ?>?text=Bonjour,%20je%20veux%20acheter%20<?= urlencode($product['name']) ?>%20à%20<?= number_format($product['price'],0,',',' ') ?>%20FCFA" target="_blank">
                        <img src="<?= $product['image'] ?>" class="w-full h-64 object-cover" alt="<?= $product['name'] ?>">
                    </a>
                    <div class="p-5">
                        <h3 class="font-semibold"><?= $product['name'] ?></h3>
                        <p class="text-emerald-600 text-sm"><?= $product['desc'] ?></p>
                        <div class="flex justify-between items-center mt-4">
                            <span class="text-3xl font-bold"><?= number_format($product['price'],0,',',' ') ?> FCFA</span>
                            <form method="POST">
                                <input type="hidden" name="product_id" value="<?= $product['id'] ?>">
                                <button type="submit" name="add_to_cart" class="bg-emerald-600 text-white px-6 py-3 rounded-2xl text-sm font-medium">
                                    Ajouter au panier
                                </button>
                            </form>
                        </div>
                    </div>
                </div>
                <?php endforeach; ?>
            </div>
        </div>

        <!-- Footer -->
        <footer class="bg-gray-900 text-white py-12 text-center">
            <p class="text-3xl font-bold mb-2">ADO SHOP</p>
            <p>Paiement Mobile Money • Livraison partout au Sénégal</p>
            <p class="text-xs mt-8">© 2026 ADO SHOP</p>
        </footer>
    </div>

    <!-- Modal Panier -->
    <div id="cart-modal" class="hidden fixed inset-0 bg-black/70 z-50 flex items-center justify-center">
        <div class="bg-white w-full max-w-lg rounded-3xl overflow-hidden">
            <div class="p-6 border-b flex justify-between">
                <h3 class="text-2xl font-bold">Votre Panier</h3>
                <button onclick="hideCart()" class="text-3xl">×</button>
            </div>
            <div class="p-6 max-h-[400px] overflow-auto">
                <?php if (empty($_SESSION['cart'])): ?>
                    <p class="text-center text-gray-500 py-12">Votre panier est vide</p>
                <?php else: ?>
                    <?php foreach ($_SESSION['cart'] as $index => $item): ?>
                    <div class="flex justify-between py-4 border-b">
                        <div>
                            <p><?= $item['name'] ?></p>
                            <p class="text-sm text-gray-500"><?= number_format($item['price'],0,',',' ') ?> FCFA × <?= $item['quantity'] ?></p>
                        </div>
                        <form method="POST">
                            <input type="hidden" name="item_index" value="<?= $index ?>">
                            <button type="submit" name="remove_from_cart" class="text-red-500">Supprimer</button>
                        </form>
                    </div>
                    <?php endforeach; ?>
                <?php endif; ?>
            </div>
            <?php if (!empty($_SESSION['cart'])): ?>
            <div class="p-6 border-t">
                <div class="flex justify-between text-xl font-bold mb-6">
                    <span>Total</span>
                    <span><?= number_format($cart_total,0,',',' ') ?> FCFA</span>
                </div>
                <form method="POST">
                    <button type="submit" name="checkout" class="w-full bg-emerald-600 text-white py-4 rounded-3xl font-semibold">
                        Commander avec Mobile Money
                    </button>
                </form>
                <p class="text-center text-xs text-gray-400 mt-4">Paiement via Orange Money, Wave ou Free Money</p>
            </div>
            <?php endif; ?>
        </div>
    </div>

<script>
// Script corrigé pour le splash screen
window.addEventListener('load', function() {
    const splash = document.getElementById('splash');
    const main = document.getElementById('main-content');

    setTimeout(() => {
        splash.style.opacity = '0';
        setTimeout(() => {
            splash.style.display = 'none';
            main.classList.remove('hidden');
        }, 800);
    }, 3500);
});

function showCart() {
    document.getElementById('cart-modal').classList.remove('hidden');
    document.getElementById('cart-modal').classList.add('flex');
}

function hideCart() {
    const modal = document.getElementById('cart-modal');
    modal.classList.add('hidden');
    modal.classList.remove('flex');
}

function showHome() {
    window.scrollTo({ top: 0, behavior: 'smooth' });
}
</script>

</body>
</html>
