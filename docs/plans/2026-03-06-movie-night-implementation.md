# Movie Night Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a single-page casino-themed app with 3 mini-games (dice, slot machine, fortune wheel) that helps couples decide what movie to watch and what to eat.

**Architecture:** Single HTML file with embedded CSS and JS. No framework, no build step, no dependencies. Game state managed via a simple JS state machine that transitions between screens: landing → dice → slot → wheel → result. Each game is a `<section>` shown/hidden via CSS classes.

**Tech Stack:** HTML5, CSS3 (animations, 3D transforms, custom properties), vanilla JavaScript, Google Fonts CDN (Bungee Shade for display, Outfit for body).

**Design Reference:** See `docs/plans/2026-03-06-movie-night-design.md` for full design spec.

---

### Task 1: HTML Scaffold & CSS Foundation

**Files:**
- Create: `index.html`

**Step 1: Create base HTML with all sections and CSS variables**

Create `index.html` with:
- HTML boilerplate with meta viewport, lang="pt-BR", title "Movie Night 🎰"
- Google Fonts link: Bungee Shade (display) + Outfit (body)
- `<style>` block with CSS custom properties:
  ```css
  :root {
    --bg-dark: #0a0015;
    --bg-purple: #1a0030;
    --bg-blue: #0d1b2a;
    --neon-pink: #ff2d75;
    --neon-cyan: #00f0ff;
    --neon-yellow: #ffe14d;
    --gold: #ffd700;
    --gold-dark: #b8860b;
    --white: #f0f0f0;
    --font-display: 'Bungee Shade', cursive;
    --font-body: 'Outfit', sans-serif;
  }
  ```
- CSS reset (box-sizing, margin, padding)
- Body: full viewport, dark gradient background, overflow-x hidden
- 5 `<section>` elements with IDs: `#landing`, `#dice-game`, `#slot-game`, `#wheel-game`, `#result`
- Each section: full viewport height, centered flexbox, hidden by default
- `.active` class: display flex
- Only `#landing` starts with `.active`

**Step 2: Add neon text utility classes and background effects**

```css
.neon-text {
  text-shadow: 0 0 10px var(--neon-pink), 0 0 40px var(--neon-pink), 0 0 80px var(--neon-pink);
}
.neon-text-cyan {
  text-shadow: 0 0 10px var(--neon-cyan), 0 0 40px var(--neon-cyan), 0 0 80px var(--neon-cyan);
}
```

Add a subtle animated gradient background on body using CSS `@keyframes` that slowly shifts the gradient angle.

**Step 3: Verify in browser**

Open `index.html` in browser. Should see dark gradient background, empty page with no errors in console.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: scaffold HTML structure with CSS foundation and neon theme"
```

---

### Task 2: Landing Screen

**Files:**
- Modify: `index.html`

**Step 1: Build landing screen HTML**

Inside `#landing` section, add:
- Title: `<h1 class="neon-text">MOVIE NIGHT</h1>` with Bungee Shade font
- Subtitle: `<p>Chega de briga! Deixa a sorte decidir. 🎰</p>`
- Three preview icons in a row: 🎲 🎰 🐯 with labels "Mood", "Filme", "Comida"
- CTA button: `<button id="btn-start" class="neon-btn">COMEÇAR A NOITE</button>`

**Step 2: Style landing screen**

```css
#landing h1 {
  font-family: var(--font-display);
  font-size: clamp(2rem, 8vw, 4.5rem);
  color: var(--neon-pink);
  text-align: center;
  animation: pulse-glow 2s ease-in-out infinite alternate;
}

@keyframes pulse-glow {
  from { text-shadow: 0 0 10px var(--neon-pink), 0 0 40px var(--neon-pink); }
  to { text-shadow: 0 0 20px var(--neon-pink), 0 0 60px var(--neon-pink), 0 0 100px var(--neon-pink); }
}

.neon-btn {
  font-family: var(--font-body);
  font-size: 1.3rem;
  font-weight: 700;
  padding: 1rem 2.5rem;
  border: 2px solid var(--neon-cyan);
  background: transparent;
  color: var(--neon-cyan);
  cursor: pointer;
  text-transform: uppercase;
  letter-spacing: 2px;
  transition: all 0.3s;
  box-shadow: 0 0 15px rgba(0, 240, 255, 0.3), inset 0 0 15px rgba(0, 240, 255, 0.1);
}
.neon-btn:hover {
  background: rgba(0, 240, 255, 0.15);
  box-shadow: 0 0 30px rgba(0, 240, 255, 0.5), inset 0 0 30px rgba(0, 240, 255, 0.2);
  transform: scale(1.05);
}
```

