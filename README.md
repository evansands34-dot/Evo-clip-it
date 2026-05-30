<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
<title>Evo – Voice Clip Recorder</title>
<style>
:root{
  --bg:#0a0a0f;--surface:#12121a;--surface2:#1a1a26;
  --accent:#7c3aed;--accent2:#a855f7;--accent-glow:rgba(124,58,237,0.4);
  --red:#ef4444;--green:#22c55e;--text:#f1f5f9;--muted:#64748b;
  --border:rgba(124,58,237,0.25);--radius:16px;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:#0d0d18;color:var(--text);font-family:'Segoe UI',system-ui,sans-serif;
  min-height:100vh;display:flex;justify-content:center;align-items:center;}

/* Phone shell */
.phone{width:390px;height:844px;background:var(--surface);border-radius:44px;
  overflow:hidden;position:relative;
  box-shadow:0 0 80px rgba(124,58,237,0.35),0 40px 80px rgba(0,0,0,0.8);
  border:1.5px solid var(--border);display:flex;flex-direction:column;}

/* Screens */
.screen{position:absolute;inset:0;display:flex;flex-direction:column;
  transition:opacity 0.4s,transform 0.4s;}
.screen.hidden{opacity:0;pointer-events:none;transform:translateY(20px);}

/* ── AUTH ── */
#auth-screen{background:linear-gradient(160deg,#0d0d18 0%,#12001f 60%,#0a0a0f 100%);
  justify-content:center;align-items:center;padding:40px 28px;gap:0;}
.auth-logo{width:80px;height:80px;background:linear-gradient(135deg,var(--accent),var(--accent2));
  border-radius:24px;display:flex;align-items:center;justify-content:center;
  font-size:36px;margin-bottom:20px;box-shadow:0 0 40px var(--accent-glow);}
