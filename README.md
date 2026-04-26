[lift_tracker.html](https://github.com/user-attachments/files/27103174/lift_tracker.html)
# lift-tracker
 "Personal workout tracker"

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#0a0a0a">
<meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
<title>Lift Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Archivo+Black&family=JetBrains+Mono:wght@400;500;700&family=Inter+Tight:wght@400;500;600;700;900&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

  :root {
    --bg: #0a0a0a;
    --surface: #141414;
    --surface-2: #1c1c1c;
    --surface-3: #242424;
    --border: #262626;
    --border-bright: #383838;
    --text: #fafafa;
    --text-dim: #888;
    --text-dimmer: #555;
    --accent: #d4ff3a;
    --push: #ff6b35;
    --pull: #3a9eff;
    --legs: #d4ff3a;
    --recovery: #b86bff;
    --success: #4ade80;
  }

  html, body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Inter Tight', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
    -webkit-font-smoothing: antialiased;
  }

  body { padding-bottom: 60px; }

  .tab-nav {
    position: sticky; top: 0; z-index: 20;
    background: rgba(10, 10, 10, 0.95);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
    padding: 16px 16px 12px;
  }

  .app-title {
    font-family: 'Archivo Black', sans-serif;
    font-size: 22px; letter-spacing: -1px;
    margin-bottom: 14px;
  }

  .app-title span { color: var(--accent); }

  .tabs {
    display: flex; gap: 6px;
    background: var(--surface);
    padding: 4px;
    border-radius: 12px;
    border: 1px solid var(--border);
  }

  .tab {
    flex: 1; padding: 10px 8px;
    background: transparent;
    color: var(--text-dim);
    border: none; border-radius: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; font-weight: 600;
    letter-spacing: 1px; text-transform: uppercase;
    cursor: pointer;
  }

  .tab.active { background: var(--accent); color: var(--bg); }

  .page { display: none; padding: 16px; }
  .page.active { display: block; }

  .day-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
    margin-bottom: 12px;
  }

  .day-header {
    padding: 18px 20px;
    display: flex; align-items: center; justify-content: space-between;
  }

  .day-left { display: flex; align-items: center; gap: 14px; flex: 1; min-width: 0; }

  .day-badge {
    width: 44px; height: 44px;
    border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    background: var(--surface-2);
    border: 1px solid var(--border);
    font-family: 'Archivo Black', sans-serif;
    font-size: 16px;
    flex-shrink: 0;
  }

  .day-card[data-type="push"] .day-badge { border-color: var(--push); color: var(--push); }
  .day-card[data-type="pull"] .day-badge { border-color: var(--pull); color: var(--pull); }
  .day-card[data-type="legs"] .day-badge { border-color: var(--legs); color: var(--legs); }

  .day-title {
    font-size: 16px; font-weight: 700;
    letter-spacing: -0.3px; margin-bottom: 3px;
    display: flex; align-items: center; gap: 8px;
  }

  .day-tag {
    font-family: 'JetBrains Mono', monospace;
    font-size: 9px; padding: 2px 7px;
    border-radius: 4px; letter-spacing: 1px;
    text-transform: uppercase; font-weight: 500;
  }

  .day-card[data-type="push"] .day-tag { background: rgba(255, 107, 53, 0.15); color: var(--push); }
  .day-card[data-type="pull"] .day-tag { background: rgba(58, 158, 255, 0.15); color: var(--pull); }
  .day-card[data-type="legs"] .day-tag { background: rgba(212, 255, 58, 0.15); color: var(--accent); }

  .day-desc {
    font-size: 12px; color: var(--text-dim);
    font-family: 'JetBrains Mono', monospace;
  }

  .start-btn {
    background: var(--accent); color: var(--bg);
    border: none; padding: 10px 16px;
    border-radius: 10px; font-weight: 700; font-size: 12px;
    letter-spacing: 1px; text-transform: uppercase;
    font-family: 'JetBrains Mono', monospace;
    cursor: pointer; flex-shrink: 0;
  }

  .start-btn:active { transform: scale(0.95); }

  .session-header {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px; padding: 20px;
    margin-bottom: 16px;
  }

  .session-header-top {
    display: flex; justify-content: space-between;
    align-items: flex-start; margin-bottom: 14px;
  }

  .session-day-name {
    font-family: 'Archivo Black', sans-serif;
    font-size: 22px; letter-spacing: -0.5px;
    margin-bottom: 4px;
  }

  .session-meta-row {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; color: var(--text-dim);
  }

  .back-btn {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text-dim);
    width: 36px; height: 36px;
    border-radius: 10px; cursor: pointer;
    font-size: 18px; flex-shrink: 0;
  }

  .bw-input-wrap {
    margin-top: 12px; padding-top: 14px;
    border-top: 1px dashed var(--border);
    display: flex; align-items: center; gap: 10px;
  }

  .bw-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; color: var(--text-dim);
    letter-spacing: 1.5px; text-transform: uppercase;
    flex: 1;
  }

  .bw-input {
    width: 80px; background: var(--surface-2);
    border: 1px solid var(--border);
    border-radius: 8px; padding: 8px 10px;
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px; text-align: center; font-weight: 700;
  }

  .bw-input:focus { outline: none; border-color: var(--accent); }

  .exercise-block {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px; margin-bottom: 12px;
    overflow: hidden;
  }

  .exercise-block.complete { border-color: rgba(74, 222, 128, 0.3); }

  .exercise-header { padding: 16px 18px; border-bottom: 1px solid var(--border); }

  .exercise-name {
    font-size: 15px; font-weight: 700;
    letter-spacing: -0.3px; margin-bottom: 8px;
    display: flex; align-items: center; justify-content: space-between; gap: 10px;
  }

  .target-pill {
    background: var(--surface-3); padding: 4px 8px;
    border-radius: 6px; color: var(--accent);
    font-weight: 700;
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
  }

  .suggestion {
    margin-top: 8px; padding: 8px 12px;
    background: rgba(212, 255, 58, 0.06);
    border: 1px solid rgba(212, 255, 58, 0.2);
    border-radius: 8px;
    display: flex; align-items: center; gap: 8px;
    font-size: 12px; color: var(--text-dim);
  }

  .suggestion strong {
    color: var(--accent);
    font-family: 'JetBrains Mono', monospace;
    font-weight: 700;
  }

  .suggestion-icon { width: 14px; height: 14px; flex-shrink: 0; }

  .sets-list { padding: 12px 18px 16px; }

  .set-row {
    display: grid;
    grid-template-columns: 36px 1fr 1fr 36px 36px;
    gap: 8px; align-items: center;
    margin-bottom: 8px;
  }

  .set-row.bw-row {
    grid-template-columns: 36px 1fr 36px 36px;
  }

  .set-num {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px; color: var(--text-dim);
    font-weight: 700; height: 36px;
    background: var(--surface-2);
    border: 1px solid var(--border);
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
  }

  .set-row.logged .set-num {
    background: var(--accent); color: var(--bg);
    border-color: var(--accent);
  }

  .set-row.extra .set-num {
    background: rgba(184, 107, 255, 0.15);
    border-color: rgba(184, 107, 255, 0.4);
    color: var(--recovery);
  }

  .set-row.extra.logged .set-num {
    background: var(--accent); border-color: var(--accent);
    color: var(--bg);
  }

  .set-input-wrap { position: relative; }

  .set-input {
    width: 100%;
    background: var(--surface-2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 9px 10px;
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    font-size: 14px; font-weight: 700;
    text-align: center;
  }

  .set-input:focus {
    outline: none; border-color: var(--accent);
    background: var(--surface-3);
  }

  .set-input::placeholder { color: var(--text-dimmer); font-weight: 400; }

  .set-input-label {
    position: absolute; top: -7px; left: 8px;
    background: var(--surface); padding: 0 4px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 8px; color: var(--text-dimmer);
    letter-spacing: 1px; text-transform: uppercase;
  }

  .icon-btn {
    width: 36px; height: 36px;
    border-radius: 8px;
    border: 1px solid var(--border);
    background: var(--surface-2);
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    padding: 0;
  }

  .icon-btn:active { transform: scale(0.92); }
  .icon-btn.logged { background: var(--accent); border-color: var(--accent); }

  .icon-btn svg {
    width: 16px; height: 16px;
    stroke: var(--text-dimmer);
    stroke-width: 2; fill: none;
    stroke-linecap: round; stroke-linejoin: round;
  }

  .icon-btn.logged svg { stroke: var(--bg); stroke-width: 3; }
  .icon-btn.has-note svg { stroke: var(--accent); }

  .note-row { margin-bottom: 8px; margin-left: 44px; }

  .note-input {
    width: 100%;
    background: var(--surface-2);
    border: 1px solid rgba(212, 255, 58, 0.2);
    border-radius: 8px; padding: 8px 10px;
    color: var(--text);
    font-family: 'Inter Tight', sans-serif;
    font-size: 12px; resize: none;
    min-height: 32px; font-style: italic;
  }

  .note-input::placeholder { color: var(--text-dimmer); font-style: italic; }
  .note-input:focus { outline: none; border-color: var(--accent); }

  .add-set-btn {
    margin-top: 4px; width: 100%;
    background: transparent; color: var(--text-dim);
    border: 1px dashed var(--border);
    border-radius: 8px; padding: 10px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; letter-spacing: 1px;
    text-transform: uppercase; cursor: pointer;
    font-weight: 600;
  }

  .add-set-btn:active { background: var(--surface-2); color: var(--text); }

  .last-time-row {
    margin-top: 10px; padding: 8px 10px;
    background: var(--surface-2); border-radius: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; color: var(--text-dimmer);
    display: flex; justify-content: space-between; align-items: center;
  }

  .last-time-row strong { color: var(--text-dim); }

  .finish-btn {
    width: 100%; background: var(--accent); color: var(--bg);
    border: none; padding: 16px;
    border-radius: 14px;
    font-family: 'Archivo Black', sans-serif;
    font-size: 14px; letter-spacing: 1.5px;
    text-transform: uppercase; cursor: pointer;
    margin-top: 8px;
  }

  .progress-summary, .day-freq-card, .chart-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px; padding: 20px;
    margin-bottom: 16px;
  }

  .progress-title {
    font-family: 'Archivo Black', sans-serif;
    font-size: 18px; margin-bottom: 16px;
    letter-spacing: -0.3px;
  }

  .stat-grid {
    display: grid; grid-template-columns: 1fr 1fr; gap: 12px;
  }

  .stat-box {
    background: var(--surface-2);
    border-radius: 12px; padding: 14px;
    border: 1px solid var(--border);
  }

  .stat-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 9px; color: var(--text-dim);
    letter-spacing: 1.5px; text-transform: uppercase;
    margin-bottom: 6px;
  }

  .stat-value {
    font-family: 'Archivo Black', sans-serif;
    font-size: 26px; line-height: 1;
    color: var(--accent);
  }

  .stat-sub {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; color: var(--text-dim);
    margin-top: 4px;
  }

  .freq-row {
    display: grid; grid-template-columns: auto 1fr auto;
    gap: 12px; align-items: center;
    padding: 10px 0;
    border-bottom: 1px dashed var(--border);
  }

  .freq-row:last-child { border-bottom: none; }

  .freq-badge {
    width: 32px; height: 32px;
    border-radius: 8px; background: var(--surface-2);
    border: 1px solid var(--border);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Archivo Black', sans-serif;
    font-size: 13px;
  }

  .freq-badge.push { border-color: var(--push); color: var(--push); }
  .freq-badge.pull { border-color: var(--pull); color: var(--pull); }
  .freq-badge.legs { border-color: var(--legs); color: var(--legs); }

  .freq-info { font-size: 13px; }
  .freq-name { font-weight: 700; margin-bottom: 2px; }
  .freq-sub { font-size: 11px; color: var(--text-dim); font-family: 'JetBrains Mono', monospace; }

  .freq-count {
    font-family: 'Archivo Black', sans-serif;
    font-size: 22px; color: var(--accent);
  }

  .chart-title {
    font-family: 'Archivo Black', sans-serif;
    font-size: 14px;
  }

  .chart-subtitle {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; color: var(--text-dim);
    letter-spacing: 0.5px; text-transform: uppercase;
    margin-bottom: 14px;
  }

  .chart-select {
    background: var(--surface-2);
    border: 1px solid var(--border);
    color: var(--text);
    border-radius: 8px;
    padding: 8px 10px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; font-weight: 600;
    width: 100%; margin-bottom: 14px;
  }

  .chart-canvas-wrap {
    position: relative; height: 200px; width: 100%;
  }

  .chart-empty {
    display: flex; align-items: center; justify-content: center;
    height: 100%; color: var(--text-dimmer);
    font-size: 12px; text-align: center;
  }

  .chart-legend {
    display: flex; gap: 16px; justify-content: center;
    margin-top: 12px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; color: var(--text-dim);
    text-transform: uppercase; letter-spacing: 1px;
  }

  .legend-dot {
    display: inline-block; width: 8px; height: 8px;
    border-radius: 50%; margin-right: 5px;
    vertical-align: middle;
  }

  .exercise-history-block {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    margin-bottom: 10px;
    overflow: hidden;
  }

  .exercise-history-header {
    padding: 14px 16px; cursor: pointer;
    display: flex; justify-content: space-between; align-items: center;
  }

  .ex-history-name {
    font-size: 14px; font-weight: 700;
    letter-spacing: -0.2px;
  }

  .ex-history-meta {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; color: var(--text-dim);
    margin-top: 2px;
  }

  .trend-pill {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; padding: 4px 8px;
    border-radius: 6px; font-weight: 700;
    letter-spacing: 0.5px;
  }

  .trend-up { background: rgba(74, 222, 128, 0.15); color: var(--success); }
  .trend-flat { background: rgba(136, 136, 136, 0.15); color: var(--text-dim); }
  .trend-new { background: rgba(212, 255, 58, 0.15); color: var(--accent); }

  .history-detail {
    display: none;
    border-top: 1px solid var(--border);
    padding: 14px 16px; background: var(--surface-2);
  }

  .exercise-history-block.expanded .history-detail { display: block; }

  .history-session {
    padding: 10px 0;
    border-bottom: 1px dashed var(--border);
  }

  .history-session:last-child { border-bottom: none; }

  .history-date {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; color: var(--text-dim);
    letter-spacing: 1px; text-transform: uppercase;
    margin-bottom: 6px;
  }

  .history-sets {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: var(--text);
    line-height: 1.6;
  }

  .history-set-tag {
    display: inline-block;
    background: var(--surface-3);
    padding: 3px 7px; border-radius: 5px;
    margin-right: 4px; margin-bottom: 4px;
    font-size: 11px;
  }

  .history-note {
    font-style: italic;
    color: var(--text-dim);
    font-size: 11px;
    margin-top: 4px; margin-bottom: 4px;
    padding: 2px 6px;
    border-left: 2px solid var(--accent);
  }

  .empty-state {
    text-align: center; padding: 60px 20px;
    color: var(--text-dim);
  }

  .empty-state .icon { font-size: 48px; margin-bottom: 16px; opacity: 0.3; }
  .empty-state p { font-size: 14px; line-height: 1.5; }

  .progress-section-title {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; margin: 8px 4px 12px;
    color: var(--text-dim); letter-spacing: 1.5px;
    text-transform: uppercase; font-weight: 600;
  }

  .modal-bg {
    position: fixed; inset: 0;
    background: rgba(0, 0, 0, 0.85);
    z-index: 100; display: none;
    align-items: center; justify-content: center;
    padding: 20px;
  }

  .modal-bg.active { display: flex; }

  .modal {
    background: var(--surface);
    border: 1px solid var(--border-bright);
    border-radius: 18px; padding: 24px;
    max-width: 400px; width: 100%;
    text-align: center;
  }

  .modal-icon { font-size: 36px; margin-bottom: 12px; }

  .modal-title {
    font-family: 'Archivo Black', sans-serif;
    font-size: 18px; margin-bottom: 8px;
  }

  .modal-text {
    color: var(--text-dim); font-size: 14px;
    margin-bottom: 20px; line-height: 1.5;
  }

  .modal-actions { display: flex; gap: 10px; }

  .modal-btn {
    flex: 1; padding: 12px;
    border-radius: 10px;
    border: 1px solid var(--border);
    background: var(--surface-2);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; letter-spacing: 1px;
    text-transform: uppercase; font-weight: 700;
    cursor: pointer;
  }

  .modal-btn.primary {
    background: var(--accent); color: var(--bg);
    border-color: var(--accent);
  }

  .rules-card, .data-section {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px; padding: 18px;
  }

  .rules-card { margin-top: 16px; }
  .data-section { margin-top: 16px; }

  .rules-card-title, .data-section-title {
    font-family: 'Archivo Black', sans-serif;
    font-size: 12px; letter-spacing: 1.5px;
    margin-bottom: 12px; text-transform: uppercase;
  }

  .rules-card-title { color: var(--accent); }
  .data-section-title { color: var(--text-dim); }

  .rules-card .rule {
    display: flex; gap: 10px;
    padding: 8px 0;
    font-size: 12px; line-height: 1.5;
    color: var(--text-dim);
  }

  .rules-card .rule-num {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; color: var(--accent);
    font-weight: 700; flex-shrink: 0;
  }

  .rules-card .rule strong { color: var(--text); }

  .data-buttons {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 10px; margin-bottom: 12px;
  }

  .data-btn {
    background: var(--surface-2);
    border: 1px solid var(--border);
    color: var(--text);
    padding: 12px 10px;
    border-radius: 10px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; font-weight: 700;
    letter-spacing: 1px; text-transform: uppercase;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    gap: 6px;
  }

  .data-btn svg {
    width: 14px; height: 14px;
    stroke: currentColor; stroke-width: 2;
    fill: none; stroke-linecap: round; stroke-linejoin: round;
  }

  .data-helper {
    font-size: 11px; color: var(--text-dimmer);
    line-height: 1.5; text-align: center;
    padding: 8px 4px 0;
    border-top: 1px dashed var(--border);
  }

  .reset-section { margin-top: 16px; text-align: center; }

  .reset-link {
    background: transparent; border: none;
    color: var(--text-dimmer);
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px; letter-spacing: 1.5px;
    text-transform: uppercase; cursor: pointer;
    padding: 8px;
  }

  .file-input-hidden { display: none; }

  .toast {
    position: fixed; top: 20px; left: 50%;
    transform: translateX(-50%) translateY(-100px);
    background: var(--surface);
    border: 1px solid var(--border-bright);
    padding: 12px 18px;
    border-radius: 10px;
    color: var(--text);
    font-size: 13px;
    z-index: 200;
    transition: transform 0.3s;
    box-shadow: 0 4px 20px rgba(0,0,0,0.5);
  }

  .toast.show { transform: translateX(-50%) translateY(0); }
  .toast.success { border-color: var(--success); }
