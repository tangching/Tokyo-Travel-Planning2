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
        .card { background: white; border-radius: 24px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
    </style>
</head>
<body class="pb-24">
    <div id="app" class="max-w-md mx-auto">
        <header class="bg-white p-6 rounded-b-[32px] shadow-sm sticky top-0 z-20">
            <h1 class="text-2xl font-bold text-blue-900">美馬溝東京六日遊 🗼</h1>
            <p class="text-sm text-gray-400">8/7 (五) - 8/12 (三)</p>
        </header>

        <div class="flex justify-around p-4 mt-2">
            <button onclick="renderContent('itinerary')" id="tab-itinerary" class="tab-active font-bold text-blue-800">行程</button>
            <button onclick="renderContent('guide')" id="tab-guide" class="text-gray-400 font-bold">指南</button>
            <button onclick="renderContent('weather')" id="tab-weather" class="text-gray-400 font-bold">天氣</button>
        </div>

        <main id="content" class="p-4"></main>

        <nav class="fixed bottom-0 w-full max-w-md bg-white border-t p-4 flex justify-around z-20">
            <button class="text-blue-800 font-bold" onclick="renderContent('itinerary')">📅 行程</button>
            <button class="text-gray-400 font-bold" onclick="renderContent('guide')">🧮 指南</button>
            <button class="text-gray-400 font-bold" onclick="renderContent('weather')">🌤️ 天氣</button>
        </nav>
    </div>

    <script>
        const tripData = {
            "Day 1": { title: "8/7 (五) 抵達 · 東京車站", items: [
                { time: "10:00", name: "桃機集合", desc: "Visit Japan Web QR先填好。" },
                { time: "12:15", name: "CI104 飛往成田", desc: "抵達 16:35，機上餐。" },
                { time: "17:35", name: "Skyliner", desc: "B1京成櫃檯取票，41分抵上野。" },
                { time: "18:20", name: "轉乘與寄物", desc: "上野至東京，找櫃子寄行李。" },
                { time: "18:40", name: "晚餐：極味屋", desc: "GRANSTA八重北店，備案要先找好。" },
                { time: "20:00", name: "一番街掃貨", desc: "Brulee Merize (20:30關) 必買！確認保存期限。" },
                { time: "22:00", name: "回飯店", desc: "東西線15分，確認最晚22:00入住。" }
            ]},
            "Day 2": { title: "8/8 (六) 明治神宮 · 澀谷", items: [
                { time: "08:40", name: "出發", desc: "東西線+千代田線(35分)，記得買早餐。" },
                { time: "09:30", name: "明治神宮", desc: "散步參拜。" },
                { time: "10:45", name: "竹下通", desc: "@cosme, Converse, 3COINS, VIVAIA。" },
                { time: "13:30", name: "表參道/貓街", desc: "Onitsuka, SOU·SOU, TEAPOND, 東急PLAZA, HUMAN MADE。" },
                { time: "17:30", name: "SHIBUYA SKY", desc: "預約黃昏(約18:40日落)，拍攝剪影。" },
                { time: "19:15", name: "晚餐：燒肉Aburu", desc: "大塚站，記得訂位。" }
            ]},
            "Day 3": { title: "8/9 (日) 鎌倉 · 江之島", items: [
                { time: "07:50", name: "出發", desc: "JR橫須賀線，車上吃早餐。" },
                { time: "09:40", name: "小町通", desc: "邊走邊吃，尋找美食。" },
                { time: "12:55", name: "灌籃高手平交道", desc: "鎌倉高校前，拍完就撤。" },
                { time: "14:00", name: "江之島", desc: "辺津宮→Sea Candle燈塔→奧津宮→岩屋(超推)。" },
                { time: "19:00", name: "晚餐：GyuTongue", desc: "牛舌料理，需預訂。" }
            ]},
            "Day 4": { title: "8/10 (一) 迪士尼海洋", items: [
                { time: "06:50", name: "前往舞濱", desc: "東西線+日比谷線+京葉線(30分)。" },
                { time: "08:00", name: "迪士尼海洋", desc: "全日暢玩，記得提前確認DPA。" },
                { time: "21:10", name: "回飯店", desc: "玩累了休息。" }
            ]},
            "Day 5": { title: "8/11 (二) 淺草 · 銀座 · 六本木", items: [
                { time: "10:00", name: "前往淺草", desc: "東西線+銀座線(25分)。" },
                { time: "10:30", name: "淺草浴衣", desc: "預約著裝，逛雷門/淺草寺。" },
                { time: "15:00", name: "銀座逛街", desc: "Loft, GINZA SIX, 伊東屋。步行者天國。" },
                { time: "17:55", name: "晚餐：入鹿TOKYO", desc: "六本木，拉麵(不收現金)。" },
                { time: "19:50", name: "東京鐵塔", desc: "芝公園拍夜景。" }
            ]},
            "Day 6": { title: "8/12 (三) 返家", items: [
                { time: "10:00", name: "退房", desc: "11:00前務必退房。" },
                { time: "10:45", name: "Access特急", desc: "認明「成田空港行」。" },
                { time: "13:00", name: "免稅店", desc: "趕快補貨！" },
                { time: "14:30", name: "起飛返台", desc: "CI101 17:15抵達。" }
            ]}
        };

        function renderContent(tab = 'itinerary', day = "Day 1") {
            const content = document.getElementById('content');
            document.querySelectorAll('button[id^="tab-"]').forEach(btn => btn.classList.remove('tab-active', 'text-blue-800'));
            
            // Set nav style
            const tabBtn = document.getElementById('tab-' + tab);
            if(tabBtn) tabBtn.classList.add('tab-active', 'text-blue-800');

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
                    </div>`;
            } else if(tab === 'guide') {
                content.innerHTML = `
                    <div class="card p-6">
                        <h2 class="text-xl font-bold mb-4">旅行指南</h2>
                        <ul class="space-y-4">
                            <li class="bg-blue-50 p-4 rounded-lg"><strong>✈️ 機場：</strong> CI104 (去), CI101 (回)。</li>
                            <li class="bg-blue-50 p-4 rounded-lg"><strong>📱 必備：</strong> Visit Japan Web 已填寫、Skyliner 預約。</li>
                            <li class="bg-blue-50 p-4 rounded-lg"><strong>👘 浴衣預約：</strong> 記得確認預約時段。</li>
                            <li class="bg-blue-50 p-4 rounded-lg"><strong>💳 付款：</strong> 準備好SUICA/PASMO，入鹿TOKYO不收現金。</li>
                        </ul>
                    </div>`;
            } else {
                content.innerHTML = `
                    <div class="card p-6 text-center">
                        <h2 class="text-xl font-bold mb-4">天氣資訊</h2>
                        <p class="text-gray-600">8月東京氣溫約 30-35°C。</p>
                        <p class="text-sm text-blue-500 mt-2 italic">記得補水與攜帶陽傘防曬！</p>
                    </div>`;
            }
        }
        renderContent();
    </script>
</body>
</html>
