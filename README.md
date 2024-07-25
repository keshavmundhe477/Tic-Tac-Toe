# Tic-Tac-Toe

This is a simple Tic-Tac-Toe game built using HTML, CSS, and JavaScript. The game allows two players to take turns marking spaces in a 3x3 grid, with the objective of placing three of their marks in a horizontal, vertical, or diagonal row.

## Table of Contents

- [Demo](#demo)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Code Overview](#code-overview)


## Demo

You can play the game [here](https://<username>.github.io/Tic-Tac-Toe/).

## Features

- Two-player game (Player X and Player O)
- Turn-based play
- Detects win conditions and announces the winner
- Detects draw conditions and announces a draw
- Option to reset the game or start a new game

## Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/yourusername/tic-tac-toe.git
    ```
2. Navigate to the project directory:
    ```sh
    cd tic-tac-toe
    ```

## Usage

1. Open `index.html` in your web browser:
    ```sh
    open index.html
    ```

2. Start playing the game by clicking on the boxes to mark your moves.

## Code Overview

### index.html

This file contains the structure of the game. It includes a title, a main section where the game is displayed, and buttons for resetting the game or starting a new game.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Tic-Tac-Toe Game</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div class="msg-container hide">
      <p id="msg">Winner</p>
      <button id="new-btn">New Game</button>
    </div>
    <main>
      <h1>Tic Tac Toe</h1>
      <div class="container">
        <div class="game">
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
          <button class="box"></button>
        </div>
      </div>
      <button id="reset-btn">Reset Game</button>
    </main>
    <script src="app.js"></script>
  </body>
</html>

### app.js

let boxes = document.querySelectorAll(".box");
let resetBtn = document.querySelector("#reset-btn");
let newGameBtn = document.querySelector("#new-btn");
let msgContainer = document.querySelector(".msg-container");
let msg = document.querySelector("#msg");

let turnO = true; // Player O starts
let count = 0; // Track the number of turns

const winPatterns = [
  [0, 1, 2],
  [0, 3, 6],
  [0, 4, 8],
  [1, 4, 7],
  [2, 5, 8],
  [2, 4, 6],
  [3, 4, 5],
  [6, 7, 8],
];

const resetGame = () => {
  turnO = true;
  count = 0;
  enableBoxes();
  msgContainer.classList.add("hide");
};

boxes.forEach((box) => {
  box.addEventListener("click", () => {
    if (turnO) {
      box.innerText = "O";
      turnO = false;
    } else {
      box.innerText = "X";
      turnO = true;
    }
    box.disabled = true;
    count++;

    let isWinner = checkWinner();

    if (count === 9 && !isWinner) {
      gameDraw();
    }
  });
});

const gameDraw = () => {
  msg.innerText = `Game was a Draw.`;
  msgContainer.classList.remove("hide");
  disableBoxes();
};

const disableBoxes = () => {
  boxes.forEach(box => box.disabled = true);
};

const enableBoxes = () => {
  boxes.forEach(box => {
    box.disabled = false;
    box.innerText = "";
  });
};

const showWinner = (winner) => {
  msg.innerText = `Congratulations, Winner is ${winner}`;
  msgContainer.classList.remove("hide");
  disableBoxes();
};

const checkWinner = () => {
  for (let pattern of winPatterns) {
    let [pos1, pos2, pos3] = pattern;
    let pos1Val = boxes[pos1].innerText;
    let pos2Val = boxes[pos2].innerText;
    let pos3Val = boxes[pos3].innerText;

    if (pos1Val !== "" && pos1Val === pos2Val && pos2Val === pos3Val) {
      showWinner(pos1Val);
      return true;
    }
  }
  return false;
};

newGameBtn.addEventListener("click", resetGame);
resetBtn.addEventListener("click", resetGame);