</style>
</head>
<body>

<div class="tab-nav">
  <div class="app-title">Lift <span>Tracker</span></div>
  <div class="tabs">
    <button class="tab active" onclick="switchTab('plan')">Plan</button>
    <button class="tab" onclick="switchTab('progress')">Progress</button>
  </div>
</div>

<div id="planPage" class="page active"></div>
<div id="sessionPage" class="page"></div>
<div id="progressPage" class="page"></div>

<div class="modal-bg" id="finishModal">
  <div class="modal">
    <div class="modal-icon">💪</div>
    <div class="modal-title">Finish Session?</div>
    <div class="modal-text" id="finishModalText">You haven't logged all sets yet. Save anyway?</div>
    <div class="modal-actions">
      <button class="modal-btn" onclick="closeModal()">Cancel</button>
      <button class="modal-btn primary" onclick="confirmFinishSession()">Save Session</button>
    </div>
  </div>
</div>

<div class="modal-bg" id="resetModal">
  <div class="modal">
    <div class="modal-icon">⚠️</div>
    <div class="modal-title">Reset All Data?</div>
    <div class="modal-text">This will delete all your logged workouts and history. This cannot be undone.</div>
    <div class="modal-actions">
      <button class="modal-btn" onclick="closeResetModal()">Cancel</button>
      <button class="modal-btn primary" onclick="confirmReset()">Delete All</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const program = {
  days: [
    {
      id: 'upperA', day: '1', title: 'Upper A', type: 'push', tag: 'Push',
      desc: 'Push-focused · ~62 min',
      exercises: [
        { id: 'incline_bench', name: 'Incline Bench Press', sets: 3, repRange: '6-10', loadType: 'weight' },
        { id: 'db_press', name: 'Dumbbell Press', sets: 2, repRange: '6-10', loadType: 'weight' },
        { id: 'pull_ups', name: 'Pull-Ups', sets: 4, repRange: '6-10', loadType: 'bw' },
        { id: 't_bar', name: 'T-Bar Row', sets: 2, repRange: '8-12', loadType: 'weight' },
        { id: 'shoulder_db_press', name: 'Shoulder DB Press', sets: 2, repRange: '6-10', loadType: 'weight' },
        { id: 'lateral_raises_a', name: 'Lateral Raises', sets: 3, repRange: '10-15', loadType: 'weight' },
        { id: 'barbell_curl_a', name: 'Barbell Bicep Curl', sets: 2, repRange: '8-12', loadType: 'weight' },
        { id: 'tricep_ext_a', name: 'Tricep Extensions', sets: 2, repRange: '10-15', loadType: 'weight' }
      ]
    },
    {
      id: 'lowerA', day: '2', title: 'Lower A', type: 'legs', tag: 'Quads',
      desc: 'Quad-focused · ~60 min',
      exercises: [
        { id: 'squat', name: 'Squat', sets: 3, repRange: '6-10', loadType: 'weight' },
        { id: 'bulgarian_split', name: 'Bulgarian Split Squat', sets: 2, repRange: '6-10', loadType: 'weight' },
        { id: 'leg_ext_a', name: 'Leg Extensions', sets: 2, repRange: '10-15', loadType: 'weight' },
        { id: 'leg_curls_a', name: 'Leg Curls', sets: 3, repRange: '10-15', loadType: 'weight' },
        { id: 'standing_calf', name: 'Standing Calf Raises', sets: 2, repRange: '10-15', loadType: 'weight' },
        { id: 'hanging_leg_raise', name: 'Hanging Leg Raises', sets: 3, repRange: '8-12', loadType: 'bw' },
        { id: 'cable_crunch', name: 'Cable Crunches', sets: 3, repRange: '10-15', loadType: 'weight' }
      ]
    },
    {
      id: 'upperB', day: '3', title: 'Upper B', type: 'pull', tag: 'Pull',
      desc: 'Pull-focused · ~65 min',
      exercises: [
        { id: 'lat_pulldown', name: 'Lat Pulldown (narrow)', sets: 4, repRange: '6-10', loadType: 'weight' },
        { id: 'machine_t', name: "Machine T's (chest-supported)", sets: 3, repRange: '8-12', loadType: 'weight' },
        { id: 'db_press_b', name: 'Dumbbell Press', sets: 2, repRange: '6-10', loadType: 'weight' },
        { id: 'cable_crossover', name: 'Cable Crossover', sets: 2, repRange: '10-15', loadType: 'weight' },
        { id: 'face_pulls', name: 'Face Pulls', sets: 2, repRange: '10-15', loadType: 'weight' },
        { id: 'lateral_raises_b', name: 'Lateral Raises', sets: 2, repRange: '10-15', loadType: 'weight' },
        { id: 'barbell_curl_b', name: 'Barbell Bicep Curl', sets: 3, repRange: '8-12', loadType: 'weight' },
        { id: 'overhead_tri', name: 'Overhead Tricep Extension', sets: 2, repRange: '10-15', loadType: 'weight' }
      ]
    },
    {
      id: 'lowerB', day: '4', title: 'Lower B', type: 'legs', tag: 'Posterior',
      desc: 'Posterior chain · ~60 min',
      exercises: [
        { id: 'seated_leg_curl', name: 'Seated Leg Curl', sets: 3, repRange: '8-12', loadType: 'weight' },
        { id: 'hip_thrust', name: 'Hip Thrust', sets: 3, repRange: '6-10', loadType: 'weight' },
        { id: 'back_extension', name: '45° Back Extension', sets: 3, repRange: '10-15', loadType: 'weight' },
        { id: 'leg_ext_b', name: 'Leg Extensions', sets: 2, repRange: '10-15', loadType: 'weight' },
        { id: 'seated_calf', name: 'Seated Calf Raises', sets: 2, repRange: '10-15', loadType: 'weight' },
        { id: 'ab_wheel', name: 'Ab Wheel Rollouts', sets: 3, repRange: '6-10', loadType: 'bw' },
        { id: 'cable_woodchopper', name: 'Cable Woodchoppers', sets: 3, repRange: '10-15/side', loadType: 'weight' }
      ]
    }
  ]
};

