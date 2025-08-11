<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Gift Audio — My Audio Library</title>
<style>
:root{
  --bg1: #fff7f2;
  --bg2: #fff0ff;
  --card: #ffffff;
  --accent: #ff6f91;
  --accent-2: #6f9bff;
  --muted: #6b6b6b;
}

*{box-sizing:border-box}
body{
  margin:0;
  font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
  background: linear-gradient(135deg,var(--bg1),var(--bg2));
  color:#222;
  min-height:100vh;
  display:flex;
  align-items:center;
  justify-content:center;
  padding:30px;
}

.page{width:100%;max-width:760px}

.header{ text-align:center; margin-bottom:20px;}
.header h1{margin:0; font-size:26px;}
.header .sub{color:var(--muted); margin-top:6px}

.audio-card{
  background:var(--card);
  border-radius:14px;
  padding:18px;
  box-shadow:0 10px 30px rgba(20,20,45,0.08);
  margin-bottom:18px;
}

.card-head{display:flex;gap:12px;align-items:center;margin-bottom:12px}
.ribbon{font-size:28px; background:linear-gradient(90deg,var(--accent),var(--accent-2));
  width:56px;height:56px;border-radius:10px;display:flex;align-items:center;justify-content:center;color:white;}
.title{margin:0;font-size:18px}
.desc{margin:2px 0 0;color:var(--muted);font-size:13px}

.controls{display:flex;gap:10px;margin:10px 0}
.btn{
  border:0; padding:10px 14px; font-size:15px; border-radius:10px; cursor:pointer;
}
.btn.play{ background: linear-gradient(90deg,var(--accent), #ff8fb3); color:white }
.btn.stop{ background: linear-gradient(90deg,#ff9a9e,#fecfef); color:#7d1b2f }

.progress-wrap{
  width:100%; height:8px; background:rgba(0,0,0,0.06); border-radius:999px; overflow:hidden; margin-top:8px;
}
.progress{ height:100%; width:0%; border-radius:999px; background:linear-gradient(90deg,var(--accent),var(--accent-2)); transition:width 0.05s linear;}

.times{display:flex;justify-content:center;align-items:center;gap:8px;color:var(--muted);font-size:13px;margin-top:8px}

.footer{ text-align:center;color:var(--muted);font-size:13px;margin-top:10px}
</style>
</head>
<body>
<main class="page">
  <header class="header">
    <h1>🎁 A Little Gift — Audio Collection</h1>
    <p class="sub">Click play to listen — made with love 💖</p>
  </header>

  <!-- Audio Card -->
  <section class="audio-card">
    <div class="card-head">
      <div class="ribbon">🎀</div>
      <div>
        <h2 class="title">Audio 1 — The Gift</h2>
        <p class="desc">Special audio just for you.</p>
      </div>
    </div>

    <div class="controls">
      <button class="btn play" data-audio="audio1">▶ Play</button>
      <button class="btn stop" data-audio="audio1">⏹ Stop</button>
    </div>

    <div class="progress-wrap">
      <div class="progress" id="progress-audio1"></div>
    </div>

    <div class="times">
      <span id="current-audio1">0:00</span>
      <span>/</span>
      <span id="duration-audio1">0:00</span>
    </div>

    <audio id="audio1" preload="metadata" src="https://raw.githubusercontent.com/EstherKamal/EstherKamal.github.io/main/audiofile.mp3"></audio>
  </section>

  <footer class="footer">More audios? Copy this card and change the ID + link!</footer>
</main>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const audios = Array.from(document.querySelectorAll('audio'));
  const playBtns = document.querySelectorAll('.btn.play');
  const stopBtns = document.querySelectorAll('.btn.stop');

  function stopAllExcept(exceptAudio) {
    audios.forEach(a => {
      if (a !== exceptAudio) {
        a.pause();
        a.currentTime = 0;
        updateUI(a);
      }
    });
  }

  playBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      const audio = document.getElementById(btn.dataset.audio);
      if (audio.paused) {
        stopAllExcept(audio);
        audio.play();
        btn.textContent = '⏸ Pause';
      } else {
        audio.pause();
        btn.textContent = '▶ Play';
      }
    });
  });

  stopBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      const audio = document.getElementById(btn.dataset.audio);
      audio.pause();
      audio.currentTime = 0;
      document.querySelector(`.btn.play[data-audio="${btn.dataset.audio}"]`).textContent = '▶ Play';
      updateUI(audio);
    });
  });

  audios.forEach(audio => {
    audio.addEventListener('timeupdate', () => updateUI(audio));
    audio.addEventListener('loadedmetadata', () => updateUI(audio));
    audio.addEventListener('ended', () => {
      document.querySelector(`.btn.play[data-audio="${audio.id}"]`).textContent = '▶ Play';
      audio.currentTime = 0;
      updateUI(audio);
    });
  });

  function updateUI(audio) {
    const prog = document.getElementById(`progress-${audio.id}`);
    const curr = document.getElementById(`current-${audio.id}`);
    const dur  = document.getElementById(`duration-${audio.id}`);
    const pct = audio.duration ? (audio.currentTime / audio.duration) * 100 : 0;
    if (prog) prog.style.width = pct + '%';
    if (curr) curr.textContent = formatTime(audio.currentTime);
    if (dur)  dur.textContent  = isFinite(audio.duration) ? formatTime(audio.duration) : '0:00';
  }

  function formatTime(s) {
    if (!isFinite(s) || s < 0) return '0:00';
    const m = Math.floor(s / 60);
    const sec = Math.floor(s % 60).toString().padStart(2, '0');
    return `${m}:${sec}`;
  }
});
</script>
</body>
</html>