**Step 3: Add preview icons row**

Style the three icons (🎲🎰🐯) in a horizontal flex row with neon labels underneath, subtle glow on each.

**Step 4: Verify in browser**

Open index.html. Should see: dark background, pulsing neon "MOVIE NIGHT" title, subtitle, three game icons, glowing cyan button.

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add landing screen with neon casino aesthetic"
```

---

### Task 3: Game State Machine (JavaScript)

**Files:**
- Modify: `index.html` (add `<script>` block)

**Step 1: Create game state machine**

At bottom of `index.html`, add `<script>` with:

```javascript
const screens = ['landing', 'dice-game', 'slot-game', 'wheel-game', 'result'];
let currentScreen = 0;
const gameState = { mood: null, movie: null, food: null };

function showScreen(id) {
  document.querySelectorAll('section').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

function nextScreen() {
  currentScreen++;
  if (currentScreen < screens.length) {
    showScreen(screens[currentScreen]);
  }
}

function resetGame() {
  currentScreen = 0;
  gameState.mood = null;
  gameState.movie = null;
  gameState.food = null;
  showScreen('landing');
}
```

**Step 2: Wire up landing button**

```javascript
document.getElementById('btn-start').addEventListener('click', () => {
  nextScreen(); // goes to dice-game
});
```

**Step 3: Add data arrays**

Add the `MOVIES` array (all 92 films from design doc), `FOODS` array (16 items with emoji+name), and `MOODS` array (6 moods with emoji+name).

```javascript
const MOODS = [
  { emoji: '💕', name: 'Romântico' },
  { emoji: '🔥', name: 'Adrenalina' },
  { emoji: '🥹', name: 'Nostalgia' },
  { emoji: '😂', name: 'Comédia' },
  { emoji: '😱', name: 'Suspense' },
  { emoji: '😭', name: 'Choradeira' }
];

const FOODS = [
  { emoji: '🍕', name: 'Pizza' },
  { emoji: '🍿', name: 'Pipoca' },
  // ... all 16
];

const MOVIES = [
  { title: 'O Senhor dos Anéis: A Sociedade do Anel', year: 2001 },
  { title: 'Star Wars – O Império Contra-Ataca', year: 1980 },
  // ... all 92
];
```

**Step 4: Verify**

Open in browser, click "COMEÇAR A NOITE" — screen should transition to the (empty) dice-game section. No console errors.

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add game state machine and data arrays"
```

---

### Task 4: Dice Game (3D CSS Dice)

**Files:**
- Modify: `index.html`

**Step 1: Build dice HTML structure**

Inside `#dice-game`:
- Title: `<h2 class="neon-text-cyan">QUAL O MOOD DA NOITE?</h2>`
- 3D dice container: a wrapper div with perspective
- Dice cube: 6 faces using CSS 3D transforms, each face shows a mood emoji
- Button: `<button id="btn-roll" class="neon-btn">JOGAR O DADO 🎲</button>`
- Result area: `<div id="dice-result"></div>` (hidden until roll completes)

```html
<div class="dice-scene">
  <div class="dice-cube" id="dice-cube">
    <div class="dice-face front">💕</div>
    <div class="dice-face back">🔥</div>
    <div class="dice-face right">🥹</div>
    <div class="dice-face left">😂</div>
    <div class="dice-face top">😱</div>
    <div class="dice-face bottom">😭</div>
  </div>
</div>
```

**Step 2: Style 3D dice with CSS**

```css
.dice-scene {
  width: 120px;
  height: 120px;
  perspective: 600px;
  margin: 2rem auto;
}
.dice-cube {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 2s cubic-bezier(0.2, 0.8, 0.3, 1);
}
.dice-face {
  position: absolute;
  width: 120px;
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3rem;
  background: rgba(26, 0, 48, 0.9);
  border: 2px solid var(--neon-cyan);
  border-radius: 12px;
  box-shadow: inset 0 0 20px rgba(0, 240, 255, 0.2);
}
.front  { transform: translateZ(60px); }
.back   { transform: rotateY(180deg) translateZ(60px); }
.right  { transform: rotateY(90deg) translateZ(60px); }
.left   { transform: rotateY(-90deg) translateZ(60px); }
.top    { transform: rotateX(90deg) translateZ(60px); }
.bottom { transform: rotateX(-90deg) translateZ(60px); }
```

**Step 3: Implement dice roll logic**

```javascript
document.getElementById('btn-roll').addEventListener('click', () => {
  const btn = document.getElementById('btn-roll');
  btn.disabled = true;

  const randomMood = Math.floor(Math.random() * 6);
  const cube = document.getElementById('dice-cube');

  // Map mood index to rotation that shows correct face
  const rotations = [
    'rotateX(0deg) rotateY(0deg)',       // front - 💕
    'rotateX(0deg) rotateY(180deg)',     // back - 🔥
    'rotateX(0deg) rotateY(-90deg)',     // right - 🥹
    'rotateX(0deg) rotateY(90deg)',      // left - 😂
    'rotateX(-90deg) rotateY(0deg)',     // top - 😱
    'rotateX(90deg) rotateY(0deg)',      // bottom - 😭
  ];

  // Add extra full rotations for dramatic spin
  const spins = `rotateX(${720 + Math.random() * 360}deg) rotateY(${720 + Math.random() * 360}deg)`;
  cube.style.transform = spins;

  // After spin, snap to correct face
  setTimeout(() => {
    cube.style.transition = 'transform 1s ease-out';
    cube.style.transform = rotations[randomMood];

    gameState.mood = MOODS[randomMood];

    setTimeout(() => {
      document.getElementById('dice-result').innerHTML =
        `<span class="result-reveal">${MOODS[randomMood].emoji} ${MOODS[randomMood].name}!</span>`;
      document.getElementById('dice-result').classList.add('show');

      setTimeout(() => nextScreen(), 2000);
    }, 1000);
  }, 2000);
});
```

**Step 4: Style result reveal animation**

```css
.result-reveal {
  font-family: var(--font-display);
  font-size: clamp(1.5rem, 5vw, 2.5rem);
  color: var(--neon-yellow);
  animation: reveal-pop 0.5s ease-out;
}
@keyframes reveal-pop {
  0% { transform: scale(0); opacity: 0; }
  70% { transform: scale(1.3); }
  100% { transform: scale(1); opacity: 1; }
}
#dice-result { min-height: 4rem; text-align: center; }
#dice-result.show { display: block; }
```

**Step 5: Verify**

Open in browser, navigate to dice screen, click "JOGAR O DADO". Dice should spin dramatically in 3D, land on a face, reveal the mood with animation, then auto-transition to slot screen after 2s.

**Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add 3D dice game for mood selection"
```

---

### Task 5: Slot Machine Game

**Files:**
- Modify: `index.html`

**Step 1: Build slot machine HTML**

Inside `#slot-game`:
- Title: `<h2 class="neon-text">QUAL FILME DA NOITE?</h2>`
- Mood indicator: small badge showing current mood
- Slot machine frame: outer border styled like a real slot machine
- 3 reel containers, each with a visible window (shows 1 item) and a tall strip of movie titles that scrolls
- Lever/button: `<button id="btn-spin" class="neon-btn">PUXAR A ALAVANCA 🎰</button>`

```html
<div class="slot-machine">
  <div class="slot-frame">
    <div class="slot-reels">
      <div class="reel-window">
        <div class="reel-strip" id="reel-1"></div>
      </div>
      <div class="reel-window">
        <div class="reel-strip" id="reel-2"></div>
      </div>
      <div class="reel-window">
        <div class="reel-strip" id="reel-3"></div>
      </div>
    </div>
  </div>
</div>
```

**Step 2: Style slot machine**

```css
.slot-machine {
  margin: 1.5rem auto;
  max-width: 380px;
}
.slot-frame {
  background: linear-gradient(145deg, #2a0045, #15002a);
  border: 3px solid var(--gold);
  border-radius: 16px;
  padding: 1.5rem;
  box-shadow: 0 0 30px rgba(255, 215, 0, 0.3), inset 0 0 30px rgba(0, 0, 0, 0.5);
}
.slot-reels {
  display: flex;
  gap: 8px;
  justify-content: center;
}
.reel-window {
  width: 110px;
  height: 80px;
  overflow: hidden;
  background: #0a0015;
  border: 2px solid var(--gold-dark);
  border-radius: 8px;
  position: relative;
}
.reel-strip {
  display: flex;
  flex-direction: column;
  transition: transform 0.5s cubic-bezier(0.1, 0.7, 0.3, 1);
}
.reel-item {
  width: 110px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4px;
  text-align: center;
  font-family: var(--font-body);
  font-size: 0.7rem;
  font-weight: 600;
  color: var(--white);
  line-height: 1.2;
}
```

**Step 3: Implement reel population and spin logic**

```javascript
function shuffleArray(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

function populateReels() {
  for (let i = 1; i <= 3; i++) {
    const strip = document.getElementById(`reel-${i}`);
    strip.innerHTML = '';
    const shuffled = shuffleArray(MOVIES);
    shuffled.forEach(movie => {
      const item = document.createElement('div');
      item.className = 'reel-item';
      item.textContent = `🎬 ${movie.title} (${movie.year})`;
      strip.appendChild(item);
    });
  }
}

document.getElementById('btn-spin').addEventListener('click', () => {
  const btn = document.getElementById('btn-spin');
  btn.disabled = true;
  populateReels();

  const selectedIndex = Math.floor(Math.random() * MOVIES.length);
  const selectedMovie = MOVIES[selectedIndex];

  // Spin each reel with increasing delay
  for (let i = 1; i <= 3; i++) {
    const strip = document.getElementById(`reel-${i}`);
    const randomStop = Math.floor(Math.random() * (MOVIES.length - 5)) + 3;
    const offset = -(randomStop * 80); // 80px per item

    setTimeout(() => {
      strip.style.transition = `transform ${1.5 + i * 0.5}s cubic-bezier(0.1, 0.7, 0.3, 1)`;
      strip.style.transform = `translateY(${offset}px)`;
    }, i * 300);
  }

  // After all reels stop, set the chosen movie
  setTimeout(() => {
    gameState.movie = selectedMovie;
    document.getElementById('slot-result').innerHTML =
      `<span class="result-reveal">🎬 ${selectedMovie.title} (${selectedMovie.year})</span>`;
    document.getElementById('slot-result').classList.add('show');

    setTimeout(() => nextScreen(), 2500);
  }, 4000);
});
```

**Step 4: Add slot result area and lever animation**

Add `<div id="slot-result"></div>` below the slot machine. Add a lever visual element on the right side of the slot frame using CSS pseudo-elements (optional decorative touch).

**Step 5: Verify**

Open in browser, navigate to slot screen. Click spin. Three reels should spin at different speeds, stop one by one left-to-right, reveal the movie title, then auto-transition.

**Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add slot machine game for movie selection"
```

---

### Task 6: Fortune Wheel Game (Tigrinho Style)

**Files:**
- Modify: `index.html`

**Step 1: Build fortune wheel HTML**

Inside `#wheel-game`:
- Title: `<h2 style="color: var(--gold);" class="neon-text">O QUE COMER? 🐯</h2>`
- Mood + movie badges showing current selections
- Wheel container with:
  - Outer ring of decorative "lights" (small circles around the wheel)
  - SVG/Canvas wheel divided into 16 colored segments with food names + emojis
  - Fixed pointer/arrow at the top
  - Golden ornate border
- Button: `<button id="btn-wheel" class="neon-btn gold-btn">GIRAR A RODA 🐯</button>`

```html
<div class="wheel-container">
  <div class="wheel-lights" id="wheel-lights"></div>
  <div class="wheel-pointer">▼</div>
  <canvas id="wheel-canvas" width="320" height="320"></canvas>
</div>
```

**Step 2: Draw the wheel with Canvas**

Use JavaScript Canvas to draw 16 colored segments. Each segment has:
- Alternating vibrant colors: red, gold, green, orange, pink, cyan, purple, lime (repeated)
- Food emoji + name drawn along the radius
- Golden border lines between segments

```javascript
const WHEEL_COLORS = [
  '#e74c3c', '#f1c40f', '#2ecc71', '#e67e22',
  '#ff2d75', '#00f0ff', '#9b59b6', '#27ae60',
  '#e74c3c', '#f1c40f', '#2ecc71', '#e67e22',
  '#ff2d75', '#00f0ff', '#9b59b6', '#27ae60'
];

function drawWheel(rotation = 0) {
  const canvas = document.getElementById('wheel-canvas');
  const ctx = canvas.getContext('2d');
  const cx = canvas.width / 2;
  const cy = canvas.height / 2;
  const r = 150;
  const segAngle = (2 * Math.PI) / FOODS.length;

  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.save();
  ctx.translate(cx, cy);
  ctx.rotate(rotation);

  FOODS.forEach((food, i) => {
    const startAngle = i * segAngle;
    const endAngle = startAngle + segAngle;

    // Draw segment
    ctx.beginPath();
    ctx.moveTo(0, 0);
    ctx.arc(0, 0, r, startAngle, endAngle);
    ctx.closePath();
    ctx.fillStyle = WHEEL_COLORS[i];
    ctx.fill();
    ctx.strokeStyle = var(--gold);
    ctx.lineWidth = 2;
    ctx.stroke();

    // Draw text
    ctx.save();
    ctx.rotate(startAngle + segAngle / 2);
    ctx.textAlign = 'right';
    ctx.fillStyle = '#fff';
    ctx.font = 'bold 11px Outfit';
    ctx.fillText(`${food.emoji} ${food.name}`, r - 10, 4);
    ctx.restore();
  });

  ctx.restore();

  // Draw center circle
  ctx.beginPath();
  ctx.arc(cx, cy, 20, 0, 2 * Math.PI);
  ctx.fillStyle = var(--gold);
  ctx.fill();
  ctx.strokeStyle = var(--gold-dark);
  ctx.lineWidth = 3;
  ctx.stroke();
}
```

**Step 3: Create decorative lights around the wheel**

Generate 24 small circles positioned around the wheel using JS. Animate them with alternating on/off states using CSS `@keyframes`.

```css
.wheel-lights {
  position: absolute;
  width: 340px;
  height: 340px;
  border-radius: 50%;
}
.wheel-light {
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--gold);
  box-shadow: 0 0 8px var(--gold);
  animation: blink-light 0.5s ease-in-out infinite alternate;
}
.wheel-light:nth-child(even) { animation-delay: 0.25s; }
@keyframes blink-light {
  from { opacity: 0.3; }
  to { opacity: 1; }
}
```

**Step 4: Implement wheel spin logic**

```javascript
let wheelAngle = 0;
let isSpinning = false;

document.getElementById('btn-wheel').addEventListener('click', () => {
  if (isSpinning) return;
  isSpinning = true;
  document.getElementById('btn-wheel').disabled = true;

  const selectedIndex = Math.floor(Math.random() * FOODS.length);
  const segAngle = 360 / FOODS.length;

  // Calculate target angle: multiple full rotations + position to land on selected food
  // The pointer is at the top (270 degrees in canvas coordinates)
  const targetSegCenter = selectedIndex * segAngle + segAngle / 2;
  const targetAngle = 360 * 6 + (360 - targetSegCenter); // 6 full spins + offset

  const startTime = Date.now();
  const duration = 5000; // 5 second spin

  function animate() {
    const elapsed = Date.now() - startTime;
    const progress = Math.min(elapsed / duration, 1);
    // Ease out cubic for dramatic deceleration
    const eased = 1 - Math.pow(1 - progress, 3);
    const currentAngle = eased * targetAngle;

    drawWheel((currentAngle * Math.PI) / 180);

    if (progress < 1) {
      requestAnimationFrame(animate);
    } else {
      isSpinning = false;
      gameState.food = FOODS[selectedIndex];
      document.getElementById('wheel-result').innerHTML =
        `<span class="result-reveal">${FOODS[selectedIndex].emoji} ${FOODS[selectedIndex].name}!</span>`;
      document.getElementById('wheel-result').classList.add('show');

      setTimeout(() => nextScreen(), 2000);
    }
  }

  animate();
});
```

**Step 5: Style the pointer and golden frame**

```css
.wheel-container {
  position: relative;
  width: 340px;
  height: 340px;
  margin: 1rem auto;
}
.wheel-pointer {
  position: absolute;
  top: -15px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 2rem;
  color: var(--gold);
  filter: drop-shadow(0 0 10px var(--gold));
  z-index: 10;
}
#wheel-canvas {
  border-radius: 50%;
  border: 6px solid var(--gold);
  box-shadow: 0 0 30px rgba(255, 215, 0, 0.4), 0 0 60px rgba(255, 215, 0, 0.2);
}
```

**Step 6: Verify**

Open in browser, navigate to wheel screen. Click spin. Wheel should spin 6 full rotations then decelerate dramatically to a stop. Lights blink around the wheel. Food result appears, auto-transitions.

**Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add fortune wheel (Tigrinho style) for food selection"
```

---

### Task 7: Result Screen

**Files:**
- Modify: `index.html`

**Step 1: Build result screen HTML**

Inside `#result`:
- Celebration title: `<h2>SUA NOITE ESTÁ PRONTA!</h2>` with animated neon
- Three result cards in a column:
  - Mood card: emoji + mood name
  - Movie card: film title + year with movie emoji
  - Food card: food emoji + name
- Each card has distinct border color (cyan, pink, gold)
- Button: `<button id="btn-retry" class="neon-btn">NÃO CURTI, GIRAR TUDO DE NOVO! 🔄</button>`

**Step 2: Style result cards**

```css
.result-card {
  background: rgba(26, 0, 48, 0.8);
  border-radius: 16px;
  padding: 1.2rem 2rem;
  margin: 0.6rem 0;
  text-align: center;
  width: 90%;
  max-width: 360px;
  backdrop-filter: blur(10px);
  animation: card-slide-in 0.5s ease-out both;
}
.result-card:nth-child(1) { border: 2px solid var(--neon-cyan); animation-delay: 0.2s; }
.result-card:nth-child(2) { border: 2px solid var(--neon-pink); animation-delay: 0.5s; }
.result-card:nth-child(3) { border: 2px solid var(--gold); animation-delay: 0.8s; }

@keyframes card-slide-in {
  from { transform: translateY(30px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
```

**Step 3: Populate result screen with gameState data**

```javascript
function showResult() {
  showScreen('result');
  const container = document.getElementById('result-cards');
  container.innerHTML = `
    <div class="result-card">
      <div class="result-label">MOOD DA NOITE</div>
      <div class="result-value">${gameState.mood.emoji} ${gameState.mood.name}</div>
    </div>
    <div class="result-card">
      <div class="result-label">FILME</div>
      <div class="result-value">🎬 ${gameState.movie.title}</div>
      <div class="result-year">(${gameState.movie.year})</div>
    </div>
    <div class="result-card">
      <div class="result-label">COMIDA</div>
      <div class="result-value">${gameState.food.emoji} ${gameState.food.name}</div>
    </div>
  `;
}
```

**Step 4: Wire retry button**

```javascript
document.getElementById('btn-retry').addEventListener('click', resetGame);
```

Update `nextScreen()` so when it reaches the result screen, it calls `showResult()` instead of just `showScreen()`.

**Step 5: Add confetti/sparkle effect on result screen**

Create a simple CSS particle effect: 20-30 small colored dots/stars that float upward with random positions, using CSS `@keyframes` only (no library).

**Step 6: Verify full flow**

Test the complete flow: Landing → Dice → Slot → Wheel → Result → Retry → Landing. All transitions should work smoothly, result screen should display correct data.

**Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add result screen with celebration effects"
```

---

### Task 8: Polish, Responsiveness & Sound Effects

**Files:**
- Modify: `index.html`

**Step 1: Add screen transition animations**

```css
section {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.4s, transform 0.4s;
}
section.active {
  opacity: 1;
  transform: translateY(0);
}
```

**Step 2: Mobile responsiveness**

- Ensure all sections use `min-height: 100dvh` (dynamic viewport height for mobile)
- Slot machine scales down on screens < 400px
- Wheel canvas scales with `max-width: 90vw`
- Font sizes use `clamp()` throughout
- Test at 375px width (iPhone SE)

**Step 3: Add progress indicator**

Add a small progress bar or step indicator at the top of each game screen showing "Passo 1/3", "Passo 2/3", "Passo 3/3".

```html
<div class="progress-bar">
  <div class="progress-step active">🎲</div>
  <div class="progress-step">🎰</div>
  <div class="progress-step">🐯</div>
</div>
```

**Step 4: Add hover/active states for all interactive elements**

Ensure buttons have `:active` states (scale down slightly) for mobile tap feedback.

**Step 5: Add a favicon**

Use an emoji favicon via SVG data URI in the `<head>`:
```html
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🎰</text></svg>">
```

**Step 6: Final verify**

Test complete flow on desktop and mobile viewport. Check: no horizontal scroll, all text readable, buttons tappable, animations smooth, no console errors.

**Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add polish, responsiveness, and progress indicator"
```

---

### Task 9: Deploy to GitHub Pages

**Files:**
- No new files

**Step 1: Push to GitHub**

```bash
git push origin main
```

**Step 2: Enable GitHub Pages**

```bash
gh api repos/ohcarioca/movie-night/pages -X POST -f source.branch=main -f source.path=/
```

**Step 3: Verify deployment**

Wait ~60s then check: `https://ohcarioca.github.io/movie-night/`

**Step 4: Commit any final fixes**

If anything needs fixing after deployment, fix and push.
