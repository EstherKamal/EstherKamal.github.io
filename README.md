<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>زر تشغيل الصوت</title>
<style>
  body {
    font-family: Arial, sans-serif;
    direction: rtl;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: #f0f2f5;
  }
  button {
    background: #4caf50;
    border: none;
    color: white;
    padding: 15px 30px;
    font-size: 18px;
    border-radius: 8px;
    cursor: pointer;
    box-shadow: 0 5px 15px rgba(76, 175, 80, 0.4);
    transition: background 0.3s ease;
  }
  button:hover {
    background: #45a049;
  }
</style>
</head>
<body>

<button id="playBtn">تشغيل الصوت</button>

<script>
  const audioUrl = "https://github.com/EstherKamal/EstherKamal.github.io/raw/main/audiofile.mp3";
  const audio = new Audio(audioUrl);
  const button = document.getElementById("playBtn");

  button.addEventListener("click", () => {
    audio.play();
  });
</script>

</body>
</html>
