# cyber-chat-ugs
نظام اتصالات ويب آمن وحديث
[index.html.html](https://github.com/user-attachments/files/26058526/index.html.html)
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>COMM_LINK | نظام UGS</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;700&family=Noto+Kufi+Arabic:wght@400;600;700;900&display=swap" rel="stylesheet">
<style>
/* ─── VARIABLES ─────────────────────────────────────── */
:root {
  --g: #39ff7a;
  --g2: #00c853;
  --g-dim: rgba(57,255,122,0.12);
  --g-glow: rgba(57,255,122,0.35);
  --r: #ff3a3a;
  --y: #ffd700;
  --bg: #030705;
  --s1: #060d08;
  --s2: #0a1410;
  --border: rgba(57,255,122,0.12);
  --border-mid: rgba(57,255,122,0.22);
  --text: #d4eed4;
  --text-dim: #4a7a54;
  --text-faint: #1e3525;
  --mono: 'IBM Plex Mono', monospace;
  --sans: 'Noto Kufi Arabic', sans-serif;
}

/* ─── RESET & BASE ──────────────────────────────────── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html, body {
  height: 100%; overflow: hidden;
  background: var(--bg);
  color: var(--text);
  font-family: var(--sans);
  direction: rtl;
}

::-webkit-scrollbar { width: 3px; height: 3px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border-mid); border-radius: 2px; }

/* ─── LAYOUT ────────────────────────────────────────── */
#app { display: flex; flex-direction: column; height: 100vh; position: relative; overflow: hidden; }

/* Background CRT grid */
#app::before {
  content: '';
  position: fixed; inset: 0; z-index: 0; pointer-events: none;
  background-image:
    linear-gradient(var(--text-faint) 1px, transparent 1px),
    linear-gradient(90deg, var(--text-faint) 1px, transparent 1px);
  background-size: 32px 32px;
  opacity: 0.35;
}

/* Scanline sweep */
@keyframes scan { from { transform: translateY(-4px); } to { transform: translateY(100vh); } }
#scanline {
  position: fixed; left: 0; right: 0; height: 3px; z-index: 9999; pointer-events: none;
  background: linear-gradient(180deg, transparent, rgba(57,255,122,0.06), transparent);
  animation: scan 10s linear infinite;
}

/* Vignette */
#app::after {
  content: '';
  position: fixed; inset: 0; z-index: 0; pointer-events: none;
  background: radial-gradient(ellipse at center, transparent 50%, rgba(0,0,0,0.6) 100%);
}

/* ─── HEADER ────────────────────────────────────────── */
#header {
  position: relative; z-index: 10;
  background: rgba(3,7,5,0.92);
  border-bottom: 1px solid var(--border-mid);
  backdrop-filter: blur(12px);
  flex-shrink: 0;
}

.header-top {
  display: flex; align-items: center; justify-content: space-between;
  padding: 10px 16px; gap: 12px;
}

.logo { display: flex; align-items: center; gap: 10px; }

.logo-icon {
  width: 34px; height: 34px; border-radius: 50%;
  border: 1.5px solid var(--g);
  background: radial-gradient(circle at 40% 40%, rgba(57,255,122,0.15), transparent);
  display: flex; align-items: center; justify-content: center;
  box-shadow: 0 0 16px var(--g-glow), inset 0 0 8px rgba(57,255,122,0.1);
  font-family: var(--mono); font-size: 14px; font-weight: 700; color: var(--g);
}

.logo-text h1 {
  font-family: var(--mono); font-size: 15px; font-weight: 700; color: var(--g);
  text-shadow: 0 0 10px var(--g-glow); letter-spacing: 0.1em;
}
.logo-text p { font-size: 9px; color: var(--text-dim); letter-spacing: 0.2em; font-family: var(--mono); margin-top: 1px; }

.header-right { display: flex; align-items: center; gap: 8px; }

/* rec badge */
.rec-badge {
  display: none; align-items: center; gap: 6px;
  background: rgba(255,58,58,0.1); border: 1px solid rgba(255,58,58,0.3);
  padding: 4px 10px; border-radius: 4px;
  font-size: 10px; color: var(--r); font-family: var(--mono); font-weight: 700;
}
.rec-badge.show { display: flex; }
@keyframes blink-rec { 0%,100% { opacity:1; } 50% { opacity:0.2; } }
.rec-dot { width: 6px; height: 6px; background: var(--r); border-radius: 50%; animation: blink-rec 1.2s ease infinite; }

/* icon btn */
.ibtn {
  width: 34px; height: 34px; background: var(--s1); border: 1px solid var(--border);
  border-radius: 6px; display: flex; align-items: center; justify-content: center;
  cursor: pointer; transition: border-color 0.2s, box-shadow 0.2s, background 0.2s;
  position: relative; color: var(--text-dim);
}
.ibtn:hover { border-color: var(--g); background: var(--g-dim); color: var(--g); }
.ibtn.on { border-color: var(--g); background: var(--g-dim); color: var(--g); box-shadow: 0 0 10px var(--g-glow); }
.ibtn.on-r { border-color: var(--r); background: rgba(255,58,58,0.1); color: var(--r); box-shadow: 0 0 10px rgba(255,58,58,0.3); }

/* status btn */
.status-wrap { position: relative; }
#status-menu {
  display: none; position: absolute; top: calc(100% + 6px); left: 0;
  background: var(--s2); border: 1px solid var(--border-mid);
  border-radius: 8px; min-width: 140px; z-index: 200; overflow: hidden;
  box-shadow: 0 8px 24px rgba(0,0,0,0.6);
}
#status-menu.open { display: block; }
.sm-item {
  display: flex; align-items: center; gap: 8px; justify-content: flex-end;
  padding: 9px 14px; font-size: 11px; cursor: pointer; transition: background 0.15s;
}
.sm-item:hover { background: rgba(255,255,255,0.04); }

.sdot { width: 8px; height: 8px; border-radius: 50%; }
.s-online { background: var(--g); box-shadow: 0 0 5px var(--g); }
.s-away { background: var(--y); box-shadow: 0 0 5px var(--y); }
.s-busy { background: var(--r); box-shadow: 0 0 5px var(--r); }
.s-offline { background: #374151; border: 1px solid #555; }

/* username input */
#username-input {
  background: var(--s2); border: 1px solid var(--border);
  color: var(--g); font-family: var(--mono); font-size: 11px;
  padding: 6px 12px; border-radius: 6px; width: 140px; text-align: right;
  transition: border-color 0.2s;
}
#username-input:focus { outline: none; border-color: var(--g); box-shadow: 0 0 8px rgba(57,255,122,0.15); }
#username-input::placeholder { color: var(--text-faint); }

/* search bar */
#search-bar {
  display: none; border-top: 1px solid var(--border);
  padding: 8px 16px; background: rgba(0,0,0,0.4);
  align-items: center; gap: 10px;
}
#search-bar.open { display: flex; }
#search-input {
  flex: 1; background: var(--s2); border: 1px solid var(--border);
  color: var(--g); font-family: var(--sans); font-size: 12px;
  padding: 7px 14px; border-radius: 6px;
}
#search-input:focus { outline: none; border-color: var(--g); }
#search-input::placeholder { color: var(--text-faint); }
#search-count { font-size: 10px; color: var(--text-dim); font-family: var(--mono); white-space: nowrap; }

/* TABS */
#tabs-bar {
  display: flex; border-top: 1px solid var(--border); background: rgba(0,0,0,0.5);
  overflow-x: auto; scrollbar-width: none;
}
#tabs-bar::-webkit-scrollbar { display: none; }

.tab {
  padding: 9px 16px; font-size: 11px; font-weight: 700; white-space: nowrap;
  cursor: pointer; border-bottom: 2px solid transparent; color: var(--text-dim);
  border-left: 1px solid var(--border); transition: color 0.2s, border-color 0.2s, background 0.2s;
  display: flex; align-items: center; gap: 6px; font-family: var(--sans);
}
.tab:hover { color: var(--text); background: rgba(255,255,255,0.02); }
.tab.active { color: var(--g); border-bottom-color: var(--g); background: var(--g-dim); }

.tab-close { opacity: 0.3; transition: opacity 0.15s, color 0.15s; font-size: 13px; line-height: 1; }
.tab-close:hover { opacity: 1; color: var(--r); }

.badge { background: var(--r); color: #fff; font-size: 9px; border-radius: 999px; padding: 1px 5px; min-width: 16px; text-align: center; font-family: var(--mono); }

.tab-add {
  padding: 9px 14px; font-size: 17px; color: var(--text-dim); cursor: pointer;
  border-left: 1px solid var(--border); transition: color 0.2s;
  display: flex; align-items: center;
}
.tab-add:hover { color: var(--g); }

/* ─── BODY ──────────────────────────────────────────── */
#body { display: flex; flex: 1; overflow: hidden; position: relative; z-index: 1; }

/* ─── CHAT AREA ─────────────────────────────────────── */
#chat-wrap { flex: 1; display: flex; flex-direction: column; overflow: hidden; }

/* pinned */
#pinned-bar {
  display: none; align-items: center; gap: 8px;
  padding: 7px 16px;
  background: rgba(255,215,0,0.04); border-bottom: 1px solid rgba(255,215,0,0.15);
  font-size: 11px; flex-shrink: 0;
}
#pinned-bar.show { display: flex; }
#pinned-icon { color: var(--y); font-size: 13px; }
#pinned-text { color: rgba(255,215,0,0.7); flex: 1; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
#unpin-btn { color: var(--text-dim); font-size: 12px; cursor: pointer; transition: color 0.15s; display: none; }
#unpin-btn:hover { color: var(--r); }

