<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">
  <title>Round | Голосовые сообщения</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
    body { font-family: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif; background: #0A0B0E; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; padding: 12px; }
    .app-container { width: 100%; max-width: 500px; height: 88vh; min-height: 620px; background: #13151A; border-radius: 40px; box-shadow: 0 25px 45px rgba(0, 0, 0, 0.6); overflow: hidden; display: flex; flex-direction: column; position: relative; }
    
    .auth-screen { background: #13151A; position: absolute; top: 0; left: 0; right: 0; bottom: 0; z-index: 30; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 24px; overflow-y: auto; }
    .auth-card { background: #1E2028; border-radius: 44px; padding: 32px 24px; width: 100%; max-width: 320px; text-align: center; }
    .auth-card h2 { color: white; font-size: 28px; margin-bottom: 4px; }
    .auth-card .sub { color: #8E8EA8; font-size: 13px; margin-bottom: 24px; }
    .auth-tabs { display: flex; gap: 12px; margin-bottom: 20px; }
    .auth-tab { flex: 1; padding: 10px; border-radius: 30px; background: #2C2F3A; color: #aaa; border: none; font-weight: 600; cursor: pointer; }
    .auth-tab.active { background: #2A6DF4; color: white; }
    .auth-input { width: 100%; background: #0A0B0E; border: 1px solid #2F3340; padding: 14px 18px; border-radius: 60px; color: white; font-size: 16px; margin-bottom: 12px; outline: none; }
    .auth-input:focus { border-color: #2A6DF4; }
    .auth-btn { background: #2A6DF4; width: 100%; border: none; padding: 14px; border-radius: 60px; font-weight: 600; font-size: 17px; color: white; cursor: pointer; margin-top: 8px; }
    
    .chats-screen, .chat-room { display: flex; flex-direction: column; height: 100%; }
    .chats-header { padding: 16px 20px 12px; background: #13151A; border-bottom: 1px solid #282C34; display: flex; justify-content: space-between; align-items: center; }
    .header-left { display: flex; align-items: center; gap: 12px; }
    .profile-icon { background: transparent; border: none; font-size: 24px; cursor: pointer; width: 36px; height: 36px; border-radius: 30px; display: flex; align-items: center; justify-content: center; }
    .profile-icon:active { background: #2C2F3A; }
    .chats-header h2 { color: white; font-size: 24px; font-weight: 700; }
    .user-badge { display: flex; align-items: center; gap: 8px; }
    .my-phone { background: #1E2028; padding: 6px 12px; border-radius: 30px; font-size: 11px; color: #8E8EA8; }
    .logout-btn { background: #2C2F3A; border: none; color: #FF9F6E; font-size: 12px; padding: 6px 12px; border-radius: 40px; cursor: pointer; }
    
    .search-section { padding: 8px 16px; }
    .search-input { width: 100%; background: #1E2028; border: 1px solid #2F3340; padding: 10px 16px; border-radius: 30px; color: white; font-size: 14px; outline: none; }
    .search-results { background: #1A1C23; border-radius: 20px; margin-top: 8px; max-height: 200px; overflow-y: auto; }
    .search-result-item { padding: 12px 16px; border-bottom: 1px solid #2F3340; display: flex; justify-content: space-between; align-items: center; cursor: pointer; }
    
    .chats-list { flex: 1; overflow-y: auto; padding: 8px 16px; }
    .chat-item { background: #1A1C23; border-radius: 24px; padding: 14px; margin-bottom: 10px; cursor: pointer; display: flex; justify-content: space-between; align-items: center; transition: 0.2s; }
    .chat-item:active { transform: scale(0.98); }
    .chat-item-left { display: flex; align-items: center; gap: 12px; flex: 1; }
    .chat-avatar { width: 52px; height: 52px; border-radius: 30px; background: linear-gradient(135deg, #2A6DF4, #6B4EFF); display: flex; align-items: center; justify-content: center; font-size: 22px; font-weight: 600; overflow: hidden; flex-shrink: 0; }
    .chat-avatar img { width: 100%; height: 100%; object-fit: cover; }
    .chat-info { flex: 1; }
    .chat-name { font-weight: 600; color: white; font-size: 16px; display: flex; align-items: center; gap: 6px; }
    .favorite-star { color: #FFB800; font-size: 14px; }
    .chat-last { font-size: 12px; color: #8E8EA8; margin-top: 2px; }
    .chat-actions { display: flex; gap: 8px; }
    .favorite-btn { background: none; border: none; font-size: 20px; cursor: pointer; padding: 4px; }
    
    .chat-room-header { background: rgba(19, 21, 26, 0.96); backdrop-filter: blur(12px); padding: 14px 20px; border-bottom: 1px solid #282C34; display: flex; align-items: center; gap: 12px; }
    .back-btn { background: #2C2F3A; border: none; color: white; width: 38px; height: 38px; border-radius: 30px; font-size: 20px; cursor: pointer; }
    .chat-title .name { font-weight: 600; color: white; font-size: 17px; }
    .messages-area { flex: 1; overflow-y: auto; padding: 16px 12px; display: flex; flex-direction: column; gap: 12px; }
    .message-row { display: flex; width: 100%; animation: fadeIn 0.2s ease; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    .my-message { justify-content: flex-end; }
    .other-message { justify-content: flex-start; }
    .message-bubble { max-width: 80%; padding: 10px 14px; border-radius: 22px; background: #2B2D36; color: #F0F0F0; }
    .my-message .message-bubble { background: #2A6DF4; }
    .message-meta { display: flex; gap: 8px; margin-bottom: 6px; font-size: 10px; opacity: 0.6; }
    .message-image { max-width: 100%; max-height: 250px; border-radius: 16px; margin-top: 8px; cursor: pointer; object-fit: cover; }
    
    /* Аудиоплеер для голосовых */
    .audio-player { display: flex; align-items: center; gap: 8px; background: rgba(0,0,0,0.3); padding: 6px 12px; border-radius: 40px; margin-top: 4px; }
    .audio-play-btn { background: none; border: none; color: white; font-size: 18px; cursor: pointer; width: 32px; height: 32px; display: flex; align-items: center; justify-content: center; border-radius: 30px; }
    .audio-play-btn:active { background: rgba(255,255,255,0.2); }
    .audio-timeline { flex: 1; height: 4px; background: rgba(255,255,255,0.3); border-radius: 4px; position: relative; cursor: pointer; }
    .audio-progress { height: 100%; background: white; border-radius: 4px; width: 0%; }
    .audio-time { font-size: 11px; font-family: monospace; }
    
    .input-panel { background: #181A20; padding: 12px 16px 18px; border-top: 1px solid #2C2F3A; display: flex; gap: 10px; align-items: center; }
    .input-panel input { flex: 1; background: #2A2B32; border: none; padding: 12px 16px; border-radius: 32px; color: white; font-size: 16px; outline: none; }
    .photo-btn, .send-btn, .voice-btn { width: 48px; height: 48px; border-radius: 40px; display: flex; align-items: center; justify-content: center; cursor: pointer; border: none; }
    .photo-btn { background: #2C2F3A; font-size: 24px; }
    .voice-btn { background: #2C2F3A; font-size: 22px; transition: 0.1s; }
    .voice-btn.recording { background: #FF3B30; transform: scale(1.05); animation: pulse 0.5s infinite; }
    @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.1); } 100% { transform: scale(1); } }
    .send-btn { background: #2A6DF4; }
    
    /* Индикатор записи */
    .recording-indicator { position: fixed; bottom: 120px; left: 50%; transform: translateX(-50%); background: #1E2028; border-radius: 60px; padding: 12px 24px; display: flex; align-items: center; gap: 12px; z-index: 150; box-shadow: 0 5px 20px rgba(0,0,0,0.5); border: 1px solid #FF3B30; }
    .recording-wave { display: flex; align-items: center; gap: 3px; }
    .wave-bar { width: 3px; height: 12px; background: #FF3B30; border-radius: 2px; animation: wave 0.5s infinite ease alternate; }
    .wave-bar:nth-child(1) { animation-delay: 0s; height: 8px; }
    .wave-bar:nth-child(2) { animation-delay: 0.1s; height: 16px; }
    .wave-bar:nth-child(3) { animation-delay: 0.2s; height: 24px; }
    .wave-bar:nth-child(4) { animation-delay: 0.3s; height: 16px; }
    .wave-bar:nth-child(5) { animation-delay: 0.4s; height: 8px; }
    @keyframes wave { 0% { height: 6px; } 100% { height: 20px; } }
    .recording-time { color: white; font-family: monospace; font-size: 16px; font-weight: 600; }
    .recording-cancel { color: #FF9F6E; font-size: 12px; margin-left: 8px; }
    
    .modal { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.9); z-index: 50; display: flex; align-items: center; justify-content: center; padding: 24px; }
    .modal-content { background: #1E2028; border-radius: 32px; width: 100%; max-width: 320px; padding: 24px; text-align: center; }
    .profile-avatar { width: 100px; height: 100px; border-radius: 60px; background: linear-gradient(135deg, #2A6DF4, #6B4EFF); margin: 0 auto 16px; display: flex; align-items: center; justify-content: center; font-size: 42px; font-weight: 600; overflow: hidden; cursor: pointer; }
    .profile-avatar img { width: 100%; height: 100%; object-fit: cover; }
    .modal input { width: 100%; background: #0A0B0E; border: 1px solid #2F3340; padding: 12px; border-radius: 30px; color: white; margin: 12px 0; outline: none; }
    .modal button { background: #2A6DF4; border: none; padding: 12px; border-radius: 30px; color: white; font-weight: 600; width: 100%; margin-top: 8px; cursor: pointer; }
    .close-modal { background: #2C2F3A; }
    
    .custom-photo-menu, .permission-dialog { position: fixed; bottom: 30px; left: 20px; right: 20px; background: #1E2028; border-radius: 32px; padding: 20px; z-index: 100; box-shadow: 0 10px 40px rgba(0,0,0,0.5); border: 1px solid #2A6DF4; animation: slideUp 0.3s ease; }
    .custom-photo-menu h3, .permission-dialog h3 { color: white; font-size: 18px; margin-bottom: 16px; text-align: center; }
    .menu-buttons { display: flex; gap: 12px; flex-direction: column; }
    .menu-camera, .menu-gallery, .perm-allow, .perm-deny { padding: 14px; border-radius: 40px; font-weight: 600; border: none; cursor: pointer; }
    .menu-camera, .perm-allow { background: #2A6DF4; color: white; }
    .menu-gallery, .perm-deny { background: #2C2F3A; color: white; }
    .menu-cancel { background: transparent; color: #8E8EA8; border: none; padding: 12px; margin-top: 8px; cursor: pointer; }
    
    .hidden { display: none !important; }
    .empty-chats { text-align: center; color: #6E6E8A; padding: 40px; }
    @keyframes slideUp { from { transform: translateY(100px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
  </style>
</head>
<body>

<div class="app-container">
  <div id="authScreen" class="auth-screen">
    <div class="auth-card">
      <h2>Round</h2>
      <div class="sub">мессенджер</div>
      <div class="auth-tabs">
        <button id="loginTabBtn" class="auth-tab active">Вход</button>
        <button id="registerTabBtn" class="auth-tab">Регистрация</button>
      </div>
      <div id="loginPanel">
        <input type="tel" id="loginPhone" class="auth-input" placeholder="Номер телефона">
        <input type="text" id="loginUsername" class="auth-input" placeholder="Юзернейм (необязательно)">
        <button id="loginBtn" class="auth-btn">Войти</button>
      </div>
      <div id="registerPanel" class="hidden">
        <input type="tel" id="regPhone" class="auth-input" placeholder="Номер телефона">
        <input type="text" id="regUsername" class="auth-input" placeholder="Придумайте юзернейм">
        <button id="registerBtn" class="auth-btn">Зарегистрироваться</button>
      </div>
    </div>
  </div>

  <div id="chatsScreen" class="chats-screen hidden">
    <div class="chats-header">
      <div class="header-left">
        <button id="profileIconBtn" class="profile-icon">⚙️</button>
        <h2>Чаты</h2>
      </div>
      <div class="user-badge">
        <span class="my-phone" id="myPhoneSpan">—</span>
        <button id="logoutBtn" class="logout-btn">Выйти</button>
      </div>
    </div>
    <div class="search-section">
      <input type="text" id="searchInput" class="search-input" placeholder="🔍 Поиск по юзернейму или номеру...">
      <div id="searchResults" class="search-results hidden"></div>
    </div>
    <button id="newChatBtn" class="new-chat-btn" style="background:#2A6DF4; border:none; color:white; padding:12px; margin:8px 16px; border-radius:40px; font-weight:600;">➕ Новый чат</button>
    <div id="chatsList" class="chats-list"><div class="empty-chats">✨ Нет чатов</div></div>
  </div>

  <div id="chatRoom" class="chat-room hidden">
    <div class="chat-room-header">
      <button id="backBtn" class="back-btn">←</button>
      <div class="chat-title"><div class="name" id="chatPartnerName">Собеседник</div></div>
    </div>
    <div id="messagesArea" class="messages-area"></div>
    <div class="input-panel">
      <input type="text" id="messageInput" placeholder="Сообщение...">
      <button id="photoBtn" class="photo-btn">📷</button>
      <button id="voiceBtn" class="voice-btn">🎤</button>
      <button id="sendBtn" class="send-btn"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2"><line x1="22" y1="2" x2="11" y2="13"></line><polygon points="22 2 15 22 11 13 2 9 22 2"></polygon></svg></button>
    </div>
  </div>

  <div id="profileModal" class="modal hidden">
    <div class="modal-content">
      <div id="profileAvatar" class="profile-avatar"></div>
      <input type="text" id="usernameInput" placeholder="Юзернейм">
      <button id="saveProfileBtn">💾 Сохранить</button>
      <button id="closeModalBtn" class="close-modal">Закрыть</button>
      <div id="creatorBadge"></div>
    </div>
  </div>

  <div id="photoMenu" class="custom-photo-menu hidden">
    <h3>📸 Выберите источник</h3>
    <div class="menu-buttons">
      <button id="cameraOption" class="menu-camera">📷 Сделать фото</button>
      <button id="galleryOption" class="menu-gallery">🖼️ Выбрать из галереи</button>
      <button id="cancelPhotoMenu" class="menu-cancel">Отмена</button>
    </div>
  </div>

  <div id="permissionDialog" class="permission-dialog hidden">
    <h3>🎙️ Доступ к микрофону</h3>
    <p>Round нужен доступ к микрофону для записи голосовых сообщений</p>
    <div class="menu-buttons">
      <button id="permAllowBtn" class="perm-allow">✅ Разрешить</button>
      <button id="permDenyBtn" class="perm-deny">❌ Не сейчас</button>
    </div>
  </div>

  <div id="recordingIndicator" class="recording-indicator hidden">
    <div class="recording-wave">
      <div class="wave-bar"></div><div class="wave-bar"></div><div class="wave-bar"></div><div class="wave-bar"></div><div class="wave-bar"></div>
    </div>
    <div class="recording-time" id="recordingTime">0:00</div>
    <div class="recording-cancel">⬆️ Свайп вверх для отмены</div>
  </div>
</div>

<script>
  // ========== ТАБЛИЧКА СОЗДАТЕЛЯ ==========
  const CREATOR_PHONE = '+7 900 111 22 33'; // ЗАМЕНИ НА СВОЙ НОМЕР!

  function isCreator() {
    return currentUser && currentUser.phone === CREATOR_PHONE;
  }

  function updateCreatorBadge() {
    const badgeContainer = document.getElementById('creatorBadge');
    if (!badgeContainer) return;
    if (isCreator()) {
      badgeContainer.innerHTML = `
        <div style="background: linear-gradient(135deg, #FFD700, #FFA500); border-radius: 20px; padding: 12px; margin-top: 16px; text-align: center;">
          <div style="font-size: 24px;">👑</div>
          <div style="font-weight: bold; color: #1E2028;">СОЗДАТЕЛЬ</div>
          <div style="font-size: 11px; color: #1E2028; opacity: 0.8;">Round Messenger</div>
        </div>
      `;
      badgeContainer.classList.remove('hidden');
    } else {
      badgeContainer.classList.add('hidden');
    }
  }

  // ========== ДАННЫЕ ==========
  let currentUser = null;
  let currentChatId = null;
  let chats = [];
  let messages = {};
  let usersDB = {};
  let favoriteChats = [];
  let galleryAllowed = false;
  let micAllowed = false;
  let mediaRecorder = null;
  let audioChunks = [];
  let recordingStartTime = null;
  let recordingTimer = null;
  let currentAudio = null;

  function saveAll() {
    localStorage.setItem('round_chats', JSON.stringify(chats));
    localStorage.setItem('round_messages', JSON.stringify(messages));
    localStorage.setItem('round_users', JSON.stringify(usersDB));
    localStorage.setItem('round_favorites', JSON.stringify(favoriteChats));
    if (currentUser) localStorage.setItem('round_currentUser', JSON.stringify({ phone: currentUser.phone }));
    localStorage.setItem('round_gallery', galleryAllowed ? 'true' : 'false');
    localStorage.setItem('round_mic', micAllowed ? 'true' : 'false');
  }

  function loadAll() {
    let savedChats = localStorage.getItem('round_chats');
    let savedMessages = localStorage.getItem('round_messages');
    let savedUsers = localStorage.getItem('round_users');
    let savedFav = localStorage.getItem('round_favorites');
    let savedPerm = localStorage.getItem('round_gallery');
    let savedMic = localStorage.getItem('round_mic');
    if (savedChats) chats = JSON.parse(savedChats);
    if (savedMessages) messages = JSON.parse(savedMessages);
    if (savedUsers) usersDB = JSON.parse(savedUsers);
    if (savedFav) favoriteChats = JSON.parse(savedFav);
    if (savedPerm === 'true') galleryAllowed = true;
    if (savedMic === 'true') micAllowed = true;
  }

  window.addEventListener('storage', (e) => {
    if (e.key?.startsWith('round_')) {
      loadAll();
      if (currentUser) { loadChatsList(); if (currentChatId) renderMessages(); }
    }
  });

  // ========== АВАТАР ==========
  function getAvatarHtml(user) {
    if (user && user.avatar) return `<img src="${user.avatar}">`;
    let letter = (user?.username || user?.phone || 'U').charAt(0).toUpperCase();
    return `<div style="width:100%; height:100%; display:flex; align-items:center; justify-content:center; background:linear-gradient(135deg,#2A6DF4,#6B4EFF);">${letter}</div>`;
  }

  // ========== ГОЛОСОВЫЕ СООБЩЕНИЯ ==========
  async function requestMicrophoneAccess() {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
      stream.getTracks().forEach(track => track.stop());
      return true;
    } catch(e) { return false; }
  }

  async function showMicPermissionDialog() {
    return new Promise(async (resolve) => {
      let dialog = document.getElementById('permissionDialog');
      dialog.querySelector('h3').innerHTML = '🎙️ Доступ к микрофону';
      dialog.querySelector('p').innerHTML = 'Round нужен доступ к микрофону для записи голосовых сообщений';
      dialog.classList.remove('hidden');
      let allowBtn = document.getElementById('permAllowBtn');
      let denyBtn = document.getElementById('permDenyBtn');
      let cleanup = () => { dialog.classList.add('hidden'); allowBtn.removeEventListener('click', allowHandler); denyBtn.removeEventListener('click', denyHandler); };
      let allowHandler = async () => { cleanup(); let granted = await requestMicrophoneAccess(); micAllowed = granted; saveAll(); if (granted) showToast('✅ Доступ к микрофону разрешён'); else showToast('❌ Доступ не предоставлен'); resolve(granted); };
      let denyHandler = () => { cleanup(); resolve(false); };
      allowBtn.addEventListener('click', allowHandler);
      denyBtn.addEventListener('click', denyHandler);
    });
  }

  async function startRecording() {
    if (!micAllowed) {
      let granted = await showMicPermissionDialog();
      if (!granted) { showToast('⚠️ Без микрофона нельзя записать голосовое'); return false; }
    }
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
      mediaRecorder = new MediaRecorder(stream);
      audioChunks = [];
      mediaRecorder.ondataavailable = (event) => { audioChunks.push(event.data); };
      mediaRecorder.start(100);
      recordingStartTime = Date.now();
      document.getElementById('recordingIndicator').classList.remove('hidden');
      document.getElementById('voiceBtn').classList.add('recording');
      recordingTimer = setInterval(() => {
        let elapsed = Math.floor((Date.now() - recordingStartTime) / 1000);
        let minutes = Math.floor(elapsed / 60);
        let seconds = elapsed % 60;
        document.getElementById('recordingTime').innerText = `${minutes}:${seconds.toString().padStart(2,'0')}`;
      }, 1000);
      return true;
    } catch(e) { showToast('❌ Ошибка доступа к микрофону'); return false; }
  }

  async function stopRecording(cancel = false) {
    if (!mediaRecorder || mediaRecorder.state !== 'recording') return null;
    clearInterval(recordingTimer);
    document.getElementById('recordingIndicator').classList.add('hidden');
    document.getElementById('voiceBtn').classList.remove('recording');
    return new Promise((resolve) => {
      mediaRecorder.onstop = async () => {
        const stream = mediaRecorder.stream;
        stream.getTracks().forEach(track => track.stop());
        if (cancel || audioChunks.length === 0) { resolve(null); return; }
        const audioBlob = new Blob(audioChunks, { type: 'audio/webm' });
        const reader = new FileReader();
        reader.onloadend = () => {
          resolve({ data: reader.result, duration: Math.floor((Date.now() - recordingStartTime) / 1000) });
        };
        reader.readAsDataURL(audioBlob);
      };
      mediaRecorder.stop();
    });
  }

  function createAudioPlayer(audioData, duration) {
    let audioId = 'audio_' + Date.now() + '_' + Math.random();
    return `
      <div class="audio-player" data-audio-id="${audioId}">
        <button class="audio-play-btn" onclick="toggleAudioPlay('${audioId}', this)">▶️</button>
        <div class="audio-timeline" onclick="seekAudio('${audioId}', event)">
          <div class="audio-progress"></div>
        </div>
        <span class="audio-time">${Math.floor(duration/60)}:${(duration%60).toString().padStart(2,'0')}</span>
      </div>
      <audio id="${audioId}" src="${audioData}" style="display:none"></audio>
    `;
  }

  window.toggleAudioPlay = function(audioId, btn) {
    let audio = document.getElementById(audioId);
    if (!audio) return;
    if (currentAudio && currentAudio !== audio) { currentAudio.pause(); currentAudio.currentTime = 0; }
    if (audio.paused) {
      audio.play();
      btn.innerHTML = '⏸️';
      currentAudio = audio;
      audio.onended = () => { btn.innerHTML = '▶️'; if (currentAudio === audio) currentAudio = null; };
      audio.ontimeupdate = () => {
        let progress = (audio.currentTime / audio.duration) * 100;
        let timeline = btn.closest('.audio-player').querySelector('.audio-progress');
        if (timeline) timeline.style.width = progress + '%';
      };
    } else {
      audio.pause();
      btn.innerHTML = '▶️';
    }
  };

  window.seekAudio = function(audioId, event) {
    let audio = document.getElementById(audioId);
    if (!audio || !audio.duration) return;
    let timeline = event.currentTarget;
    let rect = timeline.getBoundingClientRect();
    let percent = (event.clientX - rect.left) / rect.width;
    audio.currentTime = percent * audio.duration;
  };

  // ========== ОСНОВНАЯ ЛОГИКА ==========
  function getOrCreateUser(phone, username = null) {
    if (!usersDB[phone]) {
      let shortName = phone.replace(/\D/g, '').slice(-4);
      usersDB[phone] = { phone: phone, username: username || `user_${shortName}`, avatar: null };
      saveAll();
    } else if (username && !usersDB[phone].username?.startsWith('user_')) {
      usersDB[phone].username = username;
      saveAll();
    }
    return usersDB[phone];
  }

  function getUserDisplay(phone) {
    let user = usersDB[phone];
    if (!user) return `📱+${phone.replace(/\D/g, '').slice(-4)}`;
    return user.username || `📱+${phone.replace(/\D/g, '').slice(-4)}`;
  }

  function searchUsers(query) {
    if (!query.trim()) return [];
    let q = query.toLowerCase();
    return Object.values(usersDB).filter(u => u.phone !== currentUser?.phone && (u.phone.toLowerCase().includes(q) || (u.username && u.username.toLowerCase().includes(q))));
  }

  function createChat(partnerPhone) {
    let existing = chats.find(c => c.partner === partnerPhone);
    if (existing) return existing;
    let newChat = { id: Date.now().toString(), partner: partnerPhone, lastMessage: 'Чат создан', lastTime: Date.now() };
    chats.push(newChat);
    messages[newChat.id] = [];
    saveAll();
    return newChat;
  }

  function sendMessage(text, mediaData, voiceData = null) {
    if (!currentChatId || !currentUser) return;
    let newMsg = {
      id: Date.now(),
      text: text || '',
      media: mediaData || null,
      voice: voiceData || null,
      sender: currentUser.phone,
      time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
      timestamp: Date.now()
    };
    if (!messages[currentChatId]) messages[currentChatId] = [];
    messages[currentChatId].push(newMsg);
    let chat = chats.find(c => c.id === currentChatId);
    if (chat) { chat.lastMessage = text || (mediaData ? '📷 Фото' : (voiceData ? '🎤 Голосовое' : '')); chat.lastTime = Date.now(); }
    saveAll();
    renderMessages();
    loadChatsList();
  }

  function toggleFavorite(chatId) {
    if (favoriteChats.includes(chatId)) favoriteChats = favoriteChats.filter(id => id !== chatId);
    else favoriteChats.push(chatId);
    saveAll();
    loadChatsList();
  }

  function loadChatsList() {
    let container = document.getElementById('chatsList');
    if (!container) return;
    let sortedChats = [...chats].sort((a,b) => (b.lastTime || 0) - (a.lastTime || 0));
    let favChats = sortedChats.filter(c => favoriteChats.includes(c.id));
    let otherChats = sortedChats.filter(c => !favoriteChats.includes(c.id));
    let finalChats = [...favChats, ...otherChats];
    if (finalChats.length === 0) { container.innerHTML = '<div class="empty-chats">✨ Нет чатов<br>Нажмите «Новый чат»</div>'; return; }
    container.innerHTML = '';
    finalChats.forEach(chat => {
      let partnerUser = usersDB[chat.partner] || { username: getUserDisplay(chat.partner), avatar: null };
      let isFav = favoriteChats.includes(chat.id);
      let div = document.createElement('div');
      div.className = 'chat-item';
      div.innerHTML = `
        <div class="chat-item-left" onclick="window.openChat('${chat.id}')">
          <div class="chat-avatar">${getAvatarHtml(partnerUser)}</div>
          <div class="chat-info">
            <div class="chat-name">${escapeHtml(getUserDisplay(chat.partner))} ${isFav ? '<span class="favorite-star">⭐</span>' : ''}</div>
            <div class="chat-last">${escapeHtml(chat.lastMessage || 'Начните общение')}</div>
          </div>
        </div>
        <div class="chat-actions">
          <button class="favorite-btn" onclick="event.stopPropagation(); toggleFavorite('${chat.id}')">${isFav ? '⭐' : '☆'}</button>
        </div>
      `;
      container.appendChild(div);
    });
  }

  window.openChat = function(chatId) {
    currentChatId = chatId;
    let chat = chats.find(c => c.id === chatId);
    if (chat) document.getElementById('chatPartnerName').innerText = getUserDisplay(chat.partner);
    document.getElementById('chatsScreen').classList.add('hidden');
    document.getElementById('chatRoom').classList.remove('hidden');
    renderMessages();
  };
  window.toggleFavorite = toggleFavorite;

  function renderMessages() {
    let container = document.getElementById('messagesArea');
    if (!container || !currentChatId) return;
    let msgs = messages[currentChatId] || [];
    if (msgs.length === 0) { container.innerHTML = '<div style="text-align:center; color:#6E6E8A; padding:20px;">💬 Напишите первое сообщение</div>'; return; }
    container.innerHTML = '';
    msgs.forEach(msg => {
      let isOwn = msg.sender === currentUser.phone;
      let row = document.createElement('div');
      row.className = `message-row ${isOwn ? 'my-message' : 'other-message'}`;
      let mediaHtml = '';
      if (msg.media && msg.media.data && msg.media.data.startsWith('data:image')) {
        mediaHtml = `<img src="${msg.media.data}" class="message-image" onclick="window.open('${msg.media.data}','_blank')" loading="lazy">`;
      } else if (msg.media) {
        mediaHtml = `<div style="background:#1A1C23; padding:8px; border-radius:12px; margin-top:6px;">📎 Файл</div>`;
      }
      let voiceHtml = '';
      if (msg.voice && msg.voice.data) {
        voiceHtml = createAudioPlayer(msg.voice.data, msg.voice.duration || 3);
      }
      row.innerHTML = `<div class="message-bubble"><div class="message-meta"><span>${isOwn ? 'Вы' : getUserDisplay(msg.sender)}</span><span>${msg.time}</span></div>${msg.text ? `<div>${escapeHtml(msg.text)}</div>` : ''}${mediaHtml}${voiceHtml}</div>`;
      container.appendChild(row);
    });
    setTimeout(() => container.scrollTop = container.scrollHeight, 50);
  }

  // ========== ВХОД/РЕГИСТРАЦИЯ ==========
  function login() {
    let phone = document.getElementById('loginPhone').value.trim();
    let username = document.getElementById('loginUsername').value.trim();
    if (!phone || phone.replace(/\D/g, '').length < 5) { alert('Введите корректный номер'); return; }
    if (!usersDB[phone]) { alert('Пользователь не найден. Зарегистрируйтесь'); return; }
    if (username && usersDB[phone].username !== username) { alert('Неверный юзернейм'); return; }
    currentUser = { phone: phone };
    saveAll();
    afterLogin();
  }

  function register() {
    let phone = document.getElementById('regPhone').value.trim();
    let username = document.getElementById('regUsername').value.trim();
    if (!phone || phone.replace(/\D/g, '').length < 5) { alert('Введите корректный номер'); return; }
    if (!username) { alert('Придумайте юзернейм'); return; }
    if (usersDB[phone]) { alert('Пользователь уже существует, войдите'); return; }
    getOrCreateUser(phone, username);
    currentUser = { phone: phone };
    saveAll();
    afterLogin();
  }

  function afterLogin() {
    document.getElementById('myPhoneSpan').innerText = currentUser.phone;
    document.getElementById('authScreen').classList.add('hidden');
    document.getElementById('chatsScreen').classList.remove('hidden');
    loadChatsList();
    if (chats.length === 0) {
      let demoPhone = '+7 900 999 88 77';
      if (!usersDB[demoPhone]) getOrCreateUser(demoPhone, 'Демо');
      let demoChat = createChat(demoPhone);
      if (messages[demoChat.id]?.length === 0) {
        messages[demoChat.id] = [{ id: 1, text: 'Привет! Это демо-чат Round! Можно отправлять голосовые 🎤', sender: demoPhone, time: new Date().toLocaleTimeString(), timestamp: Date.now() }];
        saveAll();
      }
      loadChatsList();
    }
    updateCreatorBadge();
  }

  function logout() {
    currentUser = null; currentChatId = null;
    document.getElementById('authScreen').classList.remove('hidden');
    document.getElementById('chatsScreen').classList.add('hidden');
    document.getElementById('chatRoom').classList.add('hidden');
  }

  function autoLogin() {
    let saved = localStorage.getItem('round_currentUser');
    if (saved) {
      try { let { phone } = JSON.parse(saved); currentUser = { phone: phone }; afterLogin(); return true; }
      catch(e) {}
    }
    return false;
  }

  // ========== ФОТО ==========
  function processImageFile(file) {
    return new Promise((resolve) => {
      const reader = new FileReader();
      reader.onload = (ev) => resolve({ data: ev.target.result, type: file.type, name: file.name });
      reader.readAsDataURL(file);
    });
  }

  async function requestGalleryAccess() {
    return new Promise((resolve) => {
      const input = document.createElement('input');
      input.type = 'file';
      input.accept = 'image/*';
      input.style.display = 'none';
      document.body.appendChild(input);
      let timeout = setTimeout(() => { document.body.removeChild(input); resolve(false); }, 60000);
      input.onchange = (e) => { clearTimeout(timeout); document.body.removeChild(input); resolve(e.target.files && e.target.files.length > 0); };
      input.oncancel = () => { clearTimeout(timeout); document.body.removeChild(input); resolve(false); };
      input.click();
    });
  }

  function showGalleryPermissionDialog() {
    return new Promise((resolve) => {
      let dialog = document.getElementById('permissionDialog');
      dialog.querySelector('h3').innerHTML = '📸 Доступ к галерее';
      dialog.querySelector('p').innerHTML = 'Round нужен доступ к фото для отправки изображений';
      dialog.classList.remove('hidden');
      let allowBtn = document.getElementById('permAllowBtn');
      let denyBtn = document.getElementById('permDenyBtn');
      let cleanup = () => { dialog.classList.add('hidden'); allowBtn.removeEventListener('click', allowHandler); denyBtn.removeEventListener('click', denyHandler); };
      let allowHandler = async () => { cleanup(); let granted = await requestGalleryAccess(); galleryAllowed = granted; saveAll(); if (granted) showToast('✅ Доступ к галерее разрешён'); resolve(granted); };
      let denyHandler = () => { cleanup(); resolve(false); };
      allowBtn.addEventListener('click', allowHandler);
      denyBtn.addEventListener('click', denyHandler);
    });
  }

  async function selectFromCamera() {
    return new Promise((resolve) => {
      let input = document.createElement('input');
      input.type = 'file';
      input.accept = 'image/*';
      input.capture = 'environment';
      input.style.display = 'none';
      document.body.appendChild(input);
      input.onchange = async (e) => {
        document.body.removeChild(input);
        if (e.target.files && e.target.files[0]) resolve(await processImageFile(e.target.files[0]));
        else resolve(null);
      };
      input.click();
    });
  }

  async function selectFromGallery() {
    return new Promise((resolve) => {
      let input = document.createElement('input');
      input.type = 'file';
      input.accept = 'image/*';
      input.style.display = 'none';
      document.body.appendChild(input);
      input.onchange = async (e) => {
        document.body.removeChild(input);
        if (e.target.files && e.target.files[0]) resolve(await processImageFile(e.target.files[0]));
        else resolve(null);
      };
      input.click();
    });
  }

  async function showPhotoMenu() {
    if (!galleryAllowed) { let granted = await showGalleryPermissionDialog(); if (!granted) return null; }
    return new Promise((resolve) => {
      let menu = document.getElementById('photoMenu');
      menu.classList.remove('hidden');
      let cameraBtn = document.getElementById('cameraOption');
      let galleryBtn = document.getElementById('galleryOption');
      let cancelBtn = document.getElementById('cancelPhotoMenu');
      let cleanup = () => { menu.classList.add('hidden'); cameraBtn.removeEventListener('click', cameraHandler); galleryBtn.removeEventListener('click', galleryHandler); cancelBtn.removeEventListener('click', cancelHandler); };
      let cameraHandler = async () => { cleanup(); let result = await selectFromCamera(); resolve(result); };
      let galleryHandler = async () => { cleanup(); let result = await selectFromGallery(); resolve(result); };
      let cancelHandler = () => { cleanup(); resolve(null); };
      cameraBtn.addEventListener('click', cameraHandler);
      galleryBtn.addEventListener('click', galleryHandler);
      cancelBtn.addEventListener('click', cancelHandler);
    });
  }

  // ========== ПРОФИЛЬ ==========
  async function selectAvatar() {
    if (!galleryAllowed) { let granted = await showGalleryPermissionDialog(); if (!granted) return; }
    let input = document.createElement('input');
    input.type = 'file';
    input.accept = 'image/*';
    input.style.display = 'none';
    document.body.appendChild(input);
    input.onchange = async (e) => {
      document.body.removeChild(input);
      if (e.target.files && e.target.files[0]) {
        let img = await processImageFile(e.target.files[0]);
        usersDB[currentUser.phone].avatar = img.data;
        saveAll();
        document.getElementById('profileAvatar').innerHTML = `<img src="${img.data}">`;
        loadChatsList();
        showToast('✅ Аватар обновлён');
      }
    };
    input.click();
  }

  function showProfile() {
    let userData = usersDB[currentUser.phone] || { username: '', avatar: null };
    document.getElementById('usernameInput').value = userData.username || '';
    let avatarDiv = document.getElementById('profileAvatar');
    if (userData.avatar) avatarDiv.innerHTML = `<img src="${userData.avatar}">`;
    else avatarDiv.innerHTML = `<div style="width:100%; height:100%; display:flex; align-items:center; justify-content:center; font-size:42px;">${(userData.username || 'U').charAt(0).toUpperCase()}</div>`;
    updateCreatorBadge();
    document.getElementById('profileModal').classList.remove('hidden');
  }

  function saveProfile() {
    let newUsername = document.getElementById('usernameInput').value.trim();
    if (newUsername) usersDB[currentUser.phone].username = newUsername;
    saveAll();
    document.getElementById('profileModal').classList.add('hidden');
    loadChatsList();
    if (currentChatId) {
      let chat = chats.find(c => c.id === currentChatId);
      if (chat) document.getElementById('chatPartnerName').innerText = getUserDisplay(chat.partner);
    }
    showToast('✅ Профиль сохранён');
  }

  function newChatByNumber() {
    let phone = prompt('Введите номер телефона собеседника:', '+7');
    if (phone && phone.trim()) {
      getOrCreateUser(phone.trim());
      let chat = createChat(phone.trim());
      loadChatsList();
      openChat(chat.id);
    }
  }

  function handleSearch() {
    let query = document.getElementById('searchInput').value;
    let results = searchUsers(query);
    let resultsDiv = document.getElementById('searchResults');
    if (results.length === 0 || !query.trim()) { resultsDiv.classList.add('hidden'); return; }
    resultsDiv.classList.remove('hidden');
    resultsDiv.innerHTML = results.map(user => `<div class="search-result-item" onclick="window.addChatFromSearch('${user.phone}')"><span>${escapeHtml(getUserDisplay(user.phone))}</span><span style="color:#2A6DF4;">➕ Добавить</span></div>`).join('');
  }

  window.addChatFromSearch = function(phone) {
    let chat = createChat(phone);
    loadChatsList();
    document.getElementById('searchResults').classList.add('hidden');
    document.getElementById('searchInput').value = '';
    openChat(chat.id);
  };

  let pendingMedia = null;
  let isRecording = false;
  let touchStartY = 0;

  async function onPhotoClick() { let media = await showPhotoMenu(); if (media) { pendingMedia = media; document.getElementById('messageInput').placeholder = '📷 Фото выбрано! Добавьте текст'; setTimeout(() => document.getElementById('messageInput').placeholder = 'Сообщение...', 3000); } }

  async function onVoiceStart(e) {
    e.preventDefault();
    touchStartY = e.touches ? e.touches[0].clientY : e.clientY;
    let started = await startRecording();
    if (started) isRecording = true;
  }

  async function onVoiceEnd(e) {
    if (!isRecording) return;
    let endY = e.changedTouches ? e.changedTouches[0].clientY : e.clientY;
    let cancel = (touchStartY - endY) > 80;
    let voiceData = await stopRecording(cancel);
    isRecording = false;
    if (voiceData && !cancel) {
      sendMessage('', null, voiceData);
    } else if (cancel) {
      showToast('❌ Голосовое отменено');
    }
  }

  async function sendCurrentMessage() {
    let input = document.getElementById('messageInput');
    let text = input.value.trim();
    if (!text && !pendingMedia) return;
    sendMessage(text, pendingMedia, null);
    input.value = '';
    pendingMedia = null;
  }

  function showToast(msg) {
    let toast = document.createElement('div');
    toast.style.cssText = 'position:fixed; bottom:100px; left:20px; right:20px; background:#2A6DF4; color:white; padding:14px; border-radius:60px; text-align:center; z-index:200;';
    toast.innerText = msg;
    document.body.appendChild(toast);
    setTimeout(() => toast.remove(), 2500);
  }

  function escapeHtml(str) { if (!str) return ''; return str.replace(/[&<>]/g, m => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;' }[m])); }

  // ========== ЗАПУСК ==========
  loadAll();
  document.getElementById('loginTabBtn').onclick = () => { document.getElementById('loginTabBtn').classList.add('active'); document.getElementById('registerTabBtn').classList.remove('active'); document.getElementById('loginPanel').classList.remove('hidden'); document.getElementById('registerPanel').classList.add('hidden'); };
  document.getElementById('registerTabBtn').onclick = () => { document.getElementById('registerTabBtn').classList.add('active'); document.getElementById('loginTabBtn').classList.remove('active'); document.getElementById('registerPanel').classList.remove('hidden'); document.getElementById('loginPanel').classList.add('hidden'); };
  document.getElementById('loginBtn').onclick = login;
  document.getElementById('registerBtn').onclick = register;
  document.getElementById('logoutBtn').onclick = logout;
  document.getElementById('newChatBtn').onclick = newChatByNumber;
  document.getElementById('backBtn').onclick = () => { document.getElementById('chatRoom').classList.add('hidden'); document.getElementById('chatsScreen').classList.remove('hidden'); loadChatsList(); currentChatId = null; };
  document.getElementById('sendBtn').onclick = sendCurrentMessage;
  document.getElementById('photoBtn').onclick = onPhotoClick;
  document.getElementById('messageInput').onkeypress = (e) => { if (e.key === 'Enter') sendCurrentMessage(); };
  document.getElementById('profileIconBtn').onclick = showProfile;
  document.getElementById('saveProfileBtn').onclick = saveProfile;
  document.getElementById('closeModalBtn').onclick = () => document.getElementById('profileModal').classList.add('hidden');
  document.getElementById('profileAvatar').onclick = selectAvatar;
  document.getElementById('searchInput').oninput = handleSearch;

  let voiceBtn = document.getElementById('voiceBtn');
  voiceBtn.addEventListener('mousedown', onVoiceStart);
  voiceBtn.addEventListener('mouseup', onVoiceEnd);
  voiceBtn.addEventListener('touchstart', onVoiceStart);
  voiceBtn.addEventListener('touchend', onVoiceEnd);

  if (!autoLogin()) document.getElementById('authScreen').classList.remove('hidden');
</script>
</body>
</html>
