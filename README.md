<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Clicker Game - Farm & Animals</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background: url('https://via.placeholder.com/1200x800?text=Farm+Background') no-repeat center center fixed;
      background-size: cover;
      color: #333;
      margin: 0;
      padding: 0;
    }
    .container {
      margin-top: 50px;
      background-color: rgba(255, 255, 255, 0.8);
      border-radius: 15px;
      padding: 20px;
      display: inline-block;
    }
    #click-button, .upgrade {
      padding: 20px 40px;
      font-size: 24px;
      border-radius: 50%;
      cursor: pointer;
      transition: transform 0.2s, background-color 0.3s;
    }
    #click-button {
      background-color: #ffcc66;
      color: #333;
      border: 3px solid #996633;
    }
    #click-button:hover {
      background-color: #ffb84d;
      transform: scale(1.1);
    }
    .upgrade {
      margin: 10px;
    }
    .disabled {
      background-color: gray;
      cursor: not-allowed;
    }
    .level {
      margin-top: 20px;
      font-size: 20px;
      color: #4d4d4d;
    }
    .farm {
      margin-top: 20px;
    }
    .farm img {
      width: 100px;
      margin: 10px;
      transition: transform 0.3s;
    }
    .farm img:hover {
      transform: scale(1.2);
    }
    .points-animation {
      animation: pop 0.2s ease-in-out;
    }
    @keyframes pop {
      0% {
        transform: scale(1);
      }
      50% {
        transform: scale(1.2);
      }
      100% {
        transform: scale(1);
      }
    }
    .level-up-message {
      font-size: 24px;
      font-weight: bold;
      color: green;
      animation: fadeInOut 3s ease-in-out;
    }
    @keyframes fadeInOut {
      0%, 100% {
        opacity: 0;
      }
      50% {
        opacity: 1;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Farm Clicker Game</h1>
    <p>Points: <span id="points">0</span></p>
    <p class="level">Niveau: <span id="level">1</span></p>
    <button id="click-button">🌾 Récolte</button>

    <div class="shop">
      <h2>Shop</h2>
      <button class="upgrade" id="auto-clicker" style="background-color: hwb(141 12% 49%);" disabled>Auto Moissonneuse (+1/sec) - Cost: 50 points</button>
      <button class="upgrade" id="double-points" style="background-color: rgb(2, 66, 89);" disabled>Engrais (x2 points per Click) - Cost: 100 points</button>
      <button class="upgrade" id="level-up" style="background-color: hsl(115, 82%, 30%);" disabled>Améliorer la ferme (Next Level) - Cost: 200 points</button>
    </div>

    <div class="farm">
      <h2>Your Farm</h2>
      <div id="farm-container"></div>
    </div>

    <div id="level-up-message" class="level-up-message" style="display: none;">🎉 Level Up! 🎉</div>
  </div>

  <script>
    let points = 0;
    let pointsPerClick = 1;
    let autoClickerActive = false;
    let level = 1;
    const maxLevel = 16;

    const pointsDisplay = document.getElementById('points');
    const clickButton = document.getElementById('click-button');
    const autoClickerButton = document.getElementById('auto-clicker');
    const doublePointsButton = document.getElementById('double-points');
    const levelUpButton = document.getElementById('level-up');
    const levelDisplay = document.getElementById('level');
    const farmContainer = document.getElementById('farm-container');
    const levelUpMessage = document.getElementById('level-up-message');

    const farmImages = {
      1: '🌱',
      2: '🌿',
      3: '🌳',
      4: '🐓',
      5: '🐄',
      6: '🐖',
      7: '🐑',
      8: '🦆',
      9: '🦃',
      10: '🐇',
      11: '🐕',
      12: '🐈',
      13: '🦙',
      14: '🐎',
      15: '🐂',
      16: '🦄'
    };

    function updateFarmVisuals() {
      farmContainer.innerHTML = '';
      if (farmImages[level]) {
        const img = document.createElement('span');
        img.textContent = farmImages[level];
        img.style.fontSize = '50px';
        img.style.margin = '10px';
        img.style.display = 'inline-block';
        farmContainer.appendChild(img);
      }
    }

    function updatePoints() {
      pointsDisplay.textContent = points;
      pointsDisplay.classList.add('points-animation');
      setTimeout(() => pointsDisplay.classList.remove('points-animation'), 200);
      levelDisplay.textContent = level;

      autoClickerButton.disabled = points < 50 || autoClickerActive;
      doublePointsButton.disabled = points < 100;
      levelUpButton.disabled = points < 200 || level >= maxLevel;
    }

    function showLevelUpMessage() {
      levelUpMessage.style.display = 'block';
      setTimeout(() => {
        levelUpMessage.style.display = 'none';
      }, 3000);
    }

    clickButton.addEventListener('click', () => {
      points += pointsPerClick;
      updatePoints();
    });

    autoClickerButton.addEventListener('click', () => {
      if (points >= 50 && !autoClickerActive) {
        points -= 50;
        autoClickerActive = true;
        autoClickerButton.classList.add('disabled');
        setInterval(() => {
          points += 1;
          updatePoints();
        }, 1000);
        updatePoints();
      }
    });

    doublePointsButton.addEventListener('click', () => {
      if (points >= 100) {
        points -= 100;
        pointsPerClick *= 2;
        doublePointsButton.classList.add('disabled');
        doublePointsButton.disabled = true;
        updatePoints();
      }
    });

    levelUpButton.addEventListener('click', () => {
      if (points >= 200 && level < maxLevel) {
        points -= 200;
        level += 1;
        showLevelUpMessage();
        updatePoints();
        updateFarmVisuals();
      }
    });

    updatePoints();
    updateFarmVisuals();
  </script>
</body>
</html>
