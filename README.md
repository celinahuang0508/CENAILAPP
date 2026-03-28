<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CE Nail 美甲計價計算機</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Noto+Sans+TC:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #D4C3AC;
            --primary-dark: #C1AD94;
            --bg-color: #FDFBF7;
            --text-main: #5D544C;
            --border-color: #E9E2D7;
        }

        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }

        .brand-font {
            font-family: 'Dancing Script', cursive;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            border: 1px solid var(--border-color);
            border-radius: 1.25rem;
            padding: 1.25rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
            margin-bottom: 1.25rem;
        }

        .section-title {
            border-left: 4px solid var(--primary);
            padding-left: 10px;
            font-weight: 500;
            font-size: 1rem;
            margin-bottom: 1rem;
        }

        .stepper-btn {
            width: 36px;
            height: 36px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            background-color: #F3EEE7;
            border: 1px solid var(--border-color);
            transition: all 0.2s;
            font-size: 1.2rem;
            color: var(--text-main);
            touch-action: manipulation;
        }

        .stepper-btn:active {
            transform: scale(0.9);
            background-color: var(--primary);
            color: white;
        }

        .count-display {
            min-width: 32px;
            text-align: center;
            font-weight: 600;
            font-size: 1rem;
        }

        label, .option-label, span {
            font-size: 0.9rem !important;
        }

        input[type="number"] {
            -moz-appearance: textfield;
            background: #FFF;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 6px 4px;
            text-align: center;
            font-size: 0.9rem;
        }

        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        .option-radio:checked + label {
            background-color: var(--primary);
            color: white;
            border-color: var(--primary);
        }

        .option-label {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 10px 4px;
            border: 1px solid var(--border-color);
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s;
            background: white;
            min-height: 44px;
        }

        .sticky-footer {
            background-color: var(--text-main);
            color: var(--bg-color);
            padding: 1.25rem;
            border-radius: 1.5rem 1.5rem 0 0;
            box-shadow: 0 -10px 25px -5px rgba(0, 0, 0, 0.1);
            position: sticky;
            bottom: 0;
            left: 0;
            right: 0;
            z-index: 50;
        }

        .btn-copy {
            background-color: var(--primary);
            color: white;
            padding: 8px 16px;
            border-radius: 9999px;
            font-weight: 500;
            font-size: 0.9rem;
        }

        .toast {
            visibility: hidden;
            position: fixed;
            bottom: 220px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 12px 24px;
            border-radius: 30px;
            z-index: 100;
        }

        .toast.show {
            visibility: visible;
            animation: fadeInUp 0.3s forwards, fadeOutDown 0.3s 2.7s forwards;
        }

        @keyframes fadeInUp { from { opacity: 0; transform: translate(-50%, 20px); } to { opacity: 1; transform: translate(-50%, 0); } }
        @keyframes fadeOutDown { from { opacity: 1; transform: translate(-50%, 0); } to { opacity: 0; transform: translate(-50%, 20px); } }
    </style>
