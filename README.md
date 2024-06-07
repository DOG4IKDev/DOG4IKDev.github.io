<!DOCTYPE html>
<html lang="ru">

<head>
  <meta charset="UTF-8">
  <title>EshkereCoin</title>
<link rel="stylesheet" href="style.css">
  <style>
    body {
      text-align: center;
      font-family: 'Arial', sans-serif;
      background-color: #f7f7f7;
      margin: 0;
      padding: 20px;
    }

    #coin {
      cursor: pointer;
      transition: transform 0.1s ease;
    }

    #coin:active {
      transform: scale(0.9);
    }

    .shop-item {
      display: inline-block;
      margin: 10px;
      border-radius: 15px;
      border: 2px solid #0000FF;
      background-color: #ADD8E6;
      padding: 5px;
      transition: transform 0.1s ease;
    }

    .shop-item:active {
      transform: scale(0.9);
    }

    .shop-item.disabled {
      opacity: 0.5;
      pointer-events: none;
    }

    .shop-item img {
      width: 100px;
      height: 100px;
      border-radius: 15px;
    }

    #shop {
      margin-top: 20px;
    }

    h1,
    h2 {
      color: #333;
    }
  </style>
</head>

<body>
  <h1>Кликер Монет</h1>
  <img id="coin" src="https://camo.githubusercontent.com/3dc32273f8e4e28fcbb24de498c1808a9926e1fb9857ffd496985584141d8992/68747470733a2f2f74722e72627863646e2e636f6d2f62626366363135353761636562353639613839393430336562646561613434332f3432302f3432302f496d6167652f506e67" alt="Монета">
  <p>Монет: <span id="coin-count">0</span></p>

  <div id="shop">
    <h2>Shop</h2>
    <div class="shop-item" onclick="buyItem(this, 1000, 1, 2)">
      <img src="item1.png" alt="Товар 1">
      <p>Цена: 1000</p>
    </div>
    <div class="shop-item" onclick="buyItem(this, 5000, 1, 2)">
      <img src="item2.png" alt="Товар 2">
      <p>Цена: 5000</p>
    </div>
    <div class="shop-item" onclick="buyItem(this, 10000, 1, 2)">
      <img src="item3.png" alt="Товар 3">
      <p>Цена: 10000</p>
    </div>
    <div class="shop-item" onclick="buyItem(this, 50000, 1, 2)">
      <img src="item4.png" alt="Товар 4">
      <p>Цена: 50000</p>
    </div>
  </div>

  <script>
    let coins = parseInt(localStorage.getItem('coins')) || 0;
let incomeRate = parseInt(localStorage.getItem('incomeRate')) || 0;
let clickRate = parseInt(localStorage.getItem('clickRate')) || 1;

document.getElementById('coin-count').textContent = coins;

document.getElementById('coin').addEventListener('click', () => {
  coins += clickRate;
  updateCoins();
});

function buyItem(element, price, income, bonusClick) {
  if (coins >= price && !element.classList.contains('disabled')) {
    coins -= price;
    incomeRate += income;
    clickRate += bonusClick;
    element.classList.add('disabled');
    updateCoins();
    localStorage.setItem(element.querySelector('img').alt, 'purchased');
    localStorage.setItem('incomeRate', incomeRate);
    localStorage.setItem('clickRate', clickRate);
  }
}

function updateCoins() {
  document.getElementById('coin-count').textContent = coins;
  localStorage.setItem('coins', coins);
}

setInterval(() => {
  coins += incomeRate;
  updateCoins();
}, 1000);

document.addEventListener('DOMContentLoaded', () => {
  document.querySelectorAll('.shop-item').forEach(item => {
    let itemName = item.querySelector('img').alt;
    if (localStorage.getItem(itemName) === 'purchased') {
      item.classList.add('disabled');
      incomeRate += 1;
      clickRate += 2;
    }
  });
  localStorage.setItem('incomeRate', incomeRate);
  localStorage.setItem('clickRate', clickRate);
});
  </script>
</body>

</html>
