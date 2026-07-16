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
        .tag { display: inline-block; padding: 2px 6px; border-radius: 4px; font-size: 0.75rem; font-weight: bold; margin-bottom: 4px; }
        .tag-train { background-color: #e0f2fe; color: #0284c7; }
        .tag-alert { background-color: #fee2e2; color: #dc2626; }
    </style>
</head>
<body class="pb-24">
    <div id="app" class="max-w-md mx-auto">
        <header class="bg-white p-6 shadow-sm sticky top-0 z-20">
            <h1 class="text-2xl font-bold text-blue-900">美馬溝東京六日遊 🗼</h1>
            <p class="text-sm text-gray-500 font-bold mt-1">東京爆買啦 ❤️ (2026 / 8/7 - 8/12)</p>
        </header>

        <!-- Top Tabs -->
        <div class="flex justify-around p-4 mt-2 bg-white mx-4 rounded-xl shadow-sm border border-gray-100">
            <button onclick="renderContent('itinerary')" id="tab-itinerary" class="tab-active font-bold text-blue-800">行程</button>
            <button onclick="renderContent('guide')" id="tab-guide" class="text-gray-400 font-bold">指南</button>
            <button onclick="renderContent('weather')" id="tab-weather" class="text-gray-400 font-bold">天氣</button>
            <button onclick="renderContent('memo')" id="tab-memo" class="text-gray-400 font-bold">備忘</button>
        </div>

        <main id="content" class="p-4"></main>

        <!-- Bottom Nav -->
        <nav class="fixed bottom-0 w-full max-w-md bg-white border-t p-4 flex justify-around z-20 shadow-lg">
            <button id="nav-itinerary" class="text-blue-800 font-bold flex flex-col items-center" onclick="renderContent('itinerary')"><span class="text-xl">📅</span><span class="text-xs mt-1">行程</span></button>
            <button id="nav-guide" class="text-gray-400 font-bold flex flex-col items-center" onclick="renderContent('guide')"><span class="text-xl">🧮</span><span class="text-xs mt-1">指南</span></button>
            <button id="nav-weather" class="text-gray-400 font-bold flex flex-col items-center" onclick="renderContent('weather')"><span class="text-xl">🌤️</span><span class="text-xs mt-1">天氣</span></button>
            <button id="nav-memo" class="text-gray-400 font-bold flex flex-col items-center" onclick="renderContent('memo')"><span class="text-xl">📝</span><span class="text-xs mt-1">備忘</span></button>
        </nav>
    </div>

    <script>
        const tripData = {
            "Day 1": { title: "8/7 (五) 抵達 · 東京車站之夜", items: [
                { time: "10:00", name: "桃園機場集合", desc: "" },
                { time: "12:15", name: "飛往成田機場 (16:35抵達)", desc: "機上餐" },
                { time: "16:35", name: "入境、領行李", desc: "<span class='tag tag-alert'>⚠️ Visit Japan Web QR前一天先填好</span>" },
                { time: "17:35", name: "成田機場搭乘 Skyliner", desc: "<span class='tag tag-train'>Skyliner 41分</span> B1京成櫃檯取票。18:20到京成上野站。" },
                { time: "18:20", name: "上野站轉至東京車站", desc: "找櫃子寄放行李就出發。" },
                { time: "18:40", name: "晚餐：極味屋", desc: "GRANSTA八重北店（要想備案餐廳）。預計吃到 19:40。" },
                { time: "20:00", name: "東京車站一番街 / 丸之內廣場", desc: "逛逛買爆！拍拍丸之內站前廣場。<br><span class='tag tag-alert'>⚠️ Brulee Merize Tokyo Gift 是20:30關，要先買！</span>(羅美溝推薦，確認保存期限撐到8/12後)" },
                { time: "21:45", name: "回飯店", desc: "<span class='tag tag-train'>東西線 15分</span> 要先跟飯店確認最晚22:00 check in可以嗎（不行的話當天行程要調整）。" }
            ]},
            "Day 2": { title: "8/8 (六) 明治神宮 · 原宿 · 表參道 · 澀谷", items: [
                { time: "08:40", name: "出門 → 明治神宮前", desc: "<span class='tag tag-train'>東西線+千代田線 35分</span> 要記得買早餐吃。" },
                { time: "09:30", name: "明治神宮", desc: "走路逛逛參拜 (至 10:40)。" },
                { time: "10:45", name: "竹下通 逛逛買買買", desc: "很多店11:00才開～<br>• @cosme TOKYO (全東京最大間)<br>• CONVERSE原宿 (蔡美溝想逛)<br>• 3COINS旗艦 (很大間)<br>• VIVAIA ハラカド店 (小紅書推薦)" },
                { time: "12:15", name: "午餐 (原宿/表參道)", desc: "求推薦中餐～ (至 13:30)" },
                { time: "13:30", name: "表參道 / 貓街逛逛", desc: "推推的店：Onitsuka旗艦(最大間)、SOU·SOU青山(羅想逛)、TEAPOND(羅推薦伴手禮)、表參道之丘、東急PLAZA(3F吉伊卡哇麵包店、1F miffy style)、GYRE、HUMAN MADE咖啡。<br>貓街超有氣氛逛爆！" },
                { time: "16:00", name: "往澀谷走 (逛街)", desc: "PARCO(最潮百貨/6F寶可夢/極味や/SHIRO)、迪士尼商店(有獨家款)、Harbs下午茶、Parfaiteria bel(馬瑄推薦)、澀谷109(MOUSSY)。" },
                { time: "17:30", name: "SHIBUYA SKY", desc: "看美景日落（8/8日落約18:40）。事前要買門票！" },
                { time: "19:15", name: "晚餐：燒肉Aburu大塚", desc: "<span class='tag tag-alert'>【必吃·要訂位】</span> <span class='tag tag-train'>山手線 26分</span>" },
                { time: "20:30", name: "吃完繼續逛走 / 澀谷大馬路", desc: "拍拍澀谷壯觀的交叉大馬路！大約 22:00 回飯店。" }
            ]},
            "Day 3": { title: "8/9 (日) 鎌倉 · 江之島 一日遊", items: [
                { time: "07:50", name: "出門 → JR東京 → 鎌倉", desc: "<span class='tag tag-train'>東西線+橫須賀線 57分</span> 超商早餐車上吃。" },
                { time: "09:40", name: "鎌倉小町通", desc: "走路邊逛邊吃～找推薦必買店家，很多有名美食。" },
                { time: "11:15", name: "鶴岡八幡宮", desc: "小町通走到底參拜神社，準備零錢。" },
                { time: "12:30", name: "前往鎌倉高校前", desc: "<span class='tag tag-train'>江之電 21分</span>" },
                { time: "12:55", name: "鎌倉高校前", desc: "電車景點｜俗稱灌籃高手平交道，希望可以看到富士山！超熱拍完就撤。" },
                { time: "13:35", name: "前往江之島", desc: "<span class='tag tag-train'>江之電 5分 + 走 15分</span> 過弁天橋。" },
                { time: "14:00", name: "江之島巡禮", desc: "辺津宮 → Escar → Sea Candle燈塔(遠眺富士山) → 奧津宮 → 岩屋(超推薦可以去XDD)。邊走看風景。" },
                { time: "16:30", name: "離島衝新宿", desc: "<span class='tag tag-train'>小田急直達 70分</span> 最晚16:30離開。逛歌舞伎町 / AUX PARADIS / 塩麵包新宿店。" },
                { time: "19:00", name: "晚餐：GyuTongue", desc: "<span class='tag tag-alert'>【要預訂】</span> 牛舌料理～" },
                { time: "22:00", name: "回飯店", desc: "晚上可以去超商買個明天的早餐。" }
            ]},
            "Day 4": { title: "8/10 (一) 東京迪士尼海洋", items: [
                { time: "06:50", name: "出門 → 舞濱 → DisneySea", desc: "<span class='tag tag-train'>東西線+日比谷線+京葉線 30分</span> 帶超商早餐。07:30 抵達入園口。" },
                { time: "08:00", name: "東京迪士尼海洋", desc: "全日暢玩！" },
                { time: "21:10", name: "回飯店", desc: "<span class='tag tag-train'>原路 30分</span> 大約 22:15 回飯店休息。" }
            ]},
            "Day 5": { title: "8/11 (二) 淺草浴衣 · 銀座 · 六本木 · 東京鐵塔", items: [
                { time: "10:00", name: "出門 → 淺草", desc: "<span class='tag tag-train'>東西線+銀座線 25分</span>" },
                { time: "10:30", name: "浴衣店報到·著裝", desc: "要找浴衣店，求推薦要討論預訂。" },
                { time: "11:15", name: "浴衣散步", desc: "雷門 → 仲見世 → 淺草寺·五重塔 → 傳法院通 → 吾妻橋。<br>逛：よろし化粧堂(美溝指定)、三麗鷗淺草店(美溝推薦)。" },
                { time: "12:00", name: "午餐", desc: "求推薦～" },
                { time: "13:00", name: "歸還浴衣", desc: "租到一點，不要穿太久，熱死。" },
                { time: "13:30", name: "準備搭車往銀座", desc: "<span class='tag tag-train'>銀座線 5分</span>" },
                { time: "15:00", name: "銀座", desc: "Loft銀座(貨齊/最大)、GINZA SIX、伊東屋。<br><span class='tag tag-alert'>山之日步行者天國封街</span> (到18:00很好拍照)" },
                { time: "15:20", name: "銀座一丁目開始逛", desc: "油炸三明治(排隊超好吃)、TEN&@日比谷OKUROJI(蔡美溝推薦泡芙超好吃，傍晚大概率完售)。" },
                { time: "17:45", name: "前往六本木", desc: "<span class='tag tag-train'>日比谷線 9分</span>" },
                { time: "17:55", name: "晚餐：入鹿TOKYO六本木", desc: "<span class='tag tag-alert'>【必吃·不收現金】</span> 18:00前排隊吃拉麵。" },
                { time: "19:40", name: "六本木 / 芝公園 / 東京鐵塔", desc: "六本木之丘走走。19:50 前往拍東京鐵塔點燈夜景版 (芝公園)。" },
                { time: "20:45", name: "回飯店", desc: "<span class='tag tag-train'>大江戶線 直達15分</span> 一班車到家 (約 21:15)。" }
            ]},
            "Day 6": { title: "8/12 (三) 飯店周邊 · 直達機場", items: [
                { time: "10:00", name: "飯店早餐 慢慢來", desc: "11:00前退房。" },
                { time: "10:15", name: "前往日本橋換都營淺草線", desc: "<span class='tag tag-train'>東西線 2站</span> 10:30前一定要退房。" },
                { time: "10:45", name: "Access特急 前往成田機場", desc: "<span class='tag tag-train'>約 60 分</span> 認明「アクセス特急·成田空港行」班距40分。" },
                { time: "11:50", name: "機場報到", desc: "最晚 12:00 要到機場。" },
                { time: "13:00", name: "逛免稅店", desc: "趕快補貨!!!" },
                { time: "14:30", name: "CI101 起飛", desc: "17:15 抵達桃園機場，回家！" }
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
            if(navBtn) navBtn.classList.remove('text-gray-400'); navBtn.classList.add('text-blue-800');

            if(tab === 'itinerary') {
                content.innerHTML = `
                    <div class="flex gap-2 overflow-x-auto pb-4 mb-2">
                        ${Object.keys(tripData).map(d => `<button onclick="renderContent('itinerary', '${d}')" class="px-4 py-2 rounded-full whitespace-nowrap ${day === d ? 'bg-blue-800 text-white' : 'bg-white border'} font-bold shadow-sm">${d}</button>`).join('')}
                    </div>
                    <div class="card p-6">
                        <h2 class="text-xl font-bold mb-4 text-blue-900">${tripData[day].title}</h2>
                        ${tripData[day].items.map(i => `
                            <div class="mb-5 pb-3 border-b border-gray-50 last:border-0 last:pb-0">
                                <div class="flex gap-3">
                                    <span class="font-bold text-blue-600 mt-1">${i.time}</span>
                                    <div>
                                        <h3 class="font-bold text-gray-800">${i.name}</h3>
                                        ${i.desc ? `<p class="text-sm text-gray-600 mt-1 leading-relaxed">${i.desc}</p>` : ''}
                                    </div>
                                </div>
                            </div>
                        `).join('')}
                    </div>`;
            } else if(tab === 'guide') {
                content.innerHTML = `
                    <div class="card p-6">
                        <h2 class="text-xl font-bold mb-4">📍 旅遊地圖指南</h2>
                        <ul class="space-y-4 text-sm">
                            <li class="bg-blue-50 p-3 rounded-lg">🗼 <strong>東京 / 銀座：</strong> 主要購物中心。需注意部分伴手禮店(如Brulee Merize)打烊時間。</li>
                            <li class="bg-blue-50 p-3 rounded-lg">⛩️ <strong>原宿 / 澀谷：</strong> 表參道/貓街超好逛，澀谷Sky與部分餐廳(燒肉Aburu)務必提前預約。</li>
                            <li class="bg-blue-50 p-3 rounded-lg">🌊 <strong>鎌倉 / 江之島：</strong> 江之電轉乘，注意海邊風大防曬。岩屋與燈塔是必去景點。</li>
                            <li class="bg-blue-50 p-3 rounded-lg">🏰 <strong>舞濱：</strong> 迪士尼樂園。記得提早買票與預約DPA。</li>
                        </ul>
                        <a href="https://www.tokyometro.jp/tcn/subwaymap/" target="_blank" class="block mt-6 bg-blue-600 text-white text-center p-3 rounded-lg font-bold shadow-md active:bg-blue-800">開啟東京地鐵路線圖</a>
                    </div>`;
            } else if(tab === 'weather') {
                content.innerHTML = `<div class="card p-6"><h2 class="text-xl font-bold mb-4">🌤️ 天氣與穿搭預報</h2>
                    ${Object.entries(weatherData).map(([d, w]) => `
                        <div class="mb-4 border-b pb-3 last:border-0 last:mb-0">
                            <p class="font-bold text-blue-800">${d} (${w.city})</p>
                            <p class="text-sm text-gray-600 mt-1"><strong>天氣：</strong>${w.weather}</p>
                            <p class="text-sm text-gray-600"><strong>建議：</strong>${w.outfit}</p>
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
                {title: "東京地鐵官網", url: "https://www.tokyometro.jp/tcn/"}
            ];
            content.innerHTML = `
                <div class="card p-6">
                    <h2 class="text-xl font-bold mb-4">📝 備忘錄與實用連結</h2>
                    <div class="flex gap-2 mb-2">
                        <input type="text" id="title" placeholder="連結名稱" class="w-1/2 p-2 border rounded text-sm">
                        <input type="text" id="url" placeholder="網址" class="w-1/2 p-2 border rounded text-sm">
                    </div>
                    <button onclick="addLink()" class="w-full bg-blue-800 text-white font-bold py-2 rounded mb-6 text-sm">新增連結</button>
                    <div id="link-list" class="space-y-2 mb-6">
                        ${savedLinks.map((l, i) => `<div class="flex justify-between items-center p-2 border rounded bg-gray-50"><a href="${l.url}" target="_blank" class="text-blue-600 font-bold text-sm underline">${l.title}</a><button onclick="removeLink(${i})" class="text-red-400 font-bold px-2">✕</button></div>`).join('')}
                    </div>
                    <h3 class="font-bold text-gray-800 mb-2">快速筆記 (自動儲存)</h3>
                    <textarea id="memo-text" class="w-full h-40 p-3 border rounded-lg text-sm" placeholder="輸入你想記錄的事情，例如：要買的伴手禮清單..."></textarea>
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
