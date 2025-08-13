<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Audio Gift</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background: linear-gradient(135deg, #ffdde1, #ee9ca7);
        text-align: center;
        padding: 20px;
        color: #333;
    }
    h1 {
        color: white;
        margin-bottom: 20px;
    }
    .audio-card {
        background: white;
        border-radius: 15px;
        padding: 20px;
        margin: 15px auto;
        max-width: 350px;
        box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    }
    .audio-title {
        font-size: 18px;
        font-weight: bold;
        margin-bottom: 15px;
        color: #ff4b5c;
    }
    button {
        padding: 10px 20px;
        margin: 5px;
        border: none;
        border-radius: 10px;
        font-size: 14px;
        cursor: pointer;
        transition: all 0.3s ease;
    }
    .play-btn {
        background-color: #ff4b5c;
        color: white;
    }
    .stop-btn {
        background-color: #999;
        color: white;
    }
    button:hover {
        transform: scale(1.05);
        opacity: 0.9;
    }
</style>
</head>
<body>

<h1>💝 Audio Gift 💝</h1>

<div id="audioList"></div>

<script>
    const audios = [
        { title: "Audio File", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/audiofile.mp3" },
        { title: "Snowman", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/Snowman.mp3" },
        { title: "Anaweyak", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/anaweyak.mp3" },
        { title: "Derniere Dance", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/derniere%20dance.mp3" },
        { title: "Fur Elise", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/furelise.mp3" }
    ];

    const audioList = document.getElementById("audioList");
    let currentAudio = null;

    audios.forEach(audioData => {
        const card = document.createElement("div");
        card.className = "audio-card";
        
        const title = document.createElement("div");
        title.className = "audio-title";
        title.innerText = audioData.title;
        
        const playBtn = document.createElement("button");
        playBtn.className = "play-btn";
        playBtn.innerText = "▶ Play";
        playBtn.onclick = () => {
            if (currentAudio) {
                currentAudio.pause();
                currentAudio.currentTime = 0;
            }
            currentAudio = new Audio(audioData.src);
            currentAudio.play();
        };
        
        const stopBtn = document.createElement("button");
        stopBtn.className = "stop-btn";
        stopBtn.innerText = "⏹ Stop";
        stopBtn.onclick = () => {
            if (currentAudio) {
                currentAudio.pause();
                currentAudio.currentTime = 0;
            }
        };
        
        card.appendChild(title);
        card.appendChild(playBtn);
        card.appendChild(stopBtn);
        audioList.appendChild(card);
    });
</script>

</body>
</html>
