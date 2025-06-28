<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Send an Action!</title>
  <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&display=swap" rel="stylesheet" />
  <style>
    body {
      font-family: 'Quicksand', sans-serif;
      background: linear-gradient(135deg, #fcefe3, #ffe8cc);
      text-align: center;
      padding: 50px 20px;
      color: #4e342e;
    }
    h1 {
      font-size: 2.5rem;
      margin-bottom: 20px;
    }
    input, select, button, textarea {
      padding: 12px;
      font-size: 1rem;
      margin: 12px;
      border-radius: 10px;
      border: 1px solid #d7ccc8;
      box-shadow: 2px 2px 5px rgba(0,0,0,0.05);
      width: 250px;
      display: inline-block;
    }
    textarea {
      height: 80px;
      resize: none;
    }
    button {
      background-color: #ff7043;
      border: none;
      color: white;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #f4511e;
    }
    #response {
      margin-top: 20px;
      font-size: 1.2rem;
      color: #2c3e50;
      min-height: 24px;
    }
    .action-animation {
      animation: actionAnim 0.4s ease;
    }
    @keyframes actionAnim {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }
    ul#historyList {
      list-style: none;
      padding: 0;
      margin-top: 30px;
      text-align: left;
      max-width: 500px;
      margin-left: auto;
      margin-right: auto;
      background: #fff3e0;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.05);
    }
    ul#historyList li {
      margin-bottom: 10px;
      padding-bottom: 6px;
      border-bottom: 1px dashed #ffe0b2;
    }
    @media (max-width: 600px) {
      input, select, button, textarea {
        width: 80%;
        margin: 10px auto;
        display: block;
      }
    }
  </style>
</head>
<body>
  <h1>✨ On your mind? Let 'em know!🎉</h1>
  <p style="font-style: italic; color: #6d4c41; margin-top: -10px; margin-bottom: 30px;">
    (No chat. No buzz. Just a sweet little reminder.)
  </p>

  <input type="text" id="senderInput" placeholder="Your name" />
  <input type="text" id="receiverInput" placeholder="Who to send it to?" />
  <input type="email" id="receiverEmail" placeholder="Receiver's email address" />
  <select id="actionType">
    <option value="poked">Poke 👈</option>
    <option value="hugged">Hug 🤗</option>
    <option value="punched">Punch 👊</option>
    <option value="fist bumped">Fist Bump ✊</option>
  </select>
  <br />
  <textarea id="customMessage" placeholder="Add a silly message!"></textarea>
  <br />
  <input type="url" id="memeUrl" placeholder="Add a meme image URL (optional)" />
  <br />
  <button onclick="sendAction()">Send 🚀</button>
  <div id="response"></div>

  <h2>📜 Action History</h2>
  <ul id="historyList"></ul>
  <audio id="actionSound" src="https://www.soundjay.com/button/sounds/button-16.mp3" preload="auto"></audio>

  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>
  <script>
    emailjs.init("YOt6ObadVmSe8JS7r"); // Replace with your EmailJS public key

    async function sendAction() {
      const sender = document.getElementById('senderInput').value.trim();
      const receiver = document.getElementById('receiverInput').value.trim();
      const receiverEmail = document.getElementById('receiverEmail').value.trim();
      const action = document.getElementById('actionType').value;
      const message = document.getElementById('customMessage').value.trim();
      const memeUrl = document.getElementById('memeUrl').value.trim();
      const response = document.getElementById('response');
      const actionSound = document.getElementById('actionSound');

      if (!sender || !receiver || !receiverEmail) {
        response.textContent = "Please enter all required fields including receiver email.";
        return;
      }

      try {
        actionSound.play();
        response.textContent = `You ${action} ${receiver}!`;
        response.classList.add("action-animation");
        setTimeout(() => response.classList.remove("action-animation"), 500);

        await emailjs.send("service_oxqueld", "template_ver8gt6", {
          from_name: sender,
          to_name: receiver,
          to_email: receiverEmail,
          action_type: action,
          custom_message: message,
          meme_url: memeUrl
        });

        response.textContent += " Email sent!";
      } catch (error) {
        console.error("Error:", error);
        response.textContent = "Something went wrong!";
      }
    }
  </script>
</body>
</html>
