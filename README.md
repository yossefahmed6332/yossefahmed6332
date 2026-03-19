<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Yossef | Backend Developer</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;800&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #050a0e;
    --surface: #0b1420;
    --panel: #0f1e2e;
    --accent: #00d4ff;
    --accent2: #ff6b35;
    --accent3: #7fff6b;
    --text: #e0eaf5;
    --muted: #4a6a80;
    --border: rgba(0,212,255,0.15);
    --glow: 0 0 30px rgba(0,212,255,0.25);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Space Mono', monospace;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  #cursor {
    position: fixed;
    width: 12px; height: 12px;
    background: var(--accent);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%,-50%);
    transition: transform 0.1s, opacity 0.3s;
    box-shadow: 0 0 20px var(--accent);
  }
  #cursor-ring {
    position: fixed;
    width: 36px; height: 36px;
    border: 1px solid rgba(0,212,255,0.4);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%,-50%);
    transition: transform 0.15s ease-out, width 0.2s, height 0.2s;
  }

  /* Canvas BG */
  #bg-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    opacity: 0.55;
  }

  /* Layout */
  .wrapper {
    position: relative;
    z-index: 1;
    max-width: 1100px;
    margin: 0 auto;
    padding: 0 32px;
  }

  /* ---- HERO ---- */
  #hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    position: relative;
    padding: 80px 0 60px;
  }

  .hero-grid {
    display: grid;
    grid-template-columns: 1fr 340px;
    gap: 60px;
    align-items: center;
    width: 100%;
  }

  .hero-tag {
    font-size: 11px;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 20px;
    opacity: 0;
    animation: fadeUp 0.7s 0.2s forwards;
  }

  .hero-name {
    font-family: 'Syne', sans-serif;
    font-size: clamp(48px, 7vw, 88px);
    font-weight: 800;
    line-height: 0.95;
    margin-bottom: 8px;
    opacity: 0;
    animation: fadeUp 0.7s 0.4s forwards;
  }

  .hero-name span {
    color: var(--accent);
    display: block;
  }

  .hero-role {
    font-size: 13px;
    color: var(--muted);
    letter-spacing: 2px;
    margin-bottom: 32px;
    opacity: 0;
    animation: fadeUp 0.7s 0.6s forwards;
  }

  .hero-desc {
    font-size: 14px;
    line-height: 1.9;
    color: #8aabb8;
    max-width: 520px;
    margin-bottom: 40px;
    opacity: 0;
    animation: fadeUp 0.7s 0.8s forwards;
  }

  .hero-desc strong { color: var(--accent); }

  .cta-row {
    display: flex;
    gap: 16px;
    flex-wrap: wrap;
    opacity: 0;
    animation: fadeUp 0.7s 1s forwards;
  }

  .btn {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 12px 28px;
    border: 1px solid var(--accent);
    color: var(--accent);
    text-decoration: none;
    font-family: 'Space Mono', monospace;
    font-size: 12px;
    letter-spacing: 2px;
    text-transform: uppercase;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }

  .btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--accent);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.3s;
    z-index: -1;
  }

  .btn:hover { color: var(--bg); }
  .btn:hover::before { transform: scaleX(1); }

  .btn.solid {
    background: var(--accent);
    color: var(--bg);
    font-weight: 700;
  }
  .btn.solid::before { background: var(--accent2); }
  .btn.solid:hover { color: white; }

  /* 3D Card */
  .card-3d-wrap {
    perspective: 1000px;
    opacity: 0;
    animation: fadeIn 1s 1.2s forwards;
  }

  .card-3d {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 32px;
    transform-style: preserve-3d;
    transition: transform 0.1s ease-out;
    box-shadow: var(--glow), inset 0 1px 0 rgba(255,255,255,0.05);
    position: relative;
    overflow: hidden;
  }

  .card-3d::before {
    content: '';
    position: absolute;
    top: -50%; left: -50%;
    width: 200%; height: 200%;
    background: radial-gradient(circle at 50% 50%, rgba(0,212,255,0.04) 0%, transparent 60%);
    pointer-events: none;
  }

  .card-avatar {
    width: 80px; height: 80px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    display: flex; align-items: center; justify-content: center;
    font-family: 'Syne', sans-serif;
    font-size: 28px;
    font-weight: 800;
    color: var(--bg);
    margin-bottom: 20px;
    position: relative;
  }

  .card-avatar::after {
    content: '';
    position: absolute;
    inset: -3px;
    border-radius: 50%;
    border: 2px solid rgba(0,212,255,0.4);
    animation: spin 8s linear infinite;
    border-top-color: var(--accent);
    border-right-color: transparent;
    border-bottom-color: transparent;
  }

  .card-label {
    font-size: 10px;
    letter-spacing: 3px;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 6px;
  }

  .card-val {
    font-family: 'Syne', sans-serif;
    font-size: 20px;
    font-weight: 700;
    margin-bottom: 20px;
  }

  .stat-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-top: 20px;
    padding-top: 20px;
    border-top: 1px solid var(--border);
  }

  .stat-item { text-align: center; }
  .stat-num {
    font-family: 'Syne', sans-serif;
    font-size: 26px;
    font-weight: 800;
    color: var(--accent);
    display: block;
  }
  .stat-lbl {
    font-size: 9px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--muted);
    margin-top: 2px;
  }

  /* Status badge */
  .status-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-size: 10px;
    letter-spacing: 1px;
    color: var(--accent3);
    background: rgba(127,255,107,0.07);
    border: 1px solid rgba(127,255,107,0.2);
    padding: 6px 14px;
    border-radius: 2px;
    margin-bottom: 20px;
  }

  .status-dot {
    width: 7px; height: 7px;
    border-radius: 50%;
    background: var(--accent3);
    animation: pulse 1.5s infinite;
  }

  /* ---- SECTIONS ---- */
  section {
    padding: 100px 0;
    position: relative;
  }

  .section-header {
    display: flex;
    align-items: center;
    gap: 20px;
    margin-bottom: 60px;
  }

  .section-num {
    font-size: 11px;
    letter-spacing: 3px;
    color: var(--accent);
    opacity: 0.6;
  }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 36px;
    font-weight: 800;
    letter-spacing: -1px;
  }

  .section-line {
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  /* ---- SKILLS ---- */
  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 24px;
  }

  .skill-block {
    background: var(--panel);
    border: 1px solid var(--border);
    padding: 28px;
    border-radius: 4px;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }

  .skill-block::after {
    content: '';
    position: absolute;
    top: 0; left: 0;
    width: 3px; height: 0;
    background: var(--accent);
    transition: height 0.4s;
  }

  .skill-block:hover {
    border-color: rgba(0,212,255,0.3);
    transform: translateY(-4px);
    box-shadow: var(--glow);
  }

  .skill-block:hover::after { height: 100%; }

  .skill-icon {
    font-size: 28px;
    margin-bottom: 14px;
  }

  .skill-name {
    font-family: 'Syne', sans-serif;
    font-size: 18px;
    font-weight: 700;
    margin-bottom: 8px;
  }

  .skill-sub {
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 1px;
    margin-bottom: 18px;
  }

  .skill-bar-wrap {
    height: 3px;
    background: rgba(255,255,255,0.06);
    border-radius: 2px;
    overflow: hidden;
    margin-bottom: 6px;
  }

  .skill-bar {
    height: 100%;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    border-radius: 2px;
    width: 0;
    transition: width 1.5s ease;
  }

  .skill-pct {
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 1px;
    text-align: right;
  }

  .skill-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 16px;
  }

  .tag {
    font-size: 10px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--accent);
    border: 1px solid rgba(0,212,255,0.2);
    padding: 4px 10px;
    border-radius: 2px;
    background: rgba(0,212,255,0.04);
  }

  /* ---- STACK ---- */
  .stack-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 16px;
  }

  .stack-item {
    background: var(--panel);
    border: 1px solid var(--border);
    padding: 24px 16px;
    text-align: center;
    border-radius: 4px;
    transition: all 0.3s;
    cursor: default;
  }

  .stack-item:hover {
    border-color: var(--accent);
    transform: translateY(-6px) scale(1.03);
    box-shadow: 0 20px 40px rgba(0,0,0,0.4), var(--glow);
  }

  .stack-logo {
    font-size: 32px;
    margin-bottom: 10px;
    display: block;
  }

  .stack-name {
    font-size: 11px;
    letter-spacing: 1px;
    color: var(--muted);
  }

  /* ---- CP SECTION ---- */
  .cp-card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 40px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 48px;
    align-items: center;
  }

  .cp-title {
    font-family: 'Syne', sans-serif;
    font-size: 28px;
    font-weight: 800;
    margin-bottom: 12px;
  }

  .cp-desc {
    font-size: 13px;
    line-height: 1.8;
    color: #8aabb8;
    margin-bottom: 28px;
  }

  .cp-link {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    color: var(--accent);
    text-decoration: none;
    font-size: 12px;
    letter-spacing: 2px;
    text-transform: uppercase;
    border-bottom: 1px solid rgba(0,212,255,0.3);
    padding-bottom: 4px;
    transition: all 0.3s;
  }
  .cp-link:hover {
    color: var(--accent2);
    border-color: var(--accent2);
    gap: 16px;
  }

  .cp-visual {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
    justify-content: center;
  }

  .cp-badge {
    width: 70px; height: 70px;
    border-radius: 4px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    font-weight: 700;
    gap: 6px;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }

  .cp-badge::before {
    content: '';
    position: absolute;
    inset: 0;
    opacity: 0.1;
  }

  .cp-badge:nth-child(1) { background: rgba(0,212,255,0.08); border: 1px solid rgba(0,212,255,0.3); color: var(--accent); }
  .cp-badge:nth-child(2) { background: rgba(255,107,53,0.08); border: 1px solid rgba(255,107,53,0.3); color: var(--accent2); }
  .cp-badge:nth-child(3) { background: rgba(127,255,107,0.08); border: 1px solid rgba(127,255,107,0.3); color: var(--accent3); }
  .cp-badge:nth-child(4) { background: rgba(180,100,255,0.08); border: 1px solid rgba(180,100,255,0.3); color: #b464ff; }

  .cp-badge .badge-icon { font-size: 22px; }

  .cp-badge:hover {
    transform: scale(1.1) rotate(-3deg);
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
  }

  /* ---- EDUCATION ---- */
  .edu-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
  }

  .edu-card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 28px;
    transition: all 0.3s;
    position: relative;
  }

  .edu-card:hover {
    border-color: rgba(0,212,255,0.3);
    transform: translateY(-4px);
    box-shadow: var(--glow);
  }

  .edu-platform {
    font-size: 10px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--accent2);
    margin-bottom: 10px;
  }

  .edu-course {
    font-family: 'Syne', sans-serif;
    font-size: 17px;
    font-weight: 700;
    margin-bottom: 8px;
  }

  .edu-detail {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.7;
  }

  .cert-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    margin-top: 16px;
    font-size: 10px;
    letter-spacing: 2px;
    color: var(--accent3);
    background: rgba(127,255,107,0.06);
    border: 1px solid rgba(127,255,107,0.15);
    padding: 5px 12px;
    border-radius: 2px;
  }

  /* ---- FOOTER ---- */
  footer {
    border-top: 1px solid var(--border);
    padding: 40px 0;
    text-align: center;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 2px;
  }

  footer a { color: var(--accent); text-decoration: none; }

  /* ---- DIVIDER ---- */
  .glitch-divider {
    position: relative;
    height: 1px;
    background: var(--border);
    margin: 0;
    overflow: visible;
  }

  .glitch-divider::after {
    content: '';
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 8px; height: 8px;
    background: var(--accent);
    border-radius: 50%;
    box-shadow: 0 0 16px var(--accent);
  }

  /* ---- SCROLLBAR ---- */
  ::-webkit-scrollbar { width: 4px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 2px; }

  /* ---- KEYFRAMES ---- */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes fadeIn {
    from { opacity: 0; }
    to   { opacity: 1; }
  }
  @keyframes spin {
    to { transform: rotate(360deg); }
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.4; transform: scale(0.6); }
  }
  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-12px); }
  }

  /* ---- VISIBLE TRIGGER ---- */
  .reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* ---- TERMINAL BLOCK ---- */
  .terminal {
    background: #030d14;
    border: 1px solid rgba(0,212,255,0.12);
    border-radius: 4px;
    padding: 28px;
    font-size: 13px;
    line-height: 2;
    position: relative;
    overflow: hidden;
  }

  .terminal-bar {
    display: flex;
    gap: 8px;
    align-items: center;
    margin-bottom: 20px;
    padding-bottom: 14px;
    border-bottom: 1px solid rgba(255,255,255,0.04);
  }

  .t-dot {
    width: 11px; height: 11px; border-radius: 50%;
  }
  .t-dot:nth-child(1) { background: #ff5f57; }
  .t-dot:nth-child(2) { background: #febc2e; }
  .t-dot:nth-child(3) { background: #28c840; }

  .t-title {
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 2px;
    margin-left: auto;
  }

  .t-line { display: block; }
  .t-prompt { color: var(--accent); }
  .t-cmd { color: var(--text); }
  .t-out { color: #4a6a80; }
  .t-val { color: var(--accent3); }
  .t-str { color: var(--accent2); }
  .t-cmt { color: #2a4a5a; }

  .cursor-blink {
    display: inline-block;
    width: 8px; height: 14px;
    background: var(--accent);
    vertical-align: middle;
    animation: pulse 0.9s step-end infinite;
  }

  /* nav */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    padding: 18px 40px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: linear-gradient(to bottom, rgba(5,10,14,0.95), transparent);
    backdrop-filter: blur(8px);
  }

  .nav-logo {
    font-family: 'Syne', sans-serif;
    font-size: 20px;
    font-weight: 800;
    color: var(--accent);
    letter-spacing: -1px;
  }

  .nav-links {
    display: flex;
    gap: 32px;
    list-style: none;
  }

  .nav-links a {
    color: var(--muted);
    text-decoration: none;
    font-size: 11px;
    letter-spacing: 2px;
    text-transform: uppercase;
    transition: color 0.3s;
  }

  .nav-links a:hover { color: var(--accent); }

  /* Responsive */
  @media (max-width: 768px) {
    .hero-grid { grid-template-columns: 1fr; }
    .card-3d-wrap { display: none; }
    .cp-card { grid-template-columns: 1fr; }
    .edu-row { grid-template-columns: 1fr; }
    .nav-links { display: none; }
  }
</style>
</head>
<body>

<div id="cursor"></div>
<div id="cursor-ring"></div>

<canvas id="bg-canvas"></canvas>

<!-- NAV -->
<nav>
  <div class="nav-logo">YS.</div>
  <ul class="nav-links">
    <li><a href="#skills">Skills</a></li>
    <li><a href="#stack">Stack</a></li>
    <li><a href="#cp">CF</a></li>
    <li><a href="#edu">Certs</a></li>
  </ul>
</nav>

<div class="wrapper">

  <!-- HERO -->
  <section id="hero">
    <div class="hero-grid">
      <div>
        <div class="status-badge">
          <div class="status-dot"></div>
          Available for opportunities
        </div>
        <div class="hero-tag">// Backend Developer · Egypt</div>
        <h1 class="hero-name">
          Yossef
          <span>Ahmed</span>
        </h1>
        <p class="hero-role">Java · Spring Boot · SQL · Competitive Programmer</p>
        <p class="hero-desc">
          Building robust backend systems with <strong>Java OOP</strong> and <strong>Spring Boot</strong>.
          Designing clean data architectures with <strong>MySQL</strong>.
          Sharpening algorithmic thinking on <strong>Codeforces</strong>.
          Always shipping, always learning.
        </p>
        <div class="cta-row">
          <a href="https://codeforces.com/profile/yossef2260" target="_blank" class="btn solid">↗ Codeforces</a>
          <a href="https://github.com/" target="_blank" class="btn">⌥ GitHub</a>
        </div>
      </div>

      <!-- 3D CARD -->
      <div class="card-3d-wrap" id="card3d">
        <div class="card-3d" id="tiltCard">
          <div class="card-avatar">YS</div>
          <div class="card-label">Current Role</div>
          <div class="card-val">Junior Backend Dev</div>
          <div class="card-label">Location</div>
          <div class="card-val" style="font-size:14px;color:var(--muted);">Alexandria, Egypt 🇪🇬</div>
          <div class="stat-row">
            <div class="stat-item">
              <span class="stat-num" id="cfCount">0+</span>
              <span class="stat-lbl">CF Problems</span>
            </div>
            <div class="stat-item">
              <span class="stat-num">2</span>
              <span class="stat-lbl">Certifications</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <div class="glitch-divider"></div>

  <!-- TERMINAL -->
  <section style="padding: 80px 0 60px;">
    <div class="reveal">
      <div class="terminal">
        <div class="terminal-bar">
          <div class="t-dot"></div>
          <div class="t-dot"></div>
          <div class="t-dot"></div>
          <span class="t-title">yossef@dev ~ bash</span>
        </div>
        <span class="t-line"><span class="t-prompt">❯ </span><span class="t-cmd">cat about.json</span></span>
        <span class="t-line t-out">{</span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"name"</span>: <span class="t-str">"Yossef Ahmed"</span>,</span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"role"</span>: <span class="t-str">"Junior Backend Developer"</span>,</span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"stack"</span>: [<span class="t-str">"Java"</span>, <span class="t-str">"Spring Boot"</span>, <span class="t-str">"MySQL"</span>],</span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"competitive_programming"</span>: <span class="t-val">true</span>,</span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"platform"</span>: <span class="t-str">"Codeforces @yossef2260"</span>,</span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"IDE"</span>: <span class="t-str">"IntelliJ IDEA"</span>,</span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"database_tool"</span>: <span class="t-str">"MySQL Workbench"</span>,</span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"status"</span>: <span class="t-val">true</span>, <span class="t-cmt">// open to work</span></span>
        <span class="t-line t-out">&nbsp;&nbsp;<span style="color:#5ab4d4">"learning"</span>: <span class="t-str">"always"</span></span>
        <span class="t-line t-out">}</span>
        <span class="t-line" style="margin-top:8px"><span class="t-prompt">❯ </span><span class="cursor-blink"></span></span>
      </div>
    </div>
  </section>

  <div class="glitch-divider"></div>

  <!-- SKILLS -->
  <section id="skills">
    <div class="section-header reveal">
      <span class="section-num">01</span>
      <h2 class="section-title">Core Skills</h2>
      <div class="section-line"></div>
    </div>
    <div class="skills-grid">
      <div class="skill-block reveal" data-bar="82">
        <div class="skill-icon">☕</div>
        <div class="skill-name">Java OOP</div>
        <div class="skill-sub">IntelliJ IDEA · Coursera Certified</div>
        <div class="skill-bar-wrap"><div class="skill-bar"></div></div>
        <div class="skill-pct">Intermediate</div>
        <div class="skill-tags">
          <span class="tag">Classes</span>
          <span class="tag">Inheritance</span>
          <span class="tag">Polymorphism</span>
          <span class="tag">Collections</span>
        </div>
      </div>
      <div class="skill-block reveal" data-bar="72" style="transition-delay:0.1s">
        <div class="skill-icon">🌱</div>
        <div class="skill-name">Spring Boot</div>
        <div class="skill-sub">REST APIs · MVC · JPA</div>
        <div class="skill-bar-wrap"><div class="skill-bar"></div></div>
        <div class="skill-pct">Intermediate</div>
        <div class="skill-tags">
          <span class="tag">REST</span>
          <span class="tag">Spring MVC</span>
          <span class="tag">Spring Data</span>
          <span class="tag">JPA</span>
        </div>
      </div>
      <div class="skill-block reveal" data-bar="78" style="transition-delay:0.2s">
        <div class="skill-icon">🗄️</div>
        <div class="skill-name">SQL / MySQL</div>
        <div class="skill-sub">MySQL Workbench · Coursera Certified</div>
        <div class="skill-bar-wrap"><div class="skill-bar"></div></div>
        <div class="skill-pct">Intermediate</div>
        <div class="skill-tags">
          <span class="tag">Queries</span>
          <span class="tag">Joins</span>
          <span class="tag">Indexing</span>
          <span class="tag">Schema Design</span>
        </div>
      </div>
      <div class="skill-block reveal" data-bar="60" style="transition-delay:0.3s">
        <div class="skill-icon">⚡</div>
        <div class="skill-name">Algorithms & DS</div>
        <div class="skill-sub">Codeforces Newbie → Growing</div>
        <div class="skill-bar-wrap"><div class="skill-bar"></div></div>
        <div class="skill-pct">Beginner+</div>
        <div class="skill-tags">
          <span class="tag">Arrays</span>
          <span class="tag">Sorting</span>
          <span class="tag">Greedy</span>
          <span class="tag">BFS/DFS</span>
        </div>
      </div>
    </div>
  </section>

  <div class="glitch-divider"></div>

  <!-- STACK -->
  <section id="stack">
    <div class="section-header reveal">
      <span class="section-num">02</span>
      <h2 class="section-title">Tech Stack</h2>
      <div class="section-line"></div>
    </div>
    <div class="stack-grid">
      <div class="stack-item reveal">
        <span class="stack-logo">☕</span>
        <span class="stack-name">Java</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.05s">
        <span class="stack-logo">🌱</span>
        <span class="stack-name">Spring Boot</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.1s">
        <span class="stack-logo">🐬</span>
        <span class="stack-name">MySQL</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.15s">
        <span class="stack-logo">🛡️</span>
        <span class="stack-name">Spring Security</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.2s">
        <span class="stack-logo">🔗</span>
        <span class="stack-name">REST APIs</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.25s">
        <span class="stack-logo">🧠</span>
        <span class="stack-name">IntelliJ IDEA</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.3s">
        <span class="stack-logo">🗂️</span>
        <span class="stack-name">Git / GitHub</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.35s">
        <span class="stack-logo">🐘</span>
        <span class="stack-name">Hibernate / JPA</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.4s">
        <span class="stack-logo">📦</span>
        <span class="stack-name">Maven</span>
      </div>
      <div class="stack-item reveal" style="transition-delay:0.45s">
        <span class="stack-logo">🖥️</span>
        <span class="stack-name">MySQL Workbench</span>
      </div>
    </div>
  </section>

  <div class="glitch-divider"></div>

  <!-- CP -->
  <section id="cp">
    <div class="section-header reveal">
      <span class="section-num">03</span>
      <h2 class="section-title">Competitive Programming</h2>
      <div class="section-line"></div>
    </div>
    <div class="cp-card reveal">
      <div>
        <div class="cp-title">Codeforces<br/><span style="color:var(--accent)">@yossef2260</span></div>
        <p class="cp-desc">
          Actively competing on Codeforces, sharpening problem-solving and algorithmic thinking.
          Focused on growing from Newbie to Pupil — one problem at a time.
          Strong foundation in data structures, greedy algorithms, and brute force strategies.
        </p>
        <a href="https://codeforces.com/profile/yossef2260" target="_blank" class="cp-link">
          View Profile →
        </a>
      </div>
      <div class="cp-visual">
        <div class="cp-badge">
          <span class="badge-icon">💡</span>
          <span>Problem<br/>Solving</span>
        </div>
        <div class="cp-badge">
          <span class="badge-icon">🔥</span>
          <span>Greedy<br/>Algos</span>
        </div>
        <div class="cp-badge">
          <span class="badge-icon">🌐</span>
          <span>Graph<br/>Theory</span>
        </div>
        <div class="cp-badge">
          <span class="badge-icon">⚡</span>
          <span>Dynamic<br/>Prog</span>
        </div>
      </div>
    </div>
  </section>

  <div class="glitch-divider"></div>

  <!-- EDUCATION -->
  <section id="edu">
    <div class="section-header reveal">
      <span class="section-num">04</span>
      <h2 class="section-title">Certifications</h2>
      <div class="section-line"></div>
    </div>
    <div class="edu-row">
      <div class="edu-card reveal">
        <div class="edu-platform">📚 Coursera</div>
        <div class="edu-course">Java Object-Oriented Programming</div>
        <div class="edu-detail">
          Comprehensive coverage of OOP principles, design patterns, and Java best practices.
          Completed with hands-on project implementations using IntelliJ IDEA.
        </div>
        <a href="https://coursera.org/share/a55f4f74042f9c90967594d5e8d103c7" target="_blank" style="text-decoration:none">
          <div class="cert-badge">✓ Verified Certificate · View →</div>
        </a>
      </div>
      <div class="edu-card reveal" style="transition-delay:0.15s">
        <div class="edu-platform">📚 Coursera</div>
        <div class="edu-course">SQL for Data Science & Backends</div>
        <div class="edu-detail">
          Deep dive into relational databases, query optimization, joins, and schema design.
          Practical work with MySQL Workbench and real-world database scenarios.
        </div>
        <a href="https://coursera.org/share/db290fdd31571efef13366cb721f17eb" target="_blank" style="text-decoration:none">
          <div class="cert-badge">✓ Verified Certificate · View →</div>
        </a>
      </div>
    </div>
  </section>

</div>

<!-- FOOTER -->
<footer>
  <div class="wrapper">
    <p>Built with purpose · Yossef Ahmed · Alexandria, Egypt 🇪🇬</p>
    <p style="margin-top:10px">
      <a href="https://codeforces.com/profile/yossef2260">Codeforces</a>
      &nbsp;·&nbsp;
      <a href="https://coursera.org/share/a55f4f74042f9c90967594d5e8d103c7">Java Cert</a>
      &nbsp;·&nbsp;
      <a href="https://coursera.org/share/db290fdd31571efef13366cb721f17eb">SQL Cert</a>
    </p>
  </div>
</footer>

<script>
// ---- CURSOR ----
const cur = document.getElementById('cursor');
const ring = document.getElementById('cursor-ring');
let mx = 0, my = 0, rx = 0, ry = 0;

document.addEventListener('mousemove', e => {
  mx = e.clientX; my = e.clientY;
  cur.style.left = mx + 'px';
  cur.style.top = my + 'px';
});

function animRing() {
  rx += (mx - rx) * 0.15;
  ry += (my - ry) * 0.15;
  ring.style.left = rx + 'px';
  ring.style.top = ry + 'px';
  requestAnimationFrame(animRing);
}
animRing();

document.querySelectorAll('a, button, .stack-item, .skill-block').forEach(el => {
  el.addEventListener('mouseenter', () => {
    cur.style.transform = 'translate(-50%,-50%) scale(2)';
    ring.style.width = '60px';
    ring.style.height = '60px';
    ring.style.borderColor = 'rgba(0,212,255,0.6)';
  });
  el.addEventListener('mouseleave', () => {
    cur.style.transform = 'translate(-50%,-50%) scale(1)';
    ring.style.width = '36px';
    ring.style.height = '36px';
    ring.style.borderColor = 'rgba(0,212,255,0.4)';
  });
});

// ---- BACKGROUND CANVAS (particle field + grid) ----
const canvas = document.getElementById('bg-canvas');
const ctx = canvas.getContext('2d');
let W, H, particles = [];

function resize() {
  W = canvas.width = window.innerWidth;
  H = canvas.height = window.innerHeight;
}
resize();
window.addEventListener('resize', resize);

// particles
for (let i = 0; i < 90; i++) {
  particles.push({
    x: Math.random() * 2000,
    y: Math.random() * 2000,
    r: Math.random() * 1.5 + 0.3,
    vx: (Math.random() - 0.5) * 0.3,
    vy: (Math.random() - 0.5) * 0.3,
    alpha: Math.random() * 0.5 + 0.1,
  });
}

let frame = 0;
function draw() {
  ctx.clearRect(0, 0, W, H);

  // grid lines
  ctx.strokeStyle = 'rgba(0,212,255,0.025)';
  ctx.lineWidth = 1;
  const gs = 80;
  for (let x = 0; x < W + gs; x += gs) {
    ctx.beginPath();
    ctx.moveTo(x, 0);
    ctx.lineTo(x, H);
    ctx.stroke();
  }
  for (let y = 0; y < H + gs; y += gs) {
    ctx.beginPath();
    ctx.moveTo(0, y);
    ctx.lineTo(W, y);
    ctx.stroke();
  }

  // particles + connections
  particles.forEach(p => {
    p.x += p.vx; p.y += p.vy;
    if (p.x < 0) p.x = W;
    if (p.x > W) p.x = 0;
    if (p.y < 0) p.y = H;
    if (p.y > H) p.y = 0;

    ctx.beginPath();
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    ctx.fillStyle = `rgba(0,212,255,${p.alpha})`;
    ctx.fill();
  });

  // connections
  for (let i = 0; i < particles.length; i++) {
    for (let j = i + 1; j < particles.length; j++) {
      const dx = particles[i].x - particles[j].x;
      const dy = particles[i].y - particles[j].y;
      const dist = Math.sqrt(dx*dx + dy*dy);
      if (dist < 120) {
        ctx.beginPath();
        ctx.moveTo(particles[i].x, particles[i].y);
        ctx.lineTo(particles[j].x, particles[j].y);
        ctx.strokeStyle = `rgba(0,212,255,${0.06 * (1 - dist/120)})`;
        ctx.lineWidth = 0.5;
        ctx.stroke();
      }
    }
  }

  // floating 3D cube wireframe
  drawCube(frame);
  frame += 0.008;
  requestAnimationFrame(draw);
}
draw();

function drawCube(t) {
  const cx = W - 120, cy = 200, size = 55;
  const rx = t * 0.7, ry = t;

  const pts3d = [
    [-1,-1,-1],[1,-1,-1],[1,1,-1],[-1,1,-1],
    [-1,-1, 1],[1,-1, 1],[1,1, 1],[-1,1, 1]
  ];

  const cosx = Math.cos(rx), sinx = Math.sin(rx);
  const cosy = Math.cos(ry), siny = Math.sin(ry);

  const project = ([x, y, z]) => {
    let y2 = y * cosx - z * sinx;
    let z2 = y * sinx + z * cosx;
    let x2 = x * cosy + z2 * siny;
    let z3 = -x * siny + z2 * cosy;
    const fov = 4;
    const scale = fov / (fov + z3);
    return [cx + x2 * size * scale, cy + y2 * size * scale];
  };

  const edges = [
    [0,1],[1,2],[2,3],[3,0],
    [4,5],[5,6],[6,7],[7,4],
    [0,4],[1,5],[2,6],[3,7]
  ];

  ctx.strokeStyle = 'rgba(0,212,255,0.25)';
  ctx.lineWidth = 1;
  edges.forEach(([a, b]) => {
    const [ax, ay] = project(pts3d[a]);
    const [bx, by] = project(pts3d[b]);
    ctx.beginPath();
    ctx.moveTo(ax, ay);
    ctx.lineTo(bx, by);
    ctx.stroke();
  });

  pts3d.forEach(p => {
    const [px, py] = project(p);
    ctx.beginPath();
    ctx.arc(px, py, 2, 0, Math.PI * 2);
    ctx.fillStyle = 'rgba(0,212,255,0.6)';
    ctx.fill();
  });
}

// ---- 3D TILT CARD ----
const tiltCard = document.getElementById('tiltCard');
const card3dWrap = document.getElementById('card3d');

card3dWrap && card3dWrap.addEventListener('mousemove', e => {
  const rect = card3dWrap.getBoundingClientRect();
  const x = (e.clientX - rect.left) / rect.width - 0.5;
  const y = (e.clientY - rect.top) / rect.height - 0.5;
  tiltCard.style.transform = `rotateY(${x * 18}deg) rotateX(${-y * 18}deg)`;
});

card3dWrap && card3dWrap.addEventListener('mouseleave', () => {
  tiltCard.style.transform = 'rotateY(0) rotateX(0)';
});

// ---- SCROLL REVEAL ----
const reveals = document.querySelectorAll('.reveal');
const io = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.classList.add('visible');
      // trigger skill bars
      const bar = e.target.querySelector('.skill-bar');
      if (bar) {
        const pct = e.target.dataset.bar || 70;
        setTimeout(() => bar.style.width = pct + '%', 300);
      }
    }
  });
}, { threshold: 0.1 });

reveals.forEach(r => io.observe(r));

// ---- COUNTER ANIMATION ----
function animCount(el, target, duration) {
  let start = 0;
  const step = target / (duration / 16);
  const timer = setInterval(() => {
    start += step;
    if (start >= target) { start = target; clearInterval(timer); }
    el.textContent = Math.floor(start) + '+';
  }, 16);
}

// trigger after page loads
setTimeout(() => {
  const cfEl = document.getElementById('cfCount');
  if (cfEl) animCount(cfEl, 50, 1200);
}, 1500);

</script>
</body>
</html>
