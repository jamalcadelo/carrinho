<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Carrinho de Compras - Loja de Celulares</title>
    <link href="https://fonts.googleapis.com/css2?family=Sacramento&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <nav>
            <div class="logo">
                <h1>Loja de Celulares</h1>
            </div>
            <ul class="nav-links">
                <li><a href="#">Início</a></li>
                <li><a href="#">Produtos</a></li>
                <li><a href="#">Carrinho</a></li>
            </ul>
        </nav>
    </header>

    <section class="cart-container">
        <h2>Seu Carrinho de Compras</h2>
        
        <div class="cart-items">
            <!-- Os itens serão gerados aqui via JavaScript -->
        </div>
        
        <div class="cart-summary">
            <p><strong>Total:</strong> <span id="total">R$ 0,00</span></p>
            <button id="checkout-btn">Finalizar Compra</button>
        </div>
    </section>

    <footer>
        <p>© 2025 Loja de Celulares - Todos os direitos reservados</p>
    </footer>

    <script src="script.js"></script>
</body>
<style>
/* Definições gerais */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Sacramento', cursive;
    background-color: #000;
    color: #fff;
}

/* Cabeçalho */
header {
    background-color: #6a0dad; /* Roxo */
    padding: 20px;
    text-align: center;
}

header nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

header .logo h1 {
    font-size: 3rem;
    color: #fff;
}

header .nav-links {
    list-style: none;
    display: flex;
    gap: 20px;
}

header .nav-links li {
    font-size: 1.2rem;
}

header .nav-links li a {
    text-decoration: none;
    color: #fff;
}

header .nav-links li a:hover {
    color: #32cd32; /* Verde */
}

/* Seção do Carrinho */
.cart-container {
    text-align: center;
    padding: 50px;
    background-color: #333;
}

.cart-container h2 {
    font-size: 2.5rem;
    color: #fff;
    margin-bottom: 30px;
}

.cart-items {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 20px;
}

.cart-item {
    background-color: #222;
    padding: 20px;
    text-align: center;
    border-radius: 10px;
    width: 250px;
}

.cart-item img {
    width: 100%;
    height: auto;
    border-radius: 5px;
}

.cart-item h4 {
    margin-top: 15px;
    font-size: 1.2rem;
    color: #fff;
}

.cart-item p {
    font-size: 1.1rem;
    color: #32cd32; /* Verde */
}

.cart-item .quantity-controls {
    margin-top: 10px;
    display: flex;
    justify-content: space-between;
}

.cart-item button {
    background-color: #ff6347; /* Tom de laranja-avermelhado */
    color: white;
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.cart-item button:hover {
    background-color: #ff4500; /* Cor mais escura */
}

/* Resumo do Carrinho */
.cart-summary {
    margin-top: 30px;
    font-size: 1.2rem;
}

.cart-summary p {
    font-size: 1.5rem;
    margin-bottom: 20px;
}

.cart-summary button {
    background-color: #32cd32; /* Verde */
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.cart-summary button:hover {
    background-color: #228b22; /* Verde mais escuro */
}

/* Rodapé */
footer {
    background-color: #4b0082; /* Roxo escuro */
    text-align: center;
    padding: 15px;
    margin-top: 40px;
}

footer p {
    color: #fff;
    font-size: 1rem;
}
</style>
<script>
// Dados dos produtos (exemplo)
const products = [
    {
        id: 1,
        name: "iPhone 13",
        price: 5999.90,
        image: "https://http2.mlstatic.com/D_NQ_NP_2X_931703-MLB83136113122_032025-F-iphone-12-128-gb-roxo-vitrine-garantia-nf-bateria-100.webp"
    },
    {
        id: 2,
        name: "Samsung Galaxy S21",
        price: 4999.90,
        image: "https://mobizoo.com.br/wp-content/uploads/2021/01/samsung-galaxy-s21-1.jpg"
    },
    {
        id: 3,
        name: "Xiaomi Mi 11",
        price: 3999.90,
        image: "https://th.bing.com/th/id/OIP.T3KcYKuETNuk6fkCRGhFVgHaHa?rs=1&pid=ImgDetMain"
    }
];

// Carrinho de compras
let cart = [];

// Função para renderizar os itens do carrinho
function renderCart() {
    const cartItemsContainer = document.querySelector('.cart-items');
    cartItemsContainer.innerHTML = '';
    let total = 0;

    cart.forEach(item => {
        const itemTotal = item.price * item.quantity;
        total += itemTotal;

        const cartItem = document.createElement('div');
        cartItem.classList.add('cart-item');

        cartItem.innerHTML = `
            <img src="${item.image}" alt="${item.name}">
            <h4>${item.name}</h4>
            <p>R$ ${item.price.toFixed(2)}</p>
            <div class="quantity-controls">
                <button class="decrease-btn" onclick="updateQuantity(${item.id}, -1)">-</button>
                <span>Quantidade: ${item.quantity}</span>
                <button class="increase-btn" onclick="updateQuantity(${item.id}, 1)">+</button>
            </div>
            <button onclick="removeItem(${item.id})">Remover</button>
        `;
        cartItemsContainer.appendChild(cartItem);
    });

    // Atualizar o total
    document.getElementById('total').textContent = `R$ ${total.toFixed(2)}`;
}

// Função para adicionar um item ao carrinho
function addToCart(productId) {
    const product = products.find(p => p.id === productId);
    const existingItem = cart.find(item => item.id === productId);

    if (existingItem) {
        existingItem.quantity++;
    } else {
        cart.push({ ...product, quantity: 1 });
    }

    renderCart();
}

// Função para atualizar a quantidade de um item
function updateQuantity(productId, change) {
    const item = cart.find(p => p.id === productId);
    if (item) {
        item.quantity += change;
        if (item.quantity <= 0) {
            removeItem(productId);
        } else {
            renderCart();
        }
    }
}

// Função para remover um item do carrinho
function removeItem(productId) {
    cart = cart.filter(item => item.id !== productId);
    renderCart();
}

// Função para finalizar a compra
document.getElementById('checkout-btn').addEventListener('click', () => {
    if (cart.length === 0) {
        alert('Seu carrinho está vazio!');
    } else {
        alert('Compra realizada com sucesso!');
        cart = [];
        renderCart();
    }
});

// Adicionar produtos ao carrinho
addToCart(1);
addToCart(2);
addToCart(3);
</script>
</html>