const state = {
  history: [],
  currentSession: null,
  chartExerciseId: null
};

function saveData() {
  try {
    localStorage.setItem('liftTrackerData', JSON.stringify({ history: state.history }));
  } catch (e) { console.warn('Save failed', e); }
}

function loadData() {
  try {
    const raw = localStorage.getItem('liftTrackerData');
    if (!raw) return;
    const data = JSON.parse(raw);
    if (data && data.history) {
      state.history = data.history;
      state.history.forEach(s => {
        if (s.exercises) {
          s.exercises.forEach(ex => {
            if (ex.sets) {
              ex.sets.forEach(set => {
                if (set.note === undefined) set.note = '';
              });
            }
          });
        }
      });
    }
  } catch (e) { console.warn('Load failed', e); }
}

function switchTab(tab) {
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  if (tab === 'plan') {
    document.querySelectorAll('.tab')[0].classList.add('active');
    document.getElementById('planPage').classList.add('active');
    renderPlan();
  } else {
    document.querySelectorAll('.tab')[1].classList.add('active');
    document.getElementById('progressPage').classList.add('active');
    renderProgress();
  }
}

function renderPlan() {
  const el = document.getElementById('planPage');
  let html = '';
  program.days.forEach(d => {
    const totalSets = d.exercises.reduce((s, e) => s + e.sets, 0);
    html += `
      <div class="day-card" data-type="${d.type}">
        <div class="day-header">
          <div class="day-left">
            <div class="day-badge">${d.day}</div>
            <div>
              <div class="day-title">${d.title} <span class="day-tag">${d.tag}</span></div>
              <div class="day-desc">${d.exercises.length} ex · ${totalSets} sets · ${d.desc.split(' · ')[1]}</div>
            </div>
          </div>
          <button class="start-btn" onclick="startSession('${d.id}');">Start</button>
        </div>
      </div>
    `;
  });

  html += `
    <div class="rules-card">
      <div class="rules-card-title">Core Rules</div>
      <div class="rule"><span class="rule-num">01</span><span><strong>Last set to failure.</strong> Earlier sets stop 1-2 reps short.</span></div>
      <div class="rule"><span class="rule-num">02</span><span><strong>Hit top of rep range</strong> on all sets → add weight next session.</span></div>
      <div class="rule"><span class="rule-num">03</span><span><strong>48+ hours</strong> between same-pattern days.</span></div>
      <div class="rule"><span class="rule-num">04</span><span><strong>Compounds = tension,</strong> isolations = burn. Don't chase burn on squats.</span></div>
    </div>
    <div class="data-section">
      <div class="data-section-title">Backup & Restore</div>
      <div class="data-buttons">
        <button class="data-btn" onclick="exportData()">
          <svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
          Export
        </button>
        <button class="data-btn" onclick="document.getElementById('importInput').click()">
          <svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
          Import
        </button>
      </div>
      <input type="file" id="importInput" class="file-input-hidden" accept=".json,application/json" onchange="importData(event)">
      <div class="data-helper">Export saves your history as a backup file. Import restores from a backup.</div>
    </div>
    <div class="reset-section">
      <button class="reset-link" onclick="openResetModal()">↻ Reset All Data</button>
    </div>
  `;
  el.innerHTML = html;
}

