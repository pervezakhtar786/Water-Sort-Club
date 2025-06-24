<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Water Sort Puzzle - Dynamic Bottles</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #e0f7fa;
      text-align: center;
    }
    h1 {
      margin-top: 10px;
    }
    #game {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      padding: 20px;
    }
    .bottle {
      width: 60px;
      height: 180px;
      border: 3px solid #333;
      border-radius: 20px;
      margin: 10px;
      display: flex;
      flex-direction: column-reverse;
      overflow: hidden;
      background: #fff;
    }
    .color {
      height: 25%;
      width: 100%;
      transition: all 0.3s ease;
    }
    .selected {
      border: 3px solid red;
    }
    #level {
      margin-top: 10px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <h1>Water Sort Puzzle</h1>
  <div id="level">Level: 1</div>
  <div id="game"></div>

  <script>
    const colors = ['red', 'blue', 'green', 'yellow', 'orange', 'purple', 'pink', 'brown', 'cyan', 'lime'];
    const maxLevel = 50;
    let bottles = [];
    let selectedBottle = null;
    let currentLevel = 1;

    function getBottleCount(level) {
      return 6 + Math.floor(level / 5);
    }

    function getColorCount(level) {
      return Math.min(4 + Math.floor(level / 10), colors.length);
    }

    function createBottle() {
      return [];
    }

    function setupLevel(level) {
      document.getElementById('level').textContent = "Level: " + level;

      const bottleCount = getBottleCount(level);
      const colorCount = getColorCount(level);
      const colorUnits = [];

      for (let i = 0; i < colorCount; i++) {
        for (let j = 0; j < 4; j++) {
          colorUnits.push(colors[i]);
        }
      }

      bottles = Array.from({ length: bottleCount }, () => createBottle());

      // Shuffle and distribute
      shuffleArray(colorUnits);
      let index = 0;
      for (let i = 0; i < bottleCount; i++) {
        for (let j = 0; j < 4; j++) {
          if (index < colorUnits.length) {
            bottles[i].push(colorUnits[index++]);
          }
        }
      }

      renderBottles();
    }

    function renderBottles() {
      const game = document.getElementById('game');
      game.innerHTML = '';
      bottles.forEach((bottle, index) => {
        const div = document.createElement('div');
        div.className = 'bottle';
        div.dataset.index = index;

        if (selectedBottle === index) {
          div.classList.add('selected');
        }

        for (let color of bottle) {
          const colorDiv = document.createElement('div');
          colorDiv.className = 'color';
          colorDiv.style.backgroundColor = color;
          div.appendChild(colorDiv);
        }

        div.addEventListener('click', () => handleClick(index));
        game.appendChild(div);
      });
    }

    function handleClick(index) {
      if (selectedBottle === null) {
        if (bottles[index].length > 0) {
          selectedBottle = index;
          renderBottles();
        }
      } else {
        if (selectedBottle !== index && canPour(selectedBottle, index)) {
          pour(selectedBottle, index);
          selectedBottle = null;
          renderBottles();
          if (checkWin()) {
            setTimeout(() => {
              if (currentLevel < maxLevel) {
                currentLevel++;
                setupLevel(currentLevel);
              } else {
                alert("🎉 You completed all 50 levels!");
              }
            }, 500);
          }
        } else if (selectedBottle === index) {
          selectedBottle = null;
          renderBottles();
        }
      }
    }

    function canPour(from, to) {
      const fromBottle = bottles[from];
      const toBottle = bottles[to];

      if (fromBottle.length === 0 || toBottle.length >= 4) return false;

      const movingColor = fromBottle[fromBottle.length - 1];
      if (toBottle.length === 0) return true;

      const topColor = toBottle[toBottle.length - 1];
      return movingColor === topColor;
    }

    function pour(from, to) {
      const fromBottle = bottles[from];
      const toBottle = bottles[to];
      const movingColor = fromBottle[fromBottle.length - 1];
      let count = 0;

      while (fromBottle.length > 0 &&
        fromBottle[fromBottle.length - 1] === movingColor &&
        toBottle.length < 4) {
        toBottle.push(fromBottle.pop());
        count++;
      }
    }

    function checkWin() {
      return bottles.every(bottle =>
        bottle.length === 0 ||
        (bottle.length === 4 && bottle.every(color => color === bottle[0]))
      );
    }

    function shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }

    setupLevel(currentLevel);
  </script>
