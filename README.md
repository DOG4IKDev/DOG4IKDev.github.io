<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Кликер с Магазином</title>
<style>
    body, html {
        height: 100%;
        margin: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: 'Arial', sans-serif;
        background-color: #f0f0f0;
    }
    #app {
        text-align: center;
    }
    .button {
        background-color: #4CAF50; 
        border: none;
        color: white;
        padding: 15px 32px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        margin: 4px 2px;
        cursor: pointer;
    }
    /* Табы для переключения между игрой и магазином */
    .tab-content {
        display: none;
        animation: fadeInAnimation ease 1s;
    }
    .tab-content.active {
        display: block;
    }
    @keyframes fadeInAnimation {
        from {opacity: 0;}
        to {opacity: 1;}
    }
    img {
        height: 100px;
        cursor: pointer;
    }
</style>
</head>
<body>
<div id="app">
    <!-- Вкладка игры -->
    <div id="game-tab" class="tab-content active">
        <img src="coin.png" alt="Coin" onclick="addCoin()">
        <p><strong>Монеты:</strong> <span id="coins">0</span></p>
        <button class="button" onclick="showTab('shop')">Перейти в Магазин</button>
    </div>
    <!-- Вкладка магазина -->
    <div id="shop-tab" class="tab-content">
        <!-- Товары магазина будут добавлены здесь -->
        <button class="button" onclick="showTab('game')">Вернуться к Игре</button>
    </div>
</div>
<script>
let coins = 0;
let upgrades = [
    {cost: 10, increase: 1, level: 0},
    {cost: 30, increase: 2, level: 0},
    {cost: 90, increase: 3, level: 0},
    {cost: 270, increase: 5, level: 0}
];

function addCoin() {
    coins++;
    upgrades.forEach(upgrade => {
        coins += upgrade.increase * upgrade.level;
    });
    updateDisplay();
}

function buyUpgrade(index) {
    if (coins >= upgrades[index].cost && upgrades[index].level < 3) {
        coins -= upgrades[index].cost;
        upgrades[index].cost *= 3;
        upgrades[index].level++;
        updateDisplay();
    }
}

function updateDisplay() {
    document.getElementById('coins').textContent = coins;
    let shopTab = document.getElementById('shop-tab');
    shopTab.innerHTML = '<button class="button" onclick="showTab(\'game\')">Вернуться к Игре</button>';
    upgrades.forEach((upgrade, index) => {
        let upgradeElement = document.createElement('div');
        upgradeElement.innerHTML = `<img src="upgrade.png" alt="Upgrade"><p>Уровень: ${upgrade.level} - Цена: ${upgrade.cost}</p><button class="button" onclick="buyUpgrade(${index})">Купить Улучшение</button>`;
        shopTab.insertBefore(upgradeElement, shopTab.firstChild);
    });
}

function showTab(tabName) {
    let tabs = document.querySelectorAll('.tab-content');
    tabs.forEach(tab => {
        tab.classList.remove('active');
    });
    document.getElementById(tabName + '-tab').classList.add('active');
}

updateDisplay();
</script>
</body>
</html>
