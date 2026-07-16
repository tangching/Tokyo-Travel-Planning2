<!-- ... existing code ... -->
        function renderWeather() {
            const content = document.getElementById('content');
            // 增加防呆機制，確保資料能正確渲染
            if (!weatherData || Object.keys(weatherData).length === 0) {
                content.innerHTML = `<div class="card p-6 text-center text-gray-500">暫無天氣資料</div>`;
                return;
            }
            content.innerHTML = `
                <div class="space-y-4">
                    <h2 class="text-xl font-bold mb-4 text-blue-900">🌤️ 每日行程氣象</h2>
                    ${Object.keys(weatherData).map(day => `
                        <div class="card p-5 border-l-4 border-blue-400">
                            <h3 class="font-bold text-lg text-blue-800">${day} - ${weatherData[day].loc}</h3>
                            <div class="flex items-center justify-between mt-2">
                                <span class="text-2xl font-black text-gray-700">${weatherData[day].temp}</span>
                                <span class="bg-blue-100 text-blue-600 px-3 py-1 rounded-full text-sm font-bold">${weatherData[day].cond}</span>
                            </div>
                            <div class="mt-3 text-sm text-gray-600 bg-gray-50 p-3 rounded-lg">
                                <strong>👕 穿搭建議：</strong> ${weatherData[day].outfit}
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;
        }

        function renderContent(tab = 'itinerary', day = "Day 1") {
            const content = document.getElementById('content');
            
            // 強制重置滾動位置，確保體驗更好
            content.scrollTop = 0;
            
            // ... existing code ...
```

這份更新增加了氣象頁面的資料偵測。如果更新後還是無法顯示，建議您檢查一下是不是切換到了「東京極致旅遊 App」那個檔案。希望這能解決您的問題！