</body>
</html><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Water Sort Puzzle - Dynamic Bottles</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #e0f7fa;
      text-align: center;
    }
    h1 {
      margin-top: 10px;
    }
    #game {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      padding: 20px;
    }
    .bottle {
      width: 60px;
      height: 180px;
      border: 3px solid #333;
      border-radius: 20px;
      margin: 10px;
      display: flex;
      flex-direction: column-reverse;
      overflow: hidden;
      background: #fff;
    }
    .color {
      height: 25%;
      width: 100%;
      transition: all 0.3s ease;
    }
    .selected {
      border: 3px solid red;
    }
    #level {
      margin-top: 10px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <h1>Water Sort Puzzle</h1>
  <div id="level">Level: 1</div>
  <div id="game"></div>

  <script>
    const colors = ['red', 'blue', 'green', 'yellow', 'orange', 'purple', 'pink', 'brown', 'cyan', 'lime'];
    const maxLevel = 50;
    let bottles = [];
    let selectedBottle = null;
    let currentLevel = 1;

    function getBottleCount(level) {
      return 6 + Math.floor(level / 5);
    }

    function getColorCount(level) {
      return Math.min(4 + Math.floor(level / 10), colors.length);
    }

    function createBottle() {
      return [];
    }

    function setupLevel(level) {
      document.getElementById('level').textContent = "Level: " + level;

      const bottleCount = getBottleCount(level);
      const colorCount = getColorCount(level);
      const colorUnits = [];

      for (let i = 0; i < colorCount; i++) {
        for (let j = 0; j < 4; j++) {
          colorUnits.push(colors[i]);
        }
      }

      bottles = Array.from({ length: bottleCount }, () => createBottle());

      // Shuffle and distribute
      shuffleArray(colorUnits);
      let index = 0;
      for (let i = 0; i < bottleCount; i++) {
        for (let j = 0; j < 4; j++) {
          if (index < colorUnits.length) {
            bottles[i].push(colorUnits[index++]);
          }
        }
      }

      renderBottles();
    }

    function renderBottles() {
      const game = document.getElementById('game');
      game.innerHTML = '';
      bottles.forEach((bottle, index) => {
        const div = document.createElement('div');
        div.className = 'bottle';
        div.dataset.index = index;

        if (selectedBottle === index) {
          div.classList.add('selected');
        }

        for (let color of bottle) {
          const colorDiv = document.createElement('div');
          colorDiv.className = 'color';
          colorDiv.style.backgroundColor = color;
          div.appendChild(colorDiv);
        }

        div.addEventListener('click', () => handleClick(index));
        game.appendChild(div);
      });
    }

    function handleClick(index) {
      if (selectedBottle === null) {
        if (bottles[index].length > 0) {
          selectedBottle = index;
          renderBottles();
        }
      } else {
        if (selectedBottle !== index && canPour(selectedBottle, index)) {
          pour(selectedBottle, index);
          selectedBottle = null;
          renderBottles();
          if (checkWin()) {
            setTimeout(() => {
              if (currentLevel < maxLevel) {
                currentLevel++;
                setupLevel(currentLevel);
              } else {
                alert("🎉 You completed all 50 levels!");
              }
            }, 500);
          }
        } else if (selectedBottle === index) {
          selectedBottle = null;
          renderBottles();
        }
      }
    }

    function canPour(from, to) {
      const fromBottle = bottles[from];
      const toBottle = bottles[to];

      if (fromBottle.length === 0 || toBottle.length >= 4) return false;

      const movingColor = fromBottle[fromBottle.length - 1];
      if (toBottle.length === 0) return true;

      const topColor = toBottle[toBottle.length - 1];
      return movingColor === topColor;
    }

    function pour(from, to) {
      const fromBottle = bottles[from];
      const toBottle = bottles[to];
      const movingColor = fromBottle[fromBottle.length - 1];
      let count = 0;

      while (fromBottle.length > 0 &&
        fromBottle[fromBottle.length - 1] === movingColor &&
        toBottle.length < 4) {
        toBottle.push(fromBottle.pop());
        count++;
      }
    }

    function checkWin() {
      return bottles.every(bottle =>
        bottle.length === 0 ||
        (bottle.length === 4 && bottle.every(color => color === bottle[0]))
      );
    }

    function shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j],ray[i]];
      }
    }

    setupLevel(currentLevel);
  </script>
</body>
</html>
