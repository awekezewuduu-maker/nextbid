<!DOCTYPE html>
<html lang="am">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>NextBid - Social Media Auctions</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Telegram WebApp SDK -->
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    body {
      background-color: var(--tg-theme-bg-color, #0b1329);
      color: var(--tg-theme-text-color, #f8fafc);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    }
  </style>
</head>
<body class="p-4 flex flex-col items-center min-h-screen">

  <!-- Header Section -->
  <div class="w-full max-w-md flex justify-between items-center mb-5">
    <div>
      <p class="text-xs text-gray-400">እንኳን ደህና መጡ 👋</p>
      <h2 id="username" class="font-bold text-lg text-emerald-400">ተጫራች</h2>
    </div>
    <div class="bg-slate-800/90 border border-slate-700 px-3.5 py-1.5 rounded-full text-xs font-semibold flex items-center gap-1.5 shadow-sm">
      <span>🪙 ዋሌት:</span>
      <span id="userBalance" class="text-emerald-400 font-bold">100 ETB</span>
    </div>
  </div>

  <!-- Main Card: TikTok Auction -->
  <div class="w-full max-w-md bg-slate-900/95 border border-slate-800 rounded-3xl p-5 shadow-2xl relative overflow-hidden backdrop-blur-md">
    
    <!-- Top Tags & Timer -->
    <div class="flex justify-between items-center mb-4">
      <span class="inline-flex items-center gap-1.5 bg-black/80 border border-slate-700 text-white text-xs font-semibold px-3 py-1 rounded-full">
        🎵 TikTok Account
      </span>
      <span class="inline-flex items-center gap-1 text-xs font-mono font-bold text-amber-400 bg-amber-400/10 border border-amber-400/20 px-2.5 py-1 rounded-lg">
        ⏳ <span id="timer">02:15:30</span>
      </span>
    </div>

    <!-- Account Headline -->
    <div class="space-y-1 mb-4">
      <div class="flex items-center justify-between">
        <h3 class="text-xl font-black text-white">@ethio_tech_daily</h3>
        <span class="text-[11px] bg-emerald-500/20 text-emerald-400 border border-emerald-500/30 font-semibold px-2.5 py-0.5 rounded-full">
          USA Monetized 🇺🇸
        </span>
      </div>
      <p class="text-xs text-gray-400">ኦርጋኒክ ተከታይ ያለው፣ ቪዲዮዎች ያሉት ንቁ ገጽ</p>
    </div>

    <!-- Account Metrics -->
    <div class="grid grid-cols-3 gap-2 py-3 px-3 bg-slate-800/60 border border-slate-800 rounded-2xl text-center mb-4">
      <div>
        <p class="text-[11px] text-gray-400">ተከታዮች</p>
        <p class="font-extrabold text-base text-white">12.5K</p>
      </div>
      <div>
        <p class="text-[11px] text-gray-400">ጠቅላላ ላይክ</p>
        <p class="font-extrabold text-base text-white">180K</p>
      </div>
      <div>
        <p class="text-[11px] text-gray-400">Live ፈቃድ</p>
        <p class="font-extrabold text-base text-emerald-400">አለው ✅</p>
      </div>
    </div>

    <!-- Bidding Form Box -->
    <div class="bg-slate-950/70 border border-slate-800/90 rounded-2xl p-4 mb-4">
      <div class="flex justify-between text-xs text-gray-300 mb-2 font-medium">
        <span>የመግቢያ ክፍያ (Bid Fee):</span>
        <span class="font-bold text-emerald-400 text-sm">25 ETB</span>
      </div>
      
      <div class="relative mt-2">
        <input 
          type="number" 
          id="bidValueInput" 
          step="0.01" 
          min="0.01" 
          placeholder="ያልተደገመ ዝቅተኛ ዋጋ (ምሳሌ፦ 0.45)" 
          class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-3 text-sm text-white focus:outline-none focus:border-emerald-500 transition shadow-inner"
        >
        <span class="absolute right-3.5 top-3 text-xs font-bold text-gray-500">ETB</span>
      </div>
      <p class="text-[11px] text-gray-400 mt-2">
        💡 ማንም ያልደገመው እና ከሁሉም ዝቅተኛ የሆነው ዋጋ አሸናፊ ይሆናል።
      </p>
    </div>

    <!-- Submit Button -->
    <button 
      id="bidSubmitBtn"
      onclick="handleBidSubmission()" 
      class="w-full bg-gradient-to-r from-emerald-500 to-teal-500 hover:from-emerald-600 hover:to-teal-600 active:scale-[0.98] text-white font-black py-3 rounded-xl shadow-lg shadow-emerald-500/25 transition flex items-center justify-center gap-2 text-sm"
    >
      <span>በ 25 ብር ተጫረት</span>
      <span>🚀</span>
    </button>
  </div>

  <!-- Rules Info -->
  <div class="w-full max-w-md bg-slate-900/60 border border-slate-800/80 rounded-2xl p-4 text-xs text-gray-400 space-y-1.5 mt-4">
    <p class="font-bold text-gray-300">📌 የጨረታው ህግ በአጭሩ፦</p>
    <p>1. ሁለት ሰዎች አንድ አይነት ዋጋ ካስገቡ ያ ዋጋ ውድቅ (Duplicate) ይሆናል።</p>
    <p>2. ሰዓቱ ሲያልቅ ብቸኛና አነስተኛውን ዋጋ የሰጠው ሰው አካውንቱን ይረከባል።</p>
  </div>

  <script>
    const tg = window.Telegram ? window.Telegram.WebApp : null;

    if (tg) {
      tg.ready();
      tg.expand();

      if (tg.initDataUnsafe && tg.initDataUnsafe.user) {
        document.getElementById("username").innerText = tg.initDataUnsafe.user.first_name || "ተጫራች";
      }
    }

    let userBalance = 100.0;
    const bidFee = 25.0;

    function handleBidSubmission() {
      const input = document.getElementById("bidValueInput");
      const val = parseFloat(input.value);

      if (!val || val <= 0) {
        if (tg) tg.showAlert("እባክዎ ትክክለኛ የሳንቲም ዋጋ ያስገቡ (ምሳሌ፦ 0.45)");
        else alert("እባክዎ ትክክለኛ የሳንቲም ዋጋ ያስገቡ!");
        return;
      }

      if (userBalance < bidFee) {
        if (tg) tg.showAlert("ቀሪ ሒሳብዎ በቂ አይደለም! እባክዎ መጀመሪያ ዋሌትዎን ይሙሉ");
        else alert("ቀሪ ሒሳብዎ በቂ አይደለም!");
        return;
      }

      const confirmMessage = `${val.toFixed(2)} ብር የመጫረቻ ዋጋ አስገብተው በ ${bidFee} ብር ክፍያ መጫረት ይፈልጋሉ?`;

      if (tg && tg.showConfirm) {
        tg.showConfirm(confirmMessage, function(confirmed) {
          if (confirmed) {
            executeBid(val);
          }
        });
      } else {
        if (confirm(confirmMessage)) {
          executeBid(val);
        }
      }
    }

    function executeBid(val) {
      userBalance -= bidFee;
      document.getElementById("userBalance").innerText = userBalance.toFixed(2) + " ETB";
      document.getElementById("bidValueInput").value = "";

      const successNotice = `✅ ጨረታዎ ተመዝግቧል!\nያስገቡት ዋጋ፦ ${val.toFixed(2)} ETB\nየተቆረጠ፦ ${bidFee} ETB`;
      
      if (tg) {
        tg.showAlert(successNotice);
      } else {
        alert(successNotice);
      }
    }

    let totalSeconds = 2 * 3600 + 15 * 60 + 30;
    const timerElement = document.getElementById("timer");

    setInterval(() => {
      if (totalSeconds > 0) {
        totalSeconds--;
        const hours = String(Math.floor(totalSeconds / 3600)).padStart(2, '0');
        const minutes = String(Math.floor((totalSeconds % 3600) / 60)).padStart(2, '0');
        const seconds = String(totalSeconds % 60).padStart(2, '0');
        timerElement.innerText = `${hours}:${minutes}:${seconds}`;
      }
    }, 1000);
  </script>
</body>
</html>
