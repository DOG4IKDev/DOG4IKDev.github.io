<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Beautiful Clicker Game</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      background: linear-gradient(to bottom right, #4facfe, #00f2fe);
      font-family: 'Arial', sans-serif;
      color: white;
      text-align: center;
    }

    .container {
      transition: opacity 0.5s ease, transform 0.5s ease;
      width: 100%;
      max-width: 600px;
    }

    .hidden {
      opacity: 0;
      transform: scale(0.9);
      pointer-events: none;
    }

    .show {
      opacity: 1;
      transform: scale(1);
      pointer-events: auto;
    }

    .tab {
      display: flex;
      justify-content: center;
      width: 100%;
      margin-top: 20px;
      margin-bottom: 20px;
    }

    .tab button {
      padding: 15px 30px;
      border: none;
      background-color: #1e90ff;
      color: white;
      cursor: pointer;
      transition: background-color 0.3s ease;
      font-size: 18px;
      margin: 0 10px;
      border-radius: 10px;
    }

    .tab button.active {
      background-color: #f39c12;
      color: black;
    }

    .tab button:not(.active):hover {
      background-color: #f39c12;
      color: black;
    }

    #coins {
      font-size: 28px;
      margin-bottom: 20px;
    }

    .clicker {
      width: 180px;
      height: 180px;
      margin-bottom: 0px;
      cursor: pointer;
      border: px solid #f39c12;
      border-radius: 0%;
      transition: transform 0.1s;
    }

    .clicker:active {
      transform: scale(0.9);
    }

    .item {
      border: 2px solid #f39c12;
      border-radius: 10px;
      padding: 10px;
      margin: 10px;
      display: inline-block;
      width: 120px;
      height: 180px;
      text-align: center;
      cursor: pointer;
      background: white;
      color: black;
    }

    .item img {
      width: 70px;
      height: 70px;
    }

    .item div {
      font-size: 16px;
    }
  </style>
</head>

<body>
  <div class="tab">
    <button class="tablinks active" onclick="openTab('main')">Game</button>
    <button class="tablinks" onclick="openTab('shop')">Shop</button>
  </div>

  <div id="main" class="container show">
    <div id="coins">Coins: 0</div>
    <img id="clicker" src="https://cdn.discordapp.com/attachments/788049211831877662/1253071320036671529/image-removebg-preview_14.png?ex=6674851b&is=6673339b&hm=6394688ce6e8cbf22d23a3a8d55af1631a0a5d1b3ad4d76fe5a0efdcc3eeecbf&" alt="Clicker" class="clicker">
  </div>

  <div id="shop" class="container hidden">
    <div class="item" onclick="buyItem(0)">
      <img src="https://cdn.discordapp.com/attachments/788049211831877662/1253247626476781588/r82HLLTIAJMAEmwAQujAALoWh5hsxASbABJgAE5gfARb0bHllpkAE2ACTIAJXBgBFvQLQ803YgJMgAkwASYwPwIs6PNjyy0zASbABJgAE7gwAizoF4aab8QEmAATYAJMYH4EWNDnx5ZbZgJMgAkwASZwYQRY0C8MNdICTABJsAEmMD8CLCgz48tt8wEmAATYAJM4MIIsKBfGGqERNgAkyACTCBRH4fzZdj9CYAVgjAAAAAElFTkSuQmCC.png?ex=6675294e&is=6673d7ce&hm=17fd2e71fb0b0bde1a582f8139ebdafadb69eb126b7df375845e4953687df3e4&" alt="Item 1">
      <div>Шахтёр<br>Cost: <span id="cost0">10</span><br>Purchased: <span id="purchased0">0</span>/3</div>
    </div>
    <div class="item" onclick="buyItem(1)">
      <img src="https://cdn.discordapp.com/attachments/788049211831877662/1253248099023978547/image-removebg-preview_15.png?ex=667529bf&is=6673d83f&hm=9f67377db274043a3bf0370aa1802098550d5ce07f7b3936beb637d25dd84ecf&" alt="Item 2">
      <div>IT-шник<br>Cost: <span id="cost1">30</span><br>Purchased: <span id="purchased1">0</span>/3</div>
    </div>
    <div class="item" onclick="buyItem(2)">
      <img src="https://cdn.discordapp.com/attachments/788049211831877662/1253248600905879654/image-removebg-preview_16.png?ex=66752a36&is=6673d8b6&hm=6382de99bfff69bb94b5b1cfa124eb28a7ce5f446bb60066fb854ee444ca4e9a&" alt="Item 3">
      <div>Учёный<br>Cost: <span id="cost2">90</span><br>Purchased: <span id="purchased2">0</span>/3</div>
    </div>
    <div class="item" onclick="buyItem(3)">
      <img src="https://cdn.discordapp.com/attachments/788049211831877662/1253248858469961798/image-removebg-preview_17.png?ex=66752a74&is=6673d8f4&hm=caf012f7f1f7796475b66d415312e50d09d14199f78048bcb2d3f47ad1f0b3d7&" alt="Item 4">
      <div>Менеджер<br>Cost: <span id="cost3">270</span><br>Purchased: <span id="purchased3">0</span>/3</div>
    </div>
  </div>

  <script>
    let coins = 0;
        let upgrades = [0, 0, 0, 0];
        const baseCosts = [100, 200, 500, 1000];

        document.getElementById('clicker').addEventListener('click', () => {
            let clickValue = 1 + upgrades.reduce((acc, level) => acc + level, 0);
            coins += clickValue;
            document.getElementById('coins').textContent = `Coins: ${coins}`;
        });

        function buyItem(index) {
            let cost = baseCosts[index] * (3 ** upgrades[index]);
            if (coins >= cost && upgrades[index] < 3) {
                coins -= cost;
                upgrades[index]++;
                document.getElementById(`cost${index}`).textContent = baseCosts[index] * (3 ** upgrades[index]);
                document.getElementById(`purchased${index}`).textContent = upgrades[index];
                document.getElementById('coins').textContent = `Coins: ${coins}`;
            }
        }

        function openTab(tabName) {
            let containers = document.getElementsByClassName("container");
            for (let i = 0; i < containers.length; i++) {
                containers[i].classList.add("hidden");
                containers[i].classList.remove("show");
            }
            let tablinks = document.getElementsByClassName("tablinks");
            for (let i = 0; i < tablinks.length; i++) {
                tablinks[i].classList.remove("active");
            }
            document.getElementById(tabName).classList.add("show");
            document.getElementById(tabName).classList.remove("hidden");
            document.querySelector(`[onclick="openTab('${tabName}')"]`).classList.add("active");
        }

        // Save and load progress
        function saveProgress() {
            localStorage.setItem('coins', coins);
            localStorage.setItem('upgrades', JSON.stringify(upgrades));
        }

        function loadProgress() {
            if (localStorage.getItem('coins') !== null) {
                coins = parseInt(localStorage.getItem('coins'), 10);
                upgrades = JSON.parse(localStorage.getItem('upgrades'));
                document.getElementById('coins').textContent = `Coins: ${coins}`;
                upgrades.forEach((level, index) => {
                    document.getElementById(`cost${index}`).textContent = baseCosts[index] * (3 ** level);
                    document.getElementById(`purchased${index}`).textContent = level;
                });
            }
        }

        window.addEventListener('beforeunload', saveProgress);
        window.addEventListener('load', loadProgress);
  </script>
</body>

</html>
