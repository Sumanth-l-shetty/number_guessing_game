// ========================================
// NUMBER GUESSING GAME
// Week 03 - Web Development
// ========================================


// Game settings

const difficultySettings = {

    easy: {
        maxNumber: 50,
        maxAttempts: 10
    },

    medium: {
        maxNumber: 100,
        maxAttempts: 10
    },

    hard: {
        maxNumber: 500,
        maxAttempts: 12
    }

};


// Game variables

let secretNumber;

let attempts = 0;

let maxNumber = 50;

let maxAttempts = 10;

let gameOver = false;

let currentDifficulty = "easy";


// HTML elements

const guessInput =
    document.getElementById("guessInput");

const guessButton =
    document.getElementById("guessButton");

const restartButton =
    document.getElementById("restartButton");

const message =
    document.getElementById("message");

const attemptsDisplay =
    document.getElementById("attempts");

const remainingDisplay =
    document.getElementById("remaining");

const rangeDisplay =
    document.getElementById("range");

const progress =
    document.getElementById("progress");

const progressText =
    document.getElementById("progressText");

const difficultyButtons =
    document.querySelectorAll(".difficulty");


// ========================================
// START GAME
// ========================================

function startGame() {

    const settings =
        difficultySettings[currentDifficulty];

    maxNumber = settings.maxNumber;

    maxAttempts = settings.maxAttempts;

    secretNumber =
        Math.floor(Math.random() * maxNumber) + 1;

    attempts = 0;

    gameOver = false;


    // Update display

    attemptsDisplay.textContent = attempts;

    remainingDisplay.textContent = maxAttempts;

    rangeDisplay.textContent =
        `1 - ${maxNumber}`;

    progress.style.width = "0%";

    progressText.textContent =
        `0 / ${maxAttempts}`;


    // Reset message

    message.textContent =
        "🎮 Make your first guess!";

    message.className = "message";


    // Enable controls

    guessInput.disabled = false;

    guessButton.disabled = false;

    guessInput.value = "";

    guessInput.min = 1;

    guessInput.max = maxNumber;

    guessInput.placeholder =
        `Enter a number from 1-${maxNumber}`;

    guessInput.focus();

}


// ========================================
// CHECK GUESS
// ========================================

function checkGuess() {

    if (gameOver) {
        return;
    }


    const guess =
        Number(guessInput.value);


    // Validate input

    if (
        guessInput.value === "" ||
        !Number.isInteger(guess)
    ) {

        showMessage(
            "⚠️ Please enter a valid number.",
            "warning"
        );

        return;
    }


    if (guess < 1 || guess > maxNumber) {

        showMessage(
            `⚠️ Enter a number between 1 and ${maxNumber}.`,
            "warning"
        );

        return;
    }


    // Increase attempts

    attempts++;


    // Update stats

    attemptsDisplay.textContent = attempts;

    const remaining =
        maxAttempts - attempts;

    remainingDisplay.textContent = remaining;

    progressText.textContent =
        `${attempts} / ${maxAttempts}`;

    progress.style.width =
        `${(attempts / maxAttempts) * 100}%`;


    // Check answer

    if (guess === secretNumber) {

        const score =
            calculateScore();


        showMessage(
            `🎉 Correct! The number was ${secretNumber}. You guessed it in ${attempts} attempt${attempts === 1 ? "" : "s"}! Score: ${score}`,
            "success"
        );


        endGame();

        return;
    }


    // Too low

    if (guess < secretNumber) {

        showMessage(
            `⬆️ Too low! Try a higher number.`,
            "warning"
        );

    }


    // Too high

    else {

        showMessage(
            `⬇️ Too high! Try a lower number.`,
            "warning"
        );

    }


    // Check maximum attempts

    if (attempts >= maxAttempts) {

        showMessage(
            `💀 Game Over! The correct number was ${secretNumber}.`,
            "error"
        );

        endGame();

        return;
    }


    // Clear input

    guessInput.value = "";

    guessInput.focus();

}


// ========================================
// SCORE
// ========================================

function calculateScore() {

    const remaining =
        maxAttempts - attempts;

    return Math.max(
        100 + remaining * 10,
        10
    );

}


// ========================================
// DISPLAY MESSAGE
// ========================================

function showMessage(text, type) {

    message.textContent = text;

    message.className =
        `message ${type}`;

}


// ========================================
// END GAME
// ========================================

function endGame() {

    gameOver = true;

    guessInput.disabled = true;

    guessButton.disabled = true;

}


// ========================================
// CHANGE DIFFICULTY
// ========================================

difficultyButtons.forEach(button => {

    button.addEventListener("click", () => {

        difficultyButtons.forEach(btn => {

            btn.classList.remove("active");

        });


        button.classList.add("active");


        currentDifficulty =
            button.dataset.level;


        startGame();

    });

});


// ========================================
// BUTTON EVENTS
// ========================================

guessButton.addEventListener(
    "click",
    checkGuess
);


restartButton.addEventListener(
    "click",
    startGame
);


// ========================================
// ENTER KEY SUPPORT
// ========================================

guessInput.addEventListener(
    "keydown",
    event => {

        if (event.key === "Enter") {

            checkGuess();

        }

    }
);


// ========================================
// START GAME
// ========================================

startGame();