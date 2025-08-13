
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Modern Music Library</title>
<style>
  :root{
    --bg:#121212; --panel:#181818; --muted:#b3b3b3;
    --text:#ffffff; --accent:#1db954; --accent-2:#1ed760;
    --danger:#ff4b5c; --danger-2:#ff6b7b; --track:#3a3a3a;
    --ring: rgba(29,185,84,.35);
  }
  *{box-sizing:border-box}
  html,body{margin:0;height:100%}
  body{
    background: var(--bg);
    color: var(--text);
    font: 400 15px/1.6 system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
  }
  header{
    position:sticky; top:0; z-index:5;
    background: linear-gradient(180deg,#0f0f0f,#121212 40%);
    padding: 24px 20px 12px;
    border-bottom: 1px solid #202020;
  }
  .title{margin:0 0 6px; font-size:24px; font-weight:700}
  .subtitle{margin:0; color:var(--muted); font-size:13px}

  .wrap{max-width:1100px; margin:0 auto; padding: 18px 16px 40px;}
  .grid{
    display:grid; gap:18px;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  }

  .card{
    background: var(--panel);
    border: 1px solid #222;
    border-radius:16px;
    padding:14px;
    box-shadow: 0 8px 28px rgba(0,0,0,.25);
    display:flex; flex-direction:column; gap:12px;
    transition: transform .2s ease, border-color .2s ease;
  }
  .card:hover{ transform: translateY(-4px); border-color:#2a2a2a; }

  .cover{
    height: 180px; border-radius:12px; overflow:hidden;
    background: linear-gradient(135deg, #2a2a2a, #3b3b3b);
    position:relative;
  }
  /* subtle decorative gradient beams for a modern look (no emojis, no images) */
  .cover::before,.cover::after{
    content:""; position:absolute; inset:-30%;
    background: conic-gradient(from 120deg,#1db95422, transparent 40%, #1ed76022 60%, transparent 80%);
    filter: blur(16px); transform: rotate(8deg);
  }
  .cover::after{ transform: rotate(-12deg); }

  .meta{display:flex; align-items:center; justify-content:space-between; gap:8px}
  .title-sm{font-weight:700; font-size:16px; white-space:nowrap; overflow:hidden; text-overflow:ellipsis}
  .duration{color:var(--muted); font-size:12px}

  .bar{
    display:flex; align-items:center; gap:8px;
  }
  .time{width:42px; text-align:center; font-variant-numeric: tabular-nums; color:var(--muted); font-size:12px}
  input[type="range"]{
    -webkit-appearance:none; appearance:none; width:100%; height:6px; background:var(--track);
    border-radius:999px; outline:none; cursor:pointer; position:relative;
  }
  input[type="range"]::-webkit-slider-thumb{
    -webkit-appearance:none; appearance:none; width:14px; height:14px; border-radius:50%;
    background: var(--accent); border: 0; box-shadow: 0 0 0 6px var(--ring);
    transition: transform .06s ease;
  }
  input[type="range"]::-moz-range-thumb{
    width:14px; height:14px; border-radius:50%; background: var(--accent); border:0;
  }
  /* progress fill for Firefox */
  input[type="range"]::-moz-range-progress{ background: var(--accent); height:6px; border-radius:999px }
  input[type="range"]::-moz-range-track{ background: var(--track); height:6px; border-radius:999px }

  .controls{ display:flex; gap:10px; }
  .btn{
    display:inline-grid; place-items:center;
    width:42px; height:42px; border-radius:999px; border:1px solid #2a2a2a;
    background:#222; color:var(--text); cursor:pointer;
    transition: transform .12s ease, background .2s ease, border-color .2s ease;
  }
  .btn:hover{ transform: translateY(-2px); }
  .btn--play{ background: var(--accent); border-color: #169b45; }
  .btn--play:hover{ background: var(--accent-2); }
  .btn--stop{ background:#2a2a2a; }
  .btn--stop:hover{ background:#333; border-color:#3a3a3a; }
  .icon{ width:18px; height:18px; }

  footer{ color:var(--muted); text-align:center; font-size:12px; padding:24px 0 40px;}
</style>
</head>
<body>

<header>
  <div class="wrap">
    <h1 class="title">My music</h1>
    <p class="subtitle">click ▶ to listen.</p>
  </div>
</header>

<main class="wrap">
  <section id="library" class="grid" aria-label="Tracks grid"></section>
</main>

<footer>Enjoy✨</footer>

<script>
/* ---------- CONFIG: Your tracks (arranged alphabetically) ---------- */
const tracks = [
  { title: "505",            src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/audiofile.mp3" },
  { title: "Anaweyak",       src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/anaweyak.mp3" },
  { title: "Derniere Dance", src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/derniere%20dance.mp3" },
  { title: "Fur Elise",      src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/furelise.mp3" },
  { title: "Snowman",        src: "https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/Snowman.mp3" },
].sort((a,b)=> a.title.localeCompare(b.title));

/* ---------- Utilities ---------- */
const formatTime = s => {
  if (!Number.isFinite(s) || s < 0) return "0:00";
  const m = Math.floor(s/60);
  const sec = Math.floor(s%60).toString().padStart(2,"0");
  return `${m}:${sec}`;
};

/* ---------- Build UI ---------- */
const container = document.getElementById("library");
const audioEls = new Map();    // id -> HTMLAudioElement
const ui = new Map();          // id -> {range,current,duration,playBtn}

let currentId = null;

tracks.forEach((t, i) => {
  const id = `track-${i}`;

  const card = document.createElement("article");
  card.className = "card";
  card.setAttribute("aria-label", t.title);

  const cover = document.createElement("div");
  cover.className = "cover";
  cover.setAttribute("aria-hidden","true");

  const meta = document.createElement("div");
  meta.className = "meta";
  const title = document.createElement("div");
  title.className = "title-sm";
  title.textContent = t.title;
  const duration = document.createElement("div");
  duration.className = "duration";
  duration.textContent = "0:00";
  meta.appendChild(title); meta.appendChild(duration);

  const bar = document.createElement("div");
  bar.className = "bar";
  const current = document.createElement("div");
  current.className = "time"; current.textContent = "0:00";
  const range = document.createElement("input");
  range.type = "range"; range.min = 0; range.max = 1000; range.value = 0; range.step = 1; range.ariaLabel = `Seek ${t.title}`;
  const total = document.createElement("div");
  total.className = "time"; total.textContent = "0:00";
  bar.appendChild(current); bar.appendChild(range); bar.appendChild(total);

  const controls = document.createElement("div");
  controls.className = "controls";
  const playBtn = document.createElement("button");
  playBtn.className = "btn btn--play"; playBtn.title = "Play/Pause";
  playBtn.innerHTML = `<svg class="icon" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>`;
  const stopBtn = document.createElement("button");
  stopBtn.className = "btn btn--stop"; stopBtn.title = "Stop";
  stopBtn.innerHTML = `<svg class="icon" viewBox="0 0 24 24" fill="currentColor"><path d="M6 6h12v12H6z"/></svg>`;
  controls.appendChild(playBtn); controls.appendChild(stopBtn);

  card.appendChild(cover);
  card.appendChild(meta);
  card.appendChild(bar);
  card.appendChild(controls);
  container.appendChild(card);

  // audio element (hidden)
  const audio = new Audio(t.src);
  audio.preload = "metadata";
  audioEls.set(id, audio);
  ui.set(id, { range, current, total, duration, playBtn });

  // metadata
  audio.addEventListener("loadedmetadata", () => {
    total.textContent = formatTime(audio.duration);
    duration.textContent = formatTime(audio.duration);
  });

  // update progress
  audio.addEventListener("timeupdate", () => {
    if (!audio.duration) return;
    range.value = Math.floor((audio.currentTime / audio.duration) * 1000);
    current.textContent = formatTime(audio.currentTime);
  });

  // ended -> reset UI
  audio.addEventListener("ended", () => {
    if (currentId === id) currentId = null;
    current.textContent = "0:00";
    range.value = 0;
    playBtn.innerHTML = `<svg class="icon" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>`;
  });

  // seek by dragging the range
  let seeking = false;
  range.addEventListener("input", () => {
    if (!audio.duration) return;
    seeking = true;
    const pct = range.value / 1000;
    audio.currentTime = pct * audio.duration;
    current.textContent = formatTime(audio.currentTime);
  });
  range.addEventListener("change", () => { seeking = false; });

  // play/pause toggle
  playBtn.addEventListener("click", () => {
    // stop any other
    if (currentId && currentId !== id) {
      const otherAudio = audioEls.get(currentId);
      const otherUI = ui.get(currentId);
      if (otherAudio && otherUI) {
        otherAudio.pause();
        otherAudio.currentTime = 0;
        otherUI.range.value = 0;
        otherUI.current.textContent = "0:00";
        otherUI.playBtn.innerHTML = `<svg class="icon" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>`;
      }
    }

    if (audio.paused) {
      audio.play();
      currentId = id;
      playBtn.innerHTML = `<svg class="icon" viewBox="0 0 24 24" fill="currentColor"><path d="M6 5h4v14H6zM14 5h4v14h-4z"/></svg>`;
    } else {
      audio.pause();
      playBtn.innerHTML = `<svg class="icon" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>`;
    }
  });

  // stop button
  stopBtn.addEventListener("click", () => {
    audio.pause();
    audio.currentTime = 0;
    range.value = 0;
    current.textContent = "0:00";
    playBtn.innerHTML = `<svg class="icon" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"/></svg>`;
    if (currentId === id) currentId = null;
  });
});
</script>
</body>
</html>
