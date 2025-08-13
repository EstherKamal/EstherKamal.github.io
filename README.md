
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Modern Music Library</title>
<style>
  :root {
    --bg: #121212;
    --card: #181818;
    --text: #fff;
    --muted: #b3b3b3;
    --accent: #1db954;
  }
  body {
    font-family: Arial, sans-serif;
    margin: 0;
    background: var(--bg);
    color: var(--text);
  }
  header {
    position: sticky;
    top: 0;
    background: var(--bg);
    padding: 20px;
    z-index: 1000;
  }
  header h1 {
    margin: 0;
    font-size: 1.5em;
  }
  header p {
    margin: 4px 0 12px;
    color: var(--muted);
  }
  /* Search bar */
  .search-wrap {
    margin-top: 12px;
  }
  .search-wrap input {
    width: 100%;
    padding: 10px 14px;
    border-radius: 999px;
    border: 1px solid #2a2a2a;
    background: #181818;
    color: var(--text);
    font-size: 14px;
    outline: none;
    transition: all 0.25s ease;
  }
  .search-wrap input:focus {
    border-color: var(--accent);
    background: #202020;
    width: 105%;
  }
  mark {
    background: var(--accent);
    color: white;
    padding: 0 2px;
    border-radius: 2px;
  }
  /* Grid */
  #library {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 16px;
    padding: 20px;
  }
  .card {
    background: var(--card);
    border-radius: 12px;
    padding: 16px;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    box-shadow: 0 4px 12px rgba(0,0,0,0.4);
  }
  .cover {
    width: 100%;
    height: 180px;
    background: linear-gradient(135deg, var(--accent), #14833b);
    border-radius: 8px;
    margin-bottom: 12px;
  }
  .title-sm {
    font-size: 1em;
    font-weight: bold;
    margin: 8px 0 4px;
  }
  .controls {
    display: flex;
    gap: 6px;
    margin: 8px 0;
  }
  button {
    background: var(--accent);
    border: none;
    color: white;
    padding: 6px 10px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 12px;
  }
  .seek {
    width: 100%;
    margin: 6px 0;
  }
  .time {
    display: flex;
    justify-content: space-between;
    font-size: 12px;
    color: var(--muted);
  }
</style>
</head>
<body>

<header>
  <h1>My Music</h1>
  <p>Enjoy</p>
  <div class="search-wrap">
    <input type="text" id="search" placeholder="Search for a track...">
  </div>
</header>

<main id="library"></main>

<script>
const tracks = [
  { title: "505", src: "505.mp3" },
  { title: "Snowman", src: "Snowman.mp3" },
  { title: "Sunflower", src: "Sunflower.mp3" },
  { title: "Lovely", src: "Lovely.mp3" }
];

// Sort alphabetically
tracks.sort((a, b) => a.title.localeCompare(b.title));

const library = document.getElementById("library");

tracks.forEach((track, index) => {
  const card = document.createElement("div");
  card.className = "card";
  card.innerHTML = `
    <div class="cover"></div>
    <div class="title-sm">${track.title}</div>
    <div class="controls">
      <button onclick="playTrack(${index})">▶</button>
      <button onclick="pauseTrack(${index})">⏸</button>
      <button onclick="stopTrack(${index})">⏹</button>
    </div>
    <input type="range" min="0" value="0" class="seek" id="seek-${index}">
    <div class="time">
      <span id="current-${index}">0:00</span>
      <span id="duration-${index}">0:00</span>
    </div>
  `;
  library.appendChild(card);
});

// Audio control
let audios = tracks.map(t => new Audio(t.src));
let currentTrack = null;

function playTrack(i) {
  if (currentTrack !== null && currentTrack !== i) {
    audios[currentTrack].pause();
    audios[currentTrack].currentTime = 0;
  }
  audios[i].play();
  currentTrack = i;
}

function pauseTrack(i) {
  audios[i].pause();
}

function stopTrack(i) {
  audios[i].pause();
  audios[i].currentTime = 0;
}

audios.forEach((audio, i) => {
  audio.addEventListener("loadedmetadata", () => {
    document.getElementById(`duration-${i}`).textContent = formatTime(audio.duration);
  });
  audio.addEventListener("timeupdate", () => {
    document.getElementById(`current-${i}`).textContent = formatTime(audio.currentTime);
    document.getElementById(`seek-${i}`).value = (audio.currentTime / audio.duration) * 100 || 0;
  });
  document.getElementById(`seek-${i}`).addEventListener("input", (e) => {
    audio.currentTime = (e.target.value / 100) * audio.duration;
  });
});

function formatTime(sec) {
  const m = Math.floor(sec / 60);
  const s = Math.floor(sec % 60);
  return `${m}:${s.toString().padStart(2, "0")}`;
}

// Search with highlight
const searchInput = document.getElementById("search");

searchInput.addEventListener("input", () => {
  const term = searchInput.value.toLowerCase();
  document.querySelectorAll("#library .card").forEach(card => {
    const titleElement = card.querySelector(".title-sm");
    const trackName = titleElement.getAttribute("data-original") || titleElement.textContent;

    if (!titleElement.hasAttribute("data-original")) {
      titleElement.setAttribute("data-original", trackName);
    }

    if (trackName.toLowerCase().includes(term)) {
      card.style.display = "";

      if (term) {
        const regex = new RegExp(`(${term})`, "gi");
        titleElement.innerHTML = trackName.replace(regex, `<mark>$1</mark>`);
      } else {
        titleElement.innerHTML = trackName;
      }
    } else {
      card.style.display = "none";
    }
  });
});
</script>

</body>
</html>
