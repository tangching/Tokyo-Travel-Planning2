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
        .card { background: white; border-radius: 20px; box-shadow: 0 4px 10px -1px rgba(0, 0, 0, 0.05); }
        .tag { font-size: 0.7rem; padding: 2px 8px; border-radius: 12px; font-weight: bold; margin-right: 4px; display: inline-block; }
        .tag-train { background-color: #e0f2fe; color: #0284c7; }
        .tag-alert { background-color: #ffe4e6; color: #e11d48; }
        .tag-food { background-color: #fef3c7; color: #d97706; }
        .tag-info { background-color: #f3f4f6; color: #4b5563; }
        /* 隱藏捲軸但可滑動 */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="pb-28">
    <div id="app" class="max-w-md mx-auto">
        <!-- Header -->
        <header class="bg-white p-6 shadow-sm sticky top-0 z-20">
            <h1 class="text-2xl font-bold text-blue-900">美馬溝東京六日遊 🗼</h1>
            <p class="text-sm text-gray-500 font-bold mt-1">8/7 - 8/12 (2026) ｜ 東京爆買啦 ❤️</p>
        </header>

        <!-- Top Tabs -->
        <div class="flex justify-between p-3 mt-2 bg-white mx-4 rounded-xl shadow-sm border border-gray-100 text-sm overflow-x-auto no-scrollbar gap-2">
            <button onclick="renderContent('itinerary')" id="tab-itinerary" class="tab-active font-bold text-blue-800 px-2 whitespace-nowrap">行程</button>
            <button onclick="renderContent('guide')" id="tab-guide" class="text-gray-400 font-bold px-2 whitespace-nowrap">指南</button>
            <button onclick="renderContent('disney')" id="tab-disney" class="text-gray-400 font-bold px-2 whitespace-nowrap">迪士尼</button>
            <button onclick="renderContent('weather')" id="tab-weather" class="text-gray-400 font-bold px-2 whitespace-nowrap">天氣</button>
            <button onclick="renderContent('memo')" id="tab-memo" class="text-gray-400 font-bold px-2 whitespace-nowrap">備忘</button>
        </div>

        <!-- Main Content -->
        <main id="content" class="p-4"></main>

        <!-- Bottom Nav -->
        <nav class="fixed bottom-0 w-full max-w-md bg-white border-t p-3 flex justify-between z-20 shadow-[0_-10px_15px_-3px_rgba(0,0,0,0.05)] pb-6">
            <button id="nav-itinerary" class="flex-1 flex flex-col items-center text-blue-800 font-bold transition-colors" onclick="renderContent('itinerary')">
                <span class="text-xl mb-1">📅</span><span class="text-[10px]">行程</span>
            </button>
            <button id="nav-guide" class="flex-1 flex flex-col items-center text-gray-400 font-bold transition-colors" onclick="renderContent('guide')">
                <span class="text-xl mb-1">🧮</span><span class="text-[10px]">指南</span>
            </button>
            <button id="nav-disney" class="flex-1 flex flex-col items-center text-gray-400 font-bold transition-colors" onclick="renderContent('disney')">
                <span class="text-xl mb-1">🏰</span><span class="text-[10px]">迪士尼</span>
            </button>
            <button id="nav-weather" class="flex-1 flex flex-col items-center text-gray-400 font-bold transition-colors" onclick="renderContent('weather')">
                <span class="text-xl mb-1">🌤️</span><span class="text-[10px]">天氣</span>
            </button>
            <button id="nav-memo" class="flex-1 flex flex-col items-center text-gray-400 font-bold transition-colors" onclick="renderContent('memo')">
                <span class="text-xl mb-1">📝</span><span class="text-[10px]">備忘</span>
            </button>
        </nav>
    </div>

    <script>
        const tripData = {
            "Day 1": { title: "8/7 (五) 抵達 · 東京車站之夜", items: [
                { time: "10:00", name: "桃園機場集合", desc: "<span class='tag tag-alert'>⚠️ 重要</span> Visit Japan Web QR先填好(前一天)" },
                { time: "12:15-16:35", name: "CI104 飛往成田", desc: "機上餐，準備抵達！" },
                { time: "16:35-17:30", name: "入境、領行李", desc: "拿出VJW QR Code通關" },
                { time: "17:35-18:20", name: "搭乘 Skyliner", desc: "<span class='tag tag-train'>京成上野站</span> B1京成櫃檯取票，車程41分" },
                { time: "18:20", name: "抵達東京車站", desc: "找櫃子寄放行李就出發！" },
                { time: "18:40-19:40", name: "晚餐：極味屋", desc: "<span class='tag tag-food'>🍽️ 必吃</span> GRANSTA八重北店 (要想備案餐廳)" },
                { time: "20:00", name: "東京車站一番街", desc: "逛逛買爆！Brulee Merize (20:30關) 要先買(羅美溝推薦)！<span class='tag tag-alert'>結帳前確認保存期限撐到8/12後</span>" },
                { time: "21:45-22:00", name: "回飯店 Check-in", desc: "<span class='tag tag-train'>東西線15分</span> 要先跟飯店確認最晚22:00 check in可以嗎" }
            ]},
            "Day 2": { title: "8/8 (六) 明治神宮 · 原宿 · 澀谷", items: [
                { time: "08:40-09:25", name: "出門前往原宿", desc: "<span class='tag tag-train'>東西線+千代田線(35分)</span> 記得買早餐ㄘ" },
                { time: "09:30-10:40", name: "明治神宮", desc: "走路、早晨散步參拜" },
                { time: "10:45-12:15", name: "竹下通逛街", desc: "買買買！(很多店11:00才開)<br>• @cosme TOKYO (全東京最大)<br>• CONVERSE 原宿 (蔡美溝)<br>• 3COINS 旗艦<br>• VIVAIA (小紅書推薦)" },
                { time: "12:15-13:30", name: "午餐時間", desc: "<span class='tag tag-info'>原宿/表參道</span> 求推薦中餐～" },
                { time: "13:30-16:00", name: "表參道 / 貓街", desc: "逛爆！貓街超有氣氛。<br>• Onitsuka旗艦<br>• SOU·SOU青山 (羅美溝)<br>• TEAPOND (羅推薦伴手禮)<br>• 東急PLAZA (3F吉伊卡哇麵包店/1F miffy)<br>• GYRE / HUMAN MADE 咖啡" },
                { time: "16:00-17:20", name: "往澀谷前進", desc: "• PARCO (6F寶可夢中心, 極味や, SHIRO)<br>• 迪士尼商店 (有園區沒有的款！)<br>• Harbs (千層蛋糕)<br>• Parfaiteria bel (馬瑄推薦甜品)<br>• 澀谷109" },
                { time: "17:30-19:00", name: "SHIBUYA SKY", desc: "<span class='tag tag-alert'>🎟️ 需事前買票</span> 看美景日落 (8/8約18:40日落)" },
                { time: "19:15-20:30", name: "晚餐：燒肉Aburu", desc: "<span class='tag tag-food'>🍽️ 必吃·要訂位</span> 大塚站，吃完繼續逛十字路口" },
                { time: "22:00-22:30", name: "回飯店", desc: "休息充電！" }
            ]},
            "Day 3": { title: "8/9 (日) 鎌倉 · 江之島一日遊", items: [
                { time: "07:50-09:40", name: "出門前往鎌倉", desc: "<span class='tag tag-train'>東西線+橫須賀線(57分)</span> 超商早餐車上吃" },
                { time: "09:40-11:15", name: "鎌倉小町通", desc: "邊逛邊吃，找推薦必買店家" },
                { time: "11:15-12:15", name: "鶴岡八幡宮", desc: "小町通走到底，參拜神社 (準備零錢)" },
                { time: "12:30-12:55", name: "江之電", desc: "前往鎌倉高校前 (江之電21分)" },
                { time: "12:55-13:30", name: "灌籃高手平交道", desc: "希望可以看到富士山！<span class='tag tag-alert'>超熱·拍完就撤</span>" },
                { time: "14:00-16:30", name: "江之島探險", desc: "辺津宮 → Escar → Sea Candle燈塔 → 奧津宮 → 岩屋 (超推薦！)" },
                { time: "16:30", name: "離島衝新宿", desc: "<span class='tag tag-train'>小田急直達(70分)</span> 最晚16:30離開！可逛歌舞伎町/AUX PARADIS/塩麵包" },
                { time: "19:00", name: "晚餐：GyuTongue", desc: "<span class='tag tag-food'>🍽️ 要預訂</span> 牛舌料理" },
                { time: "22:00", name: "回飯店", desc: "晚上去超商買明天早餐" }
            ]},
            "Day 4": { title: "8/10 (一) 東京迪士尼海洋", items: [
                { time: "06:50-07:30", name: "出門前往舞濱", desc: "<span class='tag tag-train'>東西線+日比谷線+京葉線(30分)</span> 帶超商早餐" },
                { time: "08:00-21:10", name: "DisneySea 入園", desc: "全日暢玩！(打開 App 攻略頁面搶 DPA)" },
                { time: "21:10-22:15", name: "回飯店", desc: "原路返回 (30分)，洗洗睡" }
            ]},
            "Day 5": { title: "8/11 (二) 淺草浴衣 · 銀座 · 六本木", items: [
                { time: "10:00-10:30", name: "出門前往淺草", desc: "<span class='tag tag-train'>東西線+銀座線(25分)</span>" },
                { time: "10:30-11:15", name: "浴衣店報到著裝", desc: "需預訂 (討論找哪間)" },
                { time: "11:15-12:00", name: "浴衣散步", desc: "雷門→仲見世→淺草寺·五重塔→傳法院通→吾妻橋<br>• よろし化粧堂 (美溝指定)<br>• 三麗鷗淺草店 (美溝推薦)" },
                { time: "12:00-13:00", name: "午餐", desc: "求推薦～" },
                { time: "13:00", name: "歸還浴衣", desc: "<span class='tag tag-alert'>不要穿太久，熱死</span>" },
                { time: "13:30", name: "前往銀座", desc: "<span class='tag tag-train'>銀座線(5分)</span>" },
                { time: "15:00-17:45", name: "銀座逛街", desc: "山之日步行者天國封街(到18:00)<br>• Loft銀座 (最大最齊)<br>• GINZA SIX / 伊東屋<br>• 油炸三明治 (要排隊！)<br>• TEN&@日比谷 (蔡美溝推薦，泡芙超好吃！傍晚易完售)" },
                { time: "17:45-17:55", name: "前往六本木", desc: "<span class='tag tag-train'>日比谷線(9分)</span>" },
                { time: "17:55-19:30", name: "晚餐：入鹿TOKYO", desc: "<span class='tag tag-food'>🍽️ 必吃拉麵</span> 18:00前排隊！<span class='tag tag-alert'>不收現金</span>" },
                { time: "19:50-20:40", name: "東京鐵塔", desc: "點燈夜景版 (芝公園拍照)" },
                { time: "20:45-21:15", name: "回飯店", desc: "<span class='tag tag-train'>大江戶線直達(15分)</span>" }
            ]},
            "Day 6": { title: "8/12 (三) 直達機場 · 返家", items: [
                { time: "08:00-10:00", name: "起床 & 飯店早餐", desc: "慢慢來，10:30前一定要退房" },
                { time: "10:15-10:45", name: "前往日本橋換線", desc: "<span class='tag tag-train'>東西線2站</span> 轉都營淺草線" },
                { time: "10:45-11:50", name: "Access特急", desc: "<span class='tag tag-train'>約60分</span> 認明「アクセス特急·成田空港行」，班距40分" },
                { time: "11:50-13:00", name: "機場報到", desc: "<span class='tag tag-alert'>最晚12:00要到機場</span>" },
                { time: "13:00-13:50", name: "免稅店", desc: "趕快補貨！！！" },
                { time: "14:30", name: "返台", desc: "CI101 起飛 (17:15抵達)" }
            ]}
        };

        const weatherData = {
            "Day 1": { city: "東京市區", weather: "晴時多雲 28-33°C", outfit: "輕便夏季衣物，需防曬。" },
            "Day 2": { city: "原宿/澀谷", weather: "多雲 27-32°C", outfit: "穿著最舒適、好走跳的鞋子！" },
            "Day 3": { city: "鎌倉/江之島", weather: "晴朗 26-31°C", outfit: "海邊風大日照強，帶薄外套與陽傘。" },
            "Day 4": { city: "迪士尼海洋", weather: "晴朗 27-32°C", outfit: "防曬補水必備，排隊會熱，帶小風扇。" },
            "Day 5": { city: "淺草/銀座", weather: "多雲 28-33°C", outfit: "上午浴衣體驗，下午換輕便洋裝或便服。" },
            "Day 6": { city: "成田機場", weather: "陣雨機率 26-30°C", outfit: "舒適的登機便裝。" }
        };

        function renderContent(tab = 'itinerary', day = "Day 1") {
            const content = document.getElementById('content');
            
            // 重置按鈕狀態
            document.querySelectorAll('button[id^="tab-"]').forEach(btn => {
                btn.classList.remove('tab-active', 'text-blue-800');
                btn.classList.add('text-gray-400');
            });
            document.querySelectorAll('button[id^="nav-"]').forEach(btn => {
                btn.classList.remove('text-blue-800');
                btn.classList.add('text-gray-400');
            });
            
            // 設定當前選中狀態
            const tabBtn = document.getElementById('tab-' + tab);
            if(tabBtn) { tabBtn.classList.add('tab-active', 'text-blue-800'); tabBtn.classList.remove('text-gray-400'); }
            const navBtn = document.getElementById('nav-' + tab);
            if(navBtn) { navBtn.classList.remove('text-gray-400'); navBtn.classList.add('text-blue-800'); }

            if(tab === 'itinerary') {
                content.innerHTML = `
                    <div class="flex gap-2 overflow-x-auto pb-4 mb-2 no-scrollbar">
                        ${Object.keys(tripData).map(d => `<button onclick="renderContent('itinerary', '${d}')" class="px-4 py-2 rounded-full whitespace-nowrap text-sm ${day === d ? 'bg-blue-800 text-white' : 'bg-white border text-gray-600'} font-bold shadow-sm transition-colors">${d}</button>`).join('')}
                    </div>
                    <div class="card p-5 mb-6">
                        <h2 class="text-lg font-bold mb-5 pb-2 border-b-2 border-blue-100 text-blue-900">${tripData[day].title}</h2>
                        <div class="space-y-5">
                            ${tripData[day].items.map(i => `
                                <div class="relative pl-5 border-l-2 border-blue-200">
                                    <div class="absolute w-3 h-3 bg-blue-500 rounded-full -left-[7px] top-1.5 shadow-sm"></div>
                                    <div class="text-sm font-bold text-blue-800 mb-1">${i.time}</div>
                                    <div class="font-bold text-gray-800 mb-1 text-base">${i.name}</div>
                                    <div class="text-sm text-gray-600 leading-relaxed">${i.desc}</div>
                                </div>
                            `).join('')}
                        </div>
                    </div>`;
            } else if(tab === 'disney') {
                content.innerHTML = `
                    <div class="card p-6 mb-6">
                        <h2 class="text-xl font-bold mb-5 text-blue-900">🏰 東京迪士尼海洋攻略</h2>

                        <!-- App -->
                        <div class="mb-5 border-l-4 border-blue-400 pl-3">
                            <h3 class="font-bold text-lg text-gray-800">③ 📲 App 一定要下載！</h3>
                            <p class="text-sm text-gray-600 mt-1">「Tokyo Disney Resort App」功能神級👇</p>
                            <ul class="text-sm text-gray-700 list-disc ml-5 mt-1 space-y-1">
                                <li>即時排隊時間</li>
                                <li>預約餐廳與表演</li>
                                <li>購買快速通關（Premier Access）</li>
                            </ul>
                        </div>

                        <!-- DPA -->
                        <div class="mb-5 border-l-4 border-yellow-400 pl-3">
                            <h3 class="font-bold text-lg text-gray-800">⚡ 快速通關攻略</h3>
                            <p class="text-sm text-gray-600 mt-1">迪士尼 FastPass 已取消！改成付費制：<br>🎟️ <strong>Disney Premier Access ¥1,500～¥2,500/次</strong></p>
                            <div class="bg-yellow-50 p-3 rounded-lg mt-2 text-sm border border-yellow-100">
                                <strong>🔥 必搶設施：</strong>
                                <ul class="list-none mt-1 space-y-1 text-gray-800">
                                    <li>🩷 <strong>冰雪奇緣</strong> <span class="text-red-500 text-xs">(必買DPA! 開園30-60min完售)</span></li>
                                    <li>🤍 小飛俠（冒險船）</li>
                                    <li>🩵 翱翔夢幻奇航</li>
                                    <li>🌹 美女與野獸城堡</li>
                                    <li>🕊️ Soaring 飛越地平線</li>
                                    <li>🌋 地心探險之旅</li>
                                </ul>
                            </div>
                        </div>

                        <!-- Food -->
                        <div class="mb-5 border-l-4 border-orange-400 pl-3">
                            <h3 class="font-bold text-lg text-gray-800">⑤ 🍿 必吃清單</h3>
                            <ul class="text-sm text-gray-700 list-none mt-1 space-y-1">
                                <li>🍗 <strong>火雞腿</strong> (傳說中的香氣)</li>
                                <li>🍯 <strong>蜂蜜／焦糖爆米花</strong></li>
                                <li>🥨 <strong>吉拿棒</strong> (海鹽限定)</li>
                                <li>🧁 <strong>Duffy 甜點</strong> (海洋限定)</li>
                            </ul>
                            <div class="bg-orange-50 text-orange-700 text-xs font-bold p-2 mt-2 rounded">👉 小技巧：邊排邊吃才是樂園正統吃法！</div>
                        </div>

                        <!-- Souvenirs -->
                        <div class="mb-5 border-l-4 border-pink-400 pl-3">
                            <h3 class="font-bold text-lg text-gray-800">⑥ 🎁 必買紀念品</h3>
                            <ul class="text-sm text-gray-700 list-none mt-1 space-y-1">
                                <li>🎀 <strong>Duffy & Friends 系列</strong></li>
                                <li>🍿 <strong>造型爆米花桶</strong> (每季都有新造型)</li>
                            </ul>
                            <div class="text-xs text-red-500 font-bold mt-2">⚠️ 熱門款會秒殺，看到就下手別猶豫！</div>
                        </div>

                        <!-- Photos -->
                        <div class="mb-5 border-l-4 border-purple-400 pl-3">
                            <h3 class="font-bold text-lg text-gray-800">⑨ 📸 必拍景點 Top 5</h3>
                            <ol class="text-sm text-gray-700 list-decimal ml-5 mt-1 space-y-1">
                                <li>美女與野獸城堡</li>
                                <li>灰姑娘噴泉</li>
                                <li>Soaring 地球儀</li>
                                <li>Duffy 專區 Cape Cod</li>
                                <li>夜間電氣遊行 🌈</li>
                            </ol>
                            <div class="bg-purple-50 text-purple-700 text-xs font-bold p-2 mt-2 rounded">✨ 黃昏＋夜燈＝迪士尼最魔法時刻 ✨</div>
                        </div>

                        <!-- Tips -->
                        <div class="mb-2 border-l-4 border-green-400 pl-3">
                            <h3 class="font-bold text-lg text-gray-800">⑩ 💫 小提醒</h3>
                            <ul class="text-sm text-gray-700 list-none mt-1 space-y-1">
                                <li>✅ 提早入園 (開園前30～60分鐘)</li>
                                <li>✅ 帶行動電源 ＋ 好走鞋</li>
                            </ul>
                        </div>
                    </div>`;
            } else if(tab === 'guide') {
                content.innerHTML = `
                    <div class="card p-6 mb-6">
                        <h2 class="text-xl font-bold mb-4 text-blue-900">📍 旅遊地圖指南</h2>
                        <ul class="space-y-3 text-sm text-gray-700 mb-6">
                            <li class="bg-gray-50 p-3 rounded-lg border border-gray-100">🗼 <strong>東京車站/銀座：</strong> 伴手禮一級戰區、高級百貨、精品。</li>
                            <li class="bg-gray-50 p-3 rounded-lg border border-gray-100">🛍️ <strong>原宿/表參道/澀谷：</strong> 最潮服飾、藥妝、吉伊卡哇、寶可夢、日落景觀台。</li>
                            <li class="bg-gray-50 p-3 rounded-lg border border-gray-100">⛩️ <strong>淺草：</strong> 浴衣體驗、雷門打卡、下町風情。</li>
                            <li class="bg-gray-50 p-3 rounded-lg border border-gray-100">🌊 <strong>鎌倉/江之島：</strong> 海景電車、灌籃高手、古都神社散策。</li>
                        </ul>
                        
                        <h3 class="font-bold text-lg mb-2 text-blue-800">💡 實用旅遊小知識</h3>
                        <ul class="space-y-2 text-sm text-gray-700 mb-6 list-disc pl-5">
                            <li><span class="font-bold text-red-500">免稅提醒：</span> 購買免稅品需出示護照正本或 VJW QR Code，不可拆封！</li>
                            <li><span class="font-bold text-blue-600">交通卡：</span> iPhone 可直接在錢包加入 Suica (西瓜卡) 綁卡儲值最方便。</li>
                            <li><span class="font-bold">置物櫃 (Coin Locker)：</span> 車站大多有，可用 Suica 付款，拍照記下櫃號與密碼紙。</li>
                            <li><span class="font-bold">手扶梯：</span> 東京區域習慣 <strong class="text-blue-600">靠左站立</strong>，右側通行。</li>
                        </ul>

                        <a href="https://www.tokyometro.jp/tcn/subwaymap/" target="_blank" class="block bg-blue-600 hover:bg-blue-700 transition-colors text-white text-center p-3 rounded-xl font-bold shadow-md">
                            🚇 開啟東京地鐵官方圖 (中文版)
                        </a>
                    </div>`;
            } else if(tab === 'weather') {
                content.innerHTML = `
                    <div class="card p-6 mb-6">
                        <h2 class="text-xl font-bold mb-5 text-blue-900">🌤️ 六日天氣與穿搭</h2>
                        <div class="space-y-4">
                            ${Object.entries(weatherData).map(([d, w]) => `
                                <div class="bg-gray-50 rounded-xl p-4 border border-gray-100">
                                    <div class="flex justify-between items-center mb-2">
                                        <p class="font-bold text-blue-800 text-base">${d}</p>
                                        <span class="bg-blue-100 text-blue-800 text-xs px-2 py-1 rounded-full font-bold">${w.city}</span>
                                    </div>
                                    <p class="text-sm text-gray-700 font-bold mb-1">🌤️ 預報：${w.weather}</p>
                                    <p class="text-sm text-gray-600 leading-relaxed">👗 建議：${w.outfit}</p>
                                </div>
                            `).join('')}
                        </div>
                    </div>`;
            } else if(tab === 'memo') {
                renderMemo();
            }
        }

        function renderMemo() {
            const content = document.getElementById('content');
            
            // 確保預設連結存在 (包含唐吉訶德與氣象廳)
            let savedLinks = JSON.parse(localStorage.getItem('myCustomLinks'));
            if (!savedLinks || savedLinks.length === 0) {
                savedLinks = [
                    {title: "Visit Japan Web 入境", url: "https://vjw-lp.digital.go.jp/"},
                    {title: "🐧 唐吉訶德優惠券", url: "https://www.yokosojapanpass.com/donki_fuel/public/coupon/index/0004319"},
                    {title: "☀️ 日本氣象廳 (天氣預報)", url: "https://www.jma.go.jp/jma/index.html"}
                ];
                localStorage.setItem('myCustomLinks', JSON.stringify(savedLinks));
            }

            content.innerHTML = `
                <div class="card p-6 mb-6">
                    <h2 class="text-xl font-bold mb-4 text-blue-900">📝 旅遊備忘與連結</h2>
                    
                    <div class="mb-6 bg-blue-50 p-4 rounded-xl border border-blue-100">
                        <h3 class="font-bold text-sm text-blue-800 mb-2">➕ 新增自訂連結 (餐廳/門票)</h3>
                        <div class="flex gap-2 mb-2">
                            <input type="text" id="title" placeholder="標題名稱" class="w-1/3 p-2 text-sm border border-blue-200 rounded outline-none focus:border-blue-500">
                            <input type="text" id="url" placeholder="網址 (https://...)" class="w-2/3 p-2 text-sm border border-blue-200 rounded outline-none focus:border-blue-500">
                        </div>
                        <button onclick="addLink()" class="w-full bg-blue-600 text-white font-bold py-2 rounded-lg text-sm transition-colors hover:bg-blue-700">儲存連結</button>
                    </div>

                    <div id="link-list" class="space-y-3 mb-6">
                        <h3 class="font-bold text-sm text-gray-700 border-b pb-1">🔗 快速連結清單</h3>
                        ${savedLinks.map((l, i) => `
                            <div class="flex justify-between items-center bg-white p-3 border border-gray-100 rounded-lg shadow-sm">
                                <a href="${l.url}" target="_blank" class="text-blue-600 font-bold text-sm truncate mr-2 underline decoration-blue-200 hover:decoration-blue-500">${l.title}</a>
                                <button onclick="removeLink(${i})" class="text-gray-400 hover:text-red-500 px-2 text-lg transition-colors">×</button>
                            </div>
                        `).join('')}
                    </div>

                    <div>
                        <h3 class="font-bold text-sm text-gray-700 border-b pb-1 mb-3">✏️ 自訂筆記 (輸入即自動儲存)</h3>
                        <textarea id="memo-text" class="w-full h-40 p-4 text-sm border border-gray-200 rounded-xl bg-gray-50 outline-none focus:bg-white focus:border-blue-400 focus:ring-2 ring-blue-100 transition-all placeholder-gray-400 shadow-inner" placeholder="在這裡貼上訂單編號、想買的清單或是臨時的筆記..."></textarea>
                    </div>
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
            // 自動補齊 https:// 如果使用者忘記打
            const finalUrl = url.startsWith('http') ? url : 'https://' + url;
            links.push({title, url: finalUrl});
            localStorage.setItem('myCustomLinks', JSON.stringify(links));
            renderContent('memo');
        }

        function removeLink(i) {
            const links = JSON.parse(localStorage.getItem('myCustomLinks'));
            links.splice(i, 1);
            localStorage.setItem('myCustomLinks', JSON.stringify(links));
            renderContent('memo');
        }

        // 初始化顯示行程第一天
        renderContent();
    </script>
</body>
</html>


