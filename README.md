# Tokyo-Travel-Planning2
美馬溝東京六天五夜遊
<!DOCTYPE html>
<html lang="zh-Hant-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>東京自由行 App</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Zen+Maru+Gothic:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Zen Maru Gothic', sans-serif; background-color: #f0f4f8; }
        .card { background: white; border-radius: 24px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
        .tab-active { border-bottom: 3px solid #1e40af; color: #1e40af; }
    </style>
</head>
<body class="pb-24">
    <div id="app" class="max-w-md mx-auto">
        <!-- Header -->
        <header class="bg-white p-6 rounded-b-[32px] shadow-sm sticky top-0 z-20">
            <h1 class="text-2xl font-bold text-blue-900">東京自由行 🗼</h1>
            <p class="text-sm text-gray-400">2026.08.07 - 08.12</p>
        </header>

        <!-- Tabs -->
        <div class="flex justify-around p-4 mt-2">
            <button onclick="switchTab('itinerary')" id="tab-itinerary" class="tab-active font-bold">行程</button>
            <button onclick="switchTab('guide')" id="tab-guide" class="text-gray-400 font-bold">指南</button>
            <button onclick="switchTab('weather')" id="tab-weather" class="text-gray-400 font-bold">天氣</button>
        </div>

        <main id="content" class="p-4"></main>

        <!-- Bottom Nav -->
        <nav class="fixed bottom-0 w-full max-w-md bg-white border-t p-4 flex justify-around z-20">
            <button class="text-blue-800 font-bold">📅 行程</button>
            <button class="text-gray-400 font-bold" onclick="alert('記帳功能開發中')">🧮 記帳</button>
            <button class="text-gray-400 font-bold" onclick="alert('設定頁面')">⚙️ 設定</button>
        </nav>
    </div>

    <script>
        const tripData = {
            "Day 1": { title: "東京車站與銀座", weather: "晴朗 32°C", outfit: "透氣棉麻裝，必帶陽傘", items: [
                { name: "極味屋", type: "食物", label: "必吃", desc: "濃郁漢堡排，排隊名店。", tip: "建議避開午餐尖峰，傍晚去。" },
                { name: "東京車站一番街", type: "購物", label: "必逛", desc: "Brulee Merize 布丁必買。", tip: "可先查好限定店鋪位置。" }
            ]},
            "Day 2": { title: "澀谷潮流一日", weather: "多雲 30°C", outfit: "休閒球鞋，方便逛街", items: [
                { name: "SHIBUYA SKY", type: "景點", label: "必拍", desc: "最美城市眺望點。", tip: "預約黃昏時段，拍攝剪影最美。" }
            ]},
            "Day 3": { title: "鎌倉海邊散策", weather: "晴天 31°C", outfit: "輕便短褲，好走的涼鞋", items: [
                { name: "灌籃高手平交道", type: "景點", label: "必拍", desc: "還原動畫場景。", tip: "避開電車行駛時，安全第一。" }
            ]},
            "Day 4": { title: "東京迪士尼海洋", weather: "晴朗 33°C", outfit: "輕便舒適，記得防曬", items: [
                { name: "迪士尼海洋", type: "活動", label: "必玩", desc: "全日暢玩。", tip: "入園前先確認DPA預約。" }
            ]},
            "Day 5": { title: "淺草浴衣體驗", weather: "晴天 30°C", outfit: "浴衣穿搭", items: [
                { name: "淺草寺", type: "景點", label: "必拍", desc: "傳統浴衣體驗。", tip: "雷門人多，轉角小巷更好拍。" }
            ]},
            "Day 6": { title: "愉快返家", weather: "晴天 30°C", outfit: "舒適機上服", items: [
                { name: "成田機場", type: "交通", label: "必買", desc: "最後補貨。", tip: "免稅店時間要抓好。" }
            ]}
        };

        let currentDay = "Day 1";

        function switchTab(tab) {
            document.querySelectorAll('button[id^="tab-"]').forEach(btn => btn.classList.remove('tab-active', 'text-blue-800'));
            document.getElementById('tab-' + tab).classList.add('tab-active', 'text-blue-800');
            renderContent(tab);
        }

        function renderContent(tab = 'itinerary', day = "Day 1") {
            const content = document.getElementById('content');
            if(tab === 'itinerary') {
                content.innerHTML = `
                    <div class="flex gap-2 overflow-x-auto pb-4">
                        ${Object.keys(tripData).map(d => `<button onclick="renderContent('itinerary', '${d}')" class="px-4 py-2 rounded-full ${day === d ? 'bg-blue-800 text-white' : 'bg-white'} font-bold">${d}</button>`).join('')}
                    </div>
                    <div class="card p-6">
                        <h2 class="text-xl font-bold mb-4">${tripData[day].title}</h2>
                        ${tripData[day].items.map(item => `
                            <div class="border-l-4 border-blue-200 pl-4 mb-6">
                                <span class="text-[10px] bg-blue-100 px-2 py-1 rounded-full text-blue-800 font-bold">${item.label}</span>
                                <h3 class="font-bold text-lg mt-1">${item.name}</h3>
                                <p class="text-sm text-gray-600 my-1">${item.desc}</p>
                                <div class="bg-blue-50 p-3 rounded-lg text-xs mt-2 text-blue-900"><strong>📸 拍照小撇步：</strong> ${item.tip}</div>
                                <button class="mt-3 text-blue-600 font-bold underline text-sm">📍 導航前往 (Google Maps)</button>
                            </div>
                        `).join('')}
                        <div class="mt-4 bg-gray-50 p-4 rounded-xl text-sm border">
                            <strong>🌤️ 天氣：</strong> ${tripData[day].weather}<br>
                            <strong>🧥 穿著：</strong> ${tripData[day].outfit}
                        </div>
                    </div>`;
            } else {
                content.innerHTML = `<div class="card p-6 text-center text-gray-500">此頁面為輔助功能，稍後開放。</div>`;
            }
        }
        renderContent();
    </script>
</body>
</html>
