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

        /* 玻璃質感卡片 */
        .glass-card {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(10px);
            border: 1px solid var(--border-color);
            border-radius: 1.25rem;
            padding: 1.25rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
        }

        .section-title {
            border-left: 4px solid var(--primary);
            padding-left: 10px;
            font-weight: 500;
            font-size: 1.1rem;
            margin-bottom: 1rem;
        }

        /* 數字調節器優化 */
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

        /* 輸入框優化 */
        input[type="number"] {
            -moz-appearance: textfield;
            background: #FFF;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 6px 4px;
            text-align: center;
            font-size: 1rem;
        }

        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        /* 自定義按鈕樣式 */
        .option-radio:checked + label {
            background-color: var(--primary);
            color: white;
            border-color: var(--primary);
            box-shadow: 0 4px 10px rgba(212, 195, 172, 0.3);
        }

        .option-label {
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 10px 4px;
            border: 1px solid var(--border-color);
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 0.875rem;
            background: white;
            min-height: 44px; /* 手指易點擊高度 */
        }

        /* 底部結帳區 */
        .sticky-footer {
            background-color: var(--text-main);
            color: var(--bg-color);
            padding: 1.5rem;
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
            padding: 10px 20px;
            border-radius: 9999px;
            font-weight: 500;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            transition: transform 0.2s;
        }

        .btn-copy:active {
            transform: scale(0.95);
        }

        .toast {
            visibility: hidden;
            position: fixed;
            bottom: 120px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 12px 24px;
            border-radius: 30px;
            z-index: 100;
            font-size: 0.9rem;
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
    <div class="max-w-xl mx-auto px-4 pt-8 pb-4">
        <!-- Header -->
        <header class="text-center mb-8">
            <h1 class="brand-font text-5xl text-[#8E7F6D] mb-1">CE Nail</h1>
            <p class="text-[10px] tracking-[0.3em] text-[#A89A8A] mb-6">PROFESSIONAL NAIL STUDIO</p>
            
            <div class="flex justify-center gap-3">
                <input type="radio" name="artist" id="artist-yu" value="小魚" class="option-radio hidden" checked onchange="calculate()">
                <label for="artist-yu" class="px-6 py-2 border border-[#E9E2D7] rounded-full cursor-pointer text-sm bg-white">美甲師：小魚</label>
                
                <input type="radio" name="artist" id="artist-ru" value="小如" class="option-radio hidden" onchange="calculate()">
                <label for="artist-ru" class="px-6 py-2 border border-[#E9E2D7] rounded-full cursor-pointer text-sm bg-white">美甲師：小如</label>
            </div>
        </header>

        <div class="space-y-5">
            <!-- 基礎底色 -->
            <div class="glass-card">
                <h2 class="section-title">基礎底色 (每指)</h2>
                <div class="grid grid-cols-1 gap-y-4">
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

            <!-- 疊加項目 -->
            <div class="glass-card">
                <h2 class="section-title">疊加項目 (每指)</h2>
                <div class="grid grid-cols-1 gap-y-4">
                    <div class="flex justify-between items-center"><label>遮游離線 ($5)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('add-line', -1)">-</button><span class="count-display" id="add-line-val" data-price="5" data-label="疊加-遮游離線" data-unit="指">0</span><button class="stepper-btn" onclick="step('add-line', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>實色/跳色 ($10)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('add-color', -1)">-</button><span class="count-display" id="add-color-val" data-price="10" data-label="疊加-實色/跳色" data-unit="指">0</span><button class="stepper-btn" onclick="step('add-color', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>漸變 ($40)</label>
                        <div class="flex items-center gap-3"><button class="stepper-btn" onclick="step('add-grad', -1)">-</button><span class="count-display" id="add-grad-val" data-price="40" data-label="疊加-漸變" data-unit="指">0</span><button class="stepper-btn" onclick="step('add-grad', 1)">+</button></div>
                    </div>
                    <div class="flex justify-between items-center"><label>鏡面 ($50)</label>
                        <
