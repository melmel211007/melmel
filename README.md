<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Vivian de Ruru</title>
  <link href="https://fonts.googleapis.com/css2?family=Pacifico&family=Poppins:wght@400;600&display=swap" rel= "stylesheet">
  <style>
    body {
      font-family: 'Pacifico', sans-serif;
      max-width: 600px ;
      margin: 30px auto ;
      height: 100vh;
      padding: 20px;
      background: linear-gradient(to bottom, #ffe2f1 ,#ffc1dc);
      background-size: cover;
      background-repeat: no-repeat;
    }
    h1 {
      font-family: 'Pacifico', sans-serif;
      color: #333;
    }
    textarea {
      font-family: 'Pacifico', sans-serif;
      width: 100%;
      height: 100px;
      padding: 10px;
      font-size: 16px;
      margin-bottom: 10px;
      border-radius: 30px;
      box-sizing: content-box;
    }
    button {
      font-family: 'Pacifico', sans-serif;
      padding: 8px 16px;
      font-size: 16px;
      background-color: #ff6cbf;
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
    }
    .feedback {
      font-family: 'Pacifico', sans-serif;
      margin-top: 20px;
    }
    .item {
      font-family: 'Pacifico', sans-serif;
      background: white;
      padding: 12px;
      border-radius: 20px;
      margin-bottom: 10px;
      position: relative;
    }
    .tym-btn {
      font-family: 'Pacifico', sans-serif;
      background-color: #a6cfff;
      font-size: 14px;
      margin-top: 8px;
    }
    .tym {
      font-family: 'Pacifico', sans-serif;
      margin-left: 8px;
      font-weight: bold;
    }
    
  </style>
</head>
<body>
  <h1>thả link drive tại đây!! 💖</h1>
  <textarea id="input" placeholder="viết ở đây nè..."></textarea>
  <button onclick="submitFeedback()">💬</button>

  <div class="feedback" id="feedbackList"></div>
  
  <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>

  <script>
  var firebaseConfig = {
      apiKey: "AIzaSyBbykSdzfRHweXPFKkMoq8WcrcJxZ1Hje4",
      authDomain: "ntin-634f0.firebaseapp.com",
      databaseURL: "https://ntin-634f0-default-rtdb.asia-southeast1.firebasedatabase.app",
      projectId: "ntin-634f0",
      storageBucket: "ntin-634f0.firebasestorage.app",
      messagingSenderId: "1022364493878",
      appId: "1:1022364493878:web:4d4d1e74b0aedccaeed097",
      measurementId: "G-767VRYKH47"
    };
    firebase.initializeApp(firebaseConfig);
    var db = firebase.database();
    
    function submitFeedback() {
      const input = document.getElementById("input");
      const text = input.value.trim();
      if (text) {
        db.ref("feedbacks").push({
          text: text,
          tym: 0
        });
        input.value = "";
      }
    }

    function renderFeedback(key, fb) {
      const existing = document.getElementById("fb-" + key);
      if (existing) return;

    const div = document.createElement("div");
        div.className = "item";
        div.id ="fb-" + key;
        div.innerHTML = `
    <div class="text">${fb.text}</div>
          <button class="tym-btn" onclick="tym('${key}')">🪼</button>
          <span class="tym" id="tym-${key}">${fb.tym}</span>
        `;
        document.getElementById("feedbackList").prepend(div);
    }
        function tym(key) {
      const ref = db.ref("feedbacks/" + key + "/tym");
      ref.transaction((current) => (current || 0) + 1);
    }

    db.ref("feedbacks").on("child_added", function(snapshot) {
      renderFeedback(snapshot.key,
        snapshot.val());
    });

    db.ref("feedbacks").on("child_changed", function(snapshot) {
      const data = snapshot.val();
      const tymEl = document.getElementById("tym-" + snapshot.key);
      if (tymEl) {
        tymEl.innerText = data.tym;
      }
    });
  </script>
</body>
</html>