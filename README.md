# gharam-qalbi
موقع شات غرام قلبي
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>شات غرام قلبي</title>
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <style>
    :root {
      --bg: #0d1117;
      --card: #161b22;
      --primary: #e11d48;
      --accent: #f43f5e;
      --text: #f0f6fc;
      --muted: #8b949e;
      --border: #30363d;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: system-ui, -apple-system, sans-serif; }
    body { background-color: var(--bg); color: var(--text); min-height: 100vh; display: flex; flex-direction: column; }
    
    header { background: #010409; padding: 1rem; text-align: center; border-bottom: 1px solid var(--border); font-size: 1.6rem; font-weight: bold; color: var(--primary); }
    
    .nav { display: flex; justify-content: center; gap: 8px; background: var(--card); padding: 0.8rem; border-bottom: 1px solid var(--border); overflow-x: auto; }
    .nav button { background: transparent; border: 1px solid var(--border); color: var(--text); padding: 8px 14px; border-radius: 20px; cursor: pointer; transition: 0.3s; white-space: nowrap; font-size: 0.9rem; }
    .nav button.active, .nav button:hover { background: var(--primary); border-color: var(--primary); color: #fff; }

    main { flex: 1; padding: 1.5rem; max-width: 650px; width: 100%; margin: 0 auto; }
    .page { display: none; background: var(--card); padding: 1.5rem; border-radius: 12px; border: 1px solid var(--border); }
    .page.active { display: block; }

    h2 { margin-bottom: 1rem; color: var(--accent); font-size: 1.3rem; }
    .form-group { margin-bottom: 1rem; display: flex; flex-direction: column; gap: 5px; }
    label { font-size: 0.85rem; color: var(--muted); }
    input, select, textarea { padding: 10px; border-radius: 8px; border: 1px solid var(--border); background: var(--bg); color: #fff; width: 100%; outline: none; }
    
    .btn { background: var(--primary); color: white; border: none; padding: 12px; border-radius: 8px; width: 100%; cursor: pointer; font-size: 1rem; font-weight: bold; transition: 0.2s; }
    .btn:hover { background: var(--accent); }

    .room-card { background: var(--bg); padding: 1rem; border-radius: 10px; border: 1px solid var(--border); margin-bottom: 12px; cursor: pointer; display: flex; justify-content: space-between; align-items: center; transition: 0.2s; }
    .room-card:hover { border-color: var(--primary); transform: translateY(-2px); }

    .user-card { display: flex; align-items: center; gap: 12px; background: var(--bg); padding: 10px 14px; border-radius: 8px; border: 1px solid var(--border); margin-bottom: 8px; }
    .status-dot { width: 10px; height: 10px; background: #22c55e; border-radius: 50%; display: inline-block; }

    .chat-box { height: 320px; border: 1px solid var(--border); border-radius: 8px; background: var(--bg); padding: 12px; overflow-y: auto; margin-bottom: 12px; display: flex; flex-direction: column; gap: 10px; }
    .msg { background: var(--card); padding: 8px 12px; border-radius: 8px; max-width: 80%; width: fit-content; border: 1px solid var(--border); }
    .msg-user { font-size: 0.75rem; color: var(--accent); font-weight: bold; margin-bottom: 3px; }
  </style>
</head>
<body>

  <header>شات غرام قلبي 💬</header>

  <!-- شريط التنقل الأساسي -->
  <nav class="nav">
    <button onclick="showPage('home')" id="nav-home" class="active">الرئيسية</button>
    <button onclick="showPage('login')" id="nav-login">الدخول</button>
    <button onclick="showPage('register')" id="nav-register">إنشاء حساب</button>
    <button onclick="showPage('rooms')" id="nav-rooms">الغرف</button>
    <button onclick="showPage('users')" id="nav-users">المستخدمين</button>
    <button onclick="showPage('profile')" id="nav-profile">البروفايل</button>
  </nav>

  <main>
    <!-- 1. الصفحة الرئيسية -->
    <section id="home" class="page active">
      <h2>أهلاً بك في شات غرام قلبي</h2>
      <p style="margin-bottom: 1.5rem; color: var(--muted); font-size:0.95rem;">مجتمع عربي راقي للتعرف والتواصل والمحادثات الحية.</p>
      <button class="btn" onclick="showPage('register')" style="margin-bottom: 10px;">دخول الشات (إنشاء حساب)</button>
      <div style="display: flex; gap: 8px;">
        <button class="btn" style="background:var(--border);" onclick="showPage('rooms')">تصفح الغرف</button>
        <button class="btn" style="background:var(--border);" onclick="showPage('users')">المستخدمين</button>
      </div>
    </section>

    <!-- 2. تسجيل الدخول -->
    <section id="login" class="page">
      <h2>تسجيل الدخول</h2>
      <div class="form-group">
        <label>اسم المستخدم</label>
        <input type="text" placeholder="اكتب اسمك">
      </div>
      <div class="form-group">
        <label>كلمة المرور</label>
        <input type="password" placeholder="••••••••">
      </div>
      <button class="btn" onclick="showPage('rooms')">دخول</button>
    </section>

    <!-- 3. إنشاء حساب -->
    <section id="register" class="page">
      <h2>إنشاء حساب جديد</h2>
      <div class="form-group">
        <label>اسم المستخدم</label>
        <input type="text" placeholder="مثال: خالد">
      </div>
      <div class="form-group">
        <label>كلمة المرور</label>
        <input type="password">
      </div>
      <div class="form-group">
        <label>العمر</label>
        <input type="number" placeholder="22">
      </div>
      <div class="form-group">
        <label>النوع</label>
        <select>
          <option>ذكر</option>
          <option>أنثى</option>
        </select>
      </div>
      <div class="form-group">
        <label>نبذة شخصية</label>
        <textarea placeholder="اكتب القليل عن نفسك..."></textarea>
      </div>
      <button class="btn" onclick="showPage('profile')">إنشاء الحساب</button>
    </section>

    <!-- 4. الغرف -->
    <section id="rooms" class="page">
      <h2>غرف الشات الحية</h2>
      <div class="room-card" onclick="enterRoom('💬 الغرفة العامة')">
        <span>💬 الغرفة العامة</span>
        <small style="color:var(--muted)">انضم الآن ➔</small>
      </div>
      <div class="room-card" onclick="enterRoom('🌙 غرفة السهر')">
        <span>🌙 غرفة السهر</span>
        <small style="color:var(--muted)">انضم الآن ➔</small>
      </div>
      <div class="room-card" onclick="enterRoom('🎀 غرفة البنات')">
        <span>🎀 غرفة البنات</span>
        <small style="color:var(--muted)">انضم الآن ➔</small>
      </div>
      <div class="room-card" onclick="enterRoom('🎮 غرفة الترفيه')">
        <span>🎮 غرفة الترفيه</span>
        <small style="color:var(--muted)">انضم الآن ➔</small>
      </div>
    </section>

    <!-- 5. واجهة الشات (تفتح عند الضغط على غرفة) -->
    <section id="chat" class="page">
      <h2 id="current-room-title">الغرفة العامة</h2>
      <div class="chat-box" id="chat-messages">
        <div class="msg"><div class="msg-user">النظام</div>مرحباً بك في الغرفة! (ملاحظة: الربط الحقيقي للرسائل بقاعدة البيانات سيكون في المرحلة القادمة).</div>
      </div>
      <div style="display:flex; gap: 8px;">
        <input type="text" id="chat-input" placeholder="اكتب رسالتك...">
        <button class="btn" style="width: 90px;" onclick="sendMsg()">إرسال</button>
      </div>
    </section>

    <!-- 6. قائمة المستخدمين -->
    <section id="users" class="page">
      <h2>قائمة المتواجدين حالياً</h2>
      <div class="user-card">
        <span class="status-dot"></span>
        <div>
          <strong>أحمد (تجريبي)</strong>
          <p style="font-size:0.75rem; color:var(--muted)">متصل الآن</p>
        </div>
      </div>
      <div class="user-card">
        <span class="status-dot"></span>
        <div>
          <strong>سارة (تجريبي)</strong>
          <p style="font-size:0.75rem; color:var(--muted)">متصل الآن</p>
        </div>
      </div>
    </section>

    <!-- 7. البروفايل -->
    <section id="profile" class="page">
      <h2>الملف الشخصي</h2>
      <div style="text-align: center; margin-bottom: 1.2rem;">
        <div style="width:70px; height:70px; background:var(--primary); border-radius:50%; margin:0 auto 10px; display:flex; align-items:center; justify-content:center; font-size:1.8rem; color:#fff;">👤</div>
        <h3 id="prof-name">اسم المستخدم التجريبي</h3>
        <p style="color:var(--muted); font-size:0.85rem; margin-top:4px;">العمر: 22 | النوع: ذكر</p>
        <span style="display:inline-block; background:#16a34a; color:#fff; font-size:0.75rem; padding:2px 8px; border-radius:12px; margin-top:6px;">متصل</span>
      </div>
      <div class="form-group">
        <label>النبذة الشخصية</label>
        <p style="background:var(--bg); padding:10px; border-radius:8px; border:1px solid var(--border); font-size:0.9rem;">أهلاً بكم في بروفايلي الخاص على شات غرام قلبي!</p>
      </div>
      <button class="btn" onclick="alert('سيفعل حفظ التعديلات فور ربطه بالـ Supabase Database')">تعديل البروفايل</button>
    </section>
  </main>

  <script>
    const SUPABASE_URL = "https://zhbndfuzqoavotgxkqvc.supabase.co";
    const SUPABASE_KEY = "sb_publishable_8frWX1wu0lE6CTJTroly-w";
    const supabaseClient = supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

    function showPage(pageId) {
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.querySelectorAll('.nav button').forEach(b => b.classList.remove('active'));
      
      const targetPage = document.getElementById(pageId);
      if(targetPage) targetPage.classList.add('active');
      
      const targetBtn = document.getElementById('nav-' + pageId);
      if(targetBtn) targetBtn.classList.add('active');
    }

    function enterRoom(roomName) {
      document.getElementById('current-room-title').innerText = roomName;
      showPage('chat');
    }

    function sendMsg() {
      const input = document.getElementById('chat-input');
      const text = input.value.trim();
      if(!text) return;
      
      const msgBox = document.getElementById('chat-messages');
      const newMsg = document.createElement('div');
      newMsg.className = 'msg';
      newMsg.innerHTML = `<div class="msg-user">أنت</div>${text}`;
      msgBox.appendChild(newMsg);
      msgBox.scrollTop = msgBox.scrollHeight;
      input.value = '';
    }
  </script>
</body>
</html>
