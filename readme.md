body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 20px;
}

h1 {
  text-align: center;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 20px;
  padding: 20px;
}

.card {
  background-color: #fff;
  border: 1px solid #ddd;
  padding: 10px;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.card:hover {
  transform: scale(1.02);
}

.card img {
  width: 100%;
  height: 180px;
  object-fit: contain;
}

.hidden {
  display: none;
}

.detail-content {
  display: flex;
  gap: 20px;
  padding: 20px;
}

.detail-content img {
  width: 300px;
  height: auto;
  object-fit: contain;
}

#back-button {
  margin: 20px;
  padding: 10px 20px;
  font-size: 16px;
}

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Fake Store</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>Fake Store</h1>
  
  <!-- Grid View -->
  <div id="product-grid" class="grid-container"></div>

  <!-- Detail View (initially hidden) -->
  <div id="product-detail" class="hidden">
    <button id="back-button">← Back</button>
    <div class="detail-content">
      <img id="detail-image" />
      <div>
        <h2 id="detail-title"></h2>
        <p id="detail-description"></p>
        <p><strong>Price:</strong> $<span id="detail-price"></span></p>
        <p><strong>Category:</strong> <span id="detail-category"></span></p>
      </div>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>


const productGrid = document.getElementById("product-grid");
const productDetail = document.getElementById("product-detail");
const backButton = document.getElementById("back-button");

// Detail Elements
const detailImage = document.getElementById("detail-image");
const detailTitle = document.getElementById("detail-title");
const detailDescription = document.getElementById("detail-description");
const detailPrice = document.getElementById("detail-price");
const detailCategory = document.getElementById("detail-category");

// Fetch products and render grid
fetch("https://fakestoreapi.com/products")
  .then(res => res.json())
  .then(products => {
    products.forEach(product => {
      const card = document.createElement("div");
      card.className = "card";
      card.innerHTML = `
        <img src="${product.image}" alt="${product.title}" />
        <h3>${product.title.slice(0, 40)}...</h3>
        <p>$${product.price}</p>
      `;
      card.onclick = () => showProductDetail(product);
      productGrid.appendChild(card);
    });
  });

// Show product detail
function showProductDetail(product) {
  productGrid.classList.add("hidden");
  productDetail.classList.remove("hidden");

  detailImage.src = product.image;
  detailTitle.textContent = product.title;
  detailDescription.textContent = product.description;
  detailPrice.textContent = product.price;
  detailCategory.textContent = product.category;
}

// Go back to grid view
backButton.onclick = () => {
  productDetail.classList.add("hidden");
  productGrid.classList.remove("hidden");
};