function startSession(dayId) {
  const day = program.days.find(d => d.id === dayId);
  state.currentSession = {
    dayId,
    dayTitle: day.title,
    tag: day.tag,
    type: day.type,
    bw: '',
    startedAt: new Date().toISOString(),
    exercises: day.exercises.map(ex => ({
      id: ex.id,
      name: ex.name,
      loadType: ex.loadType,
      repRange: ex.repRange,
      targetSets: ex.sets,
      sets: Array.from({length: ex.sets}, () => ({ weight: '', reps: '', logged: false, note: '', noteOpen: false }))
    }))
  };

  const lastSession = [...state.history].reverse()[0];
  if (lastSession && lastSession.bw) state.currentSession.bw = lastSession.bw;

  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.getElementById('sessionPage').classList.add('active');
  renderSession();
}

function getLastPerformance(exerciseId) {
  for (let i = state.history.length - 1; i >= 0; i--) {
    const sess = state.history[i];
    const ex = sess.exercises.find(e => e.id === exerciseId);
    if (ex && ex.sets.some(s => s.weight !== '' || s.reps !== '')) {
      return { session: sess, exercise: ex };
    }
  }
  return null;
}

function getSuggestion(exercise) {
  const last = getLastPerformance(exercise.id);
  if (!last) return null;
  const validSets = last.exercise.sets.filter(s => s.reps !== '' && (s.weight !== '' || exercise.loadType === 'bw'));
  if (validSets.length === 0) return null;

  const repRangeMatch = exercise.repRange.match(/(\d+)-(\d+)/);
  const topRep = repRangeMatch ? parseInt(repRangeMatch[2]) : 10;

  const sortedByWeight = [...validSets].sort((a, b) => parseFloat(b.weight || 0) - parseFloat(a.weight || 0));
  const topSet = sortedByWeight[0];

  if (exercise.loadType === 'bw') {
    const bestReps = Math.max(...validSets.map(s => parseInt(s.reps) || 0));
    if (bestReps >= topRep) return { msg: `Try for ${topRep + 1}+ reps` };
    return { msg: `Match ${bestReps} reps` };
  }

  const lastWeight = parseFloat(topSet.weight);
  const lastTopReps = parseInt(topSet.reps);
  const allHitTop = validSets.every(s => parseInt(s.reps) >= topRep);

  if (allHitTop && validSets.length >= exercise.targetSets - 1) {
    return { msg: `Bump up: ${lastWeight} → ${lastWeight + 5} lb` };
  }
  return { msg: `Last: ${lastWeight} × ${lastTopReps}` };
}