/* messages */
#messages {
  flex: 1; overflow-y: auto; padding: 20px 20px 10px;
  display: flex; flex-direction: column; gap: 4px; scroll-behavior: smooth;
}

.msg-day {
  text-align: center; font-size: 9px; color: var(--text-faint);
  font-family: var(--mono); letter-spacing: 0.15em; margin: 12px 0 4px;
}
.msg-day span { background: var(--s1); border: 1px solid var(--border); padding: 3px 10px; border-radius: 20px; }

@keyframes fadeUp { from { opacity:0; transform:translateY(6px); } to { opacity:1; transform:translateY(0); } }

.msg-row {
  display: flex; gap: 8px; animation: fadeUp 0.25s ease-out;
  padding: 2px 4px; border-radius: 8px;
}
.msg-row.mine { flex-direction: row-reverse; }
.msg-row.gap-top { margin-top: 12px; }

.msg-content { max-width: 70%; min-width: 60px; }

.msg-meta {
  display: flex; align-items: center; gap: 6px; margin-bottom: 4px; font-size: 10px;
}
.msg-row.mine .msg-meta { flex-direction: row-reverse; }

.msg-sender { font-weight: 700; color: var(--g); font-family: var(--mono); font-size: 11px; }
.msg-sender.other { color: #a8d8c8; }
.msg-time { color: var(--text-faint); font-family: var(--mono); font-size: 9px; }
.owner-tag { background: rgba(255,58,58,0.15); border: 1px solid rgba(255,58,58,0.3); color: var(--r); font-size: 8px; padding: 1px 5px; border-radius: 3px; font-family: var(--mono); }
.edited-tag { color: var(--text-faint); font-size: 9px; font-style: italic; }

.bubble {
  padding: 9px 13px; border-radius: 10px; font-size: 13px;
  line-height: 1.65; word-break: break-word; position: relative;
  transition: background 0.3s;
}
.bubble.mine { background: rgba(57,255,122,0.1); border: 1px solid rgba(57,255,122,0.2); border-top-left-radius: 3px; }
.bubble.other { background: var(--s2); border: 1px solid var(--border); border-top-right-radius: 3px; }
.bubble.deleted { opacity: 0.4; font-style: italic; font-size: 12px; }
.bubble.sys { background: transparent; border: none; text-align: center; font-size: 10px; color: var(--text-faint); font-family: var(--mono); }

/* reply preview inside bubble */
.reply-ref {
  border-right: 2px solid rgba(57,255,122,0.4); padding: 4px 10px 4px 6px;
  margin-bottom: 7px; background: rgba(0,0,0,0.25); border-radius: 4px;
  cursor: pointer; transition: background 0.15s;
}
.reply-ref:hover { background: rgba(57,255,122,0.07); }
.reply-ref-sender { font-size: 9px; color: var(--g); font-family: var(--mono); }
.reply-ref-text { font-size: 10px; color: var(--text-dim); overflow: hidden; white-space: nowrap; text-overflow: ellipsis; max-width: 250px; }

/* image in bubble */
.bubble img { max-width: 260px; border-radius: 6px; border: 1px solid var(--border); display: block; margin-top: 4px; }

/* msg actions */
.msg-actions {
  display: flex; gap: 8px; margin-top: 3px; opacity: 0;
  transition: opacity 0.15s; font-size: 10px;
}
.msg-row:hover .msg-actions { opacity: 1; }
.msg-row.mine .msg-actions { justify-content: flex-end; }

.act-btn {
  color: var(--text-faint); cursor: pointer; transition: color 0.15s;
  font-family: var(--mono); padding: 1px 5px; border-radius: 3px; border: 1px solid transparent;
}
.act-btn:hover { color: var(--g); border-color: var(--border); }
.act-btn.del:hover { color: var(--r); border-color: rgba(255,58,58,0.3); }
.act-btn.pin:hover { color: var(--y); border-color: rgba(255,215,0,0.3); }
.act-btn.edit:hover { color: #60a5fa; border-color: rgba(96,165,250,0.3); }

/* sys message */
.sys-msg { text-align: center; padding: 4px 0; }
.sys-msg span {
  background: var(--s1); border: 1px solid var(--border);
  padding: 3px 12px; border-radius: 20px;
  font-size: 10px; color: var(--text-faint); font-family: var(--mono);
}

/* search highlight */
mark { background: rgba(255,215,0,0.2); color: #ffd700; border-radius: 2px; padding: 0 2px; }

/* empty state */
.empty-state {
  flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 10px; color: var(--text-faint);
}
.empty-state .empty-icon { font-size: 40px; opacity: 0.2; }
.empty-state p { font-size: 11px; letter-spacing: 0.1em; font-family: var(--mono); }

/* typing */
#typing { height: 18px; padding: 0 20px; font-size: 10px; color: var(--text-faint); font-style: italic; font-family: var(--mono); }

/* ─── INPUT ZONE ─────────────────────────────────────── */
#input-zone {
  border-top: 1px solid var(--border); background: rgba(3,7,5,0.95);
  padding: 10px 14px; flex-shrink: 0; position: relative;
}

/* cooldown bar */
#cooldown {
  position: absolute; bottom: 0; left: 0; right: 0; height: 2px;
  background: var(--r); display: none; transition: width 0.1s linear;
}

/* reply bar */
#reply-preview {
  display: none; align-items: center; gap: 8px;
  background: var(--s1); border: 1px solid var(--border);
  border-radius: 6px; padding: 6px 10px; margin-bottom: 8px;
}
#reply-preview.show { display: flex; }
.rp-line { width: 2px; height: 28px; background: var(--g); border-radius: 2px; flex-shrink: 0; }
.rp-info { flex: 1; min-width: 0; }
.rp-sender { font-size: 10px; color: var(--g); font-family: var(--mono); }
.rp-text { font-size: 11px; color: var(--text-dim); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.rp-close { color: var(--text-dim); cursor: pointer; font-size: 14px; transition: color 0.15s; flex-shrink: 0; }
.rp-close:hover { color: var(--r); }

/* controls */
.input-controls { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; }

.media-group { display: flex; gap: 6px; align-items: center; }
.vdivider { width: 1px; height: 26px; background: var(--border); }

/* input row */
.input-row { flex: 1; display: flex; gap: 8px; align-items: center; position: relative; min-width: 0; }

#msg-input {
  flex: 1; background: var(--s2); border: 1px solid var(--border);
  color: var(--text); font-family: var(--sans); font-size: 13px;
  padding: 10px 16px; border-radius: 8px;
  transition: border-color 0.2s, box-shadow 0.2s;
}
#msg-input:focus { outline: none; border-color: var(--g); box-shadow: 0 0 10px rgba(57,255,122,0.1); }
#msg-input::placeholder { color: var(--text-faint); }
#msg-input:disabled { opacity: 0.4; cursor: not-allowed; }
#msg-input.shake { animation: shake 0.35s ease; }
@keyframes shake { 0%,100%{transform:translateX(0);} 20%,60%{transform:translateX(-5px);} 40%,80%{transform:translateX(5px);} }

#send-btn {
  background: var(--g); color: #030705; font-family: var(--sans); font-weight: 700;
  font-size: 13px; padding: 10px 20px; border-radius: 8px; cursor: pointer; border: none;
  transition: background 0.2s, box-shadow 0.2s; flex-shrink: 0;
}
#send-btn:hover { background: #fff; box-shadow: 0 0 15px rgba(57,255,122,0.5); }
#send-btn:disabled { opacity: 0.4; cursor: not-allowed; }
#send-btn.editing { background: #60a5fa; color: #030705; }

/* mic level */
#mic-bar-wrap { height: 2px; background: var(--border); border-radius: 2px; margin-top: 6px; display: none; }
#mic-bar-wrap.show { display: block; }
#mic-bar { height: 100%; background: var(--g); border-radius: 2px; width: 0%; transition: width 0.08s; }

/* emoji picker */
#emoji-picker {
  display: none; position: absolute; bottom: calc(100% + 6px); right: 0;
  background: var(--s2); border: 1px solid var(--border-mid);
  border-radius: 10px; padding: 10px; width: 260px; max-height: 170px;
  overflow-y: auto; z-index: 300; flex-wrap: wrap; gap: 3px;
  box-shadow: 0 -8px 30px rgba(0,0,0,0.6);
}
#emoji-picker.open { display: flex; }
.emoji-btn { font-size: 18px; background: none; border: none; cursor: pointer; padding: 4px; border-radius: 5px; transition: background 0.1s; }
.emoji-btn:hover { background: rgba(255,255,255,0.08); }

/* ─── SIDEBAR ────────────────────────────────────────── */
#sidebar {
  width: 240px; border-right: 1px solid var(--border);
  background: var(--s1); display: flex; flex-direction: column;
  flex-shrink: 0; overflow: hidden;
}

