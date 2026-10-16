
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Desafio Mundial 🌍</title>
<style>
body {
  background: #101827;
  color: white;
  font-family: Arial, sans-serif;
  text-align: center;
  padding: 20px;
}
.card {
  max-width: 550px;
  margin: auto;
  background: #1e293b;
  padding: 25px;
  border-radius: 20px;
}
h1 { color: #38e8b5; }
#flag { font-size: 110px; margin: 20px; }
button {
  display: block;
  width: 100%;
  padding: 14px;
  margin: 10px 0;
  border: none;
  border-radius: 10px;
  font-size: 17px;
  cursor: pointer;
  font-weight: bold;
}
.option { background: #334155; color: white; }
.option:hover { background: #0d9488; }
#next, #restart { background: #38e8b5; color: #102027; }
#feedback { font-size: 20px; font-weight: bold; }
</style>
</head>
<body>
<div class="card">
<h1>🌍 DESAFIO MUNDIAL</h1>
<p>Você conhece as bandeiras do mundo?</p>
<p id="difficulty">Nível fácil</p>
<p>Pergunta <span id="count">1</span> de 10</p>

<div id="flag">🇧🇷</div>
<h2>Qual é o país desta bandeira?</h2>
<div id="answers"></div>
<p id="feedback"></p>
<p>🏆 Pontos: <span id="score">0</span></p>
<button id="next" onclick="nextQuestion()" hidden>Próxima ➜</button>
</div>

<script>
const questions = [
["🇧🇷","Brasil",["Brasil","Argentina","Portugal","Chile"]],
["🇯🇵","Japão",["China","Japão","Coreia do Sul","Índia"]],
["🇨🇦","Canadá",["Estados Unidos","Dinamarca","Canadá","Suíça"]],
["🇪🇬","Egito",["Egito","Iraque","Síria","Líbano"]],
["🇦🇺","Austrália",["Nova Zelândia","Austrália","Reino Unido","Fiji"]],
["🇰🇮","Kiribati",["Kiribati","Maldivas","Samoa","Palau"]],
["🇧🇹","Butão",["Nepal","Laos","Butão","Mianmar"]],
["🇸🇷","Suriname",["Guiana","Suriname","Panamá","Belize"]],
["🇰🇲","Comores",["Comores","Maurício","Barbados","Seicheles"]],
["🇱🇸","Lesoto",["Lesoto","Eswatini","Botsuana","Ruanda"]]
];

let index = 0;
let score = 0;
let locked = false;

function showQuestion() {
  locked = false;
  const q = questions[index];
  document.getElementById("flag").textContent = q[0];
  document.getElementById("count").textContent = index + 1;
  document.getElementById("difficulty").textContent =
    index < 5 ? "🟢 Nível fácil" : "🔥 Nível difícil";
  document.getElementById("feedback").textContent = "";
  document.getElementById("next").hidden = true;

  const area = document.getElementById("answers");
  area.innerHTML = "";

  const choices = [...q[2]].sort(() => Math.random() - 0.5);
  choices.forEach(choice => {
    const b = document.createElement("button");
    b.className = "option";
    b.textContent = choice;
    b.onclick = () => answer(choice, b);
    area.appendChild(b);
  });
}

function answer(choice, button) {
  if (locked) return;
  locked = true;
  const correct = questions[index][1];

  document.querySelectorAll(".option").forEach(b => {
    b.disabled = true;
    if (b.textContent === correct) b.style.background = "#15803d";
  });

  if (choice === correct) {
    const points = index < 5 ? 10 : 30;
    score += points;
    document.getElementById("feedback").textContent =
      "✅ Correto! +" + points + " pontos!";
  } else {
    button.style.background = "#b91c1c";
    document.getElementById("feedback").textContent =
      "❌ Resposta: " + correct;
  }

  document.getElementById("score").textContent = score;
  document.getElementById("next").hidden = false;
  document.getElementById("next").textContent =
    index === 9 ? "Ver resultado 🏆" : "Próxima ➜";
}

function nextQuestion() {
  index++;
  if (index >= questions.length) {
    document.querySelector(".card").innerHTML = `
      <h1>🏆 FIM DO DESAFIO!</h1>
      <p style="font-size:70px">🌎</p>
      <h2>Você fez ${score} de 200 pontos!</h2>
      <p>${score >= 150 ? "Excelente! Você conhece muito os países!" :
      score >= 80 ? "Muito bem! Continue praticando!" :
      "Tente de novo e vire um mestre das bandeiras!"}</p>
      <button id="restart" onclick="location.reload()">Jogar novamente 🔄</button>
    `;
  } else {
    showQuestion();
  }
}

showQuestion();
</script>
</body>
</html>