</head>
<body class="pb-0">
    <div class="max-w-xl mx-auto px-4 pt-6 pb-4">
        <header class="text-center mb-6">
            <h1 class="brand-font text-5xl text-[#8E7F6D] mb-1">CE Nail</h1>
            <p class="text-[10px] tracking-[0.3em] text-[#A89A8A] mb-4 uppercase">Professional Nail Studio</p>
            
            <div class="flex justify-center gap-2">
                <input type="radio" name="artist" id="artist-yu" value="小魚" class="option-radio hidden" checked onchange="calculate()">
                <label for="artist-yu" class="px-4 py-1.5 border border-[#E9E2D7] rounded-full cursor-pointer bg-white">美甲師：小魚</label>
                
                <input type="radio" name="artist" id="artist-ru" value="小如" class="option-radio hidden" onchange="calculate()">
                <label for="artist-ru" class="px-4 py-1.5 border border-[#E9E2D7] rounded-full cursor-pointer bg-white">美甲師：小如</label>
            </div>
        </header>

        <div class="space-y-4">
            <!-- 1. 基礎底色 -->
            <div class="glass-card">
                <h2 class="section-title">基礎底色 (每指)</h2>
                <div class="space-y-3">
                    <div class="flex justify-between items-center"><label>單色 ($80)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('base-single', -1)">-</button><span class="count-display" id="base-single-val" data-price="80" data-label="基礎單色" data-unit="指">0</span><button class="stepper-btn" onclick="step('base-single', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>貓眼 ($90)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('base-cat', -1)">-</button><span class="count-display" id="base-cat-val" data-price="90" data-label="貓眼" data-unit="指">0</span><button class="stepper-btn" onclick="step('base-cat', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>漸變 ($120)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('base-grad', -1)">-</button><span class="count-display" id="base-grad-val" data-price="120" data-label="漸變" data-unit="指">0</span><button class="stepper-btn" onclick="step('base-grad', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>鏡面 ($130)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('base-mirror', -1)">-</button><span class="count-display" id="base-mirror-val" data-price="130" data-label="鏡面" data-unit="指">0</span><button class="stepper-btn" onclick="step('base-mirror', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>法式 ($160)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('base-french', -1)">-</button><span class="count-display" id="base-french-val" data-price="160" data-label="法式" data-unit="指">0</span><button class="stepper-btn" onclick="step('base-french', 1)">+</button></div>
                    </div>
                </div>
            </div>

            <!-- 2. 疊加項目 -->
            <div class="glass-card">
                <h2 class="section-title">疊加項目 (每指)</h2>
                <div class="space-y-3">
                    <div class="flex justify-between items-center"><label>遮游離線 ($5)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('add-line', -1)">-</button><span class="count-display" id="add-line-val" data-price="5" data-label="疊加-遮游離線" data-unit="指">0</span><button class="stepper-btn" onclick="step('add-line', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>實色/跳色 ($10)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('add-color', -1)">-</button><span class="count-display" id="add-color-val" data-price="10" data-label="疊加-實色/跳色" data-unit="指">0</span><button class="stepper-btn" onclick="step('add-color', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>疊加漸變 ($40)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('add-grad', -1)">-</button><span class="count-display" id="add-grad-val" data-price="40" data-label="疊加-漸變" data-unit="指">0</span><button class="stepper-btn" onclick="step('add-grad', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>疊加鏡面 ($50)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('add-mirror', -1)">-</button><span class="count-display" id="add-mirror-val" data-price="50" data-label="疊加-鏡面" data-unit="指">0</span><button class="stepper-btn" onclick="step('add-mirror', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>疊加法式 ($80)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('add-french', -1)">-</button><span class="count-display" id="add-french-val" data-price="80" data-label="疊加-法式" data-unit="指">0</span><button class="stepper-btn" onclick="step('add-french', 1)">+</button></div>
                    </div>
                </div>
            </div>

            <!-- 3. 造型設計 -->
            <div class="glass-card">
                <h2 class="section-title">造型設計</h2>
                <div class="space-y-4">
                    <div class="flex flex-col gap-2">
                        <label>手繪/造型</label>
                        <div class="flex justify-between items-center">
                            <div class="flex items-center gap-2">
                                <span class="text-xs">單價$</span><input type="number" class="w-20" id="custom-style-price" placeholder="價格" onchange="calculate()">
                            </div>
                            <div class="flex items-center gap-3">
                                <button class="stepper-btn" onclick="step('style-draw', -1)">-</button>
                                <span class="count-display" id="style-draw-val">0</span>
                                <button class="stepper-btn" onclick="step('style-draw', 1)">+</button>
                            </div>
                        </div>
                    </div>
                    <div class="flex flex-col gap-2">
                        <label>排鑽/鑽球</label>
                        <div class="flex justify-between items-center">
                            <div class="flex items-center gap-2">
                                <span class="text-xs">單價$</span><input type="number" class="w-20" id="diamond-style-price" placeholder="價格" onchange="calculate()">
                            </div>
                            <div class="flex items-center gap-3">
                                <button class="stepper-btn" onclick="step('style-diamond', -1)">-</button>
                                <span class="count-display" id="style-diamond-val">0</span>
                                <button class="stepper-btn" onclick="step('style-diamond', 1)">+</button>
                            </div>
                        </div>
                    </div>
                    <div class="flex justify-between items-center border-t pt-4">
                        <label>大鑽/飾品 ($50/指)</label>
                        <div class="flex items-center gap-3">
                            <button class="stepper-btn" onclick="step('style-acc', -1)">-</button>
                            <span class="count-display" id="style-acc-val" data-price="50" data-label="大鑽/飾品" data-unit="指">0</span>
                            <button class="stepper-btn" onclick="step('style-acc', 1)">+</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 4. 延長/補甲 -->
            <div class="glass-card">
                <h2 class="section-title">甲片延長 / 補甲</h2>
                <div class="space-y-3">
                    <div class="flex justify-between items-center"><label>短甲 ($90)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('ext-short', -1)">-</button><span class="count-display ext-count" id="ext-short-val" data-price="90" data-label="延長-短甲" data-unit="指">0</span><button class="stepper-btn" onclick="step('ext-short', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>中長甲 ($120)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('ext-mid', -1)">-</button><span class="count-display ext-count" id="ext-mid-val" data-price="120" data-label="延長-中長甲" data-unit="指">0</span><button class="stepper-btn" onclick="step('ext-mid', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>超長甲 ($150)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('ext-long', -1)">-</button><span class="count-display ext-count" id="ext-long-val" data-price="150" data-label="延長-超長甲" data-unit="指">0</span><button class="stepper-btn" onclick="step('ext-long', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>歐美水管 ($180)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('ext-pipe', -1)">-</button><span class="count-display ext-count" id="ext-pipe-val" data-price="180" data-label="延長-歐美水管" data-unit="指">0</span><button class="stepper-btn" onclick="step('ext-pipe', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center border-t pt-3"><label>裂甲補甲 ($50/指)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('fix-crack', -1)">-</button><span class="count-display" id="fix-crack-val" data-price="50" data-label="裂甲補甲" data-unit="指">0</span><button class="stepper-btn" onclick="step('fix-crack', 1)">+</button></div>
                    </div>
                </div>
            </div>

            <!-- 5. 卸甲處理 -->
            <div class="glass-card">
                <h2 class="section-title">卸甲處理</h2>
                <div class="grid grid-cols-1 gap-2 mb-4">
                    <input type="radio" name="removal" value="100" class="hidden option-radio" id="rem-4weeks" onclick="toggleRadio(this)">
                    <label for="rem-4weeks" class="option-label font-medium">本店四周內續作卸甲 ($100)</label>

                    <input type="radio" name="removal" value="200" class="hidden option-radio" id="rem-home" onclick="toggleRadio(this)">
                    <label for="rem-home" class="option-label font-medium">本店續作卸甲 ($200)</label>

                    <input type="radio" name="removal" value="300" class="hidden option-radio" id="rem-other" onclick="toggleRadio(this)">
                    <label for="rem-other" class="option-label font-medium">他店續作卸甲 ($300)</label>
                    
                    <!-- 隱藏的預設選項 -->
                    <input type="radio" name="removal" value="0" class="hidden" id="rem-none" checked>
                </div>
                <div class="flex flex-col gap-3 border-t pt-4">
                    <label class="flex items-center gap-2 cursor-pointer">
                        <input type="checkbox" id="thick-base" class="w-5 h-5 accent-[#D4C3AC]" onchange="calculate()"> 建構過厚 (+$50)
                    </label>
                    <div class="flex justify-between items-center">
                        <label>鑽飾卸除 ($20/指)</label>
                        <div class="flex items-center gap-3">
                            <button class="stepper-btn" onclick="step('rem-diamond', -1)">-</button>
                            <span class="count-display" id="rem-diamond-val" data-price="20" data-label="鑽飾卸除" data-unit="指">0</span><button class="stepper-btn" onclick="step('rem-diamond', 1)">+</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 6. 優惠折扣 -->
            <div class="glass-card">
                <h2 class="section-title">優惠折扣</h2>
                <div class="grid grid-cols-3 gap-2 mb-4">
                    <input type="radio" name="discount-base" id="rate-95" value="0.95" class="hidden option-radio" onclick="toggleRadio(this)">
                    <label for="rate-95" class="option-label">新客 95折</label>
                    
                    <input type="radio" name="discount-base" id="rate-9" value="0.9" class="hidden option-radio" onclick="toggleRadio(this)">
                    <label for="rate-9" class="option-label">壽星 9折</label>
                    
                    <input type="radio" name="discount-base" id="rate-85" value="0.85" class="hidden option-radio" onclick="toggleRadio(this)">
                    <label for="rate-85" class="option-label">集點 85折</label>

                    <!-- 隱藏的預設選項 -->
                    <input type="radio" name="discount-base" id="rate-none" value="1" class="hidden" checked>
                </div>
                <div class="grid grid-cols-2 gap-2">
                    <input type="checkbox" id="promo-intro" class="hidden option-radio" onchange="calculate()">
                    <label for="promo-intro" class="option-label">介紹優惠 折100</label>

                    <input type="checkbox" id="promo-ig" class="hidden option-radio" onchange="calculate()">
                    <label for="promo-ig" class="option-label">IG打卡 折50</label>
                </div>
            </div>

            <div class="h-12"></div>
        </div>
    </div>

    <!-- 底部固定區 -->
    <div class="sticky-footer">
        <div class="max-w-xl mx-auto">
            <!-- 支付方式選擇 -->
            <div class="flex justify-center gap-3 mb-4">
                <input type="radio" name="pay-method" id="pay-cash" value="現金" class="option-radio hidden" checked onchange="calculate()">
                <label for="pay-cash" class="px-4 py-1.5 border border-white/20 rounded-full cursor-pointer text-xs">支付：現金</label>

                <input type="radio" name="pay-method" id="pay-bank" value="匯款" class="option-radio hidden" onchange="calculate()">
                <label for="pay-bank" class="px-4 py-1.5 border border-white/20 rounded-full cursor-pointer text-xs">支付：匯款</label>
            </div>

            <div class="flex justify-between items-end">
                <div>
                    <p class="text-[10px] opacity-60 mb-0.5 tracking-widest uppercase">Total Estimate</p>
                    <h3 class="text-4xl font-bold" id="total-price">$ 0</h3>
                </div>
                <button onclick="copyToClipboard()" class="btn-copy">複製報價明細</button>
            </div>
            <div id="summary-preview" class="text-[10px] opacity-50 border-t border-white/10 mt-3 pt-2 h-10 overflow-hidden leading-relaxed">
                尚未選擇項目
            </div>
        </div>
    </div>

    <div id="toast" class="toast">報價明細已複製！</div>

    <script>
        // 用來追蹤 Radio 狀態以實現「點擊第二次取消」
        let lastSelected = {
            'removal': 'rem-none',
            'discount-base': 'rate-none'
        };

        function toggleRadio(element) {
            const name = element.name;
            const currentId = element.id;
            const defaultId = name === 'removal' ? 'rem-none' : 'rate-none';

            if (lastSelected[name] === currentId) {
                // 如果點擊的是已經選中的，則切換回預設
                document.getElementById(defaultId).checked = true;
                lastSelected[name] = defaultId;
            } else {
                // 否則正常選取
                lastSelected[name] = currentId;
            }
            calculate();
        }

        function step(id, val) {
            const display = document.getElementById(id + '-val');
            let current = parseInt(display.innerText);
            current = Math.max(0, current + val);
            display.innerText = current;
            calculate();
        }

        function calculate() {
            let total = 0;
            let detail = [];
            
            const artist = document.querySelector('input[name="artist"]:checked').value;
            detail.push(`👩‍🎨 美甲師：${artist}`);

            // 基礎與疊加項目
            document.querySelectorAll('.count-display[data-price]').forEach(display => {
                const price = parseInt(display.getAttribute('data-price'));
                const count = parseInt(display.innerText);
                if (count > 0) {
                    const sub = price * count;
                    total += sub;
                    detail.push(`${display.getAttribute('data-label')} x${count}：$${sub}`);
                }
            });

            // 造型
            const sPrice = parseInt(document.getElementById('custom-style-price').value || 0);
            const sCount = parseInt(document.getElementById('style-draw-val').innerText);
            if (sCount > 0 && sPrice > 0) {
                const sub = sPrice * sCount;
                total += sub;
                detail.push(`造型/手繪 x${sCount}：$${sub}`);
            }

            const dPrice = parseInt(document.getElementById('diamond-style-price').value || 0);
            const dCount = parseInt(document.getElementById('style-diamond-val').innerText);
            if (dCount > 0 && dPrice > 0) {
                const sub = dPrice * dCount;
                total += sub;
                detail.push(`排鑽/鑽球 x${dCount}：$${sub}`);
            }

            // 十指全延優惠
            const totalExt = Array.from(document.querySelectorAll('.ext-count')).reduce((a, b) => a + parseInt(b.innerText), 0);
            if (totalExt >= 10) {
                total -= 100;
                detail.push(`✨ 十指全延優惠：-$100`);
            }

            // 卸甲
            const rem = document.querySelector('input[name="removal"]:checked');
            if (rem && rem.value !== "0") {
                let rPrice = parseInt(rem.value);
                let rText = rem.nextElementSibling.innerText.split(' (')[0];
                if (document.getElementById('thick-base').checked) {
                    rPrice += 50;
                    rText += " (含厚建構)";
                }
                total += rPrice;
                detail.push(`${rText}：$${rPrice}`);
            } else if (document.getElementById('thick-base').checked) {
                // 如果沒選卸甲但選了厚建構
                total += 50;
                detail.push(`建構過厚：$50`);
            }

            // 折扣
            if (document.getElementById('promo-intro').checked) { total -= 100; detail.push(`介紹優惠：-$100`); }
            if (document.getElementById('promo-ig').checked) { total -= 50; detail.push(`IG打卡優惠：-$50`); }

            const rateRadio = document.querySelector('input[name="discount-base"]:checked');
            if (rateRadio && rateRadio.value !== "1") {
                const rate = parseFloat(rateRadio.value);
                const beforeRate = total;
                total = Math.round(total * rate);
                detail.push(`${rateRadio.nextElementSibling.innerText}：-$${beforeRate - total}`);
            }

            // 支付方式
            const payMethod = document.querySelector('input[name="pay-method"]:checked').value;

            document.getElementById('total-price').innerText = `$ ${total}`;
            document.getElementById('summary-preview').innerHTML = detail.length > 1 ? detail.join(' | ') : "尚未選擇項目";
            
            window.finalDetail = detail.join('\n');
            window.finalTotal = total;
            window.finalPay = payMethod;
        }

        function copyToClipboard() {
            if (!window.finalDetail) return;
            const text = `✨ CE Nail 服務報價 ✨\n------------------\n${window.finalDetail}\n------------------\n💰 最終應付：$${window.finalTotal}\n💳 支付方式：${window.finalPay}\n\n感謝您的預約！`;
            const area = document.createElement('textarea');
            area.value = text;
            document.body.appendChild(area);
            area.select();
            document.execCommand('copy');
            document.body.removeChild(area);
            const toast = document.getElementById("toast");
            toast.classList.add("show");
            setTimeout(() => toast.classList.remove("show"), 3000);
        }

        calculate();
    </script>
</body>
</html>
