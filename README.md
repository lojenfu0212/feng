<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>臺中市豐原車站地區公辦都更共同負擔比率評分試算工具</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-slate-50 min-h-screen p-4 md:p-8 font-sans">

    <div class="max-w-4xl mx-auto space-y-6">
        
        <div class="bg-gradient-to-r from-blue-700 to-indigo-800 text-white p-6 rounded-2xl shadow-lg">
            <h1 class="text-2xl md:text-3xl font-bold mb-2">臺中市豐原車站地區公辦都更共同負擔比率評分試算系統</h1>
            <p class="text-blue-100 text-sm">提供承諾值檢核與審查委員評分級距自動對照</p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            
            <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100 md:col-span-1 space-y-4">
                <h2 class="text-base font-bold text-slate-800 border-b pb-2 flex items-center">
                    <span class="w-2 h-4 bg-blue-600 rounded-full mr-2"></span>參數輸入
                </h2>
                
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">上限容移(30%)時共負比 (n)</label>
                    <input type="number" id="input_n" value="70" class="w-full bg-slate-50 border border-slate-200 rounded-lg px-3 py-1.5 text-sm font-semibold focus:outline-none focus:border-blue-500" oninput="calculateAll()">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">無容移時共負比 (n')</label>
                    <input type="number" id="input_n_prime" value="65" class="w-full bg-slate-50 border border-slate-200 rounded-lg px-3 py-1.5 text-sm font-semibold focus:outline-none focus:border-blue-500" oninput="calculateAll()">
                </div>
                <div class="pt-2 border-t border-dashed">
                    <label class="block text-xs font-bold text-blue-600 mb-1">申請人承諾容移值 (T %)</label>
                    <input type="number" id="input_T" value="30" min="0" max="30" step="0.01" class="w-full bg-blue-50/50 border border-blue-200 rounded-lg px-3 py-2 text-sm font-bold text-blue-900 focus:outline-none focus:border-blue-500" oninput="if(value>30)value=30; if(value<0)value=0; calculateAll();">
                    <span class="text-[11px] text-slate-400 mt-0.5 block">例如：承諾辦理容移20%則輸入20(惟不得大於30%)</span>
                </div>
                <div>
                    <label class="block text-xs font-bold text-green-600 mb-1">申請人承諾共負比 (R %)</label>
                    <input type="number" id="input_R" value="66.68" min="0" max="100" step="0.01" class="w-full bg-green-50/50 border border-green-200 rounded-lg px-3 py-2 text-sm font-bold text-green-900 focus:outline-none focus:border-green-500" oninput="calculateAll()">
                </div>
            </div>

            <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100 md:col-span-2 flex flex-col justify-between">
                <div>
                    <h2 class="text-base font-bold text-slate-800 border-b pb-2 flex items-center mb-4">
                        <span class="w-2 h-4 bg-indigo-600 rounded-full mr-2"></span>動態試算與檢核結果
                    </h2>
                    
                    <div class="grid grid-cols-2 gap-4 mb-4">
                        <div class="bg-slate-50 p-3 rounded-xl text-center">
                            <span class="block text-xs font-medium text-slate-500 mb-0.5">有無申請容移共負比差值 (A)</span>
                            <span id="result_A" class="text-xl font-bold text-slate-800">5.00%</span>
                        </div>
                        <div class="bg-blue-50/60 p-3 rounded-xl text-center border border-blue-100">
                            <span class="block text-xs font-bold text-blue-700 mb-0.5">基準值 (N 核心指標)</span>
                            <span id="result_N" class="text-xl font-black text-blue-800">68.33%</span>
                        </div>
                    </div>
                </div>

                <div id="status_box" class="p-4 rounded-xl text-center flex flex-col items-center justify-center space-y-1 transition-all duration-300">
                    <span class="text-xs font-bold uppercase tracking-wider opacity-70">判定結果與委員評分</span>
                    <span id="status_text" class="text-2xl font-black">--</span>
                    <span id="score_text" class="text-sm font-medium opacity-90">--</span>
                </div>
            </div>
        </div>

        <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100">
            <div class="flex items-center justify-between mb-3 border-b pb-2">
                <h2 class="text-base font-bold text-slate-800 flex items-center">
                    <span class="w-2 h-4 bg-emerald-600 rounded-full mr-2"></span>委員評分級距對照表
                </h2>
                <span class="text-xs bg-slate-100 text-slate-500 px-2 py-1 rounded-md">藍色標記 ★ 為當前案件落點</span>
            </div>
            
            <div class="overflow-x-auto rounded-xl border border-slate-100">
                <table class="min-w-full table-auto text-sm text-left">
                    <thead>
                        <tr class="bg-slate-50 text-slate-600 font-semibold border-b border-slate-100">
                            <th class="px-4 py-3 w-[15%]">給予分數</th>
                            <th class="px-4 py-3 w-[30%]">評分級距條件 (R)</th>
                            <th class="px-4 py-3 w-[30%]">當前承諾容積移轉值對應級距</th>
                            <th class="px-4 py-3 w-[25%]">說明 (N 與 R 之差距範疇)</th>
                        </tr>
                    </thead>
                    <tbody id="table_body" class="divide-y divide-slate-100 text-slate-600">
                        </tbody>
                </table>
            </div>
        </div>

    </div>

    <script>
        function calculateAll() {
            // 1. 讀取輸入值
            const n = parseFloat(document.getElementById('input_n').value) || 0;
            const n_prime = parseFloat(document.getElementById('input_n_prime').value) || 0;
            let T = parseFloat(document.getElementById('input_T').value) || 0;
            const R = parseFloat(document.getElementById('input_R').value) || 0;

            // 強制防呆：若 T 超過 30，程式計算時一律帶入 30
            if (T > 30) {
                T = 30;
                document.getElementById('input_T').value = 30;
            }
            if (T < 0) {
                T = 0;
            document.getElementById('input_T').value = 0;
            }
             
            // 2. 計算 A 與 N
            const A = n - n_prime;
            document.getElementById('result_A').innerText = A.toFixed(2) + '%';

            // 公式:  N = n - (30 - T) / 30 * A -> 取小數點後兩位，第三位四捨五入
            let N = n - (30 - T) / 30 * A ;
            N = Math.round(N * 100) / 100; 
            document.getElementById('result_N').innerText = N.toFixed(2) + '%';

            // 3. 定義評分級距邏輯與動態落點判斷
            let currentScore = "";
            let currentStatus = "";
            let currentLevelIdx = -1; // 用來標記落在哪個級距

            // 檢核合不合格
            if (R > N) {
                currentScore = "不合格";
                currentStatus = "資格不符 (R > N)";
                currentLevelIdx = 0; // 不合格級距
            } else if (Math.abs(R - N) < 0.00001) {
                currentScore = "10.0 分";
                currentStatus = "符合規範 (R = N)";
                currentLevelIdx = 1;
            } else {
                const diff = Math.round((N - R) * 100) / 100; // N - R 的差距
                currentStatus = "符合規範 (R < N)";
                
                if (diff > 0 && diff <= 0.5) { currentScore = "10.5 分"; currentLevelIdx = 2; }
                else if (diff > 0.5 && diff <= 1.0) { currentScore = "11.0 分"; currentLevelIdx = 3; }
                else if (diff > 1.0 && diff <= 1.5) { currentScore = "11.5 分"; currentLevelIdx = 4; }
                else if (diff > 1.5 && diff <= 2.0) { currentScore = "12.0 分"; currentLevelIdx = 5; }
                else if (diff > 2.0 && diff <= 2.5) { currentScore = "12.5 分"; currentLevelIdx = 6; }
                else if (diff > 2.5 && diff <= 3.0) { currentScore = "13.0 分"; currentLevelIdx = 7; }
                else if (diff > 3.0 && diff <= 3.5) { currentScore = "13.5 分"; currentLevelIdx = 8; }
                else if (diff > 3.5 && diff <= 4.0) { currentScore = "14.0 分"; currentLevelIdx = 9; }
                else if (diff > 4.0 && diff <= 4.5) { currentScore = "14.5 分"; currentLevelIdx = 10; }
                else if (diff > 4.5) { currentScore = "15.0 分"; currentLevelIdx = 11; }
            }

            // 4. 更新右上角狀態面板視覺效果
            const statusBox = document.getElementById('status_box');
            const statusText = document.getElementById('status_text');
            const scoreText = document.getElementById('score_text');

            if (currentScore === "不合格") {
                statusBox.className = "p-4 rounded-xl text-center flex flex-col items-center justify-center space-y-1 bg-red-50 text-red-700 border border-red-200";
                statusText.innerText = "不合格";
                scoreText.innerText = "承諾共負比高於基準值，不得成為合格申請人";
            } else {
                statusBox.className = "p-4 rounded-xl text-center flex flex-col items-center justify-center space-y-1 bg-emerald-50 text-emerald-800 border border-emerald-200";
                statusText.innerText = currentScore;
                statusText.className = "text-3xl font-black text-emerald-700";
                scoreText.innerText = `${currentStatus}，差距值 (N - R) 為 ${(N - R).toFixed(2)}%`;
            }

            // 5. 動態渲染下方的表格並高亮顯示落點
            const tableData = [
                { score: "不合格", cond: `R > N`, currentCond: `R > ${N.toFixed(2)}%`, desc: "高於基準值，不得成為本案合格申請人、最優申請人或次優申請人" },
                { score: "10", cond: `R = N`, currentCond: `R = ${N.toFixed(2)}%`, desc: "承諾值等於基準值" },
                { score: "10.5", cond: `0 < N - R ≦ 0.5%`, currentCond: `${(N-0.5).toFixed(2)}% ≦ R < ${N.toFixed(2)}%`, desc: "0 < N - R ≦ 0.5" },
                { score: "11.0", cond: `0.5 < N - R ≦ 1.0%`, currentCond: `${(N-1.0).toFixed(2)}% ≦ R < ${(N-0.5).toFixed(2)}%`, desc: "0.5 < N - R ≦ 1.0" },
                { score: "11.5", cond: `1.0 < N - R ≦ 1.5%`, currentCond: `${(N-1.5).toFixed(2)}% ≦ R < ${(N-1.0).toFixed(2)}%`, desc: "1.0 < N - R ≦ 1.5" },
                { score: "12.0", cond: `1.5 < N - R ≦ 2.0%`, currentCond: `${(N-2.0).toFixed(2)}% ≦ R < ${(N-1.5).toFixed(2)}%`, desc: "1.5 < N - R ≦ 2.0" },
                { score: "12.5", cond: `2.0 < N - R ≦ 2.5%`, currentCond: `${(N-2.5).toFixed(2)}% ≦ R < ${(N-2.0).toFixed(2)}%`, desc: "2.0 < N - R ≦ 2.5" },
                { score: "13.0", cond: `2.5 < N - R ≦ 3.0%`, currentCond: `${(N-3.0).toFixed(2)}% ≦ R < ${(N-2.5).toFixed(2)}%`, desc: "2.5 < N - R ≦ 3.0" },
                { score: "13.5", cond: `3.0 < N - R ≦ 3.5%`, currentCond: `${(N-3.5).toFixed(2)}% ≦ R < ${(N-3.0).toFixed(2)}%`, desc: "3.0 < N - R ≦ 3.5" },
                { score: "14.0", cond: `3.5 < N - R ≦ 4.0%`, currentCond: `${(N-4.0).toFixed(2)}% ≦ R < ${(N-3.5).toFixed(2)}%`, desc: "3.5 < N - R ≦ 4.0" },
                { score: "14.5", cond: `4.0 < N - R ≦ 4.5%`, currentCond: `${(N-4.5).toFixed(2)}% ≦ R < ${(N-4.0).toFixed(2)}%`, desc: "4.0 < N - R ≦ 4.5" },
                { score: "15.0", cond: `N - R > 4.5%`, currentCond: `R < ${(N-4.5).toFixed(2)}%`, desc: "N - R > 4.5" }
            ];

            let html = "";
            tableData.forEach((row, idx) => {
                const isCurrent = (idx === currentLevelIdx);
                // 根據是否為當前落點給予不同的深藍色高亮樣式
                const rowClass = isCurrent 
                    ? "bg-blue-600 text-white font-bold shadow-sm transition-all" 
                    : (idx === 0 ? "bg-red-50/50 text-red-700" : "hover:bg-slate-50 text-slate-700");
                
                const scoreClass = isCurrent ? "text-white text-base" : "font-semibold text-slate-900";

                html += `
                    <tr class="${rowClass}">
                        <td class="px-4 py-2.5 ${scoreClass}">${row.score} ${isCurrent ? '★' : ''}</td>
                        <td class="px-4 py-2.5 font-mono">${row.cond}</td>
                        <td class="px-4 py-2.5 font-mono">${row.currentCond}</td>
                        <td class="px-4 py-2.5 text-xs opacity-90">${row.desc} ${isCurrent ? ' (本案落點區域)' : ''}</td>
                    </tr>
                `;
            });

            document.getElementById('table_body').innerHTML = html;
        }

        // 頁面載入時先執行一次初始化
        window.onload = calculateAll;
    </script>
</body>
</html>
