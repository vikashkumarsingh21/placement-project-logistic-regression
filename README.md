<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Placement Prediction — Logistic Regression</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #050a14;
    --surface: #0c1524;
    --surface2: #111d30;
    --border: #1e3a5f;
    --accent: #00d4ff;
    --accent2: #7c3aed;
    --accent3: #10b981;
    --text: #e2f0ff;
    --muted: #6b8cad;
    --code-bg: #091420;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    line-height: 1.7;
    overflow-x: hidden;
  }

  /* ── Animated grid background ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,212,255,.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,212,255,.04) 1px, transparent 1px);
    background-size: 40px 40px;
    animation: gridPan 20s linear infinite;
    pointer-events: none;
    z-index: 0;
  }

  @keyframes gridPan {
    0%   { background-position: 0 0; }
    100% { background-position: 40px 40px; }
  }

  /* ── Glowing orbs ── */
  .orb {
    position: fixed;
    border-radius: 50%;
    filter: blur(100px);
    opacity: .18;
    pointer-events: none;
    z-index: 0;
    animation: orbFloat 12s ease-in-out infinite;
  }
  .orb1 { width: 500px; height: 500px; background: var(--accent2); top: -150px; right: -100px; animation-delay: 0s; }
  .orb2 { width: 400px; height: 400px; background: var(--accent); bottom: 100px; left: -100px; animation-delay: -6s; }

  @keyframes orbFloat {
    0%, 100% { transform: translateY(0) scale(1); }
    50%       { transform: translateY(-40px) scale(1.1); }
  }

  /* ── Layout ── */
  .container {
    position: relative;
    z-index: 1;
    max-width: 900px;
    margin: 0 auto;
    padding: 0 24px 80px;
  }

  /* ── Hero ── */
  .hero {
    text-align: center;
    padding: 90px 0 60px;
    opacity: 0;
    animation: fadeUp .9s .1s forwards;
  }

  .hero-icon {
    width: 90px;
    height: 90px;
    margin: 0 auto 24px;
    position: relative;
  }

  .hero-icon svg { width: 100%; height: 100%; }

  .brain-ring {
    position: absolute;
    inset: -10px;
    border-radius: 50%;
    border: 2px solid transparent;
    border-top-color: var(--accent);
    border-right-color: var(--accent2);
    animation: spin 3s linear infinite;
  }

  @keyframes spin { to { transform: rotate(360deg); } }

  .hero h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(2rem, 5vw, 3.2rem);
    font-weight: 800;
    letter-spacing: -1px;
    background: linear-gradient(135deg, var(--accent) 0%, #a78bfa 50%, var(--accent3) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1.2;
    margin-bottom: 16px;
  }

  .hero-sub {
    color: var(--muted);
    font-size: 1.05rem;
    max-width: 560px;
    margin: 0 auto 32px;
  }

  /* ── Badges ── */
  .badges {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
    margin-bottom: 8px;
  }

  .badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 5px 14px;
    border-radius: 100px;
    font-size: .78rem;
    font-weight: 700;
    font-family: 'Space Mono', monospace;
    letter-spacing: .04em;
    border: 1px solid;
    animation: badgePop .4s both;
  }

  .badge:nth-child(1) { animation-delay: .5s; }
  .badge:nth-child(2) { animation-delay: .6s; }
  .badge:nth-child(3) { animation-delay: .7s; }
  .badge:nth-child(4) { animation-delay: .8s; }

  @keyframes badgePop {
    from { opacity: 0; transform: scale(.7); }
    to   { opacity: 1; transform: scale(1); }
  }

  .badge-blue  { color: var(--accent);  border-color: rgba(0,212,255,.35);  background: rgba(0,212,255,.07); }
  .badge-purple{ color: #a78bfa;        border-color: rgba(167,139,250,.35); background: rgba(167,139,250,.07); }
  .badge-green { color: var(--accent3); border-color: rgba(16,185,129,.35);  background: rgba(16,185,129,.07); }
  .badge-orange{ color: #fb923c;        border-color: rgba(251,146,60,.35);  background: rgba(251,146,60,.07); }

  /* ── Divider ── */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--border), transparent);
    margin: 48px 0;
    opacity: 0;
    animation: fadeIn .6s .8s forwards;
  }

  /* ── Section ── */
  .section {
    opacity: 0;
    transform: translateY(28px);
    animation: fadeUp .7s forwards;
  }

  .section:nth-child(1)  { animation-delay: .3s; }
  .section:nth-child(2)  { animation-delay: .45s; }
  .section:nth-child(3)  { animation-delay: .6s; }
  .section:nth-child(4)  { animation-delay: .75s; }
  .section:nth-child(5)  { animation-delay: .9s; }
  .section:nth-child(6)  { animation-delay: 1.05s; }
  .section:nth-child(7)  { animation-delay: 1.2s; }
  .section:nth-child(8)  { animation-delay: 1.35s; }
  .section:nth-child(9)  { animation-delay: 1.5s; }
  .section:nth-child(10) { animation-delay: 1.65s; }

  @keyframes fadeUp {
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes fadeIn {
    to { opacity: 1; }
  }

  .section-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 20px;
  }

  .section-icon {
    width: 38px;
    height: 38px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.1rem;
    flex-shrink: 0;
  }

  .icon-blue   { background: rgba(0,212,255,.12);   border: 1px solid rgba(0,212,255,.25); }
  .icon-purple { background: rgba(167,139,250,.12); border: 1px solid rgba(167,139,250,.25); }
  .icon-green  { background: rgba(16,185,129,.12);  border: 1px solid rgba(16,185,129,.25); }
  .icon-orange { background: rgba(251,146,60,.12);  border: 1px solid rgba(251,146,60,.25); }
  .icon-pink   { background: rgba(236,72,153,.12);  border: 1px solid rgba(236,72,153,.25); }

  .section-header h2 {
    font-size: 1.3rem;
    font-weight: 700;
    color: var(--text);
  }

  /* ── Card ── */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 28px;
    position: relative;
    overflow: hidden;
    transition: border-color .3s, transform .3s;
  }

  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2), var(--accent3));
    opacity: 0;
    transition: opacity .3s;
  }

  .card:hover { border-color: rgba(0,212,255,.35); transform: translateY(-2px); }
  .card:hover::before { opacity: 1; }

  .card p { color: #a8c4de; font-size: .97rem; }

  /* ── Table ── */
  .table-wrap { overflow-x: auto; border-radius: 12px; border: 1px solid var(--border); }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: .92rem;
    font-family: 'Space Mono', monospace;
  }

  thead { background: var(--surface2); }

  th {
    padding: 14px 18px;
    text-align: left;
    color: var(--accent);
    font-weight: 700;
    font-size: .8rem;
    letter-spacing: .08em;
    text-transform: uppercase;
    border-bottom: 1px solid var(--border);
  }

  td {
    padding: 13px 18px;
    color: #a8c4de;
    border-bottom: 1px solid rgba(30,58,95,.5);
  }

  tr:last-child td { border-bottom: none; }

  tr:hover td { background: rgba(0,212,255,.04); }

  td:first-child { color: var(--text); font-weight: 600; }

  /* ── Tech pills ── */
  .tech-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }

  .tech-pill {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 9px 16px;
    border-radius: 10px;
    background: var(--surface2);
    border: 1px solid var(--border);
    font-size: .87rem;
    font-weight: 600;
    color: var(--text);
    transition: all .25s;
    cursor: default;
  }

  .tech-pill:hover {
    background: var(--surface);
    border-color: var(--accent);
    color: var(--accent);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(0,212,255,.12);
  }

  .tech-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    flex-shrink: 0;
  }

  /* ── Workflow steps ── */
  .steps { display: flex; flex-direction: column; gap: 0; }

  .step {
    display: flex;
    gap: 18px;
    position: relative;
    padding-bottom: 24px;
  }

  .step:last-child { padding-bottom: 0; }

  .step-left {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex-shrink: 0;
  }

  .step-num {
    width: 36px; height: 36px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--accent2), var(--accent));
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 800;
    font-size: .8rem;
    font-family: 'Space Mono', monospace;
    color: #fff;
    flex-shrink: 0;
    box-shadow: 0 0 16px rgba(0,212,255,.3);
    animation: stepPulse 2s ease-in-out infinite;
  }

  @keyframes stepPulse {
    0%, 100% { box-shadow: 0 0 16px rgba(0,212,255,.3); }
    50%       { box-shadow: 0 0 24px rgba(0,212,255,.55); }
  }

  .step-line {
    width: 2px;
    flex: 1;
    background: linear-gradient(to bottom, var(--accent2), transparent);
    margin-top: 6px;
    min-height: 20px;
  }

  .step-content {
    padding-top: 6px;
    flex: 1;
  }

  .step-content strong {
    color: var(--text);
    font-weight: 700;
    font-size: .96rem;
  }

  /* ── File tree ── */
  .tree {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 22px 26px;
    font-family: 'Space Mono', monospace;
    font-size: .85rem;
    line-height: 2;
  }

  .tree-root { color: var(--accent); font-weight: 700; margin-bottom: 4px; }
  .tree-branch { color: var(--muted); }
  .tree-file { color: var(--text); }
  .tree-file.highlight { color: var(--accent3); }

  /* ── Code blocks ── */
  .code-block {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
  }

  .code-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 16px;
    background: var(--surface2);
    border-bottom: 1px solid var(--border);
  }

  .code-lang {
    font-family: 'Space Mono', monospace;
    font-size: .75rem;
    color: var(--accent);
    font-weight: 700;
    letter-spacing: .05em;
  }

  .code-dots { display: flex; gap: 6px; }
  .code-dot { width: 10px; height: 10px; border-radius: 50%; }
  .dot-r { background: #ff5f56; }
  .dot-y { background: #ffbd2e; }
  .dot-g { background: #27c93f; }

  pre {
    padding: 18px 20px;
    font-family: 'Space Mono', monospace;
    font-size: .83rem;
    color: #7dd3fc;
    overflow-x: auto;
    line-height: 1.8;
  }

  .cmd { color: #a78bfa; }
  .comment { color: #4a6a8a; }

  /* ── Steps (how to run) ── */
  .run-steps { display: flex; flex-direction: column; gap: 16px; }

  .run-step { border-radius: 12px; overflow: hidden; border: 1px solid var(--border); }

  .run-step-label {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 10px 16px;
    background: var(--surface2);
    font-size: .8rem;
    font-weight: 700;
    color: var(--muted);
    font-family: 'Space Mono', monospace;
    letter-spacing: .05em;
  }

  .run-num {
    width: 22px; height: 22px;
    border-radius: 6px;
    background: linear-gradient(135deg, var(--accent2), var(--accent));
    display: flex; align-items: center; justify-content: center;
    font-size: .72rem;
    font-weight: 700;
    color: #fff;
  }

  /* ── Future improvements ── */
  .future-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 14px; }

  .future-card {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 18px;
    transition: all .25s;
    cursor: default;
  }

  .future-card:hover {
    border-color: var(--accent2);
    transform: translateY(-3px);
    box-shadow: 0 8px 24px rgba(124,58,237,.15);
  }

  .future-icon { font-size: 1.5rem; margin-bottom: 10px; }
  .future-card p { font-size: .88rem; color: #9ab8d4; line-height: 1.5; }

  /* ── Author card ── */
  .author-card {
    display: flex;
    align-items: center;
    gap: 20px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 28px;
    position: relative;
    overflow: hidden;
  }

  .author-card::after {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,212,255,.04), rgba(124,58,237,.06));
    pointer-events: none;
  }

  .author-avatar {
    width: 68px; height: 68px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--accent2), var(--accent));
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.6rem;
    font-weight: 800;
    color: #fff;
    flex-shrink: 0;
    box-shadow: 0 0 30px rgba(0,212,255,.25);
    animation: avatarGlow 3s ease-in-out infinite;
  }

  @keyframes avatarGlow {
    0%, 100% { box-shadow: 0 0 30px rgba(0,212,255,.25); }
    50%       { box-shadow: 0 0 45px rgba(124,58,237,.4); }
  }

  .author-name {
    font-size: 1.2rem;
    font-weight: 800;
    color: var(--text);
    margin-bottom: 4px;
  }

  .author-role {
    font-size: .88rem;
    color: var(--muted);
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .author-role::before {
    content: '';
    width: 6px; height: 6px;
    background: var(--accent3);
    border-radius: 50%;
    animation: blink 1.5s ease-in-out infinite;
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50%       { opacity: .3; }
  }

  /* ── Model highlight ── */
  .model-highlight {
    background: linear-gradient(135deg, rgba(0,212,255,.07), rgba(124,58,237,.09));
    border: 1px solid rgba(0,212,255,.2);
    border-radius: 14px;
    padding: 26px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }

  .model-highlight::before {
    content: '';
    position: absolute;
    top: -40px; left: 50%; transform: translateX(-50%);
    width: 200px; height: 80px;
    background: radial-gradient(ellipse, rgba(0,212,255,.2), transparent 70%);
    pointer-events: none;
  }

  .model-name {
    font-size: 1.8rem;
    font-weight: 800;
    letter-spacing: -1px;
    background: linear-gradient(90deg, var(--accent), #a78bfa);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 10px;
  }

  .model-tags { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; margin-top: 14px; }

  .mtag {
    padding: 5px 14px;
    border-radius: 100px;
    font-size: .78rem;
    font-weight: 700;
    font-family: 'Space Mono', monospace;
  }

  .mtag-blue   { background: rgba(0,212,255,.1);   color: var(--accent);  border: 1px solid rgba(0,212,255,.2); }
  .mtag-purple { background: rgba(167,139,250,.1); color: #a78bfa;        border: 1px solid rgba(167,139,250,.2); }
  .mtag-green  { background: rgba(16,185,129,.1);  color: var(--accent3); border: 1px solid rgba(16,185,129,.2); }

  /* ── Scroll progress ── */
  #progress {
    position: fixed;
    top: 0; left: 0;
    height: 3px;
    background: linear-gradient(90deg, var(--accent), var(--accent2), var(--accent3));
    z-index: 999;
    width: 0%;
    transition: width .1s;
  }

  /* ── Footer ── */
  .footer {
    text-align: center;
    padding: 40px 0 10px;
    color: var(--muted);
    font-size: .82rem;
    font-family: 'Space Mono', monospace;
    opacity: 0;
    animation: fadeIn .6s 2s forwards;
  }

  .footer span { color: var(--accent); }

  @media(max-width:600px) {
    .author-card { flex-direction: column; text-align: center; }
    .author-role { justify-content: center; }
  }
</style>
</head>
<body>

<div id="progress"></div>

<div class="orb orb1"></div>
<div class="orb orb2"></div>

<div class="container">

  <!-- Hero -->
  <div class="hero">
    <div class="hero-icon">
      <div class="brain-ring"></div>
      <svg viewBox="0 0 90 90" fill="none" xmlns="http://www.w3.org/2000/svg">
        <circle cx="45" cy="45" r="40" fill="url(#hg)" opacity=".15"/>
        <path d="M30 55c0-8.284 6.716-15 15-15s15 6.716 15 15" stroke="url(#sg)" stroke-width="2.5" stroke-linecap="round"/>
        <circle cx="45" cy="35" r="8" fill="none" stroke="url(#sg)" stroke-width="2"/>
        <path d="M26 42h4M60 42h4M45 22v4M45 68v-4M31.5 29.5l2.8 2.8M55.7 53.7l2.8 2.8M31.5 60.5l2.8-2.8M55.7 36.3l2.8-2.8" stroke="#00d4ff" stroke-width="1.8" stroke-linecap="round" opacity=".6"/>
        <defs>
          <linearGradient id="hg" x1="5" y1="5" x2="85" y2="85">
            <stop stop-color="#00d4ff"/><stop offset="1" stop-color="#7c3aed"/>
          </linearGradient>
          <linearGradient id="sg" x1="30" y1="20" x2="60" y2="60">
            <stop stop-color="#00d4ff"/><stop offset="1" stop-color="#a78bfa"/>
          </linearGradient>
        </defs>
      </svg>
    </div>

    <h1>Placement Prediction<br/>using Logistic Regression</h1>
    <p class="hero-sub">An end-to-end machine learning project that predicts whether a student will get placed based on academic performance using CGPA and IQ score.</p>

    <div class="badges">
      <span class="badge badge-blue">🐍 Python</span>
      <span class="badge badge-purple">🤖 Machine Learning</span>
      <span class="badge badge-green">📊 Logistic Regression</span>
      <span class="badge badge-orange">📓 Jupyter Notebook</span>
    </div>
  </div>

  <div class="divider"></div>

  <!-- Project Overview -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-blue">🎯</div>
      <h2>Project Overview</h2>
    </div>
    <div class="card">
      <p>Campus placements play a significant role in a student's career. Educational institutions often want to analyze the factors that influence student placements.</p>
      <br/>
      <p>In this project, a machine learning model is built to analyze patterns in student data and predict whether a student is likely to get placed.</p>
      <br/>
      <p>The goal of this project is to demonstrate how <strong style="color:var(--accent)">machine learning can be applied to real-world educational data to make predictive decisions.</strong></p>
    </div>
  </div>

  <div class="divider"></div>

  <!-- Dataset -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-green">🗄️</div>
      <h2>Dataset Information</h2>
    </div>
    <div class="table-wrap">
      <table>
        <thead>
          <tr><th>Feature</th><th>Description</th></tr>
        </thead>
        <tbody>
          <tr><td>CGPA</td><td>Student's academic performance score</td></tr>
          <tr><td>IQ</td><td>Intelligence quotient score</td></tr>
          <tr><td>Placement</td><td>Target variable — 0 = Not Placed, 1 = Placed</td></tr>
        </tbody>
      </table>
    </div>
    <div style="margin-top:14px; display:flex; align-items:center; gap:8px;">
      <span style="font-size:.75rem; font-family:'Space Mono',monospace; color:var(--muted);">📂 Dataset file:</span>
      <span style="font-family:'Space Mono',monospace; font-size:.8rem; color:var(--accent3); background:rgba(16,185,129,.08); padding:3px 12px; border-radius:6px; border:1px solid rgba(16,185,129,.2);">placement.csv</span>
    </div>
  </div>

  <div class="divider"></div>

  <!-- Technologies -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-purple">⚙️</div>
      <h2>Technologies Used</h2>
    </div>
    <div class="tech-grid">
      <div class="tech-pill"><span class="tech-dot" style="background:#3b82f6"></span>Python</div>
      <div class="tech-pill"><span class="tech-dot" style="background:#f59e0b"></span>NumPy</div>
      <div class="tech-pill"><span class="tech-dot" style="background:#10b981"></span>Pandas</div>
      <div class="tech-pill"><span class="tech-dot" style="background:#ef4444"></span>Matplotlib</div>
      <div class="tech-pill"><span class="tech-dot" style="background:#8b5cf6"></span>Scikit-learn</div>
      <div class="tech-pill"><span class="tech-dot" style="background:#f97316"></span>Jupyter Notebook</div>
      <div class="tech-pill"><span class="tech-dot" style="background:#06b6d4"></span>Pickle</div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- Model -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-blue">🧠</div>
      <h2>Machine Learning Model</h2>
    </div>
    <div class="model-highlight">
      <div class="model-name">Logistic Regression</div>
      <p style="color:#9ab8d4; font-size:.95rem;">Used for <strong style="color:var(--text)">binary classification problems</strong>, where the output variable has two possible outcomes.</p>
      <div class="model-tags">
        <span class="mtag mtag-blue">0 = Not Placed</span>
        <span class="mtag mtag-green">1 = Placed</span>
        <span class="mtag mtag-purple">Binary Output</span>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- Workflow -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-orange">🔄</div>
      <h2>Project Workflow</h2>
    </div>
    <div class="card">
      <div class="steps">
        <div class="step"><div class="step-left"><div class="step-num">01</div><div class="step-line"></div></div><div class="step-content"><strong>Data Loading using Pandas</strong></div></div>
        <div class="step"><div class="step-left"><div class="step-num">02</div><div class="step-line"></div></div><div class="step-content"><strong>Data Exploration and Understanding</strong></div></div>
        <div class="step"><div class="step-left"><div class="step-num">03</div><div class="step-line"></div></div><div class="step-content"><strong>Data Visualization using Matplotlib</strong></div></div>
        <div class="step"><div class="step-left"><div class="step-num">04</div><div class="step-line"></div></div><div class="step-content"><strong>Feature Selection</strong></div></div>
        <div class="step"><div class="step-left"><div class="step-num">05</div><div class="step-line"></div></div><div class="step-content"><strong>Train-Test Split</strong></div></div>
        <div class="step"><div class="step-left"><div class="step-num">06</div><div class="step-line"></div></div><div class="step-content"><strong>Model Training using Logistic Regression</strong></div></div>
        <div class="step"><div class="step-left"><div class="step-num">07</div><div class="step-line"></div></div><div class="step-content"><strong>Model Evaluation</strong></div></div>
        <div class="step"><div class="step-left"><div class="step-num">08</div></div><div class="step-content"><strong>Saving the trained model using Pickle</strong></div></div>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- Project Structure -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-green">📁</div>
      <h2>Project Structure</h2>
    </div>
    <div class="tree">
      <div class="tree-root">📦 placement-project-logistic-regression</div>
      <div style="padding-left:16px">
        <div><span class="tree-branch">│</span></div>
        <div><span class="tree-branch">├── </span><span class="tree-file highlight">placement.csv</span></div>
        <div><span class="tree-branch">├── </span><span class="tree-file highlight">end_to_end_ml.ipynb</span></div>
        <div><span class="tree-branch">├── </span><span class="tree-file highlight">model.pkl</span></div>
        <div><span class="tree-branch">└── </span><span class="tree-file highlight">README.md</span></div>
      </div>
    </div>
    <div style="margin-top:16px; display:flex; flex-direction:column; gap:8px;">
      <div style="display:flex;gap:10px;align-items:flex-start"><span style="color:var(--accent3);font-family:'Space Mono',monospace;font-size:.82rem;min-width:180px">placement.csv</span><span style="color:var(--muted);font-size:.88rem">→ Dataset used for training</span></div>
      <div style="display:flex;gap:10px;align-items:flex-start"><span style="color:var(--accent3);font-family:'Space Mono',monospace;font-size:.82rem;min-width:180px">end_to_end_ml.ipynb</span><span style="color:var(--muted);font-size:.88rem">→ Jupyter notebook with the full ML workflow</span></div>
      <div style="display:flex;gap:10px;align-items:flex-start"><span style="color:var(--accent3);font-family:'Space Mono',monospace;font-size:.82rem;min-width:180px">model.pkl</span><span style="color:var(--muted);font-size:.88rem">→ Saved trained model</span></div>
      <div style="display:flex;gap:10px;align-items:flex-start"><span style="color:var(--accent3);font-family:'Space Mono',monospace;font-size:.82rem;min-width:180px">README.md</span><span style="color:var(--muted);font-size:.88rem">→ Project documentation</span></div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- How to Run -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-blue">🚀</div>
      <h2>How to Run the Project</h2>
    </div>
    <div class="run-steps">

      <div class="run-step">
        <div class="run-step-label"><div class="run-num">1</div>Clone the repository</div>
        <div class="code-block" style="border-top:none;border-radius:0 0 12px 12px;">
          <div class="code-header"><div class="code-dots"><div class="code-dot dot-r"></div><div class="code-dot dot-y"></div><div class="code-dot dot-g"></div></div><span class="code-lang">BASH</span></div>
          <pre><span class="cmd">git clone</span> https://github.com/your-username/placement-project-logistic-regression.git</pre>
        </div>
      </div>

      <div class="run-step">
        <div class="run-step-label"><div class="run-num">2</div>Navigate to the project directory</div>
        <div class="code-block" style="border-top:none;border-radius:0 0 12px 12px;">
          <div class="code-header"><div class="code-dots"><div class="code-dot dot-r"></div><div class="code-dot dot-y"></div><div class="code-dot dot-g"></div></div><span class="code-lang">BASH</span></div>
          <pre><span class="cmd">cd</span> placement-project-logistic-regression</pre>
        </div>
      </div>

      <div class="run-step">
        <div class="run-step-label"><div class="run-num">3</div>Install required libraries</div>
        <div class="code-block" style="border-top:none;border-radius:0 0 12px 12px;">
          <div class="code-header"><div class="code-dots"><div class="code-dot dot-r"></div><div class="code-dot dot-y"></div><div class="code-dot dot-g"></div></div><span class="code-lang">BASH</span></div>
          <pre><span class="cmd">pip install</span> pandas numpy matplotlib scikit-learn</pre>
        </div>
      </div>

      <div class="run-step">
        <div class="run-step-label"><div class="run-num">4</div>Open Jupyter Notebook</div>
        <div class="code-block" style="border-top:none;border-radius:0 0 12px 12px;">
          <div class="code-header"><div class="code-dots"><div class="code-dot dot-r"></div><div class="code-dot dot-y"></div><div class="code-dot dot-g"></div></div><span class="code-lang">BASH</span></div>
          <pre><span class="cmd">jupyter notebook</span></pre>
        </div>
      </div>

      <div class="run-step">
        <div class="run-step-label"><div class="run-num">5</div>Run the notebook</div>
        <div class="code-block" style="border-top:none;border-radius:0 0 12px 12px;">
          <div class="code-header"><div class="code-dots"><div class="code-dot dot-r"></div><div class="code-dot dot-y"></div><div class="code-dot dot-g"></div></div><span class="code-lang">NOTEBOOK</span></div>
          <pre><span class="comment"># Open and run:</span>
end_to_end_ml.ipynb</pre>
        </div>
      </div>

    </div>
  </div>

  <div class="divider"></div>

  <!-- Model Saving -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-pink">💾</div>
      <h2>Model Saving</h2>
    </div>
    <div class="card">
      <p>After training the model, it is saved using the <strong style="color:var(--accent)">Pickle library</strong>.</p>
      <br/>
      <div style="display:flex;align-items:center;gap:10px;margin-bottom:14px">
        <span style="color:var(--muted);font-size:.85rem;">Saved model file:</span>
        <span style="font-family:'Space Mono',monospace;font-size:.82rem;color:var(--accent3);background:rgba(16,185,129,.08);padding:4px 14px;border-radius:6px;border:1px solid rgba(16,185,129,.2);">model.pkl</span>
      </div>
      <p>This allows the model to be reused later without retraining.</p>
    </div>
  </div>

  <div class="divider"></div>

  <!-- Future Improvements -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-purple">🔮</div>
      <h2>Future Improvements</h2>
    </div>
    <div class="future-grid">
      <div class="future-card">
        <div class="future-icon">📊</div>
        <p>Add more student features such as internships, projects, and communication skills</p>
      </div>
      <div class="future-card">
        <div class="future-icon">🌐</div>
        <p>Build a web interface using <strong style="color:#a78bfa">Flask or Streamlit</strong></p>
      </div>
      <div class="future-card">
        <div class="future-icon">☁️</div>
        <p>Deploy the model online for real-world usage</p>
      </div>
      <div class="future-card">
        <div class="future-icon">📈</div>
        <p>Add more datasets for better prediction accuracy</p>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- Author -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-blue">👤</div>
      <h2>Author</h2>
    </div>
    <div class="author-card">
      <div class="author-avatar">VK</div>
      <div>
        <div class="author-name">Vikash Kumar</div>
        <div class="author-role">Computer Science and Engineering (AI &amp; ML)</div>
      </div>
    </div>
  </div>

  <div class="footer">Made with <span>♥</span> — Placement Prediction · Logistic Regression</div>

</div>

<script>
  // Scroll progress bar
  window.addEventListener('scroll', () => {
    const el = document.getElementById('progress');
    const h = document.body.scrollHeight - window.innerHeight;
    el.style.width = (window.scrollY / h * 100) + '%';
  });

  // Intersection Observer for scroll-triggered animations
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.style.animationPlayState = 'running';
      }
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.section').forEach(s => {
    s.style.animationPlayState = 'paused';
    observer.observe(s);
  });
</script>
</body>
</html>
