[wpt-home-prototype.html](https://github.com/user-attachments/files/27551924/wpt-home-prototype.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>WPT HOME — Just poker, Just Friends</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root {
    --bg: #0a0a0a;
    --bg-2: #111114;
    --bg-3: #16161a;
    --card: #ffffff;
    --card-soft: #f6f6f7;
    --txt: #1a1a1a;
    --txt-muted: #6b7280;
    --txt-faint: #9ca3af;
    --brand: #E50914;
    --brand-dark: #b8070f;
    --pink: #ffb7c5;
    --pink-2: #ffc7d2;
    --line-d: rgba(255,255,255,0.08);
    --line-l: rgba(0,0,0,0.08);
    --footer-bg: #141416;
  }
  * { box-sizing: border-box; }
  html { scroll-behavior: smooth; }
  body {
    margin: 0;
    background: var(--bg);
    color: white;
    font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", "Microsoft YaHei", "Noto Sans SC", sans-serif;
    -webkit-font-smoothing: antialiased;
  }
  ::selection { background: var(--brand); color: white; }

  .container-x { max-width: 1200px; margin: 0 auto; padding: 0 24px; }
  .page { display: none; animation: fadeIn .3s ease; }
  .page.active { display: block; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(6px);} to { opacity: 1; transform: none;} }

  /* ===== Header ===== */
  header {
    position: sticky; top: 0; z-index: 50;
    background: rgba(10,10,10,0.92);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--line-d);
  }
  .nav-link {
    position: relative; color: rgba(255,255,255,0.65);
    cursor: pointer; padding: 6px 0; font-size: 15px; transition: color .2s;
  }
  .nav-link:hover { color: white; }
  .nav-link.active { color: white; font-weight: 600; }
  .nav-link.active::after {
    content: ''; position: absolute; bottom: -22px; left: 50%; transform: translateX(-50%);
    width: 28px; height: 3px; background: var(--brand); border-radius: 2px;
  }

  /* ===== Section bands ===== */
  .band-1 { background: var(--bg); }
  .band-2 { background: var(--bg-2); }
  .band-3 { background: var(--bg-3); }

  /* ===== Light card (突出于黑色背景) ===== */
  .lcard {
    background: var(--card); color: var(--txt);
    border-radius: 16px; overflow: hidden;
    box-shadow: 0 4px 20px rgba(0,0,0,0.4);
    transition: transform .25s, box-shadow .25s;
    display: flex; flex-direction: column;
  }
  .lcard:hover { transform: translateY(-4px); box-shadow: 0 12px 40px rgba(229,9,20,0.15); }
  .lcard-img {
    aspect-ratio: 16/10;
    background-size: cover; background-position: center;
    background-color: #1f1f24; position: relative;
  }
  .lcard-body { padding: 20px; flex: 1; display: flex; flex-direction: column; }
  .lcard-title { font-size: 17px; font-weight: 700; line-height: 1.4; color: var(--txt); margin-bottom: 10px; }
  .lcard-desc { font-size: 13px; color: var(--txt-muted); line-height: 1.6; flex: 1; margin-bottom: 16px; }
  .lcard-foot { display: flex; align-items: center; justify-content: space-between; padding-top: 12px; border-top: 1px solid var(--line-l); }
  .lcard-link { color: var(--brand); font-weight: 600; font-size: 13px; cursor: pointer; }
  .lcard-link:hover { color: var(--brand-dark); }
  .lcard-date { color: var(--txt-faint); font-size: 12px; display: inline-flex; align-items: center; gap: 4px; }

  /* tags */
  .tag { display: inline-block; padding: 4px 12px; border-radius: 999px; font-size: 12px; font-weight: 600; line-height: 1.4; }
  .tag-pink { background: #FFB7C5; color: white; }
  .tag-orange { background: #F8A989; color: white; }
  .tag-blue { background: #B4D4F8; color: #1a1a1a; }
  .tag-mint { background: #A8DADC; color: #1a1a1a; }

  /* ===== Sub navigation (资讯页) ===== */
  .subnav-link {
    color: rgba(255,255,255,0.55); cursor: pointer;
    padding: 14px 4px; border-bottom: 2px solid transparent;
    font-size: 15px; transition: all .2s; white-space: nowrap; user-select: none;
  }
  .subnav-link:hover { color: white; }
  .subnav-link.active { color: white; border-bottom-color: var(--brand); font-weight: 600; }

  /* academy dropdown (按参考图3) */
  .academy-dd {
    position: absolute; top: calc(100% + 6px); left: -16px;
    width: 380px;
    background: #2a2a2e; border: 1px solid rgba(255,255,255,0.08);
    border-radius: 8px; padding: 18px 24px;
    box-shadow: 0 12px 40px rgba(0,0,0,0.6);
    z-index: 30;
    display: none;
    grid-template-columns: 1fr 1fr;
    gap: 12px 32px;
  }
  .academy-dd.show { display: grid; }
  .academy-dd-item {
    color: white; cursor: pointer; padding: 6px 0; font-size: 15px;
    transition: color .15s;
  }
  .academy-dd-item:hover { color: #4da6ff; }
  .academy-dd-item.active { color: #4da6ff; font-weight: 600; }

  /* ===== Search ===== */
  .search-input {
    background: rgba(255,255,255,0.08); border: 1.5px solid rgba(255,255,255,0.45);
    border-radius: 999px; padding: 11px 16px 11px 44px; color: white; width: 100%;
    transition: all .2s; font-size: 14px;
    box-shadow: 0 0 0 0 rgba(229,9,20,0);
  }
  .search-input:hover { border-color: rgba(255,255,255,0.7); background: rgba(255,255,255,0.12); }
  .search-input:focus { outline: none; border-color: var(--brand); box-shadow: 0 0 0 3px rgba(229,9,20,0.2); }
  .search-input::placeholder { color: rgba(255,255,255,0.55); }

  /* ===== Header dropdown (主导航点击浮现二级分类) ===== */
  .header-dd {
    position: absolute; top: calc(100% + 22px); left: 50%; transform: translateX(-50%);
    background: #2a2a2e; border: 1px solid rgba(255,255,255,0.1);
    border-radius: 10px; padding: 10px 0; min-width: 180px;
    display: none; z-index: 60;
    box-shadow: 0 16px 48px rgba(0,0,0,0.7);
  }
  .header-dd.show { display: block; animation: fadeIn .2s ease; }
  .header-dd::before {
    content: ''; position: absolute; top: -6px; left: 50%; transform: translateX(-50%) rotate(45deg);
    width: 10px; height: 10px; background: #2a2a2e;
    border-left: 1px solid rgba(255,255,255,0.1); border-top: 1px solid rgba(255,255,255,0.1);
  }
  .header-dd-item {
    display: block; padding: 10px 24px; cursor: pointer;
    color: rgba(255,255,255,0.75); font-size: 14px; white-space: nowrap;
    transition: all .15s;
  }
  .header-dd-item:hover { background: rgba(229,9,20,0.1); color: white; }

  /* ===== Buttons ===== */
  .btn-red { background: var(--brand); color: white; transition: background .2s; font-weight: 600; }
  .btn-red:hover { background: var(--brand-dark); }
  .btn-ghost { border: 1px solid rgba(255,255,255,0.2); color: white; transition: all .2s; }
  .btn-ghost:hover { background: rgba(255,255,255,0.05); border-color: rgba(255,255,255,0.4); }

  /* ===== Steps ===== */
  .step-num {
    color: var(--brand); font-weight: 800; font-size: 13px; letter-spacing: 1px;
  }

  /* ===== Help ===== */
  .help-side {
    background: var(--card); color: var(--txt); border-radius: 14px;
    padding: 14px; box-shadow: 0 4px 20px rgba(0,0,0,0.4);
  }
  .help-side-link {
    display: block; padding: 16px 20px; border-radius: 10px; cursor: pointer;
    color: var(--txt-muted); transition: all .15s; font-size: 15px;
    margin-bottom: 4px;
  }
  .help-side-link:hover { background: var(--card-soft); color: var(--txt); }
  .help-side-link.active { color: #2563eb; font-weight: 600; }

  .faq-item {
    background: var(--card); color: var(--txt); border-radius: 12px;
    border: 1px solid var(--line-l); margin-bottom: 16px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.2);
    overflow: hidden;
  }
  .faq-item.open { background: #f6f6f7; }
  .faq-q {
    width: 100%; text-align: left; padding: 24px 28px; cursor: pointer;
    display: flex; align-items: center; justify-content: space-between; gap: 16px;
    color: var(--txt); font-weight: 500; font-size: 15px; background: none; border: none;
  }
  .faq-icon { font-size: 24px; color: #9ca3af; line-height: 1; flex-shrink: 0; transition: transform .2s; }
  .faq-item.open .faq-icon { transform: rotate(45deg); }
  .faq-body { display: none; padding: 0 28px 24px; color: var(--txt-muted); font-size: 14px; line-height: 1.8; }
  .faq-item.open .faq-body { display: block; }

  /* help widget */
  #helpWidget {
    position: fixed; bottom: 84px; right: 24px; width: 320px;
    background: #2a2a2e; border: 1px solid rgba(255,255,255,0.1); border-radius: 12px;
    padding: 22px; display: none; z-index: 60;
    box-shadow: 0 20px 60px rgba(0,0,0,0.6);
  }
  #helpWidget.show { display: block; animation: fadeIn .25s ease; }
  #helpFab {
    position: fixed; bottom: 20px; right: 24px;
    background: var(--brand); border: none;
    display: flex; align-items: center; gap: 8px;
    cursor: pointer; z-index: 60; padding: 12px 22px; border-radius: 999px;
    transition: all .2s;
    box-shadow: 0 8px 24px rgba(229,9,20,0.45);
  }
  #helpFab:hover { background: var(--brand-dark); transform: translateY(-2px); box-shadow: 0 12px 32px rgba(229,9,20,0.55); }

  /* 热门反馈 标签按钮 (浅灰填充) */
  .pop-tag {
    background: rgba(255,255,255,0.12);
    color: white;
    padding: 8px 18px;
    border-radius: 999px;
    font-size: 14px;
    cursor: pointer;
    transition: background .2s;
    border: none;
  }
  .pop-tag:hover { background: rgba(255,255,255,0.2); }

  /* ===== Footer ===== */
  footer { background: var(--footer-bg); border-top: 1px solid var(--line-d); }
  footer a { color: rgba(255,255,255,0.5); transition: color .15s; cursor: pointer; }
  footer a:hover { color: white; }
  .footer-col-h { color: white; font-size: 14px; font-weight: 600; margin-bottom: 16px; }
  .ga-badge {
    width: 64px; height: 64px; border-radius: 50%;
    border: 2px solid #4ade80; color: #4ade80;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    font-size: 8px; font-weight: 700; line-height: 1.1; letter-spacing: 0.5px;
  }
  .social-icon {
    width: 48px; height: 48px; border-radius: 50%;
    display: inline-flex; align-items: center; justify-content: center;
    transition: transform .2s, box-shadow .2s, filter .2s;
    cursor: pointer;
    color: white;
  }
  .social-icon svg { width: 22px; height: 22px; }
  .social-icon:hover { transform: translateY(-3px); filter: brightness(1.1); box-shadow: 0 8px 20px rgba(0,0,0,0.4); }
  .social-instagram { background: linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%); }
  .social-discord { background: #5865F2; }
  .social-x { background: #000000; border: 1px solid rgba(255,255,255,0.2); }
  .social-facebook { background: #1877F2; }
  .social-youtube { background: #FF0000; }

  /* mode card (深底彩色边框) */
  .mode-card {
    background: var(--card); color: var(--txt); border-radius: 16px;
    padding: 24px; box-shadow: 0 4px 20px rgba(0,0,0,0.4);
    transition: transform .25s, box-shadow .25s; cursor: pointer;
  }
  .mode-card:hover { transform: translateY(-4px); box-shadow: 0 12px 40px rgba(229,9,20,0.15); }
  .mode-img {
    aspect-ratio: 1/1; background-size: cover; background-position: center;
    border-radius: 12px; margin-bottom: 20px; background-color: #1f1f24;
  }

  /* feature card (why) */
  .feat-card {
    background: var(--card); color: var(--txt); border-radius: 16px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.4); overflow: hidden;
    transition: transform .25s, box-shadow .25s;
  }
  .feat-card:hover { transform: translateY(-4px); box-shadow: 0 12px 40px rgba(229,9,20,0.15); }

  /* game card */
  .game-card {
    background: var(--card); color: var(--txt); border-radius: 16px;
    padding: 22px; position: relative; transition: transform .25s, box-shadow .25s;
    cursor: pointer; box-shadow: 0 4px 20px rgba(0,0,0,0.4);
    display: flex; flex-direction: column; justify-content: space-between;
    min-height: 160px;
  }
  .game-card:hover { transform: translateY(-2px); box-shadow: 0 12px 40px rgba(229,9,20,0.15); }
  .game-card.feature {
    background: linear-gradient(135deg, #fff7f8 0%, #ffe4e8 100%);
    border: 2px solid var(--brand);
  }
  .game-mini-img {
    width: 80px; height: 60px; background-size: cover; background-position: center;
    background-color: #f3f4f6; border-radius: 8px;
  }

  /* 同图不同色调差异化, 避免视觉重复 */
  .feat-card:nth-of-type(2) .lcard-img { filter: hue-rotate(20deg) saturate(1.1); }
  .feat-card:nth-of-type(3) .lcard-img { filter: hue-rotate(-25deg) brightness(1.05); }
  .mode-card:nth-of-type(2) .mode-img { filter: hue-rotate(-30deg); }
  .mode-card:nth-of-type(3) .mode-img { filter: hue-rotate(45deg) saturate(0.9); }
  .games-row:nth-of-type(3) .game-mini-img { filter: hue-rotate(30deg); }
  .news-tab .grid > article:nth-child(2n) .lcard-img { filter: hue-rotate(25deg); }
  .news-tab .grid > article:nth-child(3n+1) .lcard-img { filter: hue-rotate(-20deg) saturate(1.1); }
  .news-tab .grid > article:nth-child(5n) .lcard-img { filter: hue-rotate(60deg) brightness(1.05); }

  /* SQUID 大卡 + NLHE/PLO 上下叠 布局 */
  .games-top-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    gap: 16px;
  }
  .games-squid {
    grid-row: span 2;
    display: flex !important;
    flex-direction: column !important;
    justify-content: space-between;
    min-height: 380px;
    padding: 28px;
  }
  .games-squid-emoji {
    font-size: 96px;
    text-align: center;
    margin: 8px 0;
    line-height: 1;
  }
  .games-row {
    flex-direction: row !important;
    align-items: center;
    justify-content: space-between;
    gap: 20px;
    min-height: auto;
  }
  .games-row > div:first-child { flex: 1; }
  .games-row .game-mini-img { flex-shrink: 0; width: 100px; height: 80px; }

  @media (max-width: 768px) {
    .games-top-grid { grid-template-columns: 1fr; grid-template-rows: auto; }
    .games-squid { grid-row: auto; min-height: auto; }
  }
</style>
</head>
<body>

<!-- ============== HEADER ============== -->
<header>
  <div class="container-x h-20 flex items-center justify-between gap-8">
    <a onclick="goPage('home')" class="flex items-center gap-2 cursor-pointer flex-shrink-0">
      <div class="flex items-center">
        <div class="w-2.5 h-7 bg-red-600" style="clip-path: polygon(20% 0, 100% 0, 80% 100%, 0 100%);"></div>
        <div class="w-2.5 h-7 bg-white -ml-0.5" style="clip-path: polygon(20% 0, 100% 0, 80% 100%, 0 100%);"></div>
      </div>
      <div class="flex items-baseline ml-1">
        <span class="font-black text-xl tracking-tight">WPT</span>
        <span class="font-black text-xl tracking-tight ml-1">HOME</span>
        <sup class="text-[8px] ml-0.5 opacity-60">™</sup>
      </div>
    </a>
    <nav class="hidden md:flex gap-12">
      <a class="nav-link active" data-page="home" onclick="goPage('home')">首页</a>
      <a class="nav-link" data-page="news" onclick="goPage('news')">资讯</a>
      <a class="nav-link" data-page="about" onclick="goPage('about')">关于我们</a>
      <a class="nav-link" data-page="help" onclick="goPage('help')">帮助</a>
    </nav>
    <div class="flex items-center gap-3">
      <div class="relative hidden sm:block">
        <select class="bg-black border border-white/20 rounded pl-3 pr-8 py-1.5 text-sm appearance-none cursor-pointer">
          <option>English</option>
          <option>简体中文</option>
        </select>
        <span class="absolute right-2 top-1/2 -translate-y-1/2 text-xs pointer-events-none opacity-60">▾</span>
      </div>
      <button class="btn-red px-5 py-2 rounded text-sm">立即下载</button>
    </div>
  </div>
</header>

<!-- ============== HOME ============== -->
<section id="page-home" class="page active">

  <!-- HERO -->
  <div class="band-1 py-16 md:py-20">
    <div class="container-x">
      <div class="rounded-3xl overflow-hidden relative" style="background: linear-gradient(135deg, #1a0a0a 0%, #0a0a0a 50%, #1a0a0a 100%);">
        <div class="absolute inset-0" style="background-image: url('https://images.unsplash.com/photo-1541278107931-e006523892df?w=1920&q=80'); background-size: cover; background-position: center; opacity: 0.35;"></div>
        <div class="absolute inset-0" style="background: radial-gradient(ellipse at center, transparent 30%, rgba(10,10,10,0.85) 100%);"></div>
        <div class="relative px-6 py-24 md:py-32 text-center">
          <p class="text-[10px] tracking-[0.5em] text-white/50 mb-2 font-medium">主视觉图（牌桌、好友、德州）</p>
          <h1 class="text-5xl md:text-7xl font-black tracking-tight my-6">
            Just poker, <span class="text-red-500">Just Friends</span>
          </h1>
          <p class="text-white/70 mb-10 text-base">WPT 官方授权的免费应用</p>
          <div class="flex flex-wrap justify-center gap-3">
            <a class="bg-black border border-white/25 rounded-lg px-5 py-2.5 flex items-center gap-2.5 hover:bg-white/5 transition cursor-pointer">
              <svg viewBox="0 0 24 24" fill="white" class="w-6 h-6"><path d="M17.05 20.28c-.98.95-2.05.8-3.08.35-1.09-.46-2.09-.48-3.24 0-1.44.62-2.2.44-3.06-.35C2.79 15.25 3.51 7.59 9.05 7.31c1.35.07 2.29.74 3.08.8 1.18-.24 2.31-.93 3.57-.84 1.51.12 2.65.72 3.4 1.8-3.12 1.87-2.38 5.98.48 7.13-.57 1.5-1.31 2.99-2.54 4.09zM12.03 7.25c-.15-2.23 1.66-4.07 3.74-4.25.29 2.58-2.34 4.5-3.74 4.25z"/></svg>
              <div class="text-left leading-tight">
                <div class="text-[10px] text-white/60">Download on the</div>
                <div class="text-sm font-semibold">App Store</div>
              </div>
            </a>
            <a class="bg-black border border-white/25 rounded-lg px-5 py-2.5 flex items-center gap-2.5 hover:bg-white/5 transition cursor-pointer">
              <svg viewBox="0 0 24 24" class="w-6 h-6"><path fill="#34A853" d="M3 20.5V3.5L13.5 12 3 20.5z"/><path fill="#EA4335" d="M3 3.5L13.5 12l3.5-3.3-12-7c-.7-.4-1.5-.4-2 1.8z"/><path fill="#4285F4" d="M3 20.5L13.5 12l3.5 3.3-12 7c-.5 2.2.3 2.2 2-1.8z"/><path fill="#FBBC04" d="M13.5 12l3.5-3.3 4 2.3c1 .6 1 1.4 0 2l-4 2.3-3.5-3.3z"/></svg>
              <div class="text-left leading-tight">
                <div class="text-[10px] text-white/60">GET IT ON</div>
                <div class="text-sm font-semibold">Google Play</div>
              </div>
            </a>
            <button class="btn-red rounded-lg px-6 py-2.5 text-sm">免费开局</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- WHY WPT HOME -->
  <div class="band-2 py-20">
    <div class="container-x">
      <h2 class="text-4xl md:text-5xl font-bold text-center mb-14">为什么选择 WPT HOME?</h2>
      <div class="grid md:grid-cols-3 gap-6">
        <div class="feat-card">
          <div class="lcard-img" style="background-image: url('https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=600&q=80');"></div>
          <div class="p-6">
            <h3 class="text-lg font-bold mb-3">免费开局</h3>
            <p class="text-sm leading-relaxed" style="color: var(--txt-muted);">100% 免费不抽水<br>0 门槛入场，随进随玩，平台不收取任何手续费。</p>
          </div>
        </div>
        <div class="feat-card">
          <div class="lcard-img" style="background-image: url('https://images.unsplash.com/photo-1541278107931-e006523892df?w=600&q=80');"></div>
          <div class="p-6">
            <h3 class="text-lg font-bold mb-3">公平安全</h3>
            <p class="text-sm leading-relaxed" style="color: var(--txt-muted);">安全卫士，防伙牌实时检测<br>RNG 随机认证 + 反作弊算法，每一手牌都经得起检验。</p>
          </div>
        </div>
        <div class="feat-card">
          <div class="lcard-img" style="background-image: url('https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=600&q=80');"></div>
          <div class="p-6">
            <h3 class="text-lg font-bold mb-3">极致体验</h3>
            <p class="text-sm leading-relaxed" style="color: var(--txt-muted);">舒适流畅的游戏体验感，桌内道具互动、动态表情包、实时聊天功能、排牌特效，把现场感搬到屏幕。</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 5 SEC QUICK START -->
  <div class="band-3 py-20">
    <div class="container-x">
      <h2 class="text-4xl md:text-5xl font-bold text-center mb-14">5 秒快速开局</h2>
      <div class="grid md:grid-cols-[1.4fr_1fr] gap-12 items-center">
        <div class="rounded-2xl overflow-hidden relative aspect-video group cursor-pointer" style="background-image: linear-gradient(135deg, rgba(229,9,20,0.15), rgba(0,0,0,0.6)), url('https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=1200&q=80'); background-size: cover; background-position: center;">
          <div class="absolute inset-0 flex items-center justify-center">
            <div class="w-16 h-16 rounded-full bg-white/95 flex items-center justify-center group-hover:scale-110 transition shadow-2xl">
              <svg class="w-7 h-7 ml-1 text-red-600" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg>
            </div>
          </div>
          <div class="absolute bottom-4 left-5 text-sm text-white/80">演示视频(自动播放)</div>
        </div>
        <div class="space-y-8">
          <div>
            <p class="step-num mb-1">Step 01</p>
            <h4 class="text-lg font-bold mb-2">创建房间</h4>
            <p class="text-white/55 text-sm leading-relaxed">选择盲注、人数, 1 秒生成专属牌桌</p>
          </div>
          <div>
            <p class="step-num mb-1">Step 02</p>
            <h4 class="text-lg font-bold mb-2">分享给好友</h4>
            <p class="text-white/55 text-sm leading-relaxed">二维码、链接、邀请码, 任选 1 种发到群里</p>
          </div>
          <div>
            <p class="step-num mb-1">Step 03</p>
            <h4 class="text-lg font-bold mb-2">朋友入座</h4>
            <p class="text-white/55 text-sm leading-relaxed">扫码即入, 无需注册下载, 立刻开打</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- MODES -->
  <div class="band-2 py-20">
    <div class="container-x">
      <h2 class="text-4xl md:text-5xl font-bold text-center mb-14">多种模式玩法, 随时随地入局</h2>
      <div class="grid md:grid-cols-3 gap-6">
        <div class="mode-card">
          <h3 class="text-xl font-bold mb-1">好友桌</h3>
          <p class="text-sm mb-5" style="color: var(--txt-muted);">和朋友圈，随时开桌</p>
          <div class="mode-img" style="background-image: url('https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=600&q=80');"></div>
          <div class="grid grid-cols-2 gap-y-2 text-sm" style="color: var(--txt);">
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>密码房</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>自定义规则</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>一键加入</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>房主特权</div>
          </div>
        </div>
        <div class="mode-card">
          <h3 class="text-xl font-bold mb-1">俱乐部</h3>
          <p class="text-sm mb-5" style="color: var(--txt-muted);">长期牌友, 一起竞技</p>
          <div class="mode-img" style="background-image: url('https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=600&q=80');"></div>
          <div class="grid grid-cols-2 gap-y-2 text-sm" style="color: var(--txt);">
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>长期社群</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>竞技排行</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>联赛丰富</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>Host 管理</div>
          </div>
        </div>
        <div class="mode-card">
          <h3 class="text-xl font-bold mb-1">大厅模式</h3>
          <p class="text-sm mb-5" style="color: var(--txt-muted);">开放对局, 实时匹配</p>
          <div class="mode-img" style="background-image: url('https://images.unsplash.com/photo-1541278107931-e006523892df?w=600&q=80');"></div>
          <div class="grid grid-cols-2 gap-y-2 text-sm" style="color: var(--txt);">
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>随进随玩</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>自由选择</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>多盲注级别</div>
            <div class="flex items-center gap-2"><span class="text-emerald-500">✓</span>智能匹配</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 7 GAME TYPES -->
  <div class="band-3 py-20">
    <div class="container-x">
      <h2 class="text-4xl md:text-5xl font-bold text-center mb-2">7 种玩法 × 2 套配置 = 无限可能</h2>
      <p class="text-center text-white/55 text-sm mb-14">Quick Mode 一键开桌 / Pro Mode 自定义设置</p>
      <!-- 上区: 2 列, SQUID 占左侧整列, NLHE/PLO 在右侧上下叠 -->
      <div class="games-top-grid mb-4">
        <div class="game-card feature games-squid" style="background-image: linear-gradient(135deg, rgba(255,247,248,0.92), rgba(255,228,232,0.92)), url('https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80'); background-size: cover; background-position: center;">
          <div>
            <span class="tag tag-pink mb-3 inline-block">招牌玩法</span>
          </div>
          <div class="games-squid-emoji">🦑</div>
          <div>
            <h3 class="text-3xl font-black mb-1">SQUID</h3>
            <p class="text-sm" style="color: var(--txt-muted);">鱿鱼标记淘汰</p>
            <p class="text-sm mt-6" style="color: var(--txt-muted);">每手赢家拿筹鱼, 丢完出局, 最后留桌通吃。</p>
          </div>
        </div>
        <div class="game-card games-row">
          <div>
            <h3 class="text-xl font-bold mb-1">NLHE</h3>
            <p class="text-sm" style="color: var(--txt-muted);">无限注德州</p>
            <p class="text-xs mt-1" style="color: var(--txt-faint);">经典玩法，无注码上限</p>
          </div>
          <div class="game-mini-img" style="background-image: url('https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=300&q=80');"></div>
        </div>
        <div class="game-card games-row">
          <div>
            <h3 class="text-xl font-bold mb-1">PLO</h3>
            <p class="text-sm" style="color: var(--txt-muted);">底池限注奥马哈</p>
            <p class="text-xs mt-1" style="color: var(--txt-faint);">4或5张底牌，数学性强</p>
          </div>
          <div class="game-mini-img" style="background-image: url('https://images.unsplash.com/photo-1541278107931-e006523892df?w=300&q=80');"></div>
        </div>
      </div>
      <!-- 下区: 4 等分 -->
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
        <div class="game-card">
          <div class="game-mini-img mb-3" style="background-image: url('https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=300&q=80'); width: 100%; height: 70px;"></div>
          <h3 class="text-base font-bold">SD 6+</h3>
          <p class="text-sm" style="color: var(--txt-muted);">短牌</p>
          <p class="text-xs mt-1" style="color: var(--txt-faint);">去掉 2-5 的 36 张牌玩法</p>
        </div>
        <div class="game-card">
          <span class="tag tag-blue mb-2 inline-block self-start">俱乐部玩法</span>
          <div class="game-mini-img mb-3" style="background-image: url('https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=300&q=80'); width: 100%; height: 70px;"></div>
          <h3 class="text-base font-bold">MTT</h3>
          <p class="text-sm" style="color: var(--txt-muted);">多桌锦标赛</p>
          <p class="text-xs mt-1" style="color: var(--txt-faint);">多桌锦标赛，一路竞速到冠军产生</p>
        </div>
        <div class="game-card">
          <div class="game-mini-img mb-3" style="background-image: url('https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=300&q=80'); width: 100%; height: 70px;"></div>
          <h3 class="text-base font-bold">SNG</h3>
          <p class="text-sm" style="color: var(--txt-muted);">即使锦标赛</p>
          <p class="text-xs mt-1" style="color: var(--txt-faint);">满人数即开赛</p>
        </div>
        <div class="game-card">
          <div class="game-mini-img mb-3" style="background-image: url('https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=300&q=80'); width: 100%; height: 70px;"></div>
          <h3 class="text-base font-bold">AOF</h3>
          <p class="text-sm" style="color: var(--txt-muted);">All-In or Fold</p>
          <p class="text-xs mt-1" style="color: var(--txt-faint);">每手只能 all-in 或 fold</p>
        </div>
      </div>
    </div>
  </div>

</section>

<!-- ============== NEWS ============== -->
<section id="page-news" class="page">
  <div class="border-b border-white/5 sticky top-20 bg-black/85 backdrop-blur z-30">
    <div class="container-x">
      <div class="flex items-center justify-between gap-6 flex-wrap">
        <div class="flex flex-wrap items-end gap-x-8 gap-y-1 relative">
          <a class="subnav-link active" data-news="wpt" onclick="goNews('wpt')">WPT</a>
          <a class="subnav-link" data-news="match" onclick="goNews('match')">比赛&amp;消息</a>
          <span class="relative inline-flex">
            <a class="subnav-link" data-news="academy" onclick="goNews('academy', event)">扑克学院</a>
            <div id="academyDD" class="academy-dd">
              <span class="academy-dd-item active" data-cat="all" onclick="goAcademyCat('all', event)">全部</span>
              <span class="academy-dd-item" data-cat="advanced" onclick="goAcademyCat('advanced', event)">进阶技巧</span>
              <span class="academy-dd-item" data-cat="newbie" onclick="goAcademyCat('newbie', event)">新手教程</span>
              <span class="academy-dd-item" data-cat="pro" onclick="goAcademyCat('pro', event)">高阶策略</span>
              <span class="academy-dd-item" data-cat="term" onclick="goAcademyCat('term', event)">扑克术语</span>
              <span class="academy-dd-item" data-cat="rule" onclick="goAcademyCat('rule', event)">玩法规则</span>
            </div>
          </span>
          <a class="subnav-link" data-news="tournament" onclick="goNews('tournament')">锦标赛</a>
          <a class="subnav-link" data-news="cash" onclick="goNews('cash')">普通桌</a>
          <a class="subnav-link" data-news="dynamic" onclick="goNews('dynamic')">游戏动态</a>
        </div>
        <div class="relative w-72 flex-shrink-0 hidden md:block">
          <span class="absolute left-4 top-1/2 -translate-y-1/2 text-white/40 text-sm">🔍</span>
          <input class="search-input" placeholder="输入搜索内容" />
        </div>
      </div>
    </div>
  </div>

  <div class="container-x py-12">
    <div class="news-tab" data-news-tab="wpt"><div class="grid md:grid-cols-3 gap-6"></div></div>
    <div class="news-tab hidden" data-news-tab="match"><div class="grid md:grid-cols-3 gap-6"></div></div>
    <div class="news-tab hidden" data-news-tab="academy"><div class="grid md:grid-cols-3 gap-6"></div></div>
    <div class="news-tab hidden" data-news-tab="tournament"><div class="grid md:grid-cols-3 gap-6"></div></div>
    <div class="news-tab hidden" data-news-tab="cash"><div class="grid md:grid-cols-3 gap-6"></div></div>
    <div class="news-tab hidden" data-news-tab="dynamic"><div class="grid md:grid-cols-3 gap-6"></div></div>

    <div class="flex justify-center mt-12">
      <button class="btn-ghost rounded-full px-8 py-2.5 text-sm">加载更多</button>
    </div>
  </div>
</section>

<!-- ============== ABOUT ============== -->
<section id="page-about" class="page">
  <div class="container-x py-16">
    <!-- Hero: 左标题 + 右图 -->
    <div class="grid md:grid-cols-2 gap-12 items-center mb-12">
      <h1 class="text-5xl md:text-6xl font-black tracking-tight">关于我们</h1>
      <div class="aspect-[16/10] rounded-2xl overflow-hidden shadow-2xl" style="background-image: linear-gradient(135deg, rgba(0,0,0,0.2), rgba(0,0,0,0.4)), url('https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=1200&q=80'); background-size: cover; background-position: center;"></div>
    </div>

    <!-- 介绍段（上下分隔线） -->
    <div class="border-t border-b border-white/10 py-10 mb-20">
      <p class="text-white/85 leading-loose text-base md:text-lg max-w-4xl">
        WPT HOME™ 是 World Poker Tour® (WPT) 官方出品，专为好友对局打造的免费德州扑克平台。专为你的熟人圈打造——无论是同窗同事，还是挚爱亲朋。在这里，我们提供免费、公平的空间，回归最纯粹的对局与相聚之乐。
      </p>
    </div>

    <!-- 区块1: 左图 + 右文 (我们的使命) -->
    <div class="grid md:grid-cols-2 gap-12 md:gap-16 items-center mb-24">
      <div class="aspect-[16/10] rounded-2xl overflow-hidden shadow-2xl" style="background-image: linear-gradient(135deg, rgba(0,0,0,0.2), rgba(0,0,0,0.4)), url('https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=1200&q=80'); background-size: cover; background-position: center;"></div>
      <div>
        <h2 class="text-3xl md:text-4xl font-bold mb-6">我们的使命</h2>
        <p class="text-white/70 leading-loose text-base">让朋友间的德州牌局更纯粹。WPT HOME™ 致力于为熟人圈提供零门槛、不抽水、随时随地的免费德州对局空间，让每一次相聚都能轻松开桌。</p>
      </div>
    </div>

    <!-- 区块2: 左文 + 右图 (为什么选择) -->
    <div class="grid md:grid-cols-2 gap-12 md:gap-16 items-center mb-24">
      <div>
        <h2 class="text-3xl md:text-4xl font-bold mb-6">为什么选择 WPT HOME™?</h2>
        <p class="text-white/70 leading-loose text-base">WPT 官方授权背书，100% 免费不抽水，第三方 RNG 公平认证，5 秒一键邀请好友入桌。专为熟人社交场景设计，把一群人的牌局体验做到极致。</p>
      </div>
      <div class="aspect-[16/10] rounded-2xl overflow-hidden shadow-2xl md:order-last" style="background-image: linear-gradient(135deg, rgba(0,0,0,0.2), rgba(0,0,0,0.4)), url('https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=1200&q=80'); background-size: cover; background-position: center;"></div>
    </div>

    <!-- 区块3: 左图 + 右文 (承诺) -->
    <div class="grid md:grid-cols-2 gap-12 md:gap-16 items-center">
      <div class="aspect-[16/10] rounded-2xl overflow-hidden shadow-2xl" style="background-image: linear-gradient(135deg, rgba(0,0,0,0.2), rgba(0,0,0,0.4)), url('https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=1200&q=80'); background-size: cover; background-position: center;"></div>
      <div>
        <h2 class="text-3xl md:text-4xl font-bold mb-6">WPT HOME™ 的承诺</h2>
        <p class="text-white/70 leading-loose text-base">永远不抽水、永远不强制付费、永远守护公平。所有对局基于虚拟筹码，仅作社交娱乐用途，让每一位玩家都能放心享受牌桌时光。</p>
      </div>
    </div>
  </div>
</section>

<!-- ============== HELP ============== -->
<section id="page-help" class="page">
  <div class="container-x py-10 md:py-12">
    <div class="flex items-start justify-between gap-6 flex-wrap mb-8">
      <h1 class="text-3xl md:text-5xl font-bold leading-tight">关于线上游戏的常见问题</h1>
      <div class="relative w-full md:w-80 md:mt-2">
        <span class="absolute left-4 top-1/2 -translate-y-1/2 text-white/40 text-sm">🔍</span>
        <input class="search-input" placeholder="输入关键词查找问题" />
      </div>
    </div>

    <div class="flex items-center gap-3 mb-8 flex-wrap">
      <span class="text-base font-bold flex items-center gap-1.5 text-white">🔥 热门反馈:</span>
      <button class="pop-tag" onclick="goHelpCat('create')">创建牌桌</button>
      <button class="pop-tag" onclick="goHelpCat('club')">俱乐部玩法</button>
      <button class="pop-tag" onclick="goHelpCat('report')">举报作弊</button>
      <button class="pop-tag" onclick="goHelpCat('bind')">账号绑定</button>
    </div>

    <div class="border-t border-white/10 pt-8 grid md:grid-cols-[260px_1fr] gap-10">
      <aside class="help-side self-start">
        <a class="help-side-link" data-help="faq" onclick="goHelpCat('faq')">常见问答集</a>
        <a class="help-side-link active" data-help="bind" onclick="goHelpCat('bind')">账户绑定</a>
        <a class="help-side-link" data-help="reward" onclick="goHelpCat('reward')">邀请奖励</a>
        <a class="help-side-link" data-help="diamond" onclick="goHelpCat('diamond')">钻石作用</a>
        <a class="help-side-link" data-help="create" onclick="goHelpCat('create')">开桌教程</a>
        <a class="help-side-link" data-help="fair" onclick="goHelpCat('fair')">公平与安全</a>
        <a class="help-side-link" data-help="feedback" onclick="goHelpCat('feedback')">意见反馈 ›</a>
      </aside>
      <div id="helpFaqPanel"></div>
    </div>
  </div>

  <div id="helpFab" onclick="toggleHelpWidget()">
    <span class="text-base">💬</span>
    <span class="text-white text-sm">帮助</span>
  </div>
  <div id="helpWidget">
    <div class="flex items-start justify-between mb-3">
      <h4 class="text-base font-bold">帮助中心</h4>
      <button onclick="toggleHelpWidget()" class="text-white/60 hover:text-white text-xl leading-none">×</button>
    </div>
    <p class="text-white/60 text-sm mb-5 leading-relaxed">我们全天候为您提供服务。<br>欢迎随时联络我们。</p>
    <div class="flex gap-3">
      <button class="btn-ghost rounded-md px-4 py-2 text-sm flex-1 flex items-center justify-center gap-2">✉ 电子邮件</button>
      <button class="btn-ghost rounded-md px-4 py-2 text-sm flex-1 flex items-center justify-center gap-2">💬 聊天</button>
    </div>
  </div>
</section>

<!-- ============== FOOTER ============== -->
<footer class="pt-14 pb-8 px-6">
  <div class="container-x">
    <div class="grid md:grid-cols-[1.4fr_1fr_1fr_1fr] gap-10 mb-10">

      <!-- Brand col -->
      <div>
        <div class="flex items-center gap-2 mb-5">
          <div class="flex items-center">
            <div class="w-2.5 h-7 bg-red-600" style="clip-path: polygon(20% 0, 100% 0, 80% 100%, 0 100%);"></div>
            <div class="w-2.5 h-7 bg-white -ml-0.5" style="clip-path: polygon(20% 0, 100% 0, 80% 100%, 0 100%);"></div>
          </div>
          <div class="flex items-baseline ml-1">
            <span class="font-black tracking-tight text-base">WPT HOME</span>
            <sup class="text-[8px] ml-0.5 opacity-60">™</sup>
          </div>
        </div>

        <div class="flex items-start gap-6 mb-6">
          <div>
            <p class="text-white/40 text-xs mb-2">公平认证</p>
            <div class="ga-badge">
              <div>Gaming</div>
              <div>Associates</div>
              <div style="color: #4ade80;">CERTIFIED</div>
            </div>
          </div>
          <div>
            <p class="text-white/40 text-xs mb-2">联系我们</p>
            <a href="mailto:support@wpthome.com" class="text-sm">support@wpthome.com</a>
          </div>
        </div>

        <div class="flex gap-3">
          <a class="social-icon social-instagram" title="Instagram">
            <svg viewBox="0 0 24 24" fill="white"><path d="M12 2.2c3.2 0 3.6 0 4.8.1 1.2.1 1.8.2 2.2.4.6.2 1 .5 1.4.9.4.4.7.9.9 1.4.2.4.4 1 .4 2.2.1 1.2.1 1.6.1 4.8s0 3.6-.1 4.8c-.1 1.2-.2 1.8-.4 2.2-.2.6-.5 1-.9 1.4-.4.4-.9.7-1.4.9-.4.2-1 .4-2.2.4-1.2.1-1.6.1-4.8.1s-3.6 0-4.8-.1c-1.2-.1-1.8-.2-2.2-.4-.6-.2-1-.5-1.4-.9-.4-.4-.7-.9-.9-1.4-.2-.4-.4-1-.4-2.2C2.2 15.6 2.2 15.2 2.2 12s0-3.6.1-4.8c.1-1.2.2-1.8.4-2.2.2-.6.5-1 .9-1.4.4-.4.9-.7 1.4-.9.4-.2 1-.4 2.2-.4C8.4 2.2 8.8 2.2 12 2.2zm0 2.1c-3.1 0-3.5 0-4.7.1-1.1.1-1.7.2-2.1.4-.5.2-.9.4-1.3.8-.4.4-.6.8-.8 1.3-.2.4-.3 1-.4 2.1-.1 1.2-.1 1.6-.1 4.7s0 3.5.1 4.7c.1 1.1.2 1.7.4 2.1.2.5.4.9.8 1.3.4.4.8.6 1.3.8.4.2 1 .3 2.1.4 1.2.1 1.6.1 4.7.1s3.5 0 4.7-.1c1.1-.1 1.7-.2 2.1-.4.5-.2.9-.4 1.3-.8.4-.4.6-.8.8-1.3.2-.4.3-1 .4-2.1.1-1.2.1-1.6.1-4.7s0-3.5-.1-4.7c-.1-1.1-.2-1.7-.4-2.1-.2-.5-.4-.9-.8-1.3-.4-.4-.8-.6-1.3-.8-.4-.2-1-.3-2.1-.4-1.2-.1-1.6-.1-4.7-.1zm0 3.5a4.2 4.2 0 110 8.4 4.2 4.2 0 010-8.4zm0 6.9a2.7 2.7 0 100-5.4 2.7 2.7 0 000 5.4zm5.4-7c0 .5-.4 1-1 1s-1-.4-1-1 .4-1 1-1 1 .4 1 1z"/></svg>
          </a>
          <a class="social-icon social-discord" title="Discord">
            <svg viewBox="0 0 24 24" fill="white"><path d="M19.6 4.6A18.3 18.3 0 0015 3.2l-.2.4a16 16 0 00-5.6 0l-.2-.4a18.3 18.3 0 00-4.6 1.4 18.7 18.7 0 00-3.3 12.7 18.4 18.4 0 005.6 2.8l.4-.6a13 13 0 01-2-1l.5-.4a13 13 0 0011.2 0l.5.4c-.6.4-1.3.7-2 1l.4.6a18.4 18.4 0 005.6-2.8 18.7 18.7 0 00-3.3-12.7zM8.5 15a2.2 2.2 0 01-2-2.3 2.2 2.2 0 012-2.3 2.2 2.2 0 012 2.3 2.2 2.2 0 01-2 2.3zm7 0a2.2 2.2 0 01-2-2.3 2.2 2.2 0 012-2.3 2.2 2.2 0 012 2.3 2.2 2.2 0 01-2 2.3z"/></svg>
          </a>
          <a class="social-icon social-x" title="X">
            <svg viewBox="0 0 24 24" fill="white"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
          </a>
          <a class="social-icon social-facebook" title="Facebook">
            <svg viewBox="0 0 24 24" fill="white"><path d="M24 12c0-6.6-5.4-12-12-12S0 5.4 0 12c0 6 4.4 11 10.1 11.9v-8.4H7.1V12h3v-2.6c0-3 1.8-4.6 4.5-4.6 1.3 0 2.7.2 2.7.2v3h-1.5c-1.5 0-2 .9-2 1.9V12h3.3l-.5 3.5h-2.8v8.4C19.6 23 24 18 24 12z"/></svg>
          </a>
          <a class="social-icon social-youtube" title="YouTube">
            <svg viewBox="0 0 24 24" fill="white"><path d="M23.5 6.2a3 3 0 00-2.1-2.1C19.5 3.6 12 3.6 12 3.6s-7.5 0-9.4.5A3 3 0 00.5 6.2C0 8.1 0 12 0 12s0 3.9.5 5.8a3 3 0 002.1 2.1c1.9.5 9.4.5 9.4.5s7.5 0 9.4-.5a3 3 0 002.1-2.1c.5-1.9.5-5.8.5-5.8s0-3.9-.5-5.8zM9.6 15.6V8.4l6.3 3.6-6.3 3.6z"/></svg>
          </a>
        </div>
      </div>

      <!-- 下载 -->
      <div>
        <h5 class="footer-col-h">下载</h5>
        <ul class="space-y-3 text-sm">
          <li><a>Google Play</a></li>
          <li><a>App Store</a></li>
          <li><a>网页游戏</a></li>
        </ul>
      </div>

      <!-- 条款 -->
      <div>
        <h5 class="footer-col-h">条款</h5>
        <ul class="space-y-3 text-sm">
          <li><a>隐私协议</a></li>
          <li><a>服务条款</a></li>
          <li><a>数据安全</a></li>
          <li><a>账号安全</a></li>
          <li><a>儿童安全政策</a></li>
        </ul>
      </div>

      <!-- 关于 -->
      <div>
        <h5 class="footer-col-h">关于</h5>
        <ul class="space-y-3 text-sm">
          <li><a onclick="goPage('about')">关于我们</a></li>
          <li><a onclick="goPage('help')">问题和回复</a></li>
          <li><a>意见反馈</a></li>
          <li><a>RNG 认证</a></li>
        </ul>
      </div>
    </div>

    <div class="border-t border-white/5 pt-6 text-center space-y-2">
      <p class="text-white/40 text-xs">免责声明：WPT HOME 是一款免费的游戏应用，所有玩法使用虚拟游戏币，不具有任何现实意义和价值，不提供任何经济奖励</p>
      <p class="text-white/40 text-xs">Grand Cru Concepts</p>
      <p class="text-white/40 text-xs">Copyright © 2026 WPT Enterprises, Inc.</p>
    </div>
  </div>
</footer>

<!-- ============== JS ============== -->
<script>
  // ===== data =====
  const newsContent = {
    wpt: [
      { tag:'赛事战报', tagClass:'tag-pink', title:'2026 WPT 全球总决赛战报: Schuyler Thornton 夺冠 $2.25M', desc:'本届 WPT 全球总决赛在拉斯维加斯落幕, 新晋冠军拿下 $2,258,856 巨额奖金, 中国军团创历史新高...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
      { tag:'名人堂', tagClass:'tag-orange', title:'WPT 冠军访谈: 打牌 30 年我学到了什么', desc:'三届 WPT 冠军 Anthony Zinno 罕见接受访谈, 谈职业生涯起伏、心态管理, 以及对新一代玩家的建议...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
      { tag:'赛事战报', tagClass:'tag-pink', title:'2026 上半年全球扑克奖金榜 TOP50', desc:'截至 5 月, 2026 全球扑克奖金榜出炉, 前 50 名累计赢得 4.2 亿美元, 完整名单与数据分析...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
      { tag:'赛事战报', tagClass:'tag-pink', title:'WPT Prague 2026 主赛冠军诞生, 中国选手首次进 FT', desc:'布拉格站主赛事完美收官, 中国选手李雨晨闯入终极桌, 最终位列第 4 名, 创造亚洲选手最佳战绩...', date:'May 4, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'名人堂', tagClass:'tag-orange', title:'Daniel Negreanu 谈 GTO: 直觉是不可替代的', desc:'扑克活传奇 Daniel Negreanu 撰文反思 GTO 时代的扑克, 提出"直觉与计算并行"的全新打法理论...', date:'May 3, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'赛事战报', tagClass:'tag-pink', title:'WPT World Championship 2026 赛程公告', desc:'年度盛事 WPT 世界锦标赛将于 12 月在拉斯维加斯举行, 总奖池预计突破 5000 万美元, 报名已开放...', date:'May 2, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
    ],
    match: [
      { tag:'比赛通知', tagClass:'tag-pink', title:'WPT HOME 春季锦标赛今日开放报名', desc:'平台周年专属赛事, 总奖池 $200,000 虚拟币, 满台即开, 8 强直通月度大师赛...', date:'May 8, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
      { tag:'比赛通知', tagClass:'tag-pink', title:'每日 SQUID 挑战赛改版上线', desc:'招牌玩法 SQUID 加入排位机制, 段位决定开局筹鱼数, 周榜前十瓜分奖池...', date:'May 7, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
      { tag:'平台公告', tagClass:'tag-mint', title:'5 月版本更新: 实时聊天表情包大升级', desc:'新增 60+ 动态表情, 桌内一键投掷, 支持自定义贴纸, 让对局更有趣...', date:'May 5, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'比赛通知', tagClass:'tag-pink', title:'俱乐部联赛 S3 赛季启动', desc:'全球俱乐部联赛第三赛季正式开启, 已有 2,300+ 俱乐部完成报名, 冠军俱乐部直通线下 WPT...', date:'May 4, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'平台公告', tagClass:'tag-mint', title:'反伙牌系统升级: AI 检测准确率提升至 99.7%', desc:'新一代反作弊算法上线, 多手联动模式识别能力翻倍, 累计封禁可疑账号 1.2 万...', date:'May 2, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'比赛通知', tagClass:'tag-pink', title:'欧洲玩家专场: 周末高额桌邀请赛', desc:'欧洲时区限定的 NLHE 高额桌, 邀请赛形式, 通过资格赛免费晋级...', date:'May 1, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
    ],
    academy: [
      { tag:'新手', tagClass:'tag-pink', cat:'newbie', title:'德州扑克起手牌强度排行榜: 从 AA 到 72 的完整指南', desc:'掌握起手牌强度是德州扑克的第一课。本文系统整理 169 种起手牌的胜率与位置打法...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
      { tag:'玩法', tagClass:'tag-pink', cat:'rule', title:'SQUID 鱿鱼标记淘汰玩法详解: 从规则到必胜思路', desc:'WPT HOME 招牌玩法 SQUID 全攻略 —— 从基础规则、Marker 机制到核心策略...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
      { tag:'新手', tagClass:'tag-pink', cat:'newbie', title:'德州扑克新手 7 天速成指南', desc:'每天 30 分钟, 7 天从规则小白到能在朋友局立足。包含每日学习计划、练习题和实战检查清单...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'规则', tagClass:'tag-blue', cat:'term', title:'扑克术语大全: 从 ABI 到 Straddle', desc:'整理 80+ 高频扑克术语, 中英对照、使用场景, 看牌局直播再也不掉线, 收藏级速查手册...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
      { tag:'进阶', tagClass:'tag-pink', cat:'advanced', title:'位置的力量: 为什么 BTN 是最赚钱的位置?', desc:'按钮位 (BTN) 数据为何最强? 用 50,000 手实战样本拆解位置盈利模型与可打范围...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'进阶', tagClass:'tag-pink', cat:'advanced', title:'底池赔率与隐含赔率详解: 数学决策入门', desc:'用最直白的方式讲清底池赔率公式, 配合 5 个常见决策场景, 让数学服务直觉...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'高阶', tagClass:'tag-orange', cat:'pro', title:'GTO vs Exploitive: 高额桌玩家的真实选择', desc:'真正的高手不只用 GTO, 也不只剥削。看顶级玩家如何在两种风格间动态切换...', date:'May 5, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
      { tag:'规则', tagClass:'tag-blue', cat:'rule', title:'PLO 奥马哈完整规则: 与德州的 5 大不同', desc:'4 张底牌 vs 2 张, 必须用 2 张组牌, 底池限注 —— 奥马哈玩家必读基础课...', date:'May 4, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'新手', tagClass:'tag-pink', cat:'newbie', title:'第一次坐桌前必须知道的 10 件事', desc:'从筹码摆放到行动顺序, 从牌面术语到礼仪默契, 让你坐桌不慌不乱...', date:'May 3, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
    ],
    tournament: [
      { tag:'锦标赛', tagClass:'tag-pink', title:'MTT 多桌锦标赛进阶: ICM 模型实战应用', desc:'锦标赛后期 ICM 决策直接决定盈利, 看顶尖选手如何把数学公式转化为牌桌动作...', date:'May 7, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'锦标赛', tagClass:'tag-pink', title:'SNG 即开赛速通秘籍: 推/弃图表完整版', desc:'9 人 SNG 后期 ICM 推/弃图表, 适用于平台 90% 的常规即开赛...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'锦标赛', tagClass:'tag-pink', title:'WPT HOME 月度精英赛 5 月榜单', desc:'5 月共举办 124 场精英赛, 总奖池 $1.2M, 月榜冠军 ID: PokerKing23, 累计奖金 $86,400...', date:'May 5, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
      { tag:'锦标赛', tagClass:'tag-pink', title:'冷门买入大锦赛战报: $5.5 进 $25K', desc:'$5.5 报名费的微型锦标赛诞生 $25,000 大奖, 冠军讲述全程关键手...', date:'May 3, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
      { tag:'锦标赛', tagClass:'tag-pink', title:'重买 vs 不重买: 锦标赛 EV 计算大揭秘', desc:'什么时候应该重买? 一篇文章讲清重买阶段的 EV 模型与决策树...', date:'May 1, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'锦标赛', tagClass:'tag-pink', title:'WPT Online Series 2026 时间表公布', desc:'年度线上系列赛回归, 6 月 1 日 - 6 月 30 日, 80+ 场赛事, 保底奖池 $5M...', date:'Apr 30, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
    ],
    cash: [
      { tag:'普通桌', tagClass:'tag-mint', title:'NLHE 现金桌入门: 起手牌选择 + 位置打法', desc:'现金桌与锦标赛的根本不同, 从筹码深度到打法风格的全方位对比...', date:'May 8, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'普通桌', tagClass:'tag-mint', title:'高额桌职业玩家的 24 小时', desc:'从早 11 点到深夜, 一位高额桌职业玩家的真实日常, 包括赢牌的快感与亏牌的处理...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'普通桌', tagClass:'tag-mint', title:'微注 NL10 升级 NL50 的科学路径', desc:'用数据告诉你: 多少手实战、什么样的胜率、何时该升注 —— 完整资金管理流程...', date:'May 4, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
      { tag:'普通桌', tagClass:'tag-mint', title:'六人桌 vs 九人桌: 玩法差异完全解析', desc:'人数不同打法天差地别, 从手牌频率、位置价值到 3-bet 频率的全维度对比...', date:'May 2, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
      { tag:'普通桌', tagClass:'tag-mint', title:'PLO 现金桌: 4 张底牌的赢钱思路', desc:'PLO 高额桌为何越来越火? 多张底牌带来的不仅是胜率, 还有截然不同的策略空间...', date:'May 1, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'普通桌', tagClass:'tag-mint', title:'现金桌情绪管理: 不再 Tilt 的 5 个习惯', desc:'职业玩家亲述 Tilt 控制方法, 从生理唤醒到决策框架, 把情绪转化为优势...', date:'Apr 28, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
    ],
    dynamic: [
      { tag:'动态', tagClass:'tag-orange', title:'WPT HOME v3.5 版本上线: 全新桌面 UI', desc:'更清爽的桌面布局、动态道具系统、个性化牌桌主题 —— 这是我们最大的视觉升级...', date:'May 8, 2026', img:'https://images.unsplash.com/photo-1541278107931-e006523892df?w=800&q=80' },
      { tag:'动态', tagClass:'tag-orange', title:'好友约局功能升级: 一键群组开桌', desc:'微信群、Discord、QQ 群的好友, 现在能在 30 秒内同步进入同一张牌桌...', date:'May 6, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'动态', tagClass:'tag-orange', title:'数据中心 2.0: HUD 数据现已可视化', desc:'对手风格画像、自己的统计趋势、关键决策回顾 —— 数据不再是表格, 而是图表...', date:'May 4, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'动态', tagClass:'tag-orange', title:'手牌回放系统重大更新', desc:'支持任意手牌的多角度回放, 加上 AI 决策点评, 复盘效率提升 3 倍...', date:'May 2, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
      { tag:'动态', tagClass:'tag-orange', title:'声音特效全新升级: 沉浸式赌场氛围', desc:'与好莱坞声音工作室合作录制全新音效, 筹码碰撞、发牌声、欢呼声更逼真...', date:'Apr 30, 2026', img:'https://images.unsplash.com/photo-1518895312237-a9e23508077d?w=800&q=80' },
      { tag:'动态', tagClass:'tag-orange', title:'iPad 版本横屏适配优化', desc:'iPad 用户专享: 横屏模式下牌桌更大、动作按钮更舒适, 支持多牌桌并排...', date:'Apr 28, 2026', img:'https://images.unsplash.com/photo-1517232115160-ff93364542dd?w=800&q=80' },
    ],
  };

  const helpContent = {
    faq: { faqs:[
      { q:'WPT HOME 是什么? 与其他扑克 App 有什么不同?', a:'WPT HOME 是 WPT 官方授权的免费德州扑克社交应用, 主打"熟人约局"和"零门槛入场", 不抽水、不收费, 所有筹码均为虚拟币不具现实价值。' },
      { q:'WPT HOME 在哪些设备可以使用?', a:'WPT HOME 支持 iOS (App Store)、Android (Google Play) 以及网页版, 你的账号在所有设备间同步。' },
      { q:'游戏中可以使用真钱吗?', a:'不可以。WPT HOME 全程使用虚拟游戏币, 不提供任何形式的真钱兑换或经济奖励, 仅作为社交娱乐用途。' },
      { q:'未成年人可以使用 WPT HOME 吗?', a:'WPT HOME 仅向 18 岁 (或当地法定成年年龄) 以上玩家开放, 严格遵守儿童安全政策。' },
    ]},
    bind: { faqs:[
      { q:'为何需要绑定账号?', a:'绑定账号可保护游戏数据不丢失, 跨设备登录时可同步进度、好友列表与历史战绩。' },
      { q:'账户绑定后如何登录游戏?', a:'绑定账号后, 您可以选择已绑定的邮箱号, 输入密码后点击登录。' },
      { q:'忘记绑定密码怎么办?', a:'登录页点击"忘记密码", 系统会向你的绑定邮箱发送验证码, 输入后即可重设密码。' },
      { q:'可以更换绑定的邮箱号吗?', a:'可以。在"账号设置 → 安全中心"中点击"更换邮箱", 验证旧邮箱后输入新邮箱即可完成更换。' },
    ]},
    reward: { faqs:[
      { q:'邀请好友能获得什么奖励?', a:'每邀请 1 位新好友完成首次牌局, 邀请人和被邀请人各获得 50,000 虚拟筹码 + 5 颗钻石。' },
      { q:'邀请奖励多久到账?', a:'被邀请好友完成首次有效牌局后 24 小时内自动到账, 推送提醒会发送到游戏内消息中心。' },
      { q:'邀请人数有上限吗?', a:'每月最多 30 个邀请额度, 月度结算后重置, 邀请数据可在"我的 → 邀请奖励"中查看。' },
    ]},
    diamond: { faqs:[
      { q:'钻石可以用来做什么?', a:'钻石可购买专属牌桌主题、创建私人俱乐部、解锁高级头像框, 还可参加钻石专属锦标赛。' },
      { q:'钻石是怎么获得的?', a:'通过完成每日任务、邀请好友、参加平台活动均可获得钻石, 钻石不能通过付费购买。' },
    ]},
    create: { faqs:[
      { q:'如何创建一张好友桌?', a:'首页点击"创建房间", 选择盲注/人数/玩法, 系统会生成专属房号和邀请链接, 分享给朋友即可。' },
      { q:'好友桌可以设置密码吗?', a:'可以, 在创建房间时勾选"设置密码", 仅持有密码的玩家可入座。' },
      { q:'好友桌房主有哪些权限?', a:'房主可踢人、暂停、解散房间, 也可在 Pro Mode 中调整盲注、加注上限等高级参数。' },
    ]},
    fair: { faqs:[
      { q:'WPT HOME 如何保证公平?', a:'平台通过 Gaming Associates 认证, 使用第三方 RNG 验证, 每一手发牌可调取审计日志。' },
      { q:'怀疑遇到伙牌该怎么办?', a:'在牌桌右上角点击"举报", 选择"涉嫌伙牌", 提交后 48 小时内会有专人复核, 一经查实将永久封禁。' },
      { q:'账号被盗如何处理?', a:'立即点击登录页"账号申诉", 提交身份验证材料, 客服将在 24 小时内协助找回。' },
    ]},
    feedback: { faqs:[
      { q:'如何反馈产品建议?', a:'游戏内"我的 → 意见反馈"提交建议, 或发送邮件至 support@wpthome.com, 每周三由产品团队集中处理。' },
      { q:'Bug 反馈多久会修复?', a:'紧急 Bug (影响游戏核心功能) 24 小时内热修, 普通 Bug 在下个版本迭代中修复。' },
    ]},
    report: { faqs:[
      { q:'看到伙牌如何举报?', a:'牌桌右上角"举报" → "涉嫌伙牌" → 提交对局编号, 反作弊系统会自动调取多手数据复核。' },
      { q:'举报会暴露我的身份吗?', a:'不会。所有举报匿名处理, 被举报方不会知道举报来源。' },
    ]},
    club: { faqs:[
      { q:'如何加入俱乐部?', a:'首页"俱乐部"标签搜索俱乐部 ID 或名称, 点击申请加入, 等待 Host 审核。' },
      { q:'如何创建自己的俱乐部?', a:'达到 LV.10 后可使用 1,000 钻石创建俱乐部, 自定义 Logo、规则与会员权益。' },
    ]},
  };

  // ===== render helpers =====
  function cardHTML(item) {
    return `
      <article class="lcard cursor-pointer">
        <div class="lcard-img" style="background-image: url('${item.img}');">
          <span class="absolute top-3 right-3 z-10 tag ${item.tagClass}">${item.tag}</span>
        </div>
        <div class="lcard-body">
          <h3 class="lcard-title line-clamp-2">${item.title}</h3>
          <p class="lcard-desc line-clamp-3">${item.desc}</p>
          <div class="lcard-foot">
            <a class="lcard-link">查看细节</a>
            <span class="lcard-date">📅 ${item.date}</span>
          </div>
        </div>
      </article>
    `;
  }

  function renderNews() {
    Object.keys(newsContent).forEach(key => {
      const grid = document.querySelector(`[data-news-tab="${key}"] .grid`);
      if (grid) grid.innerHTML = newsContent[key].map(cardHTML).join('');
    });
  }

  let currentAcademyCat = 'all';
  function renderAcademy(cat) {
    currentAcademyCat = cat;
    const items = cat === 'all' ? newsContent.academy : newsContent.academy.filter(i => i.cat === cat);
    const grid = document.querySelector(`[data-news-tab="academy"] .grid`);
    if (grid) {
      grid.innerHTML = items.length ? items.map(cardHTML).join('') :
        `<div class="col-span-3 text-center py-20" style="color: rgba(255,255,255,0.4);">该分类下暂无内容</div>`;
    }
    document.querySelectorAll('.academy-dd-item').forEach(el => el.classList.toggle('active', el.dataset.cat === cat));
  }

  function renderHelp(cat) {
    const data = helpContent[cat] || helpContent.bind;
    const html = data.faqs.map((f, i) => `
      <div class="faq-item ${i === 1 ? 'open' : ''}">
        <button class="faq-q" onclick="toggleFaq(this)">
          <span>${f.q}</span>
          <span class="faq-icon">+</span>
        </button>
        <div class="faq-body">${f.a}</div>
      </div>
    `).join('');
    document.getElementById('helpFaqPanel').innerHTML = html;
    document.querySelectorAll('.help-side-link').forEach(el => el.classList.toggle('active', el.dataset.help === cat));
  }

  // ===== nav =====
  function goPage(name) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById('page-' + name).classList.add('active');
    document.querySelectorAll('.nav-link').forEach(el => el.classList.toggle('active', el.dataset.page === name));
    window.scrollTo({ top: 0, behavior: 'smooth' });
    const academyDD = document.getElementById('academyDD');
    if (academyDD) academyDD.classList.remove('show');
    const helpWidget = document.getElementById('helpWidget');
    if (helpWidget) {
      if (name === 'help') helpWidget.classList.add('show');
      else helpWidget.classList.remove('show');
    }
  }

  function goNews(tab, ev) {
    if (ev) ev.stopPropagation();
    document.querySelectorAll('.news-tab').forEach(el => {
      el.classList.toggle('hidden', el.dataset.newsTab !== tab);
    });
    document.querySelectorAll('.subnav-link').forEach(el => el.classList.toggle('active', el.dataset.news === tab));
    const dd = document.getElementById('academyDD');
    if (tab === 'academy') {
      dd.classList.add('show');
      renderAcademy(currentAcademyCat);
    } else {
      dd.classList.remove('show');
    }
  }

  function goAcademyCat(cat, ev) {
    if (ev) ev.stopPropagation();
    document.querySelectorAll('.news-tab').forEach(el => el.classList.toggle('hidden', el.dataset.newsTab !== 'academy'));
    document.querySelectorAll('.subnav-link').forEach(el => el.classList.toggle('active', el.dataset.news === 'academy'));
    renderAcademy(cat);
  }

  function goHelpCat(cat) {
    renderHelp(cat);
  }

  function toggleFaq(btn) {
    btn.parentElement.classList.toggle('open');
  }

  function toggleHelpWidget() {
    document.getElementById('helpWidget').classList.toggle('show');
  }

  // close academy dropdown when clicking outside its tab
  document.addEventListener('click', (e) => {
    const dd = document.getElementById('academyDD');
    if (dd && !dd.contains(e.target) && !e.target.closest('[data-news="academy"]')) {
      dd.classList.remove('show');
    }
    const ndd = document.getElementById('newsNavDD');
    const wrap = document.getElementById('newsNavWrap');
    if (ndd && wrap && !wrap.contains(e.target)) {
      ndd.classList.remove('show');
    }
  });

  // init
  renderNews();
  renderHelp('bind');
</script>
</body>
</html>