function getLastSetsString(exerciseId) {
  const last = getLastPerformance(exerciseId);
  if (!last) return null;
  const dateStr = formatDate(last.session.date);
  const setsStr = last.exercise.sets
    .filter(s => s.reps !== '')
    .map(s => s.weight ? `${s.weight}×${s.reps}` : `${s.reps}`)
    .join(' · ');
  return { date: dateStr, sets: setsStr };
}

function renderSession() {
  const sess = state.currentSession;
  const el = document.getElementById('sessionPage');

  let html = `
    <div class="session-header">
      <div class="session-header-top">
        <div>
          <div class="session-day-name">${sess.dayTitle}</div>
          <div class="session-meta-row">${sess.tag.toUpperCase()} · ${sess.exercises.length} EXERCISES</div>
        </div>
        <button class="back-btn" onclick="abandonSession()">←</button>
      </div>
      <div class="bw-input-wrap">
        <span class="bw-label">Bodyweight (lb)</span>
        <input type="number" inputmode="decimal" class="bw-input" id="bwInput" value="${sess.bw}"
               placeholder="196" onchange="updateBw(this.value)">
      </div>
    </div>
  `;

  sess.exercises.forEach((ex, exIdx) => {
    const allLogged = ex.sets.every(s => s.logged);
    const suggestion = getSuggestion(ex);
    const lastInfo = getLastSetsString(ex.id);

    html += `
      <div class="exercise-block ${allLogged && ex.sets.length >= ex.targetSets ? 'complete' : ''}">
        <div class="exercise-header">
          <div class="exercise-name">
            <span>${ex.name}</span>
            <span class="target-pill">${ex.targetSets}×${ex.repRange}</span>
          </div>
          ${suggestion ? `
            <div class="suggestion">
              <svg class="suggestion-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
              <span>Suggestion: <strong>${suggestion.msg}</strong></span>
            </div>
          ` : `
            <div class="suggestion" style="background: rgba(212, 255, 58, 0.04); color: var(--text-dimmer);">
              <svg class="suggestion-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
              <span>First time — start at a comfortable weight</span>
            </div>
          `}
        </div>
        <div class="sets-list">
    `;

    ex.sets.forEach((set, setIdx) => {
      const isExtra = setIdx >= ex.targetSets;
      const rowCls = `set-row ${set.logged ? 'logged' : ''} ${ex.loadType === 'bw' ? 'bw-row' : ''} ${isExtra ? 'extra' : ''}`;
      const setNumLabel = isExtra ? `+${setIdx - ex.targetSets + 1}` : (setIdx + 1);
      const noteBtnCls = `icon-btn ${set.note ? 'has-note' : ''}`;

      html += `
        <div class="${rowCls}" data-ex="${exIdx}" data-set="${setIdx}">
          <div class="set-num">${setNumLabel}</div>
          ${ex.loadType === 'bw' ? `
            <div class="set-input-wrap">
              <span class="set-input-label">Reps</span>
              <input type="number" inputmode="numeric" class="set-input"
                     value="${set.reps}" placeholder="—"
                     onchange="updateSet(${exIdx}, ${setIdx}, 'reps', this.value)">
            </div>
          ` : `
            <div class="set-input-wrap">
              <span class="set-input-label">Lb</span>
              <input type="number" inputmode="decimal" class="set-input"
                     value="${set.weight}" placeholder="—"
                     onchange="updateSet(${exIdx}, ${setIdx}, 'weight', this.value)">
            </div>
            <div class="set-input-wrap">
              <span class="set-input-label">Reps</span>
              <input type="number" inputmode="numeric" class="set-input"
                     value="${set.reps}" placeholder="—"
                     onchange="updateSet(${exIdx}, ${setIdx}, 'reps', this.value)">
            </div>
          `}
          <button class="${noteBtnCls}" onclick="toggleNote(${exIdx}, ${setIdx})" title="Note">
            <svg viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="9" y1="13" x2="15" y2="13"/><line x1="9" y1="17" x2="13" y2="17"/></svg>
          </button>
          <button class="icon-btn ${set.logged ? 'logged' : ''}" onclick="toggleSetLogged(${exIdx}, ${setIdx})">
            <svg viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"/></svg>
          </button>
        </div>
        ${(set.noteOpen || set.note) ? `
          <div class="note-row">
            <textarea class="note-input" placeholder="Note for set ${setNumLabel}..."
                      oninput="updateSet(${exIdx}, ${setIdx}, 'note', this.value)"
                      rows="1">${set.note}</textarea>
          </div>
        ` : ''}
      `;
    });

    html += `<button class="add-set-btn" onclick="addSet(${exIdx})">+ Add Set</button>`;

    if (lastInfo) {
      html += `
        <div class="last-time-row">
          <span><strong>Last (${lastInfo.date}):</strong></span>
          <span>${lastInfo.sets}</span>
        </div>
      `;
    }
    html += `</div></div>`;
  });

  html += `<button class="finish-btn" onclick="finishSession()">Finish & Save Session</button>`;
  el.innerHTML = html;
}

function updateBw(val) { state.currentSession.bw = val; }

function updateSet(exIdx, setIdx, field, value) {
  state.currentSession.exercises[exIdx].sets[setIdx][field] = value;
  if (field === 'note') return;

  const set = state.currentSession.exercises[exIdx].sets[setIdx];
  const ex = state.currentSession.exercises[exIdx];
  const hasWeight = ex.loadType === 'bw' || (set.weight !== '' && set.weight !== null);
  const hasReps = set.reps !== '' && set.reps !== null;

  if (hasWeight && hasReps && !set.logged) {
    set.logged = true;
    const row = document.querySelector(`.set-row[data-ex="${exIdx}"][data-set="${setIdx}"]`);
    if (row) {
      row.classList.add('logged');
      const checkBtn = row.querySelectorAll('.icon-btn')[1];
      if (checkBtn) checkBtn.classList.add('logged');
    }
  }
}

function toggleNote(exIdx, setIdx) {
  const set = state.currentSession.exercises[exIdx].sets[setIdx];
  set.noteOpen = !set.noteOpen;
  renderSession();
}