.sb-header {
  padding: 12px 14px; border-bottom: 1px solid var(--border);
  background: rgba(0,0,0,0.4); flex-shrink: 0;
}
.sb-title { font-size: 10px; color: var(--g); font-weight: 700; font-family: var(--mono); display: flex; align-items: center; gap: 6px; }
@keyframes pulse-g { 0%,100% { box-shadow: 0 0 4px var(--g); } 50% { box-shadow: 0 0 10px var(--g); } }
.live-dot { width: 6px; height: 6px; background: var(--g); border-radius: 50%; animation: pulse-g 2s ease infinite; }
.sb-sub { font-size: 9px; color: var(--text-dim); margin-top: 3px; font-family: var(--mono); }
.sb-sub span { color: var(--text); font-weight: 700; }

.sb-tabs { display: flex; border-bottom: 1px solid var(--border); background: rgba(0,0,0,0.3); flex-shrink: 0; }
.sb-tab {
  flex: 1; padding: 9px 4px; text-align: center; font-size: 10px; font-weight: 700;
  cursor: pointer; border-bottom: 2px solid transparent; color: var(--text-dim);
  transition: color 0.2s, background 0.2s; font-family: var(--mono); letter-spacing: 0.05em;
  position: relative;
}
.sb-tab:hover { color: var(--text); background: rgba(255,255,255,0.02); }
.sb-tab.active { color: var(--g); border-bottom-color: var(--g); background: var(--g-dim); }
#alert-dot { position: absolute; top: 6px; left: 12px; width: 6px; height: 6px; background: var(--r); border-radius: 50%; display: none; }
#alert-dot.show { display: block; }

.sb-body { flex: 1; overflow: hidden; display: flex; flex-direction: column; }

#agents-panel, #alerts-panel { flex: 1; overflow-y: auto; }

.sb-section-header {
  padding: 8px 12px; font-size: 9px; color: var(--text-faint);
  font-family: var(--mono); font-weight: 700; letter-spacing: 0.15em;
  display: flex; justify-content: space-between; align-items: center;
  cursor: pointer; transition: background 0.15s; border-bottom: 1px solid var(--border);
}
.sb-section-header:hover { background: rgba(255,255,255,0.02); color: var(--text-dim); }

.agent-list { padding: 6px; }
.agent-item {
  display: flex; align-items: center; gap: 8px; padding: 6px 8px;
  border-radius: 6px; transition: background 0.15s; font-size: 11px; cursor: default;
}
.agent-item:hover { background: var(--g-dim); }

.agent-name { flex: 1; font-weight: 700; overflow: hidden; white-space: nowrap; text-overflow: ellipsis; }
.agent-name.me { color: var(--g); }
.agent-name.other { color: var(--text); }

.agent-btns { display: none; gap: 3px; }
.agent-item:hover .agent-btns { display: flex; }

