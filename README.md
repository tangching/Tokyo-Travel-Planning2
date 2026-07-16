<!DOCTYPE html>
<html lang="zh-Hant-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>美馬溝東京六日遊</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Zen+Maru+Gothic:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Zen Maru Gothic', sans-serif; background-color: #fffaf0; }
        .tab-active { border-bottom: 4px solid #ffcad4; color: #db2777; }
        .card { background: white; border-radius: 32px; box-shadow: 0 8px 0 #ffe4e6; border: 3px solid #ffcad4; }
        .chiikawa-btn { transition: transform 0.2s; }
        .chiikawa-btn:active { transform: scale(0.95); }
    </style>
</head>
<body class="pb-24">
    <svg class="hidden">
        <symbol id="chiikawa" viewBox="0 0 100 100">
            <circle cx="50" cy="50" r="40" fill="#fff" stroke="#ffcad4" stroke-width="4"/>
            <circle cx="35" cy="45" r="5" fill="#333"/><circle cx="65" cy="45" r="5" fill="#333"/>
            <path d="M40 65 Q50 70 60 65" stroke="#333" fill="none" stroke-width="3"/>
            <circle cx="25" cy="50" r="6" fill="#fecaca" opacity="0.6"/><circle cx="75" cy="50" r="6" fill="#fecaca" opacity="0.6"/>
        </symbol>
        <symbol id="hachiware" viewBox="0 0 100 100">
            <circle cx="50" cy="50" r="40" fill="#bae6fd" stroke="#7dd3fc" stroke-width="4"/>
            <circle cx="35" cy="45" r="5" fill="#333"/><circle cx="65" cy="45" r="5" fill="#333"/>
            <path d="M40 65 Q50 70 60 65" stroke="#333" fill="none" stroke-width="3"/>
            <path d="M15 30 Q50 10 85 30" stroke="#7dd3fc" stroke-width="8" fill="none"/>
        </symbol>
        <symbol id="usagi" viewBox="0 0 100 100">
            <path d="M50 10 C30 10 30 50 40 50 L40 90 L60 90 L60 50 C70 50 70 10 50 10" fill="#fef08a" stroke="#facc15" stroke-width="4"/>
            <circle cx="35" cy="45" r="5" fill="#333"/><circle cx="65" cy="45" r="5" fill="#333"/>
            <path d="M40 65 Q50 70 60 65" stroke="#333" fill="none" stroke-width="3"/>
        </symbol>
    </svg>

    <div id="app" class="max-w-md mx-auto">
        <header class="bg-white p-6 rounded-b-[40px] shadow-lg sticky top-0 z-20 flex justify-between items-center border-b-4 border-pink-100">
            <div>
                <h1 class="text-2xl font-black text-pink-500">美馬溝東京遊 🗼</h1>
                <p class="text-base font-bold text-blue-400">一起去爆買啦 ❤️</p>
            </div>
            <svg class="w-16 h-16"><use href="#chiikawa"/></svg>
        </header>

        <div class="flex justify-around p-4 mt-2">
            <button onclick="renderContent('itinerary')" id="tab-itinerary" class="tab-active font-bold text-pink-600 chiikawa-btn">行程</button>
            <button onclick="renderContent('guide')" id="tab-guide" class="text-gray-400 font-bold chiikawa-btn">指南</button>
            <button onclick="renderContent('weather')" id="tab-weather" class="text-gray-400 font-bold chiikawa-btn">天氣</button>
            <button onclick="renderContent('memo')" id="tab-memo" class="text-gray-400 font-bold chiikawa-btn">備忘</button>
        </div>

        <main id="content" class="p-4"></main>

        <nav class="fixed bottom-0 w-full max-w-md bg-white border-t-4 border-pink-100 p-4 flex justify-around z-20 rounded-t-[32px] shadow-2xl">
            <button id="nav-itinerary" class="text-pink-600 font-bold chiikawa-btn" onclick="renderContent('itinerary')"><svg class="w-8 h-8"><use href="#hachiware"/></svg></button>
            <button id="nav-guide" class="text-gray-400 font-bold chiikawa-btn" onclick="renderContent('guide')"><svg class="w-8 h-8"><use href="#chiikawa"/></svg></button>
            <button id="nav-weather" class="text-gray-400 font-bold chiikawa-btn" onclick="renderContent('weather')"><svg class="w-8 h-8"><use href="#usagi"/></svg></button>
        </nav>
    </div>

    <script>
        const tripData = {
            "Day 1": { title: "8/7 (五) 抵達 · 東京車站", items: [
                { time: "10:00", name: "桃機集合", desc: "Visit Japan Web QR先填好。" },
                { time: "18:40", name: "極味屋", desc: "東京車站GRANSTA八重北店。" },
                { time: "20:00", name: "一番街", desc: "Brulee Merize 必買！" }
            ]},
            "Day 2": { title: "8/8 (六) 明治神宮 · 澀谷", items: [
                { time: "09:30", name: "明治神宮", desc: "漫步參拜。" },
                { time: "17:30", name: "SHIBUYA SKY", desc: "日落時刻最美。" },
                { time: "19:15", name: "燒肉Aburu", desc: "大塚站必吃燒肉。" }
            ]},
            "Day 3": { title: "8/9 (日) 鎌倉 · 江之島", items: [
                { time: "09:40", name: "小町通", desc: "鎌倉美食巡禮。" },
                { time: "12:55", name: "平交道", desc: "灌籃高手場景。" },
                { time: "14:00", name: "江之島", desc: "Sea Candle燈塔、岩屋。" }
            ]},
            "Day 4": { title: "8/10 (一) 迪士尼海洋", items: [
                { time: "08:00", name: "DisneySea", desc: "全日暢玩，記得確認DPA。" },
                { time: "21:10", name: "回飯店", desc: "好好休息。" }
            ]},
            "Day 5": { title: "8/11 (二) 淺草 · 銀座", items: [
                { time: "10:30", name: "淺草浴衣", desc: "穿浴衣逛雷門。" },
                { time: "17:55", name: "入鹿TOKYO", desc: "六本木拉麵(不收現金)。" },
                { time: "19:50", name: "東京鐵塔", desc: "芝公園夜景。" }
            ]},
            "Day 6": { title: "8/12 (三) 返家", items: [
                { time: "10:45", name: "Access特急", desc: "前往成田機場。" },
                { time: "14:30", name: "返台", desc: "CI101 飛機。" }
            ]}
        };

        function renderContent(tab = 'itinerary', day = "Day 1") {
            const content = document.getElementById('content');
            
            // Tab states
            document.querySelectorAll('button[id^="tab-"]').forEach(btn => {
                btn.classList.remove('tab-active', 'text-pink-600');
                btn.classList.add('text-gray-400');
            });
            const tabBtn = document.getElementById('tab-' + tab);
            if(tabBtn) { tabBtn.classList.add('tab-active', 'text-pink-600'); tabBtn.classList.remove('text-gray-400'); }

            if(tab === 'itinerary') {
                content.innerHTML = `
                    <div class="flex gap-2 overflow-x-auto pb-4 mb-2">
                        ${Object.keys(tripData).map(d => `<button onclick="renderContent('itinerary', '${d}')" class="px-4 py-2 rounded-full ${day === d ? 'bg-pink-400 text-white' : 'bg-white border-2 border-pink-200'} font-bold shadow-sm">${d}</button>`).join('')}
                    </div>
                    <div class="card p-6 border-pink-200">
                        <h2 class="text-xl font-black text-pink-500 mb-4">${tripData[day].title}</h2>
                        ${tripData[day].items.map(item => `
                            <div class="flex items-start gap-4 mb-6">
                                <div class="bg-pink-100 p-2 rounded-xl text-pink-600 font-bold">${item.time}</div>
                                <div>
                                    <h3 class="font-bold text-lg">${item.name}</h3>
                                    <p class="text-sm text-gray-500">${item.desc}</p>
                                </div>
                            </div>
                        `).join('')}
                    </div>`;
            } else if(tab === 'weather') {
                content.innerHTML = `<div class="card p-8 text-center"><h2 class="text-xl font-bold">🌤️ 晴朗炎熱</h2><p>適合吃冰淇淋的天氣！</p><svg class="w-32 h-32 mx-auto mt-4"><use href="#usagi"/></svg></div>`;
            } else if(tab === 'guide') {
                content.innerHTML = `<div class="card p-8"><h2 class="text-xl font-bold">📖 旅遊小冊</h2><p>這裡是你的旅行指南。</p></div>`;
            } else if(tab === 'memo') {
                content.innerHTML = `<div class="card p-8"><h2 class="text-xl font-bold">📝 筆記</h2><textarea class="w-full h-32 border-2 rounded-xl p-2 border-pink-200"></textarea></div>`;
            }
        }
        renderContent();
    </script>
</body>
</html>
