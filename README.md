
<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Shop Demo</title>

<style>
body {
  font-family: Arial;
  background: #f5f5f5;
}

.container {
  width: 90%;
  margin: auto;
}

h2 {
  text-align: center;
  color: #e74c3c;
}

/* 📦 ADDRESS */
.address-box {
  background: #fff;
  padding: 15px;
  border-radius: 10px;
  margin: 20px 0;
  text-align: center;
}

.address {
  background: #dfe6e9;
  padding: 10px;
  margin: 10px 0;
  border: 2px solid red;
}

.btn {
  padding: 8px 15px;
  border: none;
  cursor: pointer;
  margin: 5px;
}

.btn-copy {
  background: gray;
  color: white;
}

.btn-random {
  background: #e74c3c;
  color: white;
}

/* 🔍 FILTER */
.filter-box {
  display: flex;
  gap: 15px;
  background: #eee;
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 20px;
}

.filter-box input,
.filter-box select {
  flex: 1;
  padding: 10px;
  border: 1px solid #999;
}

/* 🛒 PRODUCT */
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.card {
  background: white;
  padding: 10px;
  border-radius: 10px;
  text-align: center;
}

.card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.price {
  color: red;
  font-weight: bold;
}

button {
  background: #e74c3c;
  color: white;
  border: none;
  padding: 10px;
  width: 100%;
  cursor: pointer;
}
</style>
</head>

<body>

<div class="container">
<h2>🛒 DANH SÁCH SẢN PHẨM</h2>

<!-- 📍 ADDRESS -->
<div class="address-box">
  <button class="btn btn-random" onclick="randomAddress()">🎲 Địa chỉ random</button>

  <div id="address" class="address">
    Số 269, Bùi Xương Trạch, Hà Nội
  </div>

  <button class="btn btn-copy" onclick="copyAddress()">📋 Sao chép địa chỉ</button>
</div>

<!-- 🔍 FILTER -->
<div class="filter-box">
  <input type="text" id="search" placeholder="Tìm sản phẩm...">
  <input type="number" id="money" placeholder="Số tiền bạn có (đ)">
  
  <select id="sort">
    <option value="desc">Giá: Cao đến thấp</option>
    <option value="asc">Giá: Thấp đến cao</option>
  </select>
</div>

<!-- 🛒 LIST -->
<div id="product-list" class="grid"></div>

</div>

<script>

// 📍 DANH SÁCH ĐỊA CHỈ
const addresses = [
  "Số 269, Bùi Xương Trạch, Hà Nội",
  "12 Nguyễn Trãi, Thanh Xuân, Hà Nội",
  "45 Lê Lợi, Quận 1, TP.HCM",
  "78 Trần Hưng Đạo, Đà Nẵng",
  "101 Hai Bà Trưng, Hà Nội"
];

// 🎲 RANDOM ĐỊA CHỈ
function randomAddress() {
  const index = Math.floor(Math.random() * addresses.length);
  document.getElementById("address").innerText = addresses[index];
}

// 📋 COPY
function copyAddress() {
  const text = document.getElementById("address").innerText;
  navigator.clipboard.writeText(text);
  alert("Đã sao chép!");
}

// 🔥 DATA
const products = [
  { name: "iPhone 13", price: 12000000, image: "https://picsum.photos/200?1", link: "https://shopee.vn/" },
  { name: "Samsung A36", price: 7800000, image: "https://picsum.photos/200?2", link: "https://shopee.vn/" },
  { name: "Xiaomi Redmi", price: 5000000, image: "https://picsum.photos/200?3", link: "https://shopee.vn/" },
  { name: "OPPO Reno", price: 6500000, image: "https://picsum.photos/200?4", link: "https://shopee.vn/" }
];

// render
function renderProducts(list) {
  const container = document.getElementById("product-list");
  container.innerHTML = "";

  list.forEach(p => {
    container.innerHTML += `
      <div class="card">
        <img src="${p.image}">
        <h4>${p.name}</h4>
        <p class="price">${p.price.toLocaleString()} đ</p>
        <button onclick="window.open('${p.link}')">Mua ngay</button>
      </div>
    `;
  });
}

// 🔥 FILTER
function filterProducts() {
  let filtered = [...products];

  const search = document.getElementById("search").value.toLowerCase();
  const money = document.getElementById("money").value;
  const sort = document.getElementById("sort").value;

  if (search) {
    filtered = filtered.filter(p =>
      p.name.toLowerCase().includes(search)
    );
  }

  if (money) {
    filtered = filtered.filter(p =>
      p.price <= parseInt(money)
    );
  }

  if (sort === "asc") {
    filtered.sort((a,b) => a.price - b.price);
  } else {
    filtered.sort((a,b) => b.price - a.price);
  }

  renderProducts(filtered);
}

// EVENT
document.getElementById("search").addEventListener("input", filterProducts);
document.getElementById("money").addEventListener("input", filterProducts);
document.getElementById("sort").addEventListener("change", filterProducts);

// load
renderProducts(products);

</script>

</body>
</html>