.ag-btn {
  font-size: 8px; padding: 2px 6px; border-radius: 3px; cursor: pointer;
  font-family: var(--mono); font-weight: 700; border: 1px solid transparent;
  transition: all 0.15s; color: var(--text-dim);
}
.ag-btn.dm { border-color: rgba(96,165,250,0.3); }
.ag-btn.dm:hover { background: rgba(96,165,250,0.15); color: #60a5fa; }
.ag-btn.kick { border-color: rgba(255,215,0,0.3); }
.ag-btn.kick:hover { background: rgba(255,215,0,0.15); color: var(--y); }
.ag-btn.ban { border-color: rgba(255,58,58,0.3); }
.ag-btn.ban:hover { background: rgba(255,58,58,0.15); color: var(--r); }

/* banned section */
#banned-section { border-top: 1px solid var(--border); display: none; }
#banned-section.show { display: block; }
.banned-list { padding: 6px; display: flex; flex-direction: column; gap: 4px; }
.banned-item {
  display: flex; justify-content: space-between; align-items: center;
  padding: 5px 8px; border-radius: 4px; background: rgba(255,58,58,0.05);
  border: 1px solid rgba(255,58,58,0.15); font-size: 10px;
}
.banned-name { color: rgba(255,58,58,0.7); overflow: hidden; white-space: nowrap; text-overflow: ellipsis; font-family: var(--mono); }
.unban-btn { color: var(--r); font-size: 9px; cursor: pointer; border: 1px solid rgba(255,58,58,0.3); padding: 1px 6px; border-radius: 3px; flex-shrink: 0; }
.unban-btn:hover { background: rgba(255,58,58,0.2); }

/* alerts */
.alert-item {
  margin: 6px; padding: 10px; border-radius: 6px;
  background: rgba(255,215,0,0.05); border: 1px solid rgba(255,215,0,0.15);
  font-size: 11px;
}
.no-alerts { text-align: center; padding: 40px 20px; font-size: 10px; color: var(--text-faint); font-family: var(--mono); }

/* side-collapse */
.collapsed-list { display: none; }
.collapsed-list.open { display: block; }

/* ─── OVERLAYS ───────────────────────────────────────── */
.overlay {
  display: none; position: fixed; inset: 0; z-index: 9000;
  background: rgba(0,0,0,0.88); backdrop-filter: blur(8px);
  align-items: center; justify-content: center;
}
.overlay.open { display: flex; }

.modal {
  background: var(--s2); border: 1px solid var(--border-mid);
  border-radius: 14px; padding: 28px; max-width: 420px; width: 92%;
  box-shadow: 0 0 50px rgba(57,255,122,0.08);
}
.modal h2 { font-size: 18px; font-weight: 900; color: var(--g); margin-bottom: 4px; }
.modal-sub { font-size: 10px; color: var(--text-dim); margin-bottom: 20px; font-family: var(--mono); }

.modal-step { display: none; flex-direction: column; gap: 12px; }
.modal-step.active { display: flex; }

.modal-card {
  border: 1px solid var(--border); background: var(--s1);
  border-radius: 8px; padding: 14px 16px; cursor: pointer;
  transition: border-color 0.2s, background 0.2s;
  text-align: right;
}
.modal-card:hover { border-color: var(--g); background: var(--g-dim); }
.modal-card strong { display: flex; align-items: center; gap: 8px; font-weight: 700; font-size: 14px; }
.modal-card p { font-size: 11px; color: var(--text-dim); margin-top: 5px; }

.field-label { font-size: 9px; color: var(--text-dim); letter-spacing: 0.2em; font-family: var(--mono); margin-bottom: 5px; }
.field-input {
  width: 100%; background: var(--s1); border: 1px solid var(--border);
  color: var(--g); font-family: var(--mono); font-size: 12px;
  padding: 10px 14px; border-radius: 7px; text-transform: uppercase;
  transition: border-color 0.2s;
}
.field-input:focus { outline: none; border-color: var(--g); }
.field-input::placeholder { color: var(--text-faint); text-transform: none; }

.checkbox-row {
  display: flex; align-items: center; gap: 10px;
  background: var(--s1); border: 1px solid var(--border);
  padding: 10px 14px; border-radius: 7px; cursor: pointer; font-size: 12px;
  transition: border-color 0.2s;
}
.checkbox-row:hover { border-color: var(--border-mid); }
.checkbox-row input { accent-color: var(--g); width: 14px; height: 14px; }

.btn-row { display: flex; gap: 10px; }
.btn-back {
  flex: 1; border: 1px solid var(--border); background: transparent;
  color: var(--text-dim); font-family: var(--sans); font-size: 12px;
  padding: 11px; border-radius: 7px; cursor: pointer; transition: all 0.2s;
}
.btn-back:hover { background: rgba(255,255,255,0.04); color: var(--text); }
.btn-primary {
  flex: 2; background: var(--g); color: #030705;
  font-family: var(--sans); font-size: 13px; font-weight: 700;
  padding: 11px; border-radius: 7px; cursor: pointer; border: none;
  transition: background 0.2s, box-shadow 0.2s;
}
.btn-primary:hover { background: #fff; box-shadow: 0 0 16px rgba(57,255,122,0.4); }
.btn-cancel { text-align: center; color: var(--text-faint); font-size: 11px; cursor: pointer; padding: 4px; transition: color 0.15s; }
.btn-cancel:hover { color: var(--text-dim); }

/* alert modal */
#alert-modal .modal { text-align: center; }
#alert-modal .modal-icon { font-size: 28px; margin-bottom: 12px; }
#alert-modal h3 { font-size: 15px; font-weight: 700; color: var(--g); margin-bottom: 10px; }
#alert-msg { font-size: 13px; color: var(--text-dim); line-height: 1.6; margin-bottom: 20px; }
.btn-ok {
  width: 100%; border: 1px solid var(--g); background: transparent;
  color: var(--g); font-family: var(--sans); font-weight: 700; font-size: 13px;
  padding: 11px; border-radius: 7px; cursor: pointer; transition: all 0.2s;
}
.btn-ok:hover { background: var(--g); color: #030705; }

/* quota */
#quota-overlay {
  display: none; position: fixed; inset: 0; z-index: 99999;
  background: rgba(0,0,0,0.99); align-items: center; justify-content: center;
}
#quota-overlay.show { display: flex; }
.quota-box { text-align: center; border: 1px solid rgba(255,58,58,0.4); padding: 40px; border-radius: 14px; max-width: 380px; }
.quota-box h2 { color: var(--r); font-size: 22px; font-weight: 900; margin: 16px 0 10px; font-family: var(--mono); }
.quota-box p { color: #9ca3af; font-size: 13px; }
.quota-box footer { font-size: 9px; color: #374151; margin-top: 20px; border-top: 1px solid #1f2937; padding-top: 12px; font-family: var(--mono); }

/* screen share preview */
#screen-preview {
  display: none; position: fixed;
  bottom: 90px; left: 16px; width: 270px;
  background: #000; border: 1px solid rgba(255,58,58,0.5);
  border-radius: 8px; overflow: hidden; z-index: 400;
  box-shadow: 0 0 20px rgba(255,58,58,0.2);
}
#screen-preview.show { display: block; }
.sp-header { display: flex; justify-content: space-between; align-items: center; padding: 6px 10px; border-bottom: 1px solid rgba(255,58,58,0.2); background: rgba(255,58,58,0.06); }
.sp-label { font-size: 10px; color: var(--r); font-family: var(--mono); font-weight: 700; display: flex; align-items: center; gap: 6px; }
.sp-stop { font-size: 10px; color: #6b7280; cursor: pointer; transition: color 0.15s; }
.sp-stop:hover { color: var(--r); }
#screen-preview video { width: 100%; display: block; }

/* cam mini */
#cam-mini {
  display: none; position: fixed;
  bottom: 90px; right: 256px; width: 150px;
  background: #000; border: 1px solid var(--g);
  border-radius: 8px; overflow: hidden; z-index: 400;
  box-shadow: 0 0 14px var(--g-glow);
}
#cam-mini.show { display: block; }
#cam-mini video { width: 100%; display: block; transform: scaleX(-1); }
.cam-label { position: absolute; top: 5px; right: 5px; background: rgba(0,0,0,0.7); border: 1px solid var(--border); padding: 2px 7px; font-size: 9px; border-radius: 3px; color: var(--g); font-family: var(--mono); }

/* mobile overlay */
#mob-overlay { display: none; position: fixed; inset: 0; z-index: 90; background: rgba(0,0,0,0.7); }
#mob-overlay.show { display: block; }

/* Mobile sidebar */
@media (max-width: 900px) {
  #sidebar {
    position: fixed; top: 0; right: -260px; height: 100%; z-index: 100;
    transition: right 0.3s ease; width: 240px;
  }
  #sidebar.open { right: 0; }
  #mob-open-btn { display: flex !important; }
}
#mob-open-btn { display: none; }

/* SVG icons */
svg { display: block; }
</style>
</head>
<body>
<div id="app">
  <div id="scanline"></div>

  <!-- QUOTA -->
  <div id="quota-overlay">
    <div class="quota-box">
      <div style="font-size:48px">⚠️</div>
      <h2>تجاوز حصة النظام</h2>
      <p>تم تعليق الاتصال. حصة قاعدة البيانات استُنفدت.</p>
      <footer>البروتوكول: EXHAUSTION_DETECTED // الاتصال: مغلق</footer>
    </div>
  </div>

  <!-- ALERT MODAL -->
  <div id="alert-modal" class="overlay">
    <div class="modal">
      <div class="modal-icon">📡</div>
      <h3>تنبيه النظام</h3>
      <p id="alert-msg"></p>
      <button class="btn-ok" onclick="closeAlert()">موافق</button>
    </div>
  </div>

  <!-- ROOM MODAL -->
  <div id="room-modal" class="overlay">
    <div class="modal">
      <!-- step 1 -->
      <div id="rs1" class="modal-step active">
        <h2>عمليات القناة</h2>
        <p class="modal-sub">أنشئ قناة جديدة أو انضم لقناة موجودة</p>
        <div class="modal-card" onclick="rsGo(2)">
          <strong><span>➕</span> إنشاء قناة جديدة</strong>
          <p>أنشئ قناة تواصل آمنة جديدة</p>
        </div>
        <div class="modal-card" onclick="rsGo(3)">
          <strong><span>🔗</span> الانضمام لقناة</strong>
          <p>انضم لشبكة اتصالات موجودة</p>
        </div>
        <div class="btn-cancel" onclick="closeRoomModal()">إلغاء</div>
      </div>
      <!-- step 2 -->
      <div id="rs2" class="modal-step">
        <h2>إنشاء قناة</h2>
        <p class="modal-sub">أدخل اسم القناة الجديدة</p>
        <div>
          <div class="field-label">اسم القناة</div>
          <input id="create-name" class="field-input" type="text" placeholder="مثال: الغرفة_الرئيسية">
        </div>
        <label class="checkbox-row">
          <input type="checkbox" id="create-locked">
          <span>🔒 قناة محمية (تتطلب موافقة)</span>
        </label>
        <div class="btn-row">
          <button class="btn-back" onclick="rsGo(1)">رجوع</button>
          <button class="btn-primary" onclick="doCreate()">إنشاء القناة ✓</button>
        </div>
      </div>
      <!-- step 3 -->
      <div id="rs3" class="modal-step">
        <h2>الانضمام لقناة</h2>
        <p class="modal-sub">أدخل اسم القناة للانضمام</p>
        <div>
          <div class="field-label">اسم القناة المستهدفة</div>
          <input id="join-name" class="field-input" type="text" placeholder="مثال: GLOBAL">
        </div>
        <div class="btn-row">
          <button class="btn-back" onclick="rsGo(1)">رجوع</button>
          <button class="btn-primary" onclick="doJoin()">انضمام 🔗</button>
        </div>
      </div>
    </div>
  </div>

  <!-- SCREEN SHARE PREVIEW -->
  <div id="screen-preview">
    <div class="sp-header">
      <div class="sp-label"><span class="rec-dot"></span> مشاركة الشاشة نشطة</div>
      <span class="sp-stop" onclick="stopScreenShare()">إيقاف ✕</span>
    </div>
    <video id="screen-video" autoplay muted></video>
  </div>

  <!-- CAM MINI -->
  <div id="cam-mini">
    <div class="cam-label">📷</div>
    <video id="cam-video" autoplay muted playsinline></video>
  </div>

  <!-- MOBILE OVERLAY -->
  <div id="mob-overlay" onclick="closeSidebar()"></div>

  <!-- ── HEADER ── -->
  <div id="header">
    <div class="header-top">
      <!-- logo -->
      <div class="logo">
        <div class="logo-icon">C</div>
        <div class="logo-text">
          <h1>COMM_LINK</h1>
          <p>نظام الاتصالات الآمن</p>
        </div>
      </div>
      <!-- controls -->
      <div class="header-right">
        <div id="rec-badge" class="rec-badge">
          <span class="rec-dot"></span>
          <span id="rec-time">00:00</span>
          <span>تسجيل</span>
        </div>

        <button class="ibtn" onclick="toggleSearch()" title="بحث">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/>
          </svg>
        </button>

        <div class="status-wrap">
          <button class="ibtn" id="status-btn" onclick="toggleStatusMenu()">
            <div id="my-dot" class="sdot s-online"></div>
          </button>
          <div id="status-menu">
            <div class="sm-item" onclick="setStatus('online')">متصل <div class="sdot s-online"></div></div>
            <div class="sm-item" onclick="setStatus('away')">بعيد <div class="sdot s-away"></div></div>
            <div class="sm-item" onclick="setStatus('busy')">مشغول <div class="sdot s-busy"></div></div>
            <div class="sm-item" onclick="setStatus('invisible')">مخفي <div class="sdot s-offline"></div></div>
          </div>
        </div>

        <input id="username-input" type="text" placeholder="عميل_XXXX" onchange="saveUser()">

        <button id="mob-open-btn" class="ibtn" onclick="openSidebar()">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M4 6h16M4 12h16M4 18h16"/>
          </svg>
        </button>
      </div>
    </div>

    <!-- search -->
    <div id="search-bar">
      <input id="search-input" type="text" placeholder="ابحث في الرسائل..." oninput="doSearch()">
      <span id="search-count"></span>
      <button class="ibtn" onclick="clearSearch()" style="width:28px;height:28px;font-size:14px;">✕</button>
    </div>

    <!-- tabs -->
    <div id="tabs-bar">
      <!-- rendered by JS -->
      <div class="tab-add" onclick="openRoomModal()">+</div>
    </div>
  </div>

  <!-- ── BODY ── -->
  <div id="body">

    <!-- CHAT -->
    <div id="chat-wrap">

      <!-- pinned -->
      <div id="pinned-bar">
        <span id="pinned-icon">📌</span>
        <span id="pinned-text"></span>
        <button id="unpin-btn" onclick="unpinMsg()">✕</button>
      </div>

      <!-- messages -->
      <div id="messages">
        <div class="empty-state">
          <div class="empty-icon">📡</div>
          <p>جارٍ تأسيس الاتصال الآمن...</p>
        </div>
      </div>

      <!-- typing -->
      <div id="typing"></div>

      <!-- input zone -->
      <div id="input-zone">
        <div id="cooldown"></div>

        <!-- reply preview -->
        <div id="reply-preview">
          <div class="rp-line"></div>
          <div class="rp-info">
            <div class="rp-sender">↩ رد على <span id="rp-name"></span></div>
            <div id="rp-text" class="rp-text"></div>
          </div>
          <span class="rp-close" onclick="cancelReply()">✕</span>
        </div>

        <!-- controls -->
        <div class="input-controls">
          <div class="media-group">
            <button id="btn-mic" class="ibtn" onclick="toggleMic()" title="ميكروفون">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/>
                <path d="M19 10v2a7 7 0 0 1-14 0v-2M12 19v4m-4 0h8"/>
              </svg>
            </button>
            <button id="btn-cam" class="ibtn" onclick="toggleCamera()" title="كاميرا">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M15 10l4.553-2.069A1 1 0 0 1 21 8.82v6.36a1 1 0 0 1-1.447.894L15 14M5 18h8a2 2 0 0 0 2-2V8a2 2 0 0 0-2-2H5a2 2 0 0 0-2 2v8a2 2 0 0 0 2 2z"/>
              </svg>
            </button>
            <button id="btn-screen" class="ibtn" onclick="toggleScreenShare()" title="مشاركة الشاشة">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8M12 17v4"/>
              </svg>
            </button>
            <div class="vdivider"></div>
            <button id="btn-emoji" class="ibtn" onclick="toggleEmoji()" title="إيموجي">😊</button>
            <label class="ibtn" title="صورة" style="cursor:pointer">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/>
                <path d="M20.4 14.5 16 10 4 20"/>
              </svg>
              <input type="file" id="img-input" accept="image/*" style="display:none" onchange="sendImage(event)">
            </label>
          </div>

          <div class="input-row">
            <div id="emoji-picker"></div>
            <input id="msg-input" type="text" placeholder="اكتب رسالتك هنا..."
              onkeydown="if(event.key==='Enter')sendMsg()"
              oninput="handleTyping()">
            <button id="send-btn" onclick="sendMsg()">إرسال</button>
          </div>

          <button id="sound-btn" class="ibtn" onclick="toggleSound()" title="الصوت">🔔</button>
        </div>

        <div id="mic-bar-wrap"><div id="mic-bar"></div></div>
      </div>
    </div>

    <!-- SIDEBAR -->
    <div id="sidebar">
      <div class="sb-header">
        <div class="sb-title">
          <span class="live-dot"></span>
          المستخدمون النشطون
        </div>
        <div class="sb-sub">القناة: <span id="chan-name">GLOBAL</span></div>
      </div>

      <div class="sb-tabs">
        <div id="sbt-agents" class="sb-tab active" onclick="sideTab('agents')">المستخدمون</div>
        <div id="sbt-alerts" class="sb-tab" onclick="sideTab('alerts')">التنبيهات <span id="alert-dot"></span></div>
      </div>

      <div class="sb-body">
        <div id="agents-panel">
          <div class="sb-section-header" onclick="toggleList('in-room-list')">
            <span>في القناة</span><span>▾</span>
          </div>
          <div id="in-room-list" class="collapsed-list open agent-list"></div>

          <div id="banned-section">
            <div class="sb-section-header" onclick="toggleList('banned-list')">
              <span style="color:var(--r)">المحظورون</span><span>▾</span>
            </div>
            <div id="banned-list" class="collapsed-list open banned-list"></div>
          </div>

          <div class="sb-section-header" onclick="toggleList('all-list')">
            <span>جميع المستخدمين</span><span>▾</span>
          </div>
          <div id="all-list" class="collapsed-list agent-list"></div>
        </div>

        <div id="alerts-panel" style="display:none">
          <div id="no-alerts" class="no-alerts">لا توجد تنبيهات</div>
          <div id="alerts-list"></div>
        </div>
      </div>
    </div>
  </div><!-- end body -->
</div><!-- end app -->

<!-- ── FIREBASE + LOGIC ── -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
import {
  getFirestore, collection, addDoc, onSnapshot, updateDoc, doc,
  setDoc, deleteDoc, getDocs, getDoc, query, orderBy, limit, serverTimestamp
} from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

const FB = {
  apiKey: "AIzaSyD_HRnXZsmX4cZ1jCfxQ6P7nSLybh8SVNI",
  authDomain: "ugs-chat.firebaseapp.com",
  projectId: "ugs-chat",
  storageBucket: "ugs-chat.firebasestorage.app",
  messagingSenderId: "623227159257",
  appId: "1:623227159257:web:93b6f71d8139756f0874c8"
};

const app = initializeApp(FB);
const auth = getAuth(app);
const db = getFirestore(app);
const NS = 'artifacts/ugs-system-arabic-v1/public/data';

// ── STATE ──────────────────────────────────────────────
let user = null, room = 'GLOBAL';
let allMsgs = [], agents = [], typingUsers = [], bans = [], roomsData = {};
let ownerUids = new Set();
let msgUnsub = null;
let quota = false;
let tabs = [
  { id: 'ANNOUNCEMENTS', name: 'الإعلانات' },
  { id: 'GLOBAL', name: 'العامة' }
];
let unread = {};
let replyTo = null;
let searchQ = '';
let myStatus = localStorage.getItem('c_status') || 'online';
let editId = null;
let soundOn = localStorage.getItem('c_sound') !== 'off';
let typingTO = null, isTyping = false;
let msgTimes = [];
const RATE_MAX = 4, RATE_WIN = 8000;
let cooldownTimer = null;
let rAF = null;
let presKey = '';

// media
let micStream = null, camStream = null, screenStream = null;
let micOn = false, camOn = false, screenOn = false;
let recStart = null, recTick = null;
let audioCtx = null, analyser = null, vizAF = null;

// ── UTILS ──────────────────────────────────────────────
const esc = s => s ? s.replace(/[&<>"']/g, m =>
  ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'})[m]) : '';

const timeAgo = t => {
  const d = Date.now() - t;
  if (d < 60000) return 'الآن';
  if (d < 3600000) return `${Math.floor(d/60000)}د`;
  if (d < 86400000) return `${Math.floor(d/3600000)}س`;
  return new Date(t).toLocaleDateString('ar-SA', {month:'short', day:'numeric'});
};

const fmt = s => {
  const m = Math.floor(s / 60), sec = Math.floor(s % 60);
  return `${String(m).padStart(2,'0')}:${String(sec).padStart(2,'0')}`;
};

const isOwner = () => !!(user && ownerUids.has(user.uid));
const isCreator = () => roomsData[room]?.createdBy === user?.uid;
const canMod = () => isOwner() || isCreator();

const col = path => collection(db, ...path.split('/'));
const docRef = path => doc(db, ...path.split('/'));

const handleErr = e => {
  const s = ((e?.message||'')+(e?.code||'')).toLowerCase();
  if (s.includes('quota') || s.includes('exhausted') || s.includes('resource-exhausted')) {
    quota = true;
    document.getElementById('quota-overlay').classList.add('show');
    document.querySelectorAll('input,button').forEach(el => el.disabled = true);
  }
};

// ── MODALS ─────────────────────────────────────────────
window.closeAlert = () => document.getElementById('alert-modal').classList.remove('open');
const showAlert = msg => {
  document.getElementById('alert-msg').textContent = msg;
  document.getElementById('alert-modal').classList.add('open');
};

window.openRoomModal = () => { if (quota) return; rsGo(1); document.getElementById('room-modal').classList.add('open'); };
window.closeRoomModal = () => document.getElementById('room-modal').classList.remove('open');
window.rsGo = n => { [1,2,3].forEach(i => document.getElementById(`rs${i}`)?.classList.toggle('active', i===n)); };

window.doCreate = async () => {
  const name = document.getElementById('create-name').value.trim().toUpperCase();
  if (!name) return;
  const locked = document.getElementById('create-locked').checked;
  try {
    await setDoc(docRef(`${NS}/ugs_rooms/${name}`), { id: name, createdBy: user.uid, locked, frozen: false });
    pushTab(name, name, 'room');
    closeRoomModal();
    switchChat(name);
  } catch(e) { handleErr(e); }
};

window.doJoin = async () => {
  const name = document.getElementById('join-name').value.trim().toUpperCase();
  if (!name) return;
  try {
    const snap = await getDoc(docRef(`${NS}/ugs_rooms/${name}`));
    if (snap.exists() && snap.data().locked && !isOwner() && snap.data().createdBy !== user.uid) {
      showAlert('هذه القناة مقفلة. تم إرسال طلب انضمام.');
    } else {
      pushTab(name, name, 'room');
      closeRoomModal();
      switchChat(name);
    }
  } catch(e) { handleErr(e); }
};

// ── TABS ───────────────────────────────────────────────
const pushTab = (id, name, type) => {
  if (!tabs.find(t => t.id === id)) {
    tabs.push({ id, name, type });
    localStorage.setItem('c_tabs', JSON.stringify(tabs));
  }
};

const CORE = ['GLOBAL','ANNOUNCEMENTS'];

const renderTabs = () => {
  const bar = document.getElementById('tabs-bar');
  let html = tabs.map(t => {
    const u = unread[t.id] || 0;
    const badge = u ? `<span class="badge">${u > 99 ? '99+' : u}</span>` : '';
    const close = !CORE.includes(t.id)
      ? `<span class="tab-close" onclick="event.stopPropagation();closeTab('${t.id}')">×</span>` : '';
    const icon = t.type==='dm' ? '📨' : t.id==='ANNOUNCEMENTS' ? '📢' : '#';
    return `<div class="tab${room===t.id?' active':''}" onclick="switchChat('${t.id}')">${icon} ${esc(t.name)}${badge}${close}</div>`;
  }).join('');
  html += `<div class="tab-add" onclick="openRoomModal()">+</div>`;
  bar.innerHTML = html;
};

window.closeTab = id => {
  tabs = tabs.filter(t => t.id !== id);
  localStorage.setItem('c_tabs', JSON.stringify(tabs));
  if (room === id) switchChat('GLOBAL');
  else renderTabs();
};

// ── CHAT SWITCH ────────────────────────────────────────
window.switchChat = id => {
  room = id; editId = null; cancelReply();
  unread[id] = 0;
  document.getElementById('chan-name').textContent = id.startsWith('DM_') ? 'رسالة مباشرة' : id;
  renderTabs();
  renderPinned();
  if (msgUnsub) { msgUnsub(); msgUnsub = null; }
  allMsgs = [];
  schedRender();
  msgUnsub = onSnapshot(
    query(col(`${NS}/ugs_messages_${id}`), orderBy('timestamp'), limit(80)),
    snap => {
      allMsgs = snap.docs.map(d => ({ id: d.id, ...d.data() }));
      const lr = parseInt(localStorage.getItem(`c_read_${id}`) || '0');
      if (id === room) {
        localStorage.setItem(`c_read_${id}`, Date.now());
        unread[id] = 0;
      } else {
        unread[id] = allMsgs.filter(m => m.timestamp > lr && m.userId !== user?.uid && !m.deleted).length;
      }
      updateTitle();
      schedRender();
    }, handleErr);
  updatePresence();
};

const updateTitle = () => {
  const total = Object.values(unread).reduce((s,v) => s+v, 0);
  document.title = total ? `(${total}) COMM_LINK` : 'COMM_LINK | نظام UGS';
};

// ── RENDER MESSAGES ────────────────────────────────────
const schedRender = () => {
  if (rAF) cancelAnimationFrame(rAF);
  rAF = requestAnimationFrame(() => { rAF = null; renderMsgs(); });
};

const hi = t => {
  if (!searchQ) return esc(t);
  return esc(t).replace(new RegExp(`(${esc(searchQ)})`, 'gi'), '<mark>$1</mark>');
};

const renderMsgs = () => {
  const c = document.getElementById('messages');
  if (!c) return;
  const atBottom = c.scrollHeight - c.scrollTop - c.clientHeight < 90;

  let msgs = allMsgs.slice().sort((a,b) => a.timestamp - b.timestamp);
  if (searchQ) msgs = msgs.filter(m => !m.deleted && !m.isImage && m.text?.toLowerCase().includes(searchQ));

  const cnt = document.getElementById('search-count');
  if (cnt) cnt.textContent = searchQ ? `${msgs.length} نتيجة` : '';

  if (!msgs.length) {
    c.innerHTML = `<div class="empty-state"><div class="empty-icon">${searchQ?'🔍':'💬'}</div><p>${searchQ?`لا نتائج لـ "${esc(searchQ)}"` :'لا توجد رسائل بعد'}</p></div>`;
    return;
  }

  let html = '';
  for (let i = 0; i < msgs.length; i++) {
    const m = msgs[i];
    const prev = i > 0 ? msgs[i-1] : null;
    const isMe = user && m.userId === user.uid;
    const consec = prev && prev.userId === m.userId && !prev.deleted && !m.deleted && (m.timestamp - prev.timestamp < 300000);

    if (m.isSys) {
      html += `<div class="sys-msg"><span>${esc(m.text)}</span></div>`;
      continue;
    }

    const replyHtml = m.replyTo && !m.deleted ? `
      <div class="reply-ref" onclick="scrollToMsg('${m.replyTo.id}')">
        <div class="reply-ref-sender">${esc(m.replyTo.sender||'')}</div>
        <div class="reply-ref-text">${esc((m.replyTo.text||'').substring(0,60))}</div>
      </div>` : '';

    const bodyHtml = m.deleted
      ? '<span class="deleted">[تم حذف هذه الرسالة]</span>'
      : m.isImage
        ? `<img src="${m.text}" loading="lazy" alt="صورة">`
        : hi(m.text);

    const metaHtml = !consec ? `
      <div class="msg-meta">
        <span class="msg-sender ${isMe?'':'other'}">${esc(m.sender||'مستخدم')}${m.isOwner?'<span class="owner-tag">مالك</span>':''}</span>
        <span class="msg-time">${timeAgo(m.timestamp)}${m.edited?'<span class="edited-tag"> (معدّل)</span>':''}</span>
      </div>` : '';

    const actHtml = !m.deleted && !m.isSys ? `
      <div class="msg-actions">
        <span class="act-btn" onclick="startReply('${m.id}')">↩ رد</span>
        ${canMod() && !m.isImage ? `<span class="act-btn pin" onclick="pinMsg('${m.id}')">📌</span>` : ''}
        ${(isMe || canMod()) ? `
          ${!m.isImage ? `<span class="act-btn edit" onclick="startEdit('${m.id}')">تعديل</span>` : ''}
          <span class="act-btn del" onclick="delMsg('${m.id}')">حذف</span>
        ` : ''}
      </div>` : '';

    html += `<div data-id="${m.id}" class="msg-row${isMe?' mine':''}${!consec?' gap-top':''}">
      <div class="msg-content">
        ${metaHtml}
        <div class="bubble${isMe?' mine':' other'}${m.deleted?' deleted':''}">
          ${replyHtml}${bodyHtml}
        </div>
        ${actHtml}
      </div>
    </div>`;
  }
  c.innerHTML = html;
  if (atBottom) c.scrollTop = c.scrollHeight;
};

window.startEdit = id => {
  const m = allMsgs.find(m => m.id === id);
  if (!m || m.isImage || m.isSys) return;
  document.getElementById('msg-input').value = m.text;
  editId = id;
  const btn = document.getElementById('send-btn');
  btn.textContent = 'تحديث'; btn.classList.add('editing');
  document.getElementById('msg-input').focus();
};

window.delMsg = async id => {
  if (quota) return;
  try { await updateDoc(docRef(`${NS}/ugs_messages_${room}/${id}`), { deleted: true }); }
  catch(e) { handleErr(e); }
};

window.scrollToMsg = id => {
  const el = document.querySelector(`[data-id="${id}"]`);
  if (el) {
    el.scrollIntoView({ behavior:'smooth', block:'center' });
    el.querySelector('.bubble').style.transition = 'background 0.3s';
    el.querySelector('.bubble').style.background = 'rgba(57,255,122,0.12)';
    setTimeout(() => { if(el.querySelector('.bubble')) el.querySelector('.bubble').style.background = ''; }, 1500);
  }
};

// ── SEND ───────────────────────────────────────────────
window.sendMsg = async () => {
  if (quota) return;
  const inp = document.getElementById('msg-input');
  const text = inp.value.trim();
  if (!text || !user) return;

  if (!isOwner()) {
    const now = Date.now();
    msgTimes = msgTimes.filter(t => now - t < RATE_WIN);
    if (msgTimes.length >= RATE_MAX) {
      startCooldown();
      inp.classList.add('shake');
      setTimeout(() => inp.classList.remove('shake'), 400);
      return;
    }
    msgTimes.push(now);
  }

  const sender = localStorage.getItem('c_user') || 'مستخدم';
  inp.value = '';
  clearTypingDoc();

  try {
    if (editId) {
      await updateDoc(docRef(`${NS}/ugs_messages_${room}/${editId}`), { text, edited: true });
      editId = null;
      const btn = document.getElementById('send-btn');
      btn.textContent = 'إرسال'; btn.classList.remove('editing');
    } else {
      const data = { text, sender, room, userId: user.uid, timestamp: Date.now(), isOwner: isOwner() };
      if (replyTo) { data.replyTo = replyTo; cancelReply(); }
      await addDoc(col(`${NS}/ugs_messages_${room}`), data);
    }
  } catch(e) { handleErr(e); }
};

window.sendImage = event => {
  const file = event.target.files[0];
  if (!file || quota) return;
  const reader = new FileReader();
  reader.onload = e => {
    const img = new Image();
    img.onload = async () => {
      const c = document.createElement('canvas');
      let w = img.width, h = img.height, max = 800;
      if (w > max || h > max) { if (w > h) { h = (h/w)*max; w = max; } else { w = (w/h)*max; h = max; } }
      c.width = w; c.height = h;
      c.getContext('2d').drawImage(img, 0, 0, w, h);
      const b64 = c.toDataURL('image/jpeg', 0.8);
      try {
        await addDoc(col(`${NS}/ugs_messages_${room}`), {
          text: b64, isImage: true,
          sender: localStorage.getItem('c_user') || 'مستخدم',
          room, userId: user.uid, timestamp: Date.now()
        });
      } catch(err) { handleErr(err); }
    };
    img.src = e.target.result;
  };
  reader.readAsDataURL(file);
};

// ── RATE LIMIT ────────────────────────────────────────
const startCooldown = () => {
  const bar = document.getElementById('cooldown');
  const inp = document.getElementById('msg-input');
  const btn = document.getElementById('send-btn');
  bar.style.display = 'block'; bar.style.width = '100%';
  if (cooldownTimer) clearInterval(cooldownTimer);
  const end = msgTimes[0] + RATE_WIN;
  inp.disabled = true; btn.disabled = true;
  inp.placeholder = 'انتظر قليلاً...';
  cooldownTimer = setInterval(() => {
    const r = end - Date.now();
    if (r <= 0) {
      clearInterval(cooldownTimer); bar.style.display = 'none';
      inp.disabled = false; btn.disabled = false;
      inp.placeholder = 'اكتب رسالتك هنا...';
    } else {
      bar.style.width = `${(r/RATE_WIN)*100}%`;
    }
  }, 100);
};

// ── REPLY ─────────────────────────────────────────────
window.startReply = id => {
  const m = allMsgs.find(m => m.id === id);
  if (!m || m.deleted || m.isSys) return;
  replyTo = { id, sender: m.sender, text: m.text || '' };
  const bar = document.getElementById('reply-preview');
  bar.classList.add('show');
  document.getElementById('rp-name').textContent = m.sender || 'مستخدم';
  document.getElementById('rp-text').textContent = (m.text||'').substring(0,80);
  document.getElementById('msg-input').focus();
};
window.cancelReply = () => {
  replyTo = null;
  document.getElementById('reply-preview').classList.remove('show');
};

// ── PIN ───────────────────────────────────────────────
window.pinMsg = async id => {
  if (!canMod() || quota) return;
  const m = allMsgs.find(m => m.id === id);
  if (!m || m.deleted || m.isSys) return;
  try {
    await updateDoc(docRef(`${NS}/ugs_rooms/${room}`), { pinnedMsg: { id, text: m.text, sender: m.sender||'' } });
  } catch(e) { handleErr(e); }
};
window.unpinMsg = async () => {
  if (!canMod() || quota) return;
  try { await updateDoc(docRef(`${NS}/ugs_rooms/${room}`), { pinnedMsg: null }); }
  catch(e) { handleErr(e); }
};
const renderPinned = () => {
  const bar = document.getElementById('pinned-bar');
  const txt = document.getElementById('pinned-text');
  const btn = document.getElementById('unpin-btn');
  const pin = roomsData[room]?.pinnedMsg;
  if (pin) {
    bar.classList.add('show');
    txt.textContent = `${pin.sender}: ${pin.text}`;
    btn.style.display = canMod() ? 'block' : 'none';
  } else {
    bar.classList.remove('show');
  }
};

// ── TYPING ────────────────────────────────────────────
window.handleTyping = async () => {
  if (!user || quota) return;
  if (!isTyping) {
    isTyping = true;
    try {
      await setDoc(docRef(`${NS}/ugs_typing/${user.uid}`), {
        username: localStorage.getItem('c_user') || 'مستخدم',
        room, clientTimestamp: Date.now()
      });
    } catch(_) {}
  }
  clearTimeout(typingTO);
  typingTO = setTimeout(clearTypingDoc, 2500);
};
const clearTypingDoc = async () => {
  clearTimeout(typingTO); isTyping = false;
  try { await deleteDoc(docRef(`${NS}/ugs_typing/${user.uid}`)); } catch(_) {}
};
const renderTyping = () => {
  const el = document.getElementById('typing');
  const others = typingUsers.filter(t => t.room === room && t.id !== user?.uid && Date.now() - (t.clientTimestamp||0) < 4000);
  el.textContent = others.length ? `${others.map(t=>t.username).join('، ')} يكتب${others.length>1?'ون':''}...` : '';
};

// ── AGENTS ────────────────────────────────────────────
const dotClass = a => {
  const s = a.status||'online';
  if (s==='invisible') return 's-offline';
  if (s==='busy') return 's-busy';
  if (s==='away') return 's-away';
  return a.room === room ? 's-online' : '';
};

const agentHtml = a => {
  if (a.status==='invisible' && !isOwner() && a.id !== user?.uid) return '';
  const isMe = a.id === user?.uid;
  const dBtn = !isMe ? `<div class="agent-btns">
    <span class="ag-btn dm" onclick="openDM('${a.id}','${esc(a.username)}')">DM</span>
    ${canMod() ? `<span class="ag-btn kick" onclick="modUser('${a.id}','kick')">طرد</span>
    <span class="ag-btn ban" onclick="modUser('${a.id}','${esc(a.username)}','ban')">حظر</span>` : ''}
  </div>` : '';
  return `<div class="agent-item">
    <div class="sdot ${dotClass(a)}"></div>
    <span class="agent-name ${isMe?'me':'other'}">${esc(a.username)}</span>
    ${dBtn}
  </div>`;
};

const renderAgents = () => {
  const inRoom = document.getElementById('in-room-list');
  const all = document.getElementById('all-list');
  if (inRoom) inRoom.innerHTML = agents.filter(a => a.room===room).map(agentHtml).join('') || '<div style="color:var(--text-faint);font-size:10px;text-align:center;padding:12px;">لا أحد هنا بعد</div>';
  if (all) all.innerHTML = agents.map(agentHtml).join('');
};

window.openDM = (uid, uname) => {
  if (!user || uid === user.uid) return;
  const dmId = 'DM_' + [user.uid, uid].sort().join('_');
  pushTab(dmId, `${uname}`, 'dm');
  switchChat(dmId);
};

window.modUser = async (uid, usernameOrType, typeOrUndefined) => {
  if (!canMod() || quota) return;
  const type = typeOrUndefined || usernameOrType;
  const uname = typeOrUndefined ? usernameOrType : '';
  try {
    if (type === 'kick') {
      await setDoc(docRef(`${NS}/ugs_kicks/${uid}`), { roomId: room, timestamp: Date.now() });
    } else if (type === 'ban') {
      await setDoc(docRef(`${NS}/ugs_bans/${room}_${uid}`), { roomId: room, userId: uid, username: uname, bannedBy: user.uid });
    } else if (type === 'unban') {
      await deleteDoc(docRef(`${NS}/ugs_bans/${uid}`));
    }
  } catch(e) { handleErr(e); }
};

const renderBans = () => {
  const myBans = bans.filter(b => isOwner() || b.roomId === room);
  const sec = document.getElementById('banned-section');
  const list = document.getElementById('banned-list');
  if (sec) sec.classList.toggle('show', myBans.length > 0);
  if (list) list.innerHTML = myBans.map(b => `
    <div class="banned-item">
      <span class="banned-name">${esc(b.username)} (${b.roomId})</span>
      <span class="unban-btn" onclick="modUser('${b.id}_${b.userId}','unban')">رفع</span>
    </div>`).join('');
};

// ── PRESENCE ──────────────────────────────────────────
window.updatePresence = async () => {
  if (!user || quota || document.hidden) return;
  const u = localStorage.getItem('c_user') || 'مستخدم';
  const key = `${u}::${room}`;
  if (key === presKey) return;
  presKey = key;
  try {
    await setDoc(docRef(`${NS}/ugs_presence/${user.uid}`), {
      username: u, lastActive: Date.now(), id: user.uid, room, status: myStatus
    });
  } catch(e) { handleErr(e); }
};
document.addEventListener('visibilitychange', () => { if (!document.hidden) { presKey = ''; updatePresence(); } });

window.saveUser = () => {
  const v = document.getElementById('username-input').value.trim();
  if (!v || quota) return;
  localStorage.setItem('c_user', v);
  presKey = '';
  updatePresence();
  schedRender();
};

// ── STATUS ────────────────────────────────────────────
window.toggleStatusMenu = () => document.getElementById('status-menu').classList.toggle('open');
window.setStatus = async s => {
  myStatus = s;
  localStorage.setItem('c_status', s);
  const map = { online:'s-online', away:'s-away', busy:'s-busy', invisible:'s-offline' };
  document.getElementById('my-dot').className = `sdot ${map[s]}`;
  document.getElementById('status-menu').classList.remove('open');
  if (user && !quota && s !== 'invisible') {
    try { await updateDoc(docRef(`${NS}/ugs_presence/${user.uid}`), { status: s }); } catch(_) {}
  }
};
document.addEventListener('click', e => {
  const menu = document.getElementById('status-menu');
  const btn = document.getElementById('status-btn');
  if (!menu?.contains(e.target) && !btn?.contains(e.target)) menu?.classList.remove('open');
});

// ── SEARCH ────────────────────────────────────────────
window.toggleSearch = () => {
  const bar = document.getElementById('search-bar');
  bar.classList.toggle('open');
  if (bar.classList.contains('open')) document.getElementById('search-input').focus();
  else clearSearch();
};
window.doSearch = () => { searchQ = (document.getElementById('search-input').value||'').trim().toLowerCase(); schedRender(); };
window.clearSearch = () => {
  searchQ = '';
  document.getElementById('search-input').value = '';
  document.getElementById('search-bar').classList.remove('open');
  schedRender();
};

// ── EMOJI ─────────────────────────────────────────────
const EMOJIS = ['😀','😂','😍','🤔','😎','😭','🥺','😤','🤯','👍','👎','👏','🙌','🤝','🫡','✌️','💪','🫶','🔥','💯','⚡','❤️','💀','👀','🎯','🚀','💥','✅','❌','⚠️','📡','🔒','🔓','🛡️','⚙️','📋','🗑️','🔔','📷','🎤','🖥️'];
window.toggleEmoji = () => {
  const p = document.getElementById('emoji-picker');
  if (!p.innerHTML) p.innerHTML = EMOJIS.map(e => `<button class="emoji-btn" onclick="insertEmoji('${e}')">${e}</button>`).join('');
  p.classList.toggle('open');
};
window.insertEmoji = e => {
  const inp = document.getElementById('msg-input');
  const s = inp.selectionStart, end = inp.selectionEnd;
  inp.value = inp.value.slice(0,s) + e + inp.value.slice(end);
  inp.selectionStart = inp.selectionEnd = s + e.length;
  inp.focus();
  document.getElementById('emoji-picker').classList.remove('open');
};
document.addEventListener('click', e => {
  const p = document.getElementById('emoji-picker');
  const b = document.getElementById('btn-emoji');
  if (p && !p.contains(e.target) && e.target !== b) p.classList.remove('open');
}, true);

// ── SOUND ─────────────────────────────────────────────
window.toggleSound = () => {
  soundOn = !soundOn;
  localStorage.setItem('c_sound', soundOn ? 'on' : 'off');
  document.getElementById('sound-btn').textContent = soundOn ? '🔔' : '🔕';
};

// ── SIDEBAR TABS ──────────────────────────────────────
window.sideTab = tab => {
  ['agents','alerts'].forEach(t => {
    const panel = document.getElementById(`${t}-panel`);
    const btn = document.getElementById(`sbt-${t}`);
    if (panel) panel.style.display = t === tab ? 'block' : 'none';
    if (btn) btn.classList.toggle('active', t === tab);
  });
  if (tab === 'alerts') document.getElementById('alert-dot').classList.remove('show');
};
window.toggleList = id => {
  document.getElementById(id)?.classList.toggle('open');
};

// ── MOBILE SIDEBAR ─────────────────────────────────────
window.openSidebar = () => {
  document.getElementById('sidebar').classList.add('open');
  document.getElementById('mob-overlay').classList.add('show');
};
window.closeSidebar = () => {
  document.getElementById('sidebar').classList.remove('open');
  document.getElementById('mob-overlay').classList.remove('show');
};

// ── MEDIA ─────────────────────────────────────────────
const startRec = () => {
  if (!recStart) recStart = Date.now();
  if (!recTick) {
    recTick = setInterval(() => {
      document.getElementById('rec-time').textContent = fmt((Date.now() - recStart) / 1000);
    }, 1000);
  }
  document.getElementById('rec-badge').classList.add('show');
};
const checkStopRec = () => {
  if (!micOn && !camOn && !screenOn) {
    document.getElementById('rec-badge').classList.remove('show');
    clearInterval(recTick); recTick = null; recStart = null;
  }
};

window.toggleMic = async () => {
  const btn = document.getElementById('btn-mic');
  if (micOn) {
    if (micStream) { micStream.getTracks().forEach(t => t.stop()); micStream = null; }
    micOn = false; btn.classList.remove('on');
    document.getElementById('mic-bar-wrap').classList.remove('show');
    if (vizAF) { cancelAnimationFrame(vizAF); vizAF = null; }
    checkStopRec();
  } else {
    try {
      micStream = await navigator.mediaDevices.getUserMedia({ audio: true });
      micOn = true; btn.classList.add('on');
      document.getElementById('mic-bar-wrap').classList.add('show');
      audioCtx = new (window.AudioContext||window.webkitAudioContext)();
      const src = audioCtx.createMediaStreamSource(micStream);
      analyser = audioCtx.createAnalyser(); analyser.fftSize = 256;
      src.connect(analyser);
      const data = new Uint8Array(analyser.frequencyBinCount);
      const bar = document.getElementById('mic-bar');
      const tick = () => {
        analyser.getByteFrequencyData(data);
        const avg = data.reduce((a,b) => a+b, 0) / data.length;
        bar.style.width = `${Math.min(avg*2,100)}%`;
        vizAF = requestAnimationFrame(tick);
      };
      tick();
      startRec();
    } catch(e) { showAlert('❌ تعذّر الوصول للميكروفون. تحقق من إذن المتصفح.'); }
  }
};

window.toggleCamera = async () => {
  const btn = document.getElementById('btn-cam');
  if (camOn) {
    if (camStream) { camStream.getTracks().forEach(t => t.stop()); camStream = null; }
    camOn = false; btn.classList.remove('on');
    document.getElementById('cam-mini').classList.remove('show');
    document.getElementById('cam-video').srcObject = null;
    checkStopRec();
  } else {
    try {
      camStream = await navigator.mediaDevices.getUserMedia({ video: true });
      camOn = true; btn.classList.add('on');
      const vid = document.getElementById('cam-video');
      vid.srcObject = camStream;
      document.getElementById('cam-mini').classList.add('show');
      startRec();
    } catch(e) { showAlert('❌ تعذّر الوصول للكاميرا.'); }
  }
};

window.toggleScreenShare = async () => {
  if (screenOn) { stopScreenShare(); return; }
  try {
    if (!navigator.mediaDevices.getDisplayMedia) { showAlert('❌ المتصفح لا يدعم مشاركة الشاشة.'); return; }
    screenStream = await navigator.mediaDevices.getDisplayMedia({ video: { cursor: 'always' }, audio: true });
    screenOn = true;
    const btn = document.getElementById('btn-screen');
    btn.classList.add('on-r');
    document.getElementById('screen-video').srcObject = screenStream;
    document.getElementById('screen-preview').classList.add('show');
    screenStream.getVideoTracks()[0].onended = () => stopScreenShare();
    startRec();
    await sysMsg(`📺 ${localStorage.getItem('c_user')||'مستخدم'} بدأ مشاركة الشاشة`);
  } catch(e) { if (e.name !== 'NotAllowedError') showAlert('❌ فشل تشغيل مشاركة الشاشة'); }
};

window.stopScreenShare = async () => {
  if (screenStream) { screenStream.getTracks().forEach(t => t.stop()); screenStream = null; }
  screenOn = false;
  document.getElementById('btn-screen').classList.remove('on-r');
  document.getElementById('screen-preview').classList.remove('show');
  document.getElementById('screen-video').srcObject = null;
  checkStopRec();
  await sysMsg(`🖥️ ${localStorage.getItem('c_user')||'مستخدم'} أوقف مشاركة الشاشة`);
};

const sysMsg = async text => {
  if (!user || quota) return;
  try {
    await addDoc(col(`${NS}/ugs_messages_${room}`), {
      text, sender: 'النظام', isSys: true, userId: user.uid, timestamp: Date.now(), room
    });
  } catch(_) {}
};

// ── KEYBOARD ──────────────────────────────────────────
document.addEventListener('keydown', e => {
  if (e.key !== 'Escape') return;
  if (replyTo) { cancelReply(); return; }
  if (editId) {
    editId = null;
    document.getElementById('msg-input').value = '';
    const btn = document.getElementById('send-btn');
    btn.textContent = 'إرسال'; btn.classList.remove('editing');
    return;
  }
  clearSearch();
  document.getElementById('emoji-picker').classList.remove('open');
});

// ── UNLOAD ────────────────────────────────────────────
window.addEventListener('beforeunload', async () => {
  if (user) try { await deleteDoc(docRef(`${NS}/ugs_presence/${user.uid}`)); } catch(_) {}
  if (micStream) micStream.getTracks().forEach(t => t.stop());
  if (camStream) camStream.getTracks().forEach(t => t.stop());
  if (screenStream) screenStream.getTracks().forEach(t => t.stop());
});

// ── INIT ──────────────────────────────────────────────
document.getElementById('sound-btn').textContent = soundOn ? '🔔' : '🔕';
document.getElementById('my-dot').className = `sdot ${{ online:'s-online', away:'s-away', busy:'s-busy', invisible:'s-offline' }[myStatus] || 's-online'}`;

try { await signInAnonymously(auth); } catch(e) { console.error(e); }

onAuthStateChanged(auth, async u => {
  user = u;
  if (!u) return;

  let name = localStorage.getItem('c_user') || `عميل_${Math.floor(1000+Math.random()*9000)}`;
  localStorage.setItem('c_user', name);
  document.getElementById('username-input').value = name;

  // Load saved tabs
  try {
    const saved = localStorage.getItem('c_tabs');
    if (saved) { const s = JSON.parse(saved); if (s.length) tabs = s; }
  } catch(_) {}

  // Listeners
  onSnapshot(col(`${NS}/ugs_owners`), s => { ownerUids = new Set(s.docs.map(d => d.id)); }, handleErr);

  onSnapshot(col(`${NS}/ugs_rooms`), s => {
    s.docs.forEach(d => roomsData[d.id] = d.data());
    renderPinned();
  }, handleErr);

  onSnapshot(col(`${NS}/ugs_presence`), s => {
    const now = Date.now();
    agents = s.docs.map(d => d.data()).filter(a => now - a.lastActive < 12*60*1000);
    renderAgents();
  }, handleErr);

  onSnapshot(col(`${NS}/ugs_typing`), s => {
    typingUsers = s.docs.map(d => ({ id: d.id, ...d.data() }));
    renderTyping();
  }, handleErr);

  onSnapshot(col(`${NS}/ugs_bans`), s => {
    bans = s.docs.map(d => ({ id: d.id, ...d.data() }));
    renderBans();
  }, handleErr);

  onSnapshot(docRef(`${NS}/ugs_kicks/${u.uid}`), s => {
    if (s.exists() && s.data().roomId === room) {
      showAlert('تم طردك من هذه القناة.');
      deleteDoc(docRef(`${NS}/ugs_kicks/${u.uid}`)).catch(()=>{});
      switchChat('GLOBAL');
    }
  }, handleErr);

  switchChat('GLOBAL');
  updatePresence();
  setInterval(updatePresence, 300000);
  setInterval(schedRender, 30000);
});
</script>
</body>
</html>