function toggleSetLogged(exIdx, setIdx) {
  const set = state.currentSession.exercises[exIdx].sets[setIdx];
  set.logged = !set.logged;
  renderSession();
}

function addSet(exIdx) {
  const ex = state.currentSession.exercises[exIdx];
  ex.sets.push({ weight: '', reps: '', logged: false, note: '', noteOpen: false });
  renderSession();
}

function abandonSession() {
  if (confirm('Discard this session?')) {
    state.currentSession = null;
    switchTab('plan');
  }
}

function finishSession() {
  const sess = state.currentSession;
  const totalSets = sess.exercises.reduce((s, e) => s + e.targetSets, 0);
  const loggedSets = sess.exercises.reduce((s, e) => s + e.sets.filter(set => set.logged).length, 0);

  if (loggedSets === 0) { showToast('No sets logged yet'); return; }

  if (loggedSets < totalSets) {
    document.getElementById('finishModalText').textContent =
      `${loggedSets} of ${totalSets} sets logged. Save anyway?`;
    document.getElementById('finishModal').classList.add('active');
  } else {
    confirmFinishSession();
  }
}

function confirmFinishSession() {
  closeModal();
  const sess = state.currentSession;
  const record = {
    date: new Date().toISOString(),
    dayId: sess.dayId,
    dayTitle: sess.dayTitle,
    tag: sess.tag,
    bw: sess.bw,
    exercises: sess.exercises.map(ex => ({
      id: ex.id,
      name: ex.name,
      loadType: ex.loadType,
      repRange: ex.repRange,
      sets: ex.sets.filter(s => s.logged).map(s => ({
        weight: s.weight,
        reps: s.reps,
        note: s.note || ''
      }))
    })).filter(ex => ex.sets.length > 0)
  };

  state.history.push(record);
  saveData();
  state.currentSession = null;
  showToast('Session saved 💪', 'success');
  switchTab('progress');
}

function closeModal() {
  document.getElementById('finishModal').classList.remove('active');
}

function renderProgress() {
  const el = document.getElementById('progressPage');

  if (state.history.length === 0) {
    el.innerHTML = `
      <div class="empty-state">
        <div class="icon">📊</div>
        <p><strong>No workouts logged yet.</strong><br>Complete a session to see your progress here.</p>
      </div>
    `;
    return;
  }

  const totalSessions = state.history.length;
  const totalSets = state.history.reduce((s, h) =>
    s + h.exercises.reduce((es, ex) => es + ex.sets.length, 0), 0);

  const sessionsByDay = {};
  program.days.forEach(d => sessionsByDay[d.id] = 0);
  state.history.forEach(h => {
    if (sessionsByDay[h.dayId] !== undefined) sessionsByDay[h.dayId]++;
  });

  const mostRecent = [...state.history].reverse().find(h => h.bw);
  const currentBw = mostRecent?.bw || '—';

  const exerciseStats = {};
  state.history.forEach(sess => {
    sess.exercises.forEach(ex => {
      if (!exerciseStats[ex.id]) {
        exerciseStats[ex.id] = { name: ex.name, loadType: ex.loadType, sessions: [] };
      }
      exerciseStats[ex.id].sessions.push({
        date: sess.date,
        sets: ex.sets,
        dayTitle: sess.dayTitle
      });
    });
  });

  if (!state.chartExerciseId || !exerciseStats[state.chartExerciseId]) {
    const sorted = Object.entries(exerciseStats).sort((a, b) => b[1].sessions.length - a[1].sessions.length);
    if (sorted.length > 0) state.chartExerciseId = sorted[0][0];
  }

  let html = `
    <div class="progress-summary">
      <div class="progress-title">Your Stats</div>
      <div class="stat-grid">
        <div class="stat-box">
          <div class="stat-label">Sessions</div>
          <div class="stat-value">${totalSessions}</div>
          <div class="stat-sub">All-time</div>
        </div>
        <div class="stat-box">
          <div class="stat-label">Sets Logged</div>
          <div class="stat-value">${totalSets}</div>
          <div class="stat-sub">All-time</div>
        </div>
        <div class="stat-box">
          <div class="stat-label">Current BW</div>
          <div class="stat-value">${currentBw}</div>
          <div class="stat-sub">lb</div>
        </div>
        <div class="stat-box">
          <div class="stat-label">Most Trained</div>
          <div class="stat-value" style="font-size: 16px; font-family: 'Inter Tight'; font-weight: 900;">
            ${getMostTrainedDay(sessionsByDay)}
          </div>
          <div class="stat-sub">${Math.max(...Object.values(sessionsByDay), 0)} sessions</div>
        </div>
      </div>
    </div>

    <div class="day-freq-card">
      <div class="progress-title">By Day</div>
  `;

  program.days.forEach(d => {
    const count = sessionsByDay[d.id] || 0;
    html += `
      <div class="freq-row">
        <div class="freq-badge ${d.type}">${d.day}</div>
        <div class="freq-info">
          <div class="freq-name">${d.title} <span class="freq-sub" style="margin-left: 6px;">${d.tag.toUpperCase()}</span></div>
          <div class="freq-sub">${count === 0 ? 'Not yet logged' : count === 1 ? '1 session' : `${count} sessions`}</div>
        </div>
        <div class="freq-count">${count}</div>
      </div>
    `;
  });
  html += `</div>`;

  const exerciseOptions = Object.entries(exerciseStats)
    .sort((a, b) => b[1].sessions.length - a[1].sessions.length)
    .map(([id, data]) => `<option value="${id}" ${id === state.chartExerciseId ? 'selected' : ''}>${data.name}</option>`)
    .join('');

  html += `
    <div class="chart-card">
      <div class="chart-title">Exercise Progress</div>
      <div class="chart-subtitle">Top weight & total volume per session</div>
      <select class="chart-select" onchange="updateChartExercise(this.value)">
        ${exerciseOptions}
      </select>
      <div class="chart-canvas-wrap"><canvas id="exerciseChart"></canvas></div>
      <div class="chart-legend" id="exerciseChartLegend"></div>
    </div>

    <div class="chart-card">
      <div class="chart-title">Total Sets Per Session</div>
      <div class="chart-subtitle">All exercises combined</div>
      <div class="chart-canvas-wrap"><canvas id="volumeChart"></canvas></div>
    </div>
  `;

  const bwData = state.history.filter(h => h.bw && parseFloat(h.bw) > 0);
  if (bwData.length > 0) {
    html += `
      <div class="chart-card">
        <div class="chart-title">Bodyweight</div>
        <div class="chart-subtitle">Tracked across sessions</div>
        <div class="chart-canvas-wrap"><canvas id="bwChart"></canvas></div>
      </div>
    `;
  }

  html += `<div class="progress-section-title">Exercise History</div>`;

  const sortedExercises = Object.entries(exerciseStats)
    .sort((a, b) => b[1].sessions.length - a[1].sessions.length);

  sortedExercises.forEach(([id, data]) => {
    const trend = analyzeTrend(data);
    html += `
      <div class="exercise-history-block">
        <div class="exercise-history-header" onclick="toggleHistoryBlock(this)">
          <div>
            <div class="ex-history-name">${data.name}</div>
            <div class="ex-history-meta">${data.sessions.length} session${data.sessions.length === 1 ? '' : 's'} · ${trend.summary}</div>
          </div>
          <div class="trend-pill ${trend.cls}">${trend.label}</div>
        </div>
        <div class="history-detail">
          ${data.sessions.slice().reverse().map(s => `
            <div class="history-session">
              <div class="history-date">${formatDate(s.date)} · ${s.dayTitle}</div>
              <div class="history-sets">
                ${s.sets.map(set => {
                  if (data.loadType === 'bw') return `<span class="history-set-tag">${set.reps} reps</span>`;
                  return `<span class="history-set-tag">${set.weight}lb × ${set.reps}</span>`;
                }).join('')}
              </div>
              ${s.sets.filter(set => set.note).map(set => `
                <div class="history-note">"${set.note}"</div>
              `).join('')}
            </div>
          `).join('')}
        </div>
      </div>
    `;
  });

  el.innerHTML = html;

  setTimeout(() => {
    renderExerciseChart(exerciseStats[state.chartExerciseId]);
    renderVolumeChart();
    if (bwData.length > 0) renderBwChart(bwData);
  }, 50);
}

