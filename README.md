# EstherKamal.github.io
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>For You ❤️</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>For My Love 💖</h1>
    <p>I made this just for you.</p>

    <div class="player">
      <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/36/Love_Heart_symbol.svg/1024px-Love_Heart_symbol.svg.png" alt="Heart" class="album-art">
      <audio controls>
        <source src="mysong.mp3" type="audio/mpeg">
        Your browser does not support the audio tag.
      </audio>
    </div>

    <p class="note">Whenever you miss me, play this. 🎶</p>
  </div>
</body>
</html>


body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(to right, #ff9a9e, #fad0c4);
  text-align: center;
  color: #333;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 10px;
}

.player {
  margin: 30px 0;
}

.album-art {
  width: 200px;
  height: 200px;
  object-fit: cover;
  border-radius: 50%;
  margin-bottom: 15px;
  border: 5px solid white;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

audio {
  width: 100%;
}

.note {
  font-size: 1.1rem;
  color: #444;
  margin-top: 20px;
}
