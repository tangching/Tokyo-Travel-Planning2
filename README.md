<!DOCTYPE html>
<html lang="zh-Hant-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>美馬溝東京六日遊</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Zen+Maru+Gothic:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Zen Maru Gothic', sans-serif; background-color: #f8fafc; }
        .tab-active { border-bottom: 3px solid #1e40af; color: #1e40af; }
        .card { background: white; border-radius: 24px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); border: 2px solid #fee2e2; }
    </style>
</head>
<body class="pb-24">
    <div id="app" class="max-w-md mx-auto">
        <header class="bg-white p-6 rounded-b-[32px] shadow-sm sticky top-0 z-20 flex justify-between items-center">
            <div>
                <h1 class="text-2xl font-bold text-blue-900">美馬溝東京六日遊 🗼</h1>
                <p class="text-base font-bold text-pink-500">東京爆買啦 ❤️</p>
                <p class="text-xs text-gray-400">2026年 8/7 (五) - 8/12 (三)</p>
            </div>
            <!-- 吉伊卡哇風格插圖 -->
            <svg viewBox="0 0 100 100" class="w-16 h-16">
                <circle cx="50" cy="50" r="40" fill="#fff" stroke="#fecaca" stroke-width="4"/>
                <circle cx="35" cy="40" r="5" fill="#333"/>
                <circle cx="65" cy="40" r="5" fill="#333"/>
                <path d="M40 60 Q50 65 60 60" stroke="#333" fill="none" stroke-width="3"/>
                <circle cx="20" cy="50" r="6" fill="#fecaca" opacity="0.6"/>
                <circle cx="80" cy="50" r="6" fill="#fecaca" opacity="0.6"/>
            </svg>
        </header>

        <!-- Top Tabs -->
        <div class="flex justify-around p-4 mt-2">
            <button onclick="renderContent('itinerary')" id="tab-itinerary" class="tab-active font-bold text-blue-800">行程</button>
            <button onclick="renderContent('guide')" id="tab-guide" class="text-gray-400 font-bold">指南</button>
            <button onclick="renderContent('weather')" id="tab-weather" class="text-gray-400 font-bold">天氣</button>
            <button onclick="renderContent('memo')" id="tab-memo" class="text-gray-400 font-bold">備忘</button>
        </div>

        <main id="content" class="p-4"></main>

        <!-- Bottom Nav -->
        <nav class="fixed bottom-0 w-full max-w-md bg-white border-t p-4 flex justify-around z-20">
            <button id="nav-itinerary" class="text-blue-800 font-bold" onclick="renderContent('itinerary')">📅 行程</button>
            <button id="nav-guide" class="text-gray-400 font-bold" onclick="renderContent('guide')">📖 指南</button>
            <button id="nav-weather" class="text-gray-400 font-bold" onclick="renderContent('weather')">🌤️ 天氣</button>
            <button id="nav-memo" class="text-gray-400 font-bold" onclick="renderContent('memo')">📝 備忘</button>
        </nav>
    </div>

    <script>
        const tripData = {
            "Day 1": { title: "8/7 (五) 抵達 · 東京車站之夜", items: [
                { time: "10:00", name: "桃機集合", desc: "Visit Japan Web QR先填好。" },
                { time: "12:15-16:35", name: "CI104 飛往成田", desc: "機上餐。" },
                { time: "16:35-17:30", name: "入境、領行李", desc: "Visit Japan Web QR先填好。" },
                { time: "17:35-18:20", name: "Skyliner", desc: "B1京成櫃檯取票，41分抵上野。" },
                { time: "18:20", name: "東京車站", desc: "找櫃子寄放行李。" },
                { time: "18:40-19:40", name: "晚餐：極味屋", desc: "GRANSTA八重北店，備案要先找好。" },
                { time: "20:00", name: "東京車站一番街", desc: "Brulee Merize (20:30關) 必買！確認保存期限。" },
                { time: "21:45-22:00", name: "回飯店", desc: "東西線15分，確認最晚22:00 check in。" }
            ]},
            "Day 2": { title: "8/8 (六) 明治神宮 · 澀谷", items: [
                { time: "08:40-09:25", name: "前往明治神宮", desc: "東西線+千代田線，35分。記得買早餐。" },
                { time: "09:30-10:40", name: "明治神宮", desc: "散步參拜。" },
                { time: "10:45-12:15", name: "竹下通", desc: "@cosme, Converse, 3COINS, VIVAIA。" },
                { time: "12:15-13:30", name: "午餐", desc: "原宿/表參道。" },
                { time: "13:30-16:00", name: "表參道/貓街", desc: "Onitsuka, SOU·SOU, TEAPOND, 東急PLAZA, HUMAN MADE。" },
                { time: "16:00-17:20", name: "澀谷", desc: "PARCO(寶可夢中心), 迪士尼商店, Harbs。" },
                { time: "17:30-19:00", name: "SHIBUYA SKY", desc: "預約黃昏(約18:40日落)，拍攝剪影。" },
                { time: "19:15-20:30", name: "晚餐：燒肉Aburu", desc: "大塚站，必吃！記得訂位。" },
                { time: "20:30-22:30", name: "逛逛/拍拍", desc: "澀谷十字路口、回飯店。" }
            ]},
            "Day 3": { title: "8/9 (日) 鎌倉 · 江之島", items: [
                { time: "07:50-09:40", name: "前往鎌倉", desc: "JR東京->鎌倉，超商早餐車上吃。" },
                { time: "09:40-11:15", name: "小町通", desc: "邊走邊吃，尋找美食。" },
                { time: "11:15-12:15", name: "鶴岡八幡宮", desc: "參拜，記得準備零錢。" },
                { time: "12:30-12:55", name: "江之電", desc: "前往鎌倉高校前。" },
                { time: "12:55-13:30", name: "灌籃高手平交道", desc: "希望看得到富士山！拍完就撤。" },
                { time: "14:00-16:30", name: "江之島", desc: "Sea Candle燈塔、岩屋(超推)。" },
                { time: "16:30-19:00", name: "新宿", desc: "小田急直達，逛AUX PARADIS。" },
                { time: "19:00-21:00", name: "晚餐：GyuTongue", desc: "牛舌料理，需預訂。" }
            ]},
            "Day 4": { title: "8/10 (一) 迪士尼海洋", items: [
                { time: "06:50-07:30", name: "前往舞濱", desc: "東西線+日比谷線+京葉線(30分)。" },
                { time: "08:00-21:10", name: "迪士尼海洋", desc: "全日暢玩，記得提前確認DPA。" },
                { time: "21:10-22:15", name: "回飯店", desc: "玩累了休息。" }
            ]},
            "Day 5": { title: "8/11 (二) 淺草 · 銀座 · 六本木", items: [
                { time: "10:00-10:30", name: "前往淺草", desc: "東西線+銀座線(25分)。" },
                { time: "10:30-12:00", name: "淺草浴衣", desc: "預約著裝，逛雷門、淺草寺。" },
                { time: "12:00-13:00", name: "午餐", desc: "吃美食。" },
                { time: "13:30-15:00", name: "銀座", desc: "步行者天國，逛Loft, GINZA SIX。" },
                { time: "15:20-17:00", name: "逛逛", desc: "油炸三明治、泡芙。" },
                { time: "17:55-19:30", name: "晚餐：入鹿TOKYO", desc: "六本木，拉麵(不收現金)。" },
                { time: "19:50-20:40", name: "東京鐵塔", desc: "芝公園拍夜景。" },
                { time: "20:45-21:15", name: "回飯店", desc: "大江戶線直達。" }
            ]},
            "Day 6": { title: "8/12 (三) 返家", items: [
                { time: "10:00-10:15", name: "退房", desc: "11:00前務必退房。" },
                { time: "10:45-11:50", name: "Access特急", desc: "日本橋->成田，認明「成田空港行」。" },
                { time: "12:00-13:00", name: "機場報到", desc: "最晚12:00到機場。" },
                { time: "13:00-14:30", name: "免稅店", desc: "趕快補貨！" },
                { time: "14:30", name: "起飛返台", desc: "CI101 17:15抵達。" }
            ]}
        };

        const weatherData = {
            "Day 1": { loc: "東京", temp: "32°C", cond: "晴時多雲", outfit: "輕便透氣，帶陽傘防曬" },
            "Day 2": { loc: "澀谷/原宿", temp: "31°C", cond: "多雲午後陣雨", outfit: "好走的球鞋，備摺疊傘" },
            "Day 3": { loc: "鎌倉", temp: "30°C", cond: "晴朗", outfit: "涼爽短袖，注意防曬" },
            "Day 4": { loc: "迪士尼", temp: "33°C", cond: "晴朗炎熱", outfit: "透氣衣物，墨鏡、充足補水" },
            "Day 5": { loc: "淺草/銀座", temp: "30°C", cond: "多雲", outfit: "浴衣內穿吸汗內搭，防曬" },
            "Day 6": { loc: "東京", temp: "30°C", cond: "晴朗", outfit: "寬鬆機上服" }
        };

        const guideData = [
            { title: "交通必備", desc: "手機綁定 Suica/Pasmo，準備好 Visit Japan Web QR。" },
            { title: "支付提醒", desc: "準備好西瓜卡與信用卡，入鹿TOKYO等名店不收現金。" },
            { title: "購物攻略", desc: "攜帶護照隨時辦理退稅，準備好優惠券連結。" }
        ];

        function renderContent(tab = 'itinerary', day = "Day 1") {
            const content = document.getElementById('content');
            
            // Update Tab states
            document.querySelectorAll('button[id^="tab-"]').forEach(btn => {
                btn.classList.remove('tab-active', 'text-blue-800');
                btn.classList.add('text-gray-400');
            });
            document.querySelectorAll('button[id^="nav-"]').forEach(btn => {
                btn.classList.remove('text-blue-800');
                btn.classList.add('text-gray-400');
            });
            
            const tabBtn = document.getElementById('tab-' + tab);
            if(tabBtn) { tabBtn.classList.add('tab-active', 'text-blue-800'); tabBtn.classList.remove('text-gray-400'); }
            const navBtn = document.getElementById('nav-' + tab);
            if(navBtn) { navBtn.classList.remove('text-gray-400'); navBtn.classList.add('text-blue-800'); }

            const cuteFooter = `
                <div class="flex justify-center mt-6 opacity-50">
                    <svg viewBox="0 0 100 40" class="w-24 h-10">
                        <path d="M10 30 Q25 10 40 30 T70 30 T100 30" stroke="#fecaca" fill="none" stroke-width="4"/>
                    </svg>
                </div>
            `;

            if(tab === 'itinerary') {
                content.innerHTML = `
                    <div class="flex gap-2 overflow-x-auto pb-4 mb-2">
                        ${Object.keys(tripData).map(d => `<button onclick="renderContent('itinerary', '${d}')" class="px-4 py-2 rounded-full ${day === d ? 'bg-blue-800 text-white' : 'bg-white border'} font-bold shadow-sm whitespace-nowrap">${d}</button>`).join('')}
                    </div>
                    <div class="card p-6">
                        <h2 class="text-xl font-bold mb-4">${tripData[day].title}</h2>
                        ${tripData[day].items.map(item => `
                            <div class="border-l-4 border-blue-200 pl-4 mb-6">
                                <h3 class="font-bold text-lg">${item.time} ${item.name}</h3>
                                <p class="text-sm text-gray-600 my-1">${item.desc}</p>
                            </div>
                        `).join('')}
                    </div>
                    ${cuteFooter}`;
            } else if(tab === 'weather') {
                content.innerHTML = `
                    <div class="space-y-4">
                        <h2 class="text-xl font-bold mb-4 text-blue-900">🌤️ 每日行程氣象建議</h2>
                        ${Object.keys(weatherData).map(d => `
                            <div class="card p-5 border-l-4 border-blue-400">
                                <h3 class="font-bold text-lg text-blue-800">${d} - ${weatherData[d].loc}</h3>
                                <div class="flex items-center justify-between mt-2">
                                    <span class="text-2xl font-black text-gray-700">${weatherData[d].temp}</span>
                                    <span class="bg-blue-100 text-blue-600 px-3 py-1 rounded-full text-sm font-bold">${weatherData[d].cond}</span>
                                </div>
                                <div class="mt-3 text-sm text-gray-600 bg-gray-50 p-3 rounded-lg">
                                    <strong>👕 建議：</strong> ${weatherData[d].outfit}
                                </div>
                            </div>
                        `).join('')}
                    </div>
                    ${cuteFooter}`;
            } else if(tab === 'guide') {
                content.innerHTML = `
                    <div class="space-y-4">
                        <h2 class="text-xl font-bold mb-4 text-blue-900">📖 旅遊指南與地圖</h2>
                        <div class="card p-5 bg-blue-50 border border-blue-100">
                            <h3 class="font-bold text-blue-800 mb-3 text-lg">📍 景點分區導覽</h3>
                            <ul class="space-y-2 text-sm text-gray-700">
                                <li>🗼 <strong>東京車站/銀座：</strong> 交通樞紐，適合購物與美食。</li>
                                <li>⛩️ <strong>原宿/澀谷：</strong> 潮流聚集地，明治神宮、SHIBUYA SKY。</li>
                                <li>🌊 <strong>鎌倉/江之島：</strong> 海景與復古電車之旅。</li>
                                <li>🏰 <strong>舞濱：</strong> 東京迪士尼海洋度假區。</li>
                                <li>🏮 <strong>淺草：</strong> 經典雷門與浴衣散步。</li>
                            </ul>
                        </div>
                        ${guideData.map(g => `
                            <div class="card p-5">
                                <h3 class="font-bold text-blue-800 mb-1">${g.title}</h3>
                                <p class="text-gray-600">${g.desc}</p>
                            </div>
                        `).join('')}
                        <div class="card p-5 bg-indigo-900 text-white">
                            <h3 class="font-bold mb-2">🚇 東京地鐵路線圖</h3>
                            <p class="text-xs text-indigo-200 mb-4">自助旅遊必備，點擊查看官方地圖。</p>
                            <a href="https://www.tokyometro.jp/tcn/subwaymap/" target="_blank" class="block w-full bg-white text-indigo-900 text-center py-3 rounded-xl font-bold shadow-lg">查看官方路線圖</a>
                        </div>
                    </div>`;
            } else if(tab === 'memo') {
                renderMemo();
            }
        }

        function renderMemo() {
            const content = document.getElementById('content');
            const savedLinks = JSON.parse(localStorage.getItem('myCustomLinks')) || [
                {title: "Visit Japan Web", url: "https://vjw-lp.digital.go.jp/"},
                {title: "唐吉訶德優惠券", url: "https://www.donki.co.jp/"},
                {title: "Bic Camera 優惠券", url: "https://www.biccamera.com/"}
            ];
            
            content.innerHTML = `
                <div class="card p-6 bg-yellow-50 border-t-8 border-yellow-300">
                    <h2 class="text-xl font-bold mb-4 text-yellow-800">📝 備忘錄</h2>
                    <div class="mb-6">
                        <h3 class="font-bold text-gray-700 mb-2">➕ 新增常用連結</h3>
                        <input type="text" id="link-title" placeholder="連結名稱" class="w-full p-2 mb-1 rounded border">
                        <input type="text" id="link-url" placeholder="網址 (https://...)" class="w-full p-2 mb-2 rounded border">
                        <button onclick="addCustomLink()" class="w-full bg-blue-600 text-white font-bold py-2 rounded">新增連結</button>
                    </div>
                    <div id="links-container" class="space-y-2 mb-6">
                        ${savedLinks.map((l, i) => `
                            <div class="flex justify-between items-center bg-white p-3 rounded-lg shadow-sm border">
                                <a href="${l.url}" target="_blank" class="text-blue-600 font-bold underline truncate mr-2">${l.title}</a>
                                <button onclick="removeLink(${i})" class="text-red-400 font-bold px-2">✕</button>
                            </div>
                        `).join('')}
                    </div>
                    <div>
                        <h3 class="font-bold text-gray-700 mb-2">✏️ 自訂筆記 (自動儲存)</h3>
                        <textarea id="memo-text" class="w-full h-32 p-3 rounded-lg border border-gray-300 shadow-inner"></textarea>
                    </div>
                </div>`;
            
            const memoArea = document.getElementById('memo-text');
            memoArea.value = localStorage.getItem('tokyoTripMemo') || '';
            memoArea.addEventListener('input', (e) => localStorage.setItem('tokyoTripMemo', e.target.value));
        }

        function addCustomLink() {
            const title = document.getElementById('link-title').value;
            const url = document.getElementById('link-url').value;
            if(!title || !url) return;
            const links = JSON.parse(localStorage.getItem('myCustomLinks')) || [];
            links.push({title, url: url.startsWith('http') ? url : 'https://' + url});
            localStorage.setItem('myCustomLinks', JSON.stringify(links));
            renderContent('memo');
        }

        function removeLink(index) {
            const links = JSON.parse(localStorage.getItem('myCustomLinks'));
            links.splice(index, 1);
            localStorage.setItem('myCustomLinks', JSON.stringify(links));
            renderContent('memo');
        }

        // Initial render
        renderContent();
    </script>
</body>
</html>
