<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Teacher Seb's Site</title>
  <style>
    /* General Body Styling */
    body {
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
      background: url('https://images.pexels.com/photos/6985128/pexels-photo-6985128.jpeg') no-repeat center center fixed;
      background-size: cover;
      color: white;
    }

    /* Title Section */
    .title-container {
      text-align: center;
      margin-top: 15%;
    }

    .title-container h1 {
      font-size: 60px;
      font-weight: bold;
      color: white;
      text-shadow: 0px 0px 10px #ffffff;
    }

    .start-container {
      text-align: center;
      margin-top: 30px;
    }

    #nameInput {
      padding: 15px;
      font-size: 18px;
      width: 300px;
      border: 2px solid white;
      background: transparent;
      color: white;
      border-radius: 15px;
      text-align: center;
    }

    #startButton {
      display: inline-block;
      padding: 15px 30px;
      font-size: 18px;
      font-weight: bold;
      color: white;
      border: 2px solid white;
      border-radius: 20px;
      background: transparent;
      cursor: pointer;
      margin-top: 15px;
    }

    /* Game Section */
    .game-container {
      display: none;
      text-align: center;
      margin: 20px;
    }

    .game-container h2 {
      font-size: 36px;
      margin-bottom: 20px;
    }

    .question-image {
      margin: 20px auto;
      width: 600px;
      height: 400px; /* Ensures consistent size for all images */
      object-fit: cover;
      border-radius: 10px;
    }

    .answers {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .answer-option {
      margin: 10px 0;
      padding: 15px;
      width: 60%;
      border: 2px solid white;
      border-radius: 20px;
      font-size: 18px;
      color: white;
      background: transparent;
      cursor: pointer;
      text-align: center;
    }

    .answer-option:hover {
      background: rgba(255, 255, 255, 0.1);
    }

    .answer-option.correct {
      background-color: green;
    }

    .answer-option.incorrect {
      background-color: red;
    }

    /* Leaderboard Section */
    .leaderboard {
      display: none;
      text-align: left;
      margin: 50px auto;
      width: 60%;
      background: rgba(0, 0, 0, 0.5);
      padding: 30px;
      border-radius: 15px;
    }

    .leaderboard h2 {
      text-align: center;
      font-size: 36px;
      margin-bottom: 20px;
      text-shadow: 0 0 10px white;
      color: gold;
    }

    .leaderboard table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
    }

    .leaderboard th,
    .leaderboard td {
      padding: 10px;
      text-align: left;
      border-bottom: 1px solid rgba(255, 255, 255, 0.3);
    }

    .leaderboard th {
      font-weight: bold;
      font-size: 18px;
      color: gold;
    }

    .leaderboard td {
      font-size: 16px;
      color: white;
    }

    .leaderboard tr:nth-child(even) {
      background-color: rgba(255, 255, 255, 0.1);
    }
  </style>
</head>
<body>
  <!-- Title Section -->
  <div class="title-container">
    <h1>Welcome to Teacher Seb's Site</h1>
  </div>

  <!-- Enter Name Section -->
  <div class="start-container">
    <input type="text" id="nameInput" placeholder="Enter your name">
    <button id="startButton">Start Game</button>
  </div>

  <!-- Game Section -->
  <div class="game-container" id="gameContainer">
    <h2 id="questionText"></h2>
    <img src="" alt="Question Image" id="questionImage" class="question-image">
    <div class="answers" id="answersContainer"></div>
  </div>

  <!-- Leaderboard Section -->
  <div class="leaderboard" id="leaderboard">
    <h2>Congratulations!</h2>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Score</th>
        </tr>
      </thead>
      <tbody id="leaderboardBody">
        <!-- Scores will be populated dynamically -->
      </tbody>
    </table>
  </div>

  <script>
    const questions = [
      {
        text: "CAUSE: What happens if students sleep late?",
        image: "https://images.stockcake.com/public/e/f/e/efe81e4d-27b0-472a-855c-724370c17d59_large/late-night-studying-stockcake.jpg",
        answers: ["They feel tired", "They score high on exams", "They are energized"],
        correct: 0
      },
      {
        text: "CAUSE: What happens if students don't review?",
        image: "https://s39613.pcdn.co/wp-content/uploads/2019/07/rethinking-deadline-and-late-penalty-policies-080519.jpg",
        answers: ["They fail", "They pass easily", "They win awards"],
        correct: 0
      },
      {
        text: "CAUSE: What happens if students eat healthy food?",
        image: "https://healthy-food-choices-in-schools.extension.org/wp-content/uploads/2019/06/iStock_000005310253Small.jpg",
        answers: ["They perform better", "They feel lazy", "They become sick"],
        correct: 0
      },
      {
        text: "CAUSE: What happens if students manage their time well?",
        image: "https://blog.mentoria.com/wp-content/uploads/2022/08/1_11zon-1024x576.jpg",
        answers: ["They complete tasks", "They procrastinate", "They get low grades"],
        correct: 0
      },
      {
        text: "CAUSE: What happens if students play mobile games all day?",
        image: "https://watermark.lovepik.com/photo/20211210/large/lovepik-playing-mobile-game-in-the-college-students-in-the-picture_501788251.jpg",
        answers: ["They neglect studies", "They excel in class", "They feel fresh"],
        correct: 0
      }
    ];

    let currentQuestion = 0;
    let score = 0;
    const nameInput = document.getElementById("nameInput");
    const startButton = document.getElementById("startButton");
    const gameContainer = document.getElementById("gameContainer");
    const questionText = document.getElementById("questionText");
    const questionImage = document.getElementById("questionImage");
    const answersContainer = document.getElementById("answersContainer");
    const leaderboard = document.getElementById("leaderboard");
    const leaderboardBody = document.getElementById("leaderboardBody");

    startButton.addEventListener("click", () => {
      const playerName = nameInput.value.trim();
      if (!playerName) {
        alert("Please enter your name!");
        return;
      }
      document.querySelector(".title-container").style.display = "none";
      document.querySelector(".start-container").style.display = "none";
      gameContainer.style.display = "block";
      showQuestion();
    });

    function showQuestion() {
      if (currentQuestion >= questions.length) {
        showLeaderboard();
        return;
      }
      const q = questions[currentQuestion];
      questionText.textContent = q.text;
      questionImage.src = q.image;
      answersContainer.innerHTML = "";
      q.answers.forEach((answer, index) => {
        const button = document.createElement("button");
        button.textContent = answer;
        button.className = "answer-option";
        button.addEventListener("click", () => handleAnswer(index, button));
        answersContainer.appendChild(button);
      });
    }

    function handleAnswer(selected, button) {
      const q = questions[currentQuestion];
      if (selected === q.correct) {
        score++;
        button.classList.add("correct");
      } else {
        button.classList.add("incorrect");
      }
      setTimeout(() => {
        currentQuestion++;
        showQuestion();
      }, 1000);
    }

    function showLeaderboard() {
      gameContainer.style.display = "none";
      leaderboard.style.display = "block";
      leaderboardBody.innerHTML = `<tr>
        <td>${nameInput.value}</td>
        <td>${score}</td>
      </tr>`;
    }
  </script>
</body>
</html>
