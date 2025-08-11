# EstherKamal.github.io
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Play Audio</title>
<style>
  body {
    text-align: center;
    background-color: #f9f9f9;
    font-family: Arial, sans-serif;
    padding-top: 50px;
  }
  h1 {
    color: #333;
  }
  .btn {
    border: none;
    color: white;
    padding: 15px 30px;
    margin: 10px;
    font-size: 18px;
    border-radius: 12px;
    cursor: pointer;
    transition: all 0.3s ease;
  }
  .play-btn {
    background-color: #4CAF50;
  }
  .stop-btn {
    background-color: #f44336;
  }
  .btn:hover {
    transform: scale(1.05);
    opacity: 0.9;
  }
</style>
</head>
<body>

<h1>🎶 شغل الصوت أو وقفه 🎶</h1>

<button class="btn play-btn" onclick="playAudio()">▶️ تشغيل</button>
<button class="btn stop-btn" onclick="stopAudio()">⏹ إيقاف</button>

<audio id="myAudio" src="https://github.com/EstherKamal/EstherKamal.github.io/raw/refs/heads/main/whatsApp%20Video%202025-08-10%20at%2017.06.45.mp3"></audio>

<script>
  var audio = document.getElementById("myAudio");

  function playAudio() {
    audio.play();
  }

  function stopAudio() {
    audio.pause();
    audio.currentTime = 0; // يرجع الصوت من الأول
  }
</script>

</body>
</html>
