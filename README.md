<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" /> 
  <title>🎉 Happy Birthday</title>
  <meta name="description" content="A special birthday surprise for my boyfriend – apology and celebration page" />
  <style>
    :root{
      --bg:#0f1220;
      --panel:#14182b;
      --card:#1b2140;
      --text:#f8f9ff;
      --muted:#c9cbe3;
      --accent:#7c5cff;
      --accent-2:#00e5ff;
      --good:#22c55e;
      --shadow: 0 20px 40px rgba(0,0,0,.35);
      --radius: 22px;
    }
    html,body{height:100%}
    body{
      margin:0; background: radial-gradient(1200px 700px at 80% -10%, rgba(124,92,255,.25), transparent),
                            radial-gradient(1000px 600px at -10% 110%, rgba(0,229,255,.15), transparent),
                            var(--bg);
      color:var(--text); font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
    }
    .container{max-width:1100px; margin:0 auto; padding:24px}
    header{
      position:relative; overflow:hidden; border-radius: calc(var(--radius) + 8px);
      background: linear-gradient(135deg, rgba(124,92,255,.18), rgba(0,229,255,.18));
      border:1px solid rgba(255,255,255,.08);
      box-shadow: var(--shadow);
      margin:24px; padding:48px 24px;
      text-align:center;
    }
    header .title{font-size: clamp(28px, 6vw, 48px); font-weight:800; margin:0 0 8px}
    header .subtitle{color:var(--muted); font-size: clamp(14px, 3.2vw, 18px); margin:0}
    .btn{
      background: linear-gradient(135deg, var(--accent), var(--accent-2));
      border:none; padding:12px 20px; border-radius:999px; color:#081018; font-weight:700; cursor:pointer;
    }
    .countdown{ display:grid; grid-template-columns: repeat(4, 1fr); gap:14px; margin-top:26px }
    .box{ background:rgba(255,255,255,.05); border-radius:var(--radius); padding:16px; text-align:center }
    .num{ font-size: clamp(24px, 5vw, 42px); font-weight:800 }
    .label{ color:var(--muted); font-size:12px }
    .panel{ background:rgba(255,255,255,.05); border-radius:var(--radius); padding:24px; margin:24px }
    .panel h2{ margin-top:0; font-size: clamp(22px, 4vw, 28px) }
    .panel p{ color:var(--muted); line-height:1.9; font-size:16px }
    .grid{ display:grid; grid-template-columns: repeat( auto-fit, minmax(220px, 1fr)); gap:16px }
    .card{ background: var(--card); border-radius:var(--radius); padding:18px }
    footer{ text-align:center; color:var(--muted); padding:42px 0 }

    /* Floating heart styles */
    .heart {
      position: fixed;
      pointer-events: none;
      user-select: none;
      z-index: 9999;
      transition: all 2s ease-out;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1 class="title">🎂 Happy Birthday, <span id="name">My Love</span>!</h1>
      <p class="subtitle">A small apology… but huge in my heart 💜</p>

      <!-- Play Music Button -->
      <button class="btn" id="playBtn">▶ Play Music</button>
      <audio id="birthdayAudio">
        <source src="Human Music - instrumental demo - Talk To Trees (1).mp4" type="video/mp4">
      </audio>

      <div class="countdown" id="countdown">
        <div class="box"><div class="num" id="days">--</div><div class="label">Days</div></div>
        <div class="box"><div class="num" id="hours">--</div><div class="label">Hours</div></div>
        <div class="box"><div class="num" id="mins">--</div><div class="label">Minutes</div></div>
        <div class="box"><div class="num" id="secs">--</div><div class="label">Seconds</div></div>
      </div>
    </header>

    <section class="panel">
      <h2>My Message to You</h2>
      <p>
        Maybe my words were misunderstood when I said I didn’t want to waste your time. The truth is, I appreciate your hard work and your precious time, and I never meant to upset you. I just love talking to you when you’re free and happy, and I’m always wishing you success in your work. Sorry if there was any confusion, you are very important to me ❤️
      </p>
    </section>

    <section class="panel">
      <h2>3 Things I Love About You</h2>
      <div class="grid">
        <article class="card">
          <h3>Your Dedication</h3>
          <p>You work 10 hours a day and still care enough to talk, which is truly admirable.</p>
        </article>
        <article class="card">
          <h3>Your Kind Heart</h3>
          <p>Your calm reply: "You’re not wasting my time" – unforgettable.</p>
        </article>
        <article class="card">
          <h3>Your Humor</h3>
          <p>You make conversations light and fun even after a long day, and that’s rare.</p>
        </article>
      </div>
    </section>

    <section class="panel">
      <h2>Memories & Photos</h2>
      <div class="grid">
        <img src="https://images.unsplash.com/photo-1529336953121-ad3c0f42f0ca?q=80&w=1200&auto=format&fit=crop" style="width:100%; border-radius:var(--radius)"/>
        <img src="https://images.unsplash.com/photo-1512003867696-6d5644166b18?q=80&w=1200&auto=format&fit=crop" style="width:100%; border-radius:var(--radius)"/>
        <img src="https://images.unsplash.com/photo-1478147427282-58a87a120781?q=80&w=1200&auto=format&fit=crop" style="width:100%; border-radius:var(--radius)"/>
      </div>
    </section>

    <footer>
      Made with love 💜 — <span id="year"></span>
    </footer>
  </div>

  <!-- Floating Hearts Button -->
  <button id="heartsBtn" class="btn" style="position:fixed;bottom:30px;right:30px;padding:10px 14px;font-size:16px;">❤️</button>

  <script>
    const RECEIVER_NAME = "My Love";
    const BIRTHDAY_DATE = new Date("2025-08-18T00:00:00");
    document.getElementById('name').textContent = RECEIVER_NAME;
    document.getElementById('year').textContent = new Date().getFullYear();

    // Countdown
    const dEl = document.getElementById('days');
    const hEl = document.getElementById('hours');
    const mEl = document.getElementById('mins');
    const sEl = document.getElementById('secs');

    function updateCountdown(){
      const now = new Date();
      const diff = BIRTHDAY_DATE - now;
      if(diff <= 0){
        dEl.textContent = '0'; hEl.textContent = '0'; mEl.textContent = '0'; sEl.textContent = '0';
        return;
      }
      const days = Math.floor(diff / (1000*60*60*24));
      const hours = Math.floor((diff / (1000*60*60)) % 24);
      const mins = Math.floor((diff / (1000*60)) % 60);
      const secs = Math.floor((diff / 1000) % 60);
      dEl.textContent = days; hEl.textContent = hours; mEl.textContent = mins; sEl.textContent = secs;
    }
    updateCountdown();
    setInterval(updateCountdown, 1000);

    // Music Play Button
    const audio = document.getElementById('birthdayAudio');
    const playBtn = document.getElementById('playBtn');

    playBtn.addEventListener('click', () => {
      audio.play();
      playBtn.style.display = 'none'; // hide button after click
    });

    // Hearts Button
    const heartsBtn = document.getElementById('heartsBtn');

    heartsBtn.addEventListener('click', () => {
      const heart = document.createElement('div');
      heart.textContent = '❤️';
      heart.className = 'heart';
      heart.style.left = Math.random() * window.innerWidth + 'px';
      heart.style.top = window.innerHeight + 'px';
      heart.style.fontSize = Math.random() * 24 + 24 + 'px';
      document.body.appendChild(heart);

      setTimeout(() => {
        heart.style.top = (Math.random() * window.innerHeight/2) + 'px';
        heart.style.opacity = '0';
      }, 50);

      setTimeout(() => heart.remove(), 2000);
    });
  </script>
</body>
</html>