.auth-title{font-size:36px;font-weight:800;letter-spacing:-1px;margin-bottom:6px;}
.auth-title span{background:linear-gradient(90deg,var(--accent2),#e879f9);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.auth-sub{color:var(--muted);font-size:14px;margin-bottom:36px;text-align:center;}
.tabs-row{display:flex;background:var(--surface2);border-radius:12px;padding:4px;
  margin-bottom:28px;width:100%;}
.tab-btn{flex:1;padding:10px;border:none;background:none;color:var(--muted);
  font-size:14px;font-weight:600;border-radius:10px;cursor:pointer;transition:all 0.2s;}
.tab-btn.active{background:var(--accent);color:white;}
.auth-form{display:flex;flex-direction:column;gap:14px;width:100%;}
.input-group label{font-size:12px;color:var(--muted);margin-bottom:6px;display:block;
  font-weight:600;letter-spacing:0.5px;text-transform:uppercase;}
.input-group input{width:100%;padding:14px 16px;background:var(--surface2);
  border:1.5px solid var(--border);border-radius:12px;color:var(--text);
  font-size:15px;outline:none;transition:border-color 0.2s;}
.input-group input:focus{border-color:var(--accent2);}
.input-group input::placeholder{color:var(--muted);}
.auth-btn{width:100%;padding:16px;margin-top:6px;
  background:linear-gradient(135deg,var(--accent),var(--accent2));
  border:none;border-radius:14px;color:white;font-size:16px;font-weight:700;
  cursor:pointer;box-shadow:0 4px 24px var(--accent-glow);transition:transform 0.15s;}
.auth-btn:active{transform:scale(0.97);}
.auth-note{font-size:12px;color:var(--muted);text-align:center;margin-top:16px;}

/* ── MAIN ── */
#main-screen{background:var(--bg);}

.status-bar{display:flex;justify-content:space-between;align-items:center;
  padding:14px 24px 8px;font-size:12px;font-weight:600;color:var(--text);}
.status-right{display:flex;gap:6px;align-items:center;}
.rec-dot{width:8px;height:8px;border-radius:50%;background:var(--red);animation:pulse-red 1.2s infinite;}
@keyframes pulse-red{0%,100%{opacity:1;box-shadow:0 0 0 0 rgba(239,68,68,0.6)}50%{opacity:0.7;box-shadow:0 0 0 6px rgba(239,68,68,0)}}

.app-header{display:flex;align-items:center;justify-content:space-between;padding:8px 20px 10px;}
.app-logo{display:flex;align-items:center;gap:10px;}
.app-logo .icon{width:36px;height:36px;background:linear-gradient(135deg,var(--accent),var(--accent2));
  border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:18px;}
.app-logo .name{font-size:22px;font-weight:800;letter-spacing:-0.5px;}
.app-logo .name span{background:linear-gradient(90deg,var(--accent2),#e879f9);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.user-chip{display:flex;align-items:center;gap:8px;background:var(--surface2);
  border:1px solid var(--border);border-radius:20px;padding:6px 12px 6px 8px;}
.avatar{width:26px;height:26px;background:linear-gradient(135deg,var(--accent),#e879f9);
  border-radius:50%;display:flex;align-items:center;justify-content:center;
  font-size:11px;font-weight:700;}

/* Recording Banner */
.rec-banner{margin:0 16px 10px;background:linear-gradient(135deg,rgba(239,68,68,0.12),rgba(239,68,68,0.05));
  border:1px solid rgba(239,68,68,0.3);border-radius:12px;padding:10px 14px;
  display:flex;align-items:center;gap:10px;}
.rec-info{flex:1;}
.rec-label{font-size:13px;font-weight:700;color:#f87171;}
.rec-sub{font-size:10px;color:var(--muted);margin-top:2px;}
.rec-timer{font-size:14px;font-weight:700;font-family:monospace;color:#f87171;}

/* Screen Preview */
.preview-wrap{margin:0 16px 10px;border-radius:12px;overflow:hidden;
  background:#000;position:relative;border:1.5px solid var(--border);}
#screen-preview{width:100%;height:110px;object-fit:cover;display:block;}
.preview-label{position:absolute;top:6px;left:8px;background:rgba(0,0,0,0.7);
  color:#f87171;font-size:10px;font-weight:700;padding:3px 8px;border-radius:6px;
  display:flex;align-items:center;gap:4px;}
.preview-dot{width:6px;height:6px;border-radius:50%;background:#f87171;animation:pulse-red 1s infinite;}
.no-preview{height:110px;display:flex;flex-direction:column;align-items:center;
  justify-content:center;gap:6px;color:var(--muted);font-size:12px;text-align:center;padding:8px;}
.start-rec-btn{padding:8px 16px;background:linear-gradient(135deg,var(--accent),var(--accent2));
  border:none;border-radius:10px;color:white;font-size:13px;font-weight:700;
  cursor:pointer;margin-top:4px;}

/* Voice Section */
.voice-section{display:flex;flex-direction:column;align-items:center;padding:8px 20px;flex-shrink:0;}
.voice-ring{position:relative;width:130px;height:130px;display:flex;align-items:center;
  justify-content:center;margin-bottom:10px;}
.ring1,.ring2,.ring3{position:absolute;border-radius:50%;border:2px solid var(--accent);
  opacity:0;transition:opacity 0.3s;}
.ring1{width:100%;height:100%;}
.ring2{width:76%;height:76%;}
.ring3{width:55%;height:55%;}
.voice-ring.listening .ring1{opacity:0.15;animation:ring-pulse 1.5s infinite 0s;}
.voice-ring.listening .ring2{opacity:0.25;animation:ring-pulse 1.5s infinite 0.2s;}
.voice-ring.listening .ring3{opacity:0.4;animation:ring-pulse 1.5s infinite 0.4s;}
@keyframes ring-pulse{0%,100%{transform:scale(1)}50%{transform:scale(1.08)}}
.mic-btn{width:76px;height:76px;border-radius:50%;
  background:linear-gradient(135deg,var(--accent),var(--accent2));
  border:none;cursor:pointer;font-size:30px;display:flex;align-items:center;
  justify-content:center;box-shadow:0 0 30px var(--accent-glow);
  transition:transform 0.15s,box-shadow 0.2s;z-index:2;}
.mic-btn.listening{box-shadow:0 0 50px rgba(124,58,237,0.7);animation:mic-glow 1s infinite alternate;}
@keyframes mic-glow{from{box-shadow:0 0 30px var(--accent-glow)}to{box-shadow:0 0 60px rgba(168,85,247,0.8)}}
.voice-label{font-size:13px;font-weight:700;margin-bottom:2px;}
.voice-hint{font-size:11px;color:var(--muted);text-align:center;max-width:240px;}

/* Command Bubble */
.cmd-bubble{background:var(--surface2);border:1px solid var(--border);border-radius:12px;
  padding:10px 16px;margin:6px 16px 0;font-size:12px;color:var(--accent2);font-weight:600;
  text-align:center;min-height:38px;display:flex;align-items:center;justify-content:center;transition:all 0.3s;}
.cmd-bubble.success{border-color:rgba(34,197,94,0.4);color:var(--green);background:rgba(34,197,94,0.08);}
.cmd-bubble.error{border-color:rgba(239,68,68,0.4);color:#f87171;background:rgba(239,68,68,0.08);}

/* Quick Clips */
.quick-row{display:flex;gap:8px;padding:8px 16px 0;}
.quick-btn{flex:1;padding:10px 4px;background:var(--surface2);border:1.5px solid var(--border);
  border-radius:12px;color:var(--text);font-size:11px;font-weight:700;cursor:pointer;
  text-align:center;transition:all 0.2s;}
.quick-btn:hover{border-color:var(--accent2);color:var(--accent2);}
.quick-btn:active{transform:scale(0.95);}
.quick-btn .qs{font-size:16px;display:block;margin-bottom:3px;}

/* Library */
.library{flex:1;display:flex;flex-direction:column;min-height:0;margin-top:10px;}
.lib-header{display:flex;align-items:center;justify-content:space-between;padding:0 16px 8px;}
.lib-title{font-size:15px;font-weight:800;}
.lib-count{font-size:11px;color:var(--muted);background:var(--surface2);padding:2px 9px;border-radius:20px;}
.lib-clear{font-size:11px;color:#f87171;cursor:pointer;background:none;border:none;font-weight:600;}
.clips-list{flex:1;overflow-y:auto;padding:0 16px 12px;display:flex;flex-direction:column;gap:10px;
  scrollbar-width:thin;scrollbar-color:var(--accent) transparent;}
.clip-empty{display:flex;flex-direction:column;align-items:center;justify-content:center;
  gap:10px;padding:24px 0;color:var(--muted);}
.clip-empty .ei{font-size:36px;}
.clip-empty p{font-size:12px;text-align:center;max-width:200px;}

.clip-card{background:var(--surface2);border:1.5px solid var(--border);border-radius:14px;overflow:hidden;}
.clip-thumb{width:100%;height:80px;position:relative;cursor:pointer;overflow:hidden;background:#000;}
.clip-thumb video{width:100%;height:100%;object-fit:cover;display:block;}
.clip-thumb .play-overlay{position:absolute;inset:0;display:flex;align-items:center;
  justify-content:center;background:rgba(0,0,0,0.3);}
.play-overlay-btn{width:38px;height:38px;border-radius:50%;background:rgba(124,58,237,0.85);
  display:flex;align-items:center;justify-content:center;font-size:16px;transition:transform 0.15s;}
.clip-thumb:hover .play-overlay-btn{transform:scale(1.1);}
.duration-badge{position:absolute;bottom:5px;right:7px;background:rgba(0,0,0,0.75);
  color:white;font-size:10px;font-weight:700;padding:2px 6px;border-radius:5px;font-family:monospace;}
.clip-info{padding:8px 12px 10px;}
.clip-name{font-size:13px;font-weight:700;margin-bottom:2px;}
.clip-meta{font-size:10px;color:var(--muted);display:flex;gap:8px;align-items:center;}
.clip-actions{display:flex;gap:7px;margin-top:8px;}
.clip-act-btn{flex:1;padding:6px;border:1px solid var(--border);border-radius:8px;
  background:none;color:var(--muted);font-size:11px;font-weight:600;cursor:pointer;
  display:flex;align-items:center;justify-content:center;gap:3px;transition:all 0.15s;}
.clip-act-btn:hover{color:var(--accent2);border-color:var(--accent2);}
.clip-act-btn.del:hover{color:#f87171;border-color:rgba(239,68,68,0.4);}

/* Playback Modal */
.modal-overlay{position:absolute;inset:0;background:rgba(0,0,0,0.9);display:flex;
  align-items:center;justify-content:center;z-index:100;
  opacity:0;pointer-events:none;transition:opacity 0.3s;}
.modal-overlay.open{opacity:1;pointer-events:all;}
.modal-box{width:340px;background:var(--surface2);border:1.5px solid var(--border);
  border-radius:20px;overflow:hidden;position:relative;}
#modal-video{width:100%;max-height:200px;background:#000;display:block;}
.modal-close{position:absolute;top:10px;right:10px;background:rgba(0,0,0,0.6);
  border:none;color:white;font-size:16px;width:28px;height:28px;border-radius:50%;
  cursor:pointer;display:flex;align-items:center;justify-content:center;z-index:10;}
.modal-info{padding:14px 16px 18px;}
.modal-clip-name{font-size:16px;font-weight:800;margin-bottom:3px;}
.modal-clip-date{font-size:11px;color:var(--muted);margin-bottom:12px;}
.modal-actions{display:flex;gap:8px;}
.modal-dl-btn{flex:1;padding:10px;background:linear-gradient(135deg,var(--accent),var(--accent2));
  border:none;border-radius:10px;color:white;font-size:13px;font-weight:700;cursor:pointer;}
.modal-close-btn{padding:10px 16px;background:var(--surface);border:1px solid var(--border);
  border-radius:10px;color:var(--muted);font-size:13px;font-weight:600;cursor:pointer;}

/* Toast */
.toast{position:absolute;top:76px;left:50%;transform:translateX(-50%) translateY(-20px);
  background:var(--surface2);border:1px solid var(--accent);border-radius:12px;
  padding:10px 18px;font-size:12px;font-weight:600;color:var(--accent2);
  opacity:0;transition:all 0.3s;white-space:nowrap;z-index:200;pointer-events:none;}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0);}

/* Clipping flash */
.clip-flash{position:absolute;inset:0;background:rgba(124,58,237,0.15);
  border-radius:44px;pointer-events:none;opacity:0;z-index:50;
  animation:none;}
.clip-flash.flash{animation:do-flash 0.5s ease-out forwards;}
@keyframes do-flash{0%{opacity:1}100%{opacity:0}}
</style>
</head>
<body>
<div class="phone" id="phone">
  <div class="clip-flash" id="clip-flash"></div>

  <!-- ══ AUTH SCREEN ══ -->
  <div class="screen" id="auth-screen">
    <div class="auth-logo">🎮</div>
    <div class="auth-title"><span>Evo</span></div>
    <div class="auth-sub">Voice-powered gaming clip recorder</div>
    <div class="tabs-row">
      <button class="tab-btn active" id="tab-reg" onclick="switchTab('register')">Register</button>
      <button class="tab-btn" id="tab-log" onclick="switchTab('login')">Log In</button>
    </div>
    <div class="auth-form">
      <div class="input-group" id="name-group">
        <label>Your Name</label>
        <input type="text" id="inp-name" placeholder="e.g. xX_Gamer_Xx"/>
      </div>
      <div class="input-group">
        <label>Username</label>
        <input type="text" id="inp-user" placeholder="@username"/>
      </div>
      <div class="input-group">
        <label>Password</label>
        <input type="password" id="inp-pass" placeholder="••••••••"/>
      </div>
      <button class="auth-btn" id="auth-submit" onclick="handleAuth()">Create Account &amp; Launch Evo 🚀</button>
    </div>
    <div class="auth-note">Evo remembers you forever — no re-login needed.</div>
  </div>

  <!-- ══ MAIN APP SCREEN ══ -->
  <div class="screen hidden" id="main-screen">

    <div class="status-bar">
      <span id="time-display">9:41</span>
      <div class="status-right">
        <div class="rec-dot" id="top-rec-dot" style="background:var(--muted)"></div>
        <span id="top-rec-label" style="color:var(--muted);font-size:11px;font-weight:700">READY</span>
        <span>📶</span><span>🔋</span>
      </div>
    </div>

    <div class="app-header">
      <div class="app-logo">
        <div class="icon">🎮</div>
        <div class="name"><span>Evo</span></div>
      </div>
      <div class="user-chip">
        <div class="avatar" id="header-avatar">?</div>
        <span style="font-size:13px;font-weight:600" id="header-username">Player</span>
      </div>
    </div>

    <!-- Recording Banner -->
    <div class="rec-banner">
      <div>⏺</div>
      <div class="rec-info">
        <div class="rec-label" id="rec-label-text">Tap preview to start screen recording</div>
        <div class="rec-sub" id="rec-sub-text">Evo needs your screen to clip gameplay</div>
      </div>
      <div class="rec-timer" id="rec-timer">00:00</div>
    </div>

    <!-- Live Screen Preview -->
    <div class="preview-wrap" id="preview-wrap">
      <div class="no-preview" id="no-preview">
        <div style="font-size:32px">📺</div>
        <p>Tap below to start screen recording</p>
        <button class="start-rec-btn" onclick="startScreenRecording()">▶ Start Screen Recording</button>
      </div>
      <video id="screen-preview" autoplay muted playsinline style="display:none;width:100%;height:110px;object-fit:cover;"></video>
      <div class="preview-label" id="preview-label" style="display:none">
        <div class="preview-dot"></div> LIVE
      </div>
    </div>

    <!-- Voice Control -->
    <div class="voice-section">
      <div class="voice-ring" id="voice-ring">
        <div class="ring1"></div><div class="ring2"></div><div class="ring3"></div>
        <button class="mic-btn" id="mic-btn" onclick="toggleVoice()">🎙️</button>
      </div>
      <div class="voice-label" id="voice-label">Tap mic to activate voice</div>
      <div class="voice-hint" id="voice-hint">Say: <strong>"Evo clip it for 15 seconds"</strong></div>
    </div>

    <div class="cmd-bubble" id="cmd-bubble">🎙️ Start recording then activate voice</div>

    <!-- Quick Clip Buttons -->
    <div class="quick-row">
      <button class="quick-btn" onclick="clipNow(10)"><span class="qs">⚡</span>10s</button>
      <button class="quick-btn" onclick="clipNow(15)"><span class="qs">🎯</span>15s</button>
      <button class="quick-btn" onclick="clipNow(30)"><span class="qs">🔥</span>30s</button>
      <button class="quick-btn" onclick="clipNow(60)"><span class="qs">💥</span>1min</button>
    </div>

    <!-- Clip Library -->
    <div class="library">
      <div class="lib-header">
        <div class="lib-title">📁 Clip Library</div>
        <div style="display:flex;gap:10px;align-items:center">
          <span class="lib-count" id="clip-count">0 clips</span>
          <button class="lib-clear" onclick="clearAll()">Clear All</button>
        </div>
      </div>
      <div class="clips-list" id="clips-list">
        <div class="clip-empty">
          <div class="ei">🎮</div>
          <p>No clips yet! Start recording then say "Evo clip it for 15 seconds"</p>
        </div>
      </div>
    </div>
  </div>

  <!-- Playback Modal -->
  <div class="modal-overlay" id="modal">
    <div class="modal-box">
      <button class="modal-close" onclick="closeModal()">✕</button>
      <video id="modal-video" controls playsinline></video>
      <div class="modal-info">
        <div class="modal-clip-name" id="modal-name">Clip</div>
        <div class="modal-clip-date" id="modal-date"></div>
        <div class="modal-actions">
          <button class="modal-dl-btn" onclick="downloadClip()">⬇ Download Clip</button>
          <button class="modal-close-btn" onclick="closeModal()">Close</button>
        </div>
      </div>
    </div>
  </div>

  <div class="toast" id="toast"></div>
</div>

<script>
// ── State ───────────────────────────────────────────────────────────
const STORE_KEY = 'evo_user_v2';

let mediaStream = null;
let mediaRecorder = null;
// Rolling buffer: array of { blob, ts } — ts = Date.now() when chunk arrived
let rollingChunks = [];
const MAX_BUFFER_SECONDS = 120; // keep last 2 min in buffer
let isRecording = false;
let recSeconds = 0;
let recInterval = null;

// In-memory clip store: { id, name, blobUrl, duration, date, time, blob }
let clips = [];

let isListening = false;
let recognition = null;
let isAuthMode = 'register';
let currentModalClip = null;

// ── Boot ────────────────────────────────────────────────────────────
window.addEventListener('load', () => {
  updateTime();
  setInterval(updateTime, 30000);
  const user = loadUser();
  if (user) launchApp(user);
});

function updateTime() {
  const d = new Date();
  document.getElementById('time-display').textContent =
    d.getHours() + ':' + String(d.getMinutes()).padStart(2,'0');
}

function saveUser(u) { localStorage.setItem(STORE_KEY, JSON.stringify(u)); }
function loadUser() { try { return JSON.parse(localStorage.getItem(STORE_KEY)); } catch { return null; } }

// ── Auth ────────────────────────────────────────────────────────────
function switchTab(mode) {
  isAuthMode = mode;
  document.getElementById('tab-reg').classList.toggle('active', mode === 'register');
  document.getElementById('tab-log').classList.toggle('active', mode === 'login');
  document.getElementById('name-group').style.display = mode === 'register' ? '' : 'none';
  document.getElementById('auth-submit').textContent =
    mode === 'register' ? 'Create Account & Launch Evo 🚀' : 'Log In & Play 🎮';
}

function handleAuth() {
  const name = document.getElementById('inp-name').value.trim();
  const user = document.getElementById('inp-user').value.trim();
  const pass = document.getElementById('inp-pass').value.trim();
  if (!user || !pass) { showToast('⚠️ Fill in all fields!'); return; }
  if (isAuthMode === 'register' && !name) { showToast('⚠️ Enter your name!'); return; }
  const userData = { name: name || user, username: user, joinDate: new Date().toLocaleDateString() };
  saveUser(userData);
  launchApp(userData);
}

function launchApp(user) {
  document.getElementById('header-username').textContent = user.username;
  document.getElementById('header-avatar').textContent = (user.name || user.username)[0].toUpperCase();
  document.getElementById('auth-screen').classList.add('hidden');
  document.getElementById('main-screen').classList.remove('hidden');
  renderClips();
  showToast(`👋 Welcome back, ${user.name || user.username}!`);
}

// ── Screen Recording ────────────────────────────────────────────────
async function startScreenRecording() {
  try {
    showToast('📺 Select your screen/game window…');

    mediaStream = await navigator.mediaDevices.getDisplayMedia({
      video: { frameRate: 30, cursor: 'always' },
      audio: true
    });

    // Show live preview
    const preview = document.getElementById('screen-preview');
    preview.srcObject = mediaStream;
    preview.style.display = 'block';
    document.getElementById('no-preview').style.display = 'none';
    document.getElementById('preview-label').style.display = 'flex';

    // Update banner
    document.getElementById('rec-label-text').textContent = 'Screen Recording Active';
    document.getElementById('rec-sub-text').textContent = 'Say "Evo clip it for Xs" to save gameplay';
    document.getElementById('rec-label-text').style.color = '#f87171';

    // Update status bar
    document.getElementById('top-rec-dot').style.background = '#ef4444';
    document.getElementById('top-rec-label').style.color = '#f87171';
    document.getElementById('top-rec-label').textContent = 'REC';

    isRecording = true;
    recSeconds = 0;
    recInterval = setInterval(() => {
      recSeconds++;
      const m = String(Math.floor(recSeconds / 60)).padStart(2, '0');
      const s = String(recSeconds % 60).padStart(2, '0');
      document.getElementById('rec-timer').textContent = m + ':' + s;
    }, 1000);

    // Start MediaRecorder with rolling buffer
    startRollingRecorder();

    // Handle stream end (user stops sharing)
    mediaStream.getVideoTracks()[0].addEventListener('ended', stopScreenRecording);

    showToast('✅ Screen recording started!');
    document.getElementById('cmd-bubble').textContent = '🎙️ Recording! Activate mic & say "Evo clip it for 15 seconds"';

  } catch (err) {
    showToast('❌ Screen share cancelled or denied');
    console.warn('getDisplayMedia error:', err);
  }
}

function startRollingRecorder() {
  rollingChunks = [];

  // Pick a supported mime type
  const mimeType = ['video/webm;codecs=vp9,opus', 'video/webm;codecs=vp8,opus',
    'video/webm', 'video/mp4'].find(t => MediaRecorder.isTypeSupported(t)) || '';

  const options = mimeType ? { mimeType } : {};
  mediaRecorder = new MediaRecorder(mediaStream, options);

  mediaRecorder.ondataavailable = (e) => {
    if (e.data && e.data.size > 0) {
      rollingChunks.push({ blob: e.data, ts: Date.now() });
      // Prune chunks older than MAX_BUFFER_SECONDS
      const cutoff = Date.now() - MAX_BUFFER_SECONDS * 1000;
      // Keep at least the first chunk (codec init data) + recent chunks
      rollingChunks = [
        rollingChunks[0], // always keep first for codec headers
        ...rollingChunks.slice(1).filter(c => c.ts >= cutoff)
      ];
    }
  };

  // Collect a chunk every 1 second for fine-grained slicing
  mediaRecorder.start(1000);
}

function stopScreenRecording() {
  isRecording = false;
  clearInterval(recInterval);
  if (mediaRecorder && mediaRecorder.state !== 'inactive') mediaRecorder.stop();
  if (mediaStream) { mediaStream.getTracks().forEach(t => t.stop()); mediaStream = null; }

  const preview = document.getElementById('screen-preview');
  preview.srcObject = null;
  preview.style.display = 'none';
  document.getElementById('no-preview').style.display = 'flex';
  document.getElementById('preview-label').style.display = 'none';
  document.getElementById('top-rec-dot').style.background = 'var(--muted)';
  document.getElementById('top-rec-label').style.color = 'var(--muted)';
  document.getElementById('top-rec-label').textContent = 'READY';
  document.getElementById('rec-label-text').textContent = 'Tap preview to start screen recording';
  document.getElementById('rec-sub-text').textContent = 'Evo needs your screen to clip gameplay';
  showToast('⏹ Screen recording stopped');
}

// ── Clipping ─────────────────────────────────────────────────────────
async function clipNow(seconds) {
  if (!isRecording || rollingChunks.length === 0) {
    showToast('⚠️ Start screen recording first!');
    document.getElementById('cmd-bubble').textContent = '⚠️ Tap "Start Screen Recording" first!';
    document.getElementById('cmd-bubble').className = 'cmd-bubble error';
    setTimeout(() => {
      document.getElementById('cmd-bubble').className = 'cmd-bubble';
      document.getElementById('cmd-bubble').textContent = '🎙️ Start recording then activate voice';
    }, 2500);
    return;
  }

  // Flash effect
  const flash = document.getElementById('clip-flash');
  flash.classList.remove('flash');
  void flash.offsetWidth; // reflow
  flash.classList.add('flash');

  showToast(`🎬 Clipping last ${seconds}s…`);
  document.getElementById('cmd-bubble').textContent = `⏳ Saving last ${seconds} seconds…`;
  document.getElementById('cmd-bubble').className = 'cmd-bubble';

  // Stop recorder briefly to flush, then restart
  await new Promise(resolve => {
    const onStop = () => { mediaRecorder.removeEventListener('stop', onStop); resolve(); };
    mediaRecorder.addEventListener('stop', onStop);
    mediaRecorder.stop();
  });

  // Gather chunks for the last `seconds`
  const cutoff = Date.now() - seconds * 1000;
  const first = rollingChunks[0]; // init chunk (always needed)
  const recent = rollingChunks.slice(1).filter(c => c.ts >= cutoff);
  const toUse = first ? [first.blob, ...recent.map(c => c.blob)] : recent.map(c => c.blob);

  const mimeType = mediaRecorder.mimeType || 'video/webm';
  const blob = new Blob(toUse, { type: mimeType });
  const blobUrl = URL.createObjectURL(blob);

  const now = new Date();
  const clip = {
    id: Date.now(),
    name: `Gameplay Clip`,
    duration: seconds,
    date: now.toLocaleDateString([], { month: 'short', day: 'numeric' }),
    time: now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
    blobUrl,
    blob,
    mimeType
  };

  clips.unshift(clip);
  renderClips();

  showToast(`✅ ${seconds}s clip saved!`);
  document.getElementById('cmd-bubble').textContent = `✅ Clip saved — ${seconds} seconds captured!`;
  document.getElementById('cmd-bubble').className = 'cmd-bubble success';
  setTimeout(() => {
    document.getElementById('cmd-bubble').className = 'cmd-bubble';
    document.getElementById('cmd-bubble').textContent = isListening ? '🎙️ Listening…' : '🎙️ Recording — tap mic or quick clip';
  }, 2500);

  // Restart rolling recorder
  startRollingRecorder();
}

// ── Render Clips ──────────────────────────────────────────────────────
function renderClips() {
  const list = document.getElementById('clips-list');
  document.getElementById('clip-count').textContent = clips.length + (clips.length === 1 ? ' clip' : ' clips');

  if (clips.length === 0) {
    list.innerHTML = `<div class="clip-empty"><div class="ei">🎮</div><p>No clips yet! Start recording then say "Evo clip it for 15 seconds"</p></div>`;
    return;
  }

  list.innerHTML = '';
  clips.forEach(clip => {
    const card = document.createElement('div');
    card.className = 'clip-card';
    card.id = 'clip-' + clip.id;
    card.innerHTML = `
      <div class="clip-thumb" onclick="openModal(${clip.id})">
        <video src="${clip.blobUrl}" muted preload="metadata" style="width:100%;height:100%;object-fit:cover;pointer-events:none;"></video>
        <div class="play-overlay"><div class="play-overlay-btn">▶</div></div>
        <div class="duration-badge">${formatDur(clip.duration)}</div>
      </div>
      <div class="clip-info">
        <div class="clip-name">${clip.name}</div>
        <div class="clip-meta">
          <span>📅 ${clip.date}</span>
          <span>⏰ ${clip.time}</span>
          <span>⏱ ${clip.duration}s</span>
        </div>
        <div class="clip-actions">
          <button class="clip-act-btn" onclick="openModal(${clip.id})">▶ Play</button>
          <button class="clip-act-btn" onclick="downloadClipById(${clip.id})">⬇ Save</button>
          <button class="clip-act-btn del" onclick="deleteClip(${clip.id})">🗑</button>
        </div>
      </div>`;
    list.appendChild(card);
  });
}

function formatDur(s) {
  if (s < 60) return `0:${String(s).padStart(2, '0')}`;
  return `${Math.floor(s / 60)}:${String(s % 60).padStart(2, '0')}`;
}

function deleteClip(id) {
  const clip = clips.find(c => c.id === id);
  if (clip) URL.revokeObjectURL(clip.blobUrl);
  clips = clips.filter(c => c.id !== id);
  renderClips();
  showToast('🗑 Clip deleted');
}

function clearAll() {
  clips.forEach(c => URL.revokeObjectURL(c.blobUrl));
  clips = [];
  renderClips();
  showToast('🗑 All clips cleared');
}

// ── Modal Playback ─────────────────────────────────────────────────────
function openModal(id) {
  currentModalClip = clips.find(c => c.id === id);
  if (!currentModalClip) return;
  const vid = document.getElementById('modal-video');
  vid.src = currentModalClip.blobUrl;
  vid.load();
  document.getElementById('modal-name').textContent = currentModalClip.name + ' – ' + currentModalClip.duration + 's';
  document.getElementById('modal-date').textContent = currentModalClip.date + ' at ' + currentModalClip.time;
  document.getElementById('modal').classList.add('open');
  vid.play().catch(() => {});
}

function closeModal() {
  const vid = document.getElementById('modal-video');
  vid.pause();
  vid.src = '';
  document.getElementById('modal').classList.remove('open');
  currentModalClip = null;
}

function downloadClip() {
  if (currentModalClip) downloadClipById(currentModalClip.id);
}

function downloadClipById(id) {
  const clip = clips.find(c => c.id === id);
  if (!clip) return;
  const ext = clip.mimeType.includes('mp4') ? 'mp4' : 'webm';
  const a = document.createElement('a');
  a.href = clip.blobUrl;
  a.download = `evo-clip-${clip.date.replace(' ','-')}-${clip.time.replace(':','-')}.${ext}`;
  a.click();
  showToast('⬇ Downloading clip…');
}

// ── Voice ─────────────────────────────────────────────────────────────
function toggleVoice() {
  isListening = !isListening;
  const ring = document.getElementById('voice-ring');
  const btn = document.getElementById('mic-btn');

  if (isListening) {
    ring.classList.add('listening');
    btn.classList.add('listening');
    btn.textContent = '🔴';
    document.getElementById('voice-label').textContent = 'Listening…';
    document.getElementById('cmd-bubble').textContent = '🎙️ Listening — say "Evo clip it for 15 seconds"';
    startRecognition();
  } else {
    ring.classList.remove('listening');
    btn.classList.remove('listening');
    btn.textContent = '🎙️';
    document.getElementById('voice-label').textContent = 'Tap mic to activate voice';
    document.getElementById('cmd-bubble').textContent = '🎙️ Mic off — tap to activate';
    stopRecognition();
  }
}

function startRecognition() {
  if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
    showToast('❌ Voice not supported in this browser');
    return;
  }
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  recognition = new SR();
  recognition.continuous = true;
  recognition.interimResults = true;
  recognition.lang = 'en-US';

  recognition.onresult = (e) => {
    for (let i = e.resultIndex; i < e.results.length; i++) {
      const t = e.results[i][0].transcript.toLowerCase().trim();
      if (e.results[i].isFinal) {
        processVoiceCommand(t);
      } else {
        document.getElementById('cmd-bubble').textContent = '🎙️ Hearing: "' + t + '"';
        document.getElementById('cmd-bubble').className = 'cmd-bubble';
      }
    }
  };
  recognition.onend = () => { if (isListening) recognition.start(); };
  recognition.onerror = () => {};
  recognition.start();
}

function stopRecognition() {
  if (recognition) { try { recognition.stop(); } catch(e){} recognition = null; }
}

function processVoiceCommand(text) {
  const bubble = document.getElementById('cmd-bubble');
  bubble.textContent = '🎙️ Heard: "' + text + '"';

  const m = text.match(/evo\s+clip\s+(?:it\s+)?(?:for\s+)?(\d+)\s*(?:seconds?|secs?|s\b)?/i);
  if (m) {
    const secs = Math.min(parseInt(m[1]), MAX_BUFFER_SECONDS);
    bubble.textContent = `✅ Clipping last ${secs} seconds of your screen…`;
    bubble.className = 'cmd-bubble success';
    setTimeout(() => clipNow(secs), 300);
    return;
  }
  if (text.includes('evo') && (text.includes('clip') || text.includes('save') || text.includes('record'))) {
    bubble.textContent = `✅ Clipping last 15 seconds…`;
    bubble.className = 'cmd-bubble success';
    setTimeout(() => clipNow(15), 300);
    return;
  }
  bubble.textContent = '❌ Say: "Evo clip it for 15 seconds"';
  bubble.className = 'cmd-bubble error';
  setTimeout(() => {
    bubble.className = 'cmd-bubble';
    bubble.textContent = '🎙️ Listening — say "Evo clip it for 15 seconds"';
  }, 2500);
}

// ── Toast ──────────────────────────────────────────────────────────────
let toastTimer = null;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 2500);
}
</script>
</body>
</html>
# Evo-clip-it
