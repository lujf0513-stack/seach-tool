<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Infinity Search</title>
    <style>
        :root {
            --apple-blue: #007AFF;
            --glass-bg: rgba(255, 255, 255, 0.7);
            --glass-border: rgba(255, 255, 255, 0.4);
        }

        body {
            margin: 0;
            height: 100vh;
            font-family: -apple-system, "SF Pro Display", sans-serif;
            /* 氛围感背景：流体渐变 */
            background: radial-gradient(at 0% 0%, #e2eafc 0, transparent 50%), 
                        radial-gradient(at 100% 0%, #f1f1f1 0, transparent 50%), 
                        radial-gradient(at 100% 100%, #e8f1ff 0, transparent 50%), 
                        radial-gradient(at 0% 100%, #ffffff 0, transparent 50%);
            background-attachment: fixed;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            padding-top: 18vh;
            overflow: hidden;
            -webkit-font-smoothing: antialiased;
        }

        .container {
            width: 90%;
            max-width: 580px;
            text-align: center;
            z-index: 1;
        }

        /* 标题：极致高级感 */
        h1 {
            font-size: 64px;
            font-weight: 200;
            margin: 0 0 40px 0;
            letter-spacing: -3px;
            color: #1d1d1f;
            opacity: 0.9;
            /* 只有 Apple 风格会用的微妙字母动画 */
            animation: tracking-in 0.8s cubic-bezier(0.215, 0.610, 0.355, 1.000) both;
        }

        @keyframes tracking-in {
            0% { letter-spacing: -10px; opacity: 0; filter: blur(10px); }
            100% { letter-spacing: -3px; opacity: 0.9; filter: blur(0); }
        }

        /* 搜索框：毛玻璃效果 */
        .search-wrapper {
            background: var(--glass-bg);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            padding: 8px;
            display: flex;
            box-shadow: 0 20px 40px rgba(0,0,0,0.05);
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            margin-bottom: 20px;
        }

        .search-wrapper:focus-within {
            transform: scale(1.02);
            box-shadow: 0 30px 60px rgba(0,0,0,0.1);
            background: rgba(255, 255, 255, 0.9);
        }

        input {
            flex: 1;
            border: none;
            background: transparent;
            padding: 15px 20px;
            font-size: 19px;
            outline: none;
            color: #1d1d1f;
        }

        button {
            background: #1d1d1f;
            color: white;
            border: none;
            padding: 0 28px;
            border-radius: 18px;
            font-size: 15px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s;
        }

        button:hover {
            background: #000;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        /* 引擎选择：滑动底块效果 */
        .engine-tabs {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            background: rgba(0, 0, 0, 0.05);
            border-radius: 16px;
            padding: 4px;
            position: relative;
            gap: 4px;
        }

        .engine-tabs label {
            padding: 12px 0;
            font-size: 14px;
            font-weight: 500;
            color: #86868b;
            cursor: pointer;
            z-index: 1;
            transition: color 0.3s;
            border-radius: 12px;
            text-align: center;
        }

        .engine-tabs input { display: none; }

        /* 选中态：白色滑块效果 */
        .engine-tabs input:checked + label {
            color: #1d1d1f;
            background: #ffffff;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .engine-tabs label:hover:not(input:checked + label) {
            color: #1d1d1f;
        }

        .hint {
            margin-top: 30px;
            font-size: 13px;
            color: #86868b;
            font-weight: 400;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Search</h1>

    <div class="search-wrapper">
        <input type="text" id="q" placeholder="有什么可以帮到你？" autocomplete="off" autofocus>
        <button onclick="s()">搜索</button>
    </div>

    <div class="engine-tabs">
        <input type="radio" name="e" id="bd" value="bd">
        <label for="bd">Baidu</label>

        <input type="radio" name="e" id="gg" value="gg">
        <label for="gg">Google</label>

        <input type="radio" name="e" id="gpt" value="gpt">
        <label for="gpt">ChatGPT</label>

        <input type="radio" name="e" id="gm" value="gm">
        <label for="gm">Gemini</label>
    </div>

    <div class="hint">Shift + Enter 开启新窗口查询</div>
</div>

<script>
    const KEY = 'infinite_pref';
    
    window.onload = () => {
        const saved = localStorage.getItem(KEY) || 'gg';
        document.getElementById(saved).checked = true;
    }

    function s() {
        const txt = document.getElementById('q').value.trim();
        if(!txt) return;

        const engine = document.querySelector('input[name="e"]:checked').id;
        localStorage.setItem(KEY, engine);

        const urls = {
            bd: `https://www.baidu.com/s?wd=${encodeURIComponent(txt)}`,
            gg: `https://www.google.com/search?q=${encodeURIComponent(txt)}`,
            gpt: `https://chatgpt.com/?q=${encodeURIComponent(txt)}`,
            gm: `https://gemini.google.com/app?q=${encodeURIComponent(txt)}`
        };

        window.open(urls[engine], '_blank');
    }

    document.getElementById('q').onkeydown = (e) => {
        if(e.key === 'Enter') s();
    }
</script>

</body>
</html>
