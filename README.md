<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Music Player</title>
<style>
    body {
        background-color: #121212;
        color: #fff;
        font-family: 'Helvetica Neue', Arial, sans-serif;
        margin: 0;
        padding: 0;
    }
    header {
        padding: 20px;
        font-size: 24px;
        font-weight: bold;
        background-color: #181818;
        text-align: center;
        box-shadow: 0 2px 10px rgba(0,0,0,0.4);
    }
    .container {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
        gap: 20px;
        padding: 20px;
    }
    .card {
        background-color: #181818;
        border-radius: 12px;
        padding: 15px;
        text-align: center;
        transition: transform 0.3s ease;
    }
    .card:hover {
        transform: translateY(-5px);
    }
    .cover {
        width: 100%;
        height: 220px;
        border-radius: 10px;
        background: linear-gradient(135deg, #ff4b5c, #ff7b7b);
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 48px;
        color: rgba(255,255,255,0.8);
        margin-bottom: 15px;
    }
    .title {
        font-size: 18px;
        font-weight: bold;
        margin-bottom: 10px;
    }
    .controls {
        display: flex;
        justify-content: center;
        gap: 10px;
        margin-top: 10px;
    }
    button {
        background-color: #1db954;
        border: none;
        border-radius: 50%;
        width: 45px;
        height: 45px;
        font-size: 20px;
        color: white;
        cursor: pointer;
        transition: background 0.3s ease;
    }
    button:hover {
        background-color: #1ed760;
    }
    .stop-btn {
        background-color: #ff4b5c;
    }
    .stop-btn:hover {
        background-color: #ff6b7b;
    }
    .progress-container {
        width: 100%;
        height: 5px;
        background: #404040;
        border-radius: 5px;
        margin-top: 10px;
        position: relative;
    }
    .progress {
        height: 100%;
        width: 0%;
        background: #1db954;
        border-radius: 5px;
        transition: width 0.1s linear;
    }
    .time {
        font-size: 12px;
        color: #b3b3b3;
        margin-top: 5px;
    }
</style>
</head>
<body>

<header>🎵 My Music Collection</header>

<div class="container" id="audioList"></div>

<script>
    const audios = [
        { title: "505", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/audiofile.mp3" },
        { title: "Snowman", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/Snowman.mp3" },
        { title: "Anaweyak", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/anaweyak.mp3" },
        { title: "Derniere Dance", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/derniere%20dance.mp3" },
        { title: "Fur Elise", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/furelise.mp3" }
    ];

    const audioList = document.getElementById("audioList");
    let currentAudio = null;
    let currentProgress = null;
    let currentTimeDisplay = null;
    let updateInterval = null;

    audios.forEach(audioData => {
        const card = document.createElement("div");
        card.className = "card";

        const cover = document.createElement("div");
        cover.className = "cover";
        cover.innerHTML = "🎶";

        const title = document.createElement("div");
        title.className = "title";
        title.innerText = audioData.title;

        const progressContainer = document.createElement("div");
        progressContainer.className = "progress-container";
        const progressBar = document.createElement("div");
        progressBar.className = "progress";
        progressContainer.appendChild(progressBar);

        const timeDisplay = document.createElement("div");
        timeDisplay.className = "time";
        timeDisplay.innerText = "0:00 / 0:00";

        const controls = document.createElement("div");
        controls.className = "controls";

        const playBtn = document.createElement("button");
        playBtn.innerHTML = "▶";
        playBtn.onclick = () => {
            if (currentAudio) {
                currentAudio.pause();
                currentAudio.currentTime = 0;
                if (currentProgress) currentProgress.style.width = "0%";
                if (currentTimeDisplay) currentTimeDisplay.innerText = "0:00 / 0:00";
                clearInterval(updateInterval);
            }
            currentAudio = new Audio(audioData.src);
            currentProgress = progressBar;
            currentTimeDisplay = timeDisplay;
            currentAudio.play();
            currentAudio.onloadedmetadata = () => {
                timeDisplay.innerText = `0:00 / ${formatTime(currentAudio.duration)}`;
            };
            updateInterval = setInterval(() => {
                if (currentAudio && currentAudio.duration) {
                    progressBar.style.width = `${(currentAudio.currentTime / currentAudio.duration) * 100}%`;
                    timeDisplay.innerText = `${formatTime(currentAudio.currentTime)} / ${formatTime(currentAudio.duration)}`;
                }
            }, 500);
        };

        const stopBtn = document.createElement("button");
        stopBtn.className = "stop-btn";
        stopBtn.innerHTML = "⏹";
        stopBtn.onclick = () => {
            if (currentAudio) {
                currentAudio.pause();
                currentAudio.currentTime = 0;
                progressBar.style.width = "0%";
                timeDisplay.innerText = "0:00 / 0:00";
                clearInterval(updateInterval);
            }
        };

        controls.appendChild(playBtn);
        controls.appendChild(stopBtn);
        card.appendChild(cover);
        card.appendChild(title);
        card.appendChild(progressContainer);
        card.appendChild(timeDisplay);
        card.appendChild(controls);
        audioList.appendChild(card);
    });

    function formatTime(seconds) {
        const m = Math.floor(seconds / 60);
        const s = Math.floor(seconds % 60);
        return `${m}:${s < 10 ? "0" : ""}${s}`;
    }
</script>

</body>
</html>