function updateChartExercise(id) {
  state.chartExerciseId = id;
  renderProgress();
}

function renderExerciseChart(exerciseData) {
  if (!exerciseData) return;
  const canvas = document.getElementById('exerciseChart');
  if (!canvas) return;
  const legendEl = document.getElementById('exerciseChartLegend');

  const dataPoints = exerciseData.sessions.map(s => {
    const validSets = s.sets.filter(set => set.reps !== '');
    const topWeight = exerciseData.loadType === 'bw'
      ? Math.max(...validSets.map(set => parseInt(set.reps) || 0), 0)
      : Math.max(...validSets.map(set => parseFloat(set.weight) || 0), 0);
    const volume = validSets.reduce((sum, set) => {
      const w = exerciseData.loadType === 'bw' ? 1 : (parseFloat(set.weight) || 0);
      const r = parseInt(set.reps) || 0;
      return sum + (w * r);
    }, 0);
    return { date: s.date, topWeight, volume };
  });

  const isBW = exerciseData.loadType === 'bw';
  legendEl.innerHTML = isBW
    ? `<span><span class="legend-dot" style="background: #d4ff3a;"></span>Max Reps</span>
       <span><span class="legend-dot" style="background: #3a9eff;"></span>Total Reps</span>`
    : `<span><span class="legend-dot" style="background: #d4ff3a;"></span>Top Weight (lb)</span>
       <span><span class="legend-dot" style="background: #3a9eff;"></span>Volume (lb×reps)</span>`;

  drawDualLineChart(canvas, dataPoints, {
    yLeft: 'topWeight', yRight: 'volume',
    yLeftColor: '#d4ff3a', yRightColor: '#3a9eff'
  });
}

function renderVolumeChart() {
  const canvas = document.getElementById('volumeChart');
  if (!canvas) return;
  const dataPoints = state.history.map(s => ({
    date: s.date,
    sets: s.exercises.reduce((sum, ex) => sum + ex.sets.length, 0)
  }));
  drawSingleLineChart(canvas, dataPoints, 'sets', '#d4ff3a');
}

function renderBwChart(bwData) {
  const canvas = document.getElementById('bwChart');
  if (!canvas) return;
  const dataPoints = bwData.map(s => ({ date: s.date, bw: parseFloat(s.bw) }));
  drawSingleLineChart(canvas, dataPoints, 'bw', '#ff6b35');
}

function drawSingleLineChart(canvas, points, yKey, color) {
  if (points.length === 0) return;
  const dpr = window.devicePixelRatio || 1;
  const rect = canvas.parentElement.getBoundingClientRect();
  const w = rect.width;
  const h = rect.height;
  canvas.width = w * dpr; canvas.height = h * dpr;
  canvas.style.width = w + 'px'; canvas.style.height = h + 'px';

  const ctx = canvas.getContext('2d');
  ctx.scale(dpr, dpr);
  ctx.clearRect(0, 0, w, h);

  const padL = 36, padR = 16, padT = 16, padB = 28;
  const chartW = w - padL - padR;
  const chartH = h - padT - padB;

  const values = points.map(p => p[yKey]);
  const minV = Math.min(...values);
  const maxV = Math.max(...values);
  const range = maxV - minV || 1;
  const padYRange = range * 0.15;
  const yMin = Math.max(0, minV - padYRange);
  const yMax = maxV + padYRange;

  ctx.strokeStyle = '#262626';
  ctx.lineWidth = 1;
  ctx.font = '9px JetBrains Mono, monospace';
  ctx.fillStyle = '#555';
  for (let i = 0; i <= 3; i++) {
    const y = padT + (chartH / 3) * i;
    ctx.beginPath(); ctx.moveTo(padL, y); ctx.lineTo(w - padR, y); ctx.stroke();
    const v = yMax - ((yMax - yMin) / 3) * i;
    ctx.fillText(Math.round(v), 4, y + 3);
  }

  ctx.fillStyle = '#888';
  if (points.length === 1) {
    ctx.fillText(formatDateShort(points[0].date), padL, h - 8);
  } else {
    ctx.fillText(formatDateShort(points[0].date), padL, h - 8);
    const xLabelN = formatDateShort(points[points.length - 1].date);
    const lastWidth = ctx.measureText(xLabelN).width;
    ctx.fillText(xLabelN, w - padR - lastWidth, h - 8);
  }

  if (points.length > 1) {
    ctx.strokeStyle = color;
    ctx.lineWidth = 2;
    ctx.beginPath();
    points.forEach((p, i) => {
      const x = padL + (chartW / (points.length - 1)) * i;
      const y = padT + chartH - ((p[yKey] - yMin) / (yMax - yMin)) * chartH;
      if (i === 0) ctx.moveTo(x, y); else ctx.lineTo(x, y);
    });
    ctx.stroke();

    ctx.fillStyle = color + '15';
    ctx.lineTo(padL + chartW, padT + chartH);
    ctx.lineTo(padL, padT + chartH);
    ctx.closePath();
    ctx.fill();
  }

  points.forEach((p, i) => {
    const x = points.length === 1 ? padL + chartW / 2 : padL + (chartW / Math.max(1, points.length - 1)) * i;
    const y = padT + chartH - ((p[yKey] - yMin) / (yMax - yMin)) * chartH;
    ctx.fillStyle = color;
    ctx.beginPath(); ctx.arc(x, y, 4, 0, Math.PI * 2); ctx.fill();
    ctx.fillStyle = '#0a0a0a';
    ctx.beginPath(); ctx.arc(x, y, 2, 0, Math.PI * 2); ctx.fill();
  });
}

