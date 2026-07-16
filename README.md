<!DOCTYPE html>
<html lang="zh-Hant-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>美馬溝東京六日遊</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; background-color: #f8fafc; }
        .tab-active { border-bottom: 3px solid #1e40af; color: #1e40af; }
        .card { background: white; border-radius: 20px; box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1); }
    </style>
</head>
<body class="pb-24">
    <div id="app" class="max-w-md mx-auto">
        <header class="bg-white p-6 shadow-sm sticky top-0 z-20">
            <h1 class="text-2xl font-bold text-blue-900">美馬溝東京六日遊 🗼</h1>
            <p class="text-sm text-gray-500 font-bold mt-1">東京爆買啦 ❤️ (2026)</p>
            <p class="text-xs text-blue-600 font-bold mt-1">2026/8/7 - 8/12</p>
        </header>

        <div class="flex justify-around p-4 mt-2 bg-white mx-4 rounded-xl shadow-sm border border-gray-100">
            <button onclick="renderContent('itinerary')" id="tab-itinerary" class="tab-active font-bold text-blue-800">行程</button>
            <button onclick="renderContent('guide')" id="tab-guide" class="text-gray-400 font-bold">指南</button>
            <button onclick="renderContent('weather')" id="tab-weather" class="text-gray-400 font-bold">天氣</button>
            <button onclick="renderContent('memo')" id="tab-memo" class="text-gray-400 font-bold">備忘</button>
        </div>

        <main id="content" class="p-4"></main>

        <nav class="fixed bottom-0 w-full max-w-md bg-white border-t p-4 flex justify-around z-20 shadow-lg">
            <button id="nav-itinerary" class="text-blue-800 font-bold flex flex-col items-center" onclick="renderContent('itinerary')"><span class="text-xl">📅</span><span class="text-xs mt-1">行程</span></button>
            <button id="nav-guide" class="text-gray-400 font-bold flex flex-col items-center" onclick="renderContent('guide')"><span class="text-xl">🧮</span><span class="text-xs mt-1">指南</span></button>
            <button id="nav-weather" class="text-gray-400 font-bold flex flex-col items-center" onclick="renderContent('weather')"><span class="text-xl">🌤️</span><span class="text-xs mt-1">天氣</span></button>
            <button id="nav-memo" class="text-gray-400 font-bold flex flex-col items-center" onclick="renderContent('memo')"><span class="text-xl">📝</span><span class="text-xs mt-1">備忘</span></button>
        </nav>
    </div>

    <script>
        const tripData = {
            "Day 1": { title: "8/7 (五) 抵達 · 東京", items: [
                { time: "10:00", name: "桃機集合", desc: "Visit Japan Web QR先填好。" },
                { time: "12:15", name: "CI104 飛往成田", desc: "機上享用餐點。" },
                { time: "18:40", name: "東京車站", desc: "晚餐：極味屋，逛一番街(Brulee Merize必買)。" }
            ]},
            "Day 2": { title: "8/8 (六) 明治神宮 · 澀谷", items: [
                { time: "09:30", name: "明治神宮", desc: "早晨漫步參拜。" },
                { time: "10:45", name: "竹下通", desc: "@cosme, Converse, 3COINS, VIVAIA。" },
                { time: "13:30", name: "表參道/貓街", desc: "逛SOU·SOU, TEAPOND, HUMAN MADE。" },
                { time: "17:30", name: "SHIBUYA SKY", desc: "欣賞黃昏日落美景。" },
                { time: "19:15", name: "晚餐：燒肉Aburu", desc: "務必預訂。" }
            ]},
            "Day 3": { title: "8/9 (日) 鎌倉 · 江之島", items: [
                { time: "07:50", name: "前往鎌倉", desc: "JR橫須賀線。" },
                { time: "09:40", name: "小町通", desc: "特色小吃巡禮。" },
                { time: "12:55", name: "平交道", desc: "灌籃高手平交道打卡。" },
                { time: "14:00", name: "江之島", desc: "Sea Candle燈塔、岩屋。" },
                { time: "19:00", name: "晚餐：GyuTongue", desc: "牛舌料理。" }
            ]},
            "Day 4": { title: "8/10 (一) 迪士尼海洋", items: [
                { time: "07:30", name: "前往舞濱", desc: "提早出發排隊。" },
                { time: "08:00", name: "DisneySea", desc: "全日暢玩，記得確認DPA。" }
            ]},
            "Day 5": { title: "8/11 (二) 淺草 · 銀座", items: [
                { time: "10:30", name: "淺草浴衣", desc: "體驗浴衣漫步淺草寺。" },
                { time: "15:00", name: "銀座", desc: "Loft, GINZA SIX購物。" },
                { time: "17:55", name: "晚餐：入鹿TOKYO", desc: "拉麵(不收現金)。" },
                { time: "19:50", name: "東京鐵塔", desc: "芝公園夜景。" }
            ]},
            "Day 6": { title: "8/12 (三) 返家", items: [
                { time: "10:45", name: "Access特急", desc: "前往成田機場。" },
                { time: "13:00", name: "免稅店", desc: "最後補貨。" },
                { time: "14:30", name: "返台", desc: "CI101 起飛。" }
            ]}
        };

        const weatherData = {
            "Day 1": { city: "東京", weather: "晴時多雲 28-33°C", outfit: "輕便夏季衣物，需防曬。" },
            "Day 2": { city: "澀谷/原宿", weather: "多雲 27-32°C", outfit: "舒適好走的鞋子。" },
            "Day 3": { city: "鎌倉/江之島", weather: "晴朗 26-31°C", outfit: "海邊風大，帶薄外套。" },
            "Day 4": { city: "舞濱(迪士尼)", weather: "晴朗 27-32°C", outfit: "運動服，防曬補水。" },
            "Day 5": { city: "淺草/銀座", weather: "多雲 28-33°C", outfit: "浴衣或優雅休閒裝。" },
            "Day 6": { city: "成田機場", weather: "陣雨機率 26-30°C", outfit: "舒適便裝。" }
        };

        function renderContent(tab = 'itinerary', day = "Day 1") {
            const content = document.getElementById('content');
            document.querySelectorAll('button[id^="tab-"], button[id^="nav-"]').forEach(btn => {
                btn.classList.remove('tab-active', 'text-blue-800');
                btn.classList.add('text-gray-400');
            });
            const tabBtn = document.getElementById('tab-' + tab);
            if(tabBtn) tabBtn.classList.add('tab-active', 'text-blue-800');
            const navBtn = document.getElementById('nav-' + tab);
            if(navBtn) { navBtn.classList.remove('text-gray-400'); navBtn.classList.add('text-blue-800'); }

            if(tab === 'itinerary') {
                content.innerHTML = `
                    <div class="flex gap-2 overflow-x-auto pb-4 mb-2">
                        ${Object.keys(tripData).map(d => `<button onclick="renderContent('itinerary', '${d}')" class="px-4 py-2 rounded-full ${day === d ? 'bg-blue-800 text-white' : 'bg-white border'} font-bold shadow-sm">${d}</button>`).join('')}
                    </div>
                    <div class="card p-6">
                        <h2 class="text-xl font-bold mb-4">${tripData[day].title}</h2>
                        ${tripData[day].items.map(i => `<div class="mb-4"><strong>${i.time} ${i.name}</strong><br><span class="text-sm text-gray-600">${i.desc}</span></div>`).join('')}
                    </div>`;
            } else if(tab === 'guide') {
                content.innerHTML = `
                    <div class="card p-6">
                        <h2 class="text-xl font-bold mb-4">📍 旅遊地圖指南</h2>
                        <ul class="space-y-2 text-sm">
                            <li>🗼 <strong>東京：</strong> 主要住宿、購物中心。</li>
                            <li>⛩️ <strong>淺草/原宿：</strong> 文青、神社參拜。</li>
                            <li>🌊 <strong>鎌倉：</strong> 悠閒海景與歷史。</li>
                            <li>🏰 <strong>舞濱：</strong> 迪士尼樂園。</li>
                        </ul>
                        <a href="https://www.tokyometro.jp/tcn/subwaymap/" target="_blank" class="block mt-6 bg-blue-600 text-white text-center p-3 rounded-lg font-bold">開啟東京地鐵圖</a>
                    </div>`;
            } else if(tab === 'weather') {
                content.innerHTML = `<div class="card p-6"><h2 class="text-xl font-bold mb-4">🌤️ 天氣與穿搭</h2>
                    ${Object.entries(weatherData).map(([d, w]) => `
                        <div class="mb-4 border-b pb-2">
                            <p class="font-bold text-blue-800">${d} (${w.city})</p>
                            <p class="text-sm text-gray-600">天氣：${w.weather}</p>
                            <p class="text-sm text-gray-600">建議：${w.outfit}</p>
                        </div>`).join('')}
                </div>`;
            } else if(tab === 'memo') {
                renderMemo();
            }
        }

        function renderMemo() {
            const content = document.getElementById('content');
            const savedLinks = JSON.parse(localStorage.getItem('myCustomLinks')) || [
                {title: "Visit Japan Web", url: "https://vjw-lp.digital.go.jp/"},
                {title: "唐吉訶德優惠券", url: "https://www.donki.co.jp/"}
            ];
            content.innerHTML = `
                <div class="card p-6">
                    <h2 class="text-xl font-bold mb-4">📝 備忘錄</h2>
                    <input type="text" id="title" placeholder="標題" class="w-full p-2 mb-2 border rounded">
                    <input type="text" id="url" placeholder="網址" class="w-full p-2 mb-2 border rounded">
                    <button onclick="addLink()" class="w-full bg-blue-800 text-white font-bold py-2 rounded mb-6">新增連結</button>
                    <div id="link-list" class="space-y-2 mb-6">
                        ${savedLinks.map((l, i) => `<div class="flex justify-between p-2 border rounded"><a href="${l.url}" class="text-blue-600">${l.title}</a><button onclick="removeLink(${i})">✕</button></div>`).join('')}
                    </div>
                    <textarea id="memo-text" class="w-full h-32 p-3 border rounded-lg" placeholder="自動儲存筆記..."></textarea>
                </div>`;
            const area = document.getElementById('memo-text');
            area.value = localStorage.getItem('tokyoTripMemo') || '';
            area.addEventListener('input', (e) => localStorage.setItem('tokyoTripMemo', e.target.value));
        }

        function addLink() {
            const title = document.getElementById('title').value;
            const url = document.getElementById('url').value;
            if(!title || !url) return;
            const links = JSON.parse(localStorage.getItem('myCustomLinks')) || [];
            links.push({title, url: url.startsWith('http') ? url : 'https://' + url});
            localStorage.setItem('myCustomLinks', JSON.stringify(links));
            renderMemo();
        }

        function removeLink(i) {
            const links = JSON.parse(localStorage.getItem('myCustomLinks'));
            links.splice(i, 1);
            localStorage.setItem('myCustomLinks', JSON.stringify(links));
            renderMemo();
        }
        renderContent();
    </script>
</body>
</html>
