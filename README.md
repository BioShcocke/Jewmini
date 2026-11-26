# Jewmini
Sparx reader jewmini

script:


(() => {
  document.getElementById('sr-bibi-shrine-style')?.remove();
  document.getElementById('sr-bibi-shrine-frame')?.remove();
  document.getElementById('sr-bibi-shrine-font')?.remove();

  // 1. Load Israeli Editorial & UI Fonts (Frank Ruhl Libre & Rubik)
  const fontLink = document.createElement('link');
  fontLink.id = 'sr-bibi-shrine-font';
  fontLink.rel = 'stylesheet';
  fontLink.href = 'https://fonts.googleapis.com/css2?family=Frank+Ruhl+Libre:wght@500;700&family=Rubik:wght@400;600;700&display=swap';
  document.head.appendChild(fontLink);

  // 2. Israeli Flag Background, Typography & Text-Safe Layering
  const style = document.createElement('style');
  style.id = 'sr-bibi-shrine-style';
  style.textContent = `
    html, body, #root {
      background: linear-gradient(
        180deg,
        #0038b8 0%,
        #002885 15%,
        #f8fafc 15.5%,
        #ffffff 84.5%,
        #002885 85%,
        #0038b8 100%
      ) fixed !important;
      font-family: 'Rubik', 'Segoe UI', system-ui, sans-serif !important;
    }

    /* Prevent inner divs from stacking white layers over text */
    #root > div {
      background: transparent !important;
    }
    #root div:not([id^="sr-"]) div:not([id^="sr-"]) {
      background-color: transparent !important;
    }

    /* Soft translucent backing only on major containers so text stays crisp */
    #root > div > div,
    main,
    article,
    [class*="reader"],
    [class*="passage"],
    [class*="story"],
    [class*="card"],
    [class*="panel"] {
      background-color: rgba(255, 255, 255, 0.72) !important;
      border-color: #0038b8 !important;
    }

    /* Keep all text elements clearly legible with high contrast */
    p, span, label, li, h1, h2, h3, h4, blockquote, div:not([id^="sr-"]) {
      color: #0f172a !important;
    }
    p, h1, h2, h3, h4, blockquote, [class*="passage"], [class*="story"] {
      font-family: 'Frank Ruhl Libre', Georgia, serif !important;
    }

    /* Style Sparx buttons in Israeli Blue with crisp white text */
    #root button:not([id^="sr-"]),
    #root [role="button"]:not([id^="sr-"]) {
      background-color: #0038b8 !important;
      color: #ffffff !important;
      border: 2px solid #93c5fd !important;
      box-shadow: 0 2px 8px rgba(0, 56, 184, 0.3) !important;
    }
    #root button:not([id^="sr-"]) *,
    #root [role="button"]:not([id^="sr-"]) * {
      color: #ffffff !important;
      background-color: transparent !important;
    }

    @keyframes sr-candle-flicker {
      0%, 100% { transform: scale(1) rotate(-2deg); opacity: 0.95; }
      50% { transform: scale(1.12) rotate(2deg); opacity: 1; }
    }
    .sr-flame {
      transform-origin: center bottom;
      animation: sr-candle-flicker 0.8s infinite ease-in-out;
    }
  `;
  document.head.appendChild(style);

  // 3. Bottom-Left Shrine Card + Copy & Jewmini Controls
  const shrine = document.createElement('div');
  shrine.id = 'sr-bibi-shrine-frame';
  Object.assign(shrine.style, {
    position: 'fixed',
    bottom: '16px',
    left: '16px',
    zIndex: '999990',
    width: '242px',
    background: 'linear-gradient(145deg, #002885, #0038b8)',
    border: '3px solid #facc15',
    borderRadius: '14px',
    padding: '12px',
    boxShadow: '0 10px 30px rgba(0, 24, 80, 0.55), inset 0 0 15px rgba(250, 204, 21, 0.25)',
    color: '#ffffff',
    fontFamily: "'Frank Ruhl Libre', Georgia, serif",
    textAlign: 'center',
    userSelect: 'none'
  });

  shrine.innerHTML = `
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;">
      <span style="font-size:10px;letter-spacing:1.2px;color:#fde047;font-weight:700;">★ SHRINE OF BIBI ★</span>
      <button id="sr-shrine-reset" style="background:rgba(255,255,255,0.18);color:#fff;border:1px solid rgba(255,255,255,0.4);border-radius:4px;padding:1px 6px;font-size:10px;cursor:pointer;">Reset</button>
    </div>

    <div style="position:relative;background:#ffffff;border-radius:8px;padding:8px;border:2px solid #fde047;margin-bottom:8px;">
      <svg width="100%" height="78" viewBox="0 0 220 120" xmlns="http://www.w3.org/2000/svg">
        <rect width="220" height="120" fill="#ffffff"/>
        <rect y="8" width="220" height="16" fill="#0038b8"/>
        <rect y="96" width="220" height="16" fill="#0038b8"/>
        <polygon points="110,32 132,70 88,70" fill="none" stroke="#0038b8" stroke-width="4.5"/>
        <polygon points="110,84 132,46 88,46" fill="none" stroke="#0038b8" stroke-width="4.5"/>
        <rect x="26" y="68" width="10" height="24" rx="2" fill="#fef08a" stroke="#ca8a04" stroke-width="1"/>
        <ellipse class="sr-flame" cx="31" cy="61" rx="4" ry="7" fill="#f97316"/>
        <rect x="184" y="68" width="10" height="24" rx="2" fill="#fef08a" stroke="#ca8a04" stroke-width="1"/>
        <ellipse class="sr-flame" cx="189" cy="61" rx="4" ry="7" fill="#f97316"/>
      </svg>
      <div style="color:#0038b8;font-weight:700;font-size:13.5px;margin-top:2px;">Benjamin Netanyahu</div>
      <div style="color:#475569;font-size:10px;font-family:'Rubik',sans-serif;">Prime Minister of Israel</div>
    </div>

    <div style="display:flex;gap:6px;margin-bottom:6px;font-family:'Rubik',sans-serif;">
      <button id="sr-shrine-copy" style="flex:1;background:#3b82f6;color:#fff;border:none;border-radius:6px;padding:6px 8px;font-size:11px;font-weight:700;cursor:pointer;">
        Copy Text
      </button>
      <button id="sr-shrine-jewmini" style="flex:1.2;background:#8b5cf6;color:#fff;border:none;border-radius:6px;padding:6px 8px;font-size:11px;font-weight:700;cursor:pointer;">
        Send to Jewmini
      </button>
    </div>

    <a href="https://en.wikipedia.org/wiki/Bibi:_My_Story" target="_blank"
       style="display:block;background:#facc15;color:#0f172a;text-decoration:none;font-family:'Rubik',sans-serif;font-weight:700;font-size:11px;padding:5px 8px;border-radius:6px;">
      Read "Bibi: My Story" ↗
    </a>
  `;

  document.body.appendChild(shrine);

  const getCleanText = () =>
    document.body.innerText
      .split('\n')
      .map(l => l.trim())
      .filter(Boolean)
      .join('\n');

  const copyBtn = document.getElementById('sr-shrine-copy');
  copyBtn.onclick = async () => {
    await navigator.clipboard.writeText(getCleanText());
    copyBtn.textContent = 'Copied!';
    setTimeout(() => (copyBtn.textContent = 'Copy Text'), 1500);
  };

  const jewminiBtn = document.getElementById('sr-shrine-jewmini');
  jewminiBtn.onclick = async () => {
    await navigator.clipboard.writeText(getCleanText());
    jewminiBtn.textContent = 'Sent!';
    window.open('https://gemini.google.com/app', '_blank');
    setTimeout(() => (jewminiBtn.textContent = 'Send to Jewmini'), 1500);
  };

  document.getElementById('sr-shrine-reset').onclick = () => {
    style.remove();
    shrine.remove();
    fontLink.remove();
  };
})();
