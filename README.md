# demo-1
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Birthday Greeting</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to right, #ffecd2, #fcb69f);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }

    .container {
      text-align: center;
    }

    #greeting {
      font-size: 3em;
      font-weight: bold;
      color: #fff;
      margin-top: 20px;
      animation: pop 0.6s ease-in-out;
      display: none;
    }

    @keyframes pop {
      0% { transform: scale(0.6); opacity: 0; }
      100% { transform: scale(1); opacity: 1; }
    }

    #confetti {
      position: fixed;
      top: 0;
      left: 0;
      pointer-events: none;
      z-index: 1000;
    }

    input, button {
      padding: 12px 20px;
      border: none;
      border-radius: 10px;
      font-size: 1em;
      margin: 10px;
    }

    button {
      background-color: #ff6f61;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background-color: #ff3b2e;
    }
  </style>
</head>
<body>
  <canvas id="confetti"></canvas>

  <div class="container" id="form-container">
    <h2>Enter your name 🎂</h2>
    <input type="text" id="nameInput" placeholder="Your Name" />
    <br>
    <button onclick="startBirthday()">Submit</button>
  </div>

  <div class="container">
    <div id="greeting"></div>
  </div>

  <script>
    function startBirthday() {
      const name = document.getElementById("nameInput").value.trim();
      if (!name) {
        alert("Please enter a name!");
        return;
      }

      document.getElementById("form-container").style.display = "none";
      const greeting = document.getElementById("greeting");
      greeting.innerText = `🎉 Happy Birthday, ${name}! 🎉`;
      greeting.style.display = "block";
      launchConfetti();
    }

    // Confetti animation
    const confetti = document.getElementById('confetti');
    const ctx = confetti.getContext('2d');
    confetti.width = window.innerWidth;
    confetti.height = window.innerHeight;

    const pieces = [];
    for (let i = 0; i < 150; i++) {
      pieces.push({
        x: Math.random() * confetti.width,
        y: Math.random() * confetti.height - confetti.height,
        r: Math.random() * 6 + 4,
        d: Math.random() * 50 + 10,
        color: `hsl(${Math.random() * 360}, 100%, 50%)`,
        tilt: Math.random() * 10 - 10,
        tiltAngle: 0,
        tiltAngleIncrement: Math.random() * 0.07 + 0.05
      });
    }

    function drawConfetti() {
      ctx.clearRect(0, 0, confetti.width, confetti.height);
      for (let p of pieces) {
        ctx.beginPath();
        ctx.lineWidth = p.r;
        ctx.strokeStyle = p.color;
        ctx.moveTo(p.x + p.tilt + p.r / 2, p.y);
        ctx.lineTo(p.x + p.tilt, p.y + p.tilt + p.r / 2);
        ctx.stroke();
      }
      update();
    }

    function update() {
      for (let p of pieces) {
        p.tiltAngle += p.tiltAngleIncrement;
        p.y += (Math.cos(p.d) + 3 + p.r / 2) / 2;
        p.tilt = Math.sin(p.tiltAngle - p.d / 3) * 15;

        if (p.y > confetti.height) {
          p.y = -10;
          p.x = Math.random() * confetti.width;
        }
      }
    }

    function launchConfetti() {
      setInterval(drawConfetti, 30);
    }

    window.addEventListener("resize", () => {
      confetti.width = window.innerWidth;
      confetti.height = window.innerHeight;
    });
  </script>
</body>
</html>