function drawDualLineChart(canvas, points, opts) {
  if (points.length === 0) return;
  const dpr = window.devicePixelRatio || 1;
  const rect = canvas.parentElement.getBoundingClientRect();
  const w = rect.width;
  const h = rect.height;
  canvas.width = w * dpr; canvas.height = h * dpr;
  canvas.style.width = w + 'px'; canvas.style.height = h + 'px';

  const ctx = canvas.getContext('2d');
  ctx.scale(dpr, dpr);
  ctx.clearRect(0, 0, w, h);

  const padL = 36, padR = 36, padT = 16, padB = 28;
  const chartW = w - padL - padR;
  const chartH = h - padT - padB;

  const leftValues = points.map(p => p[opts.yLeft]);
  const rightValues = points.map(p => p[opts.yRight]);

  const lMin = Math.min(...leftValues);
  const lMax = Math.max(...leftValues);
  const lRange = lMax - lMin || 1;
  const lYMin = Math.max(0, lMin - lRange * 0.15);
  const lYMax = lMax + lRange * 0.15;

  const rMin = Math.min(...rightValues);
  const rMax = Math.max(...rightValues);
  const rRange = rMax - rMin || 1;
  const rYMin = Math.max(0, rMin - rRange * 0.15);
  const rYMax = rMax + rRange * 0.15;

  ctx.strokeStyle = '#262626';
  ctx.lineWidth = 1;
  ctx.font = '9px JetBrains Mono, monospace';

  for (let i = 0; i <= 3; i++) {
    const y = padT + (chartH / 3) * i;
    ctx.beginPath(); ctx.moveTo(padL, y); ctx.lineTo(w - padR, y); ctx.stroke();
    ctx.fillStyle = opts.yLeftColor;
    const lv = lYMax - ((lYMax - lYMin) / 3) * i;
    ctx.fillText(Math.round(lv), 4, y + 3);
    ctx.fillStyle = opts.yRightColor;
    const rv = rYMax - ((rYMax - rYMin) / 3) * i;
    ctx.fillText(Math.round(rv).toString(), w - padR + 4, y + 3);
  }

  ctx.fillStyle = '#888';
  if (points.length === 1) {
    ctx.fillText(formatDateShort(points[0].date), padL, h - 8);
  } else {
    ctx.fillText(formatDateShort(points[0].date), padL, h - 8);
    const xLabelN = formatDateShort(points[points.length - 1].date);
    const lastWidth = ctx.measureText(xLabelN).width;
    ctx.fillText(xLabelN, w - padR - lastWidth, h - 8);
  }

  function drawLine(values, yMin, yMax, color) {
    if (values.length > 1) {
      ctx.strokeStyle = color;
      ctx.lineWidth = 2;
      ctx.beginPath();
      values.forEach((v, i) => {
        const x = padL + (chartW / (values.length - 1)) * i;
        const y = padT + chartH - ((v - yMin) / (yMax - yMin)) * chartH;
        if (i === 0) ctx.moveTo(x, y); else ctx.lineTo(x, y);
      });
      ctx.stroke();
    }
    values.forEach((v, i) => {
      const x = values.length === 1 ? padL + chartW / 2 : padL + (chartW / Math.max(1, values.length - 1)) * i;
      const y = padT + chartH - ((v - yMin) / (yMax - yMin)) * chartH;
      ctx.fillStyle = color;
      ctx.beginPath(); ctx.arc(x, y, 3.5, 0, Math.PI * 2); ctx.fill();
    });
  }

  drawLine(rightValues, rYMin, rYMax, opts.yRightColor);
  drawLine(leftValues, lYMin, lYMax, opts.yLeftColor);
}

function formatDateShort(iso) {
  const d = new Date(iso);
  return d.toLocaleDateString('en-US', { month: 'short', day: 'numeric' });
}

function getMostTrainedDay(sessionsByDay) {
  const max = Math.max(...Object.values(sessionsByDay), 0);
  if (max === 0) return '—';
  const dayId = Object.entries(sessionsByDay).find(([k, v]) => v === max)[0];
  const day = program.days.find(d => d.id === dayId);
  return day ? day.title : '—';
}

function analyzeTrend(exerciseData) {
  const sessions = exerciseData.sessions;
  if (sessions.length < 2) return { label: 'NEW', cls: 'trend-new', summary: 'First time logged' };

  const first = sessions[0];
  const last = sessions[sessions.length - 1];

  if (exerciseData.loadType === 'bw') {
    const firstReps = Math.max(...first.sets.map(s => parseInt(s.reps) || 0));
    const lastReps = Math.max(...last.sets.map(s => parseInt(s.reps) || 0));
    const diff = lastReps - firstReps;
    if (diff > 0) return { label: `+${diff} REPS`, cls: 'trend-up', summary: `${firstReps} → ${lastReps} reps` };
    if (diff < 0) return { label: `${diff} REPS`, cls: 'trend-flat', summary: `${firstReps} → ${lastReps} reps` };
    return { label: 'STEADY', cls: 'trend-flat', summary: `${lastReps} reps top set` };
  }

  const firstTop = Math.max(...first.sets.map(s => parseFloat(s.weight) || 0));
  const lastTop = Math.max(...last.sets.map(s => parseFloat(s.weight) || 0));
  const diff = lastTop - firstTop;
  if (diff > 0) return { label: `+${diff}LB`, cls: 'trend-up', summary: `${firstTop} → ${lastTop} lb top set` };
  if (diff < 0) return { label: `${diff}LB`, cls: 'trend-flat', summary: `${firstTop} → ${lastTop} lb top set` };
  return { label: 'STEADY', cls: 'trend-flat', summary: `${lastTop} lb top set` };
}

function toggleHistoryBlock(header) {
  header.parentElement.classList.toggle('expanded');
}

function formatDate(iso) {
  const d = new Date(iso);
  const now = new Date();
  const diff = (now - d) / (1000 * 60 * 60 * 24);
  if (diff < 1) return 'Today';
  if (diff < 2) return 'Yesterday';
  if (diff < 7) return `${Math.floor(diff)}d ago`;
  return d.toLocaleDateString('en-US', { month: 'short', day: 'numeric' });
}

function showToast(msg, variant = '') {
  const toast = document.getElementById('toast');
  toast.textContent = msg;
  toast.className = 'toast show ' + variant;
  setTimeout(() => toast.classList.remove('show'), 2200);
}

function openResetModal() { document.getElementById('resetModal').classList.add('active'); }
function closeResetModal() { document.getElementById('resetModal').classList.remove('active'); }

function confirmReset() {
  state.history = [];
  state.currentSession = null;
  saveData();
  closeResetModal();
  showToast('All data cleared');
  switchTab('plan');
}

function exportData() {
  if (state.history.length === 0) { showToast('No data to export yet'); return; }
  const exportObj = {
    app: 'lift-tracker', version: 2,
    exportedAt: new Date().toISOString(),
    history: state.history
  };
  const json = JSON.stringify(exportObj, null, 2);
  const blob = new Blob([json], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const date = new Date().toISOString().split('T')[0];
  const filename = `lift-tracker-backup-${date}.json`;
  const a = document.createElement('a');
  a.href = url; a.download = filename;
  document.body.appendChild(a); a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
  showToast(`Exported ${state.history.length} sessions ✓`, 'success');
}

function importData(event) {
  const file = event.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = (e) => {
    try {
      const data = JSON.parse(e.target.result);
      if (!data.history || !Array.isArray(data.history)) {
        showToast('Invalid backup file');
        return;
      }
      const incomingCount = data.history.length;
      const currentCount = state.history.length;
      const confirmMsg = currentCount === 0
        ? `Import ${incomingCount} sessions?`
        : `You have ${currentCount} sessions logged. Replace with ${incomingCount} imported sessions? Your current data will be overwritten.`;
      if (confirm(confirmMsg)) {
        state.history = data.history;
        saveData();
        showToast(`Imported ${incomingCount} sessions ✓`, 'success');
        renderPlan();
      }
    } catch (err) {
      showToast('Could not read backup file');
    }
    event.target.value = '';
  };
  reader.readAsText(file);
}

loadData();
renderPlan();

window.addEventListener('resize', () => {
  if (document.getElementById('progressPage').classList.contains('active')) {
    renderProgress();
  }
});
</script>

</body>
</html>
