<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitMatch: Smash or Pass</title>
    <style>
        :root {
            --bg-color: #0d1117;
            --card-bg: #161b22;
            --border-color: #30363d;
            --text-color: #c9d1d9;
            --smash-color: #238636;
            --pass-color: #da3633;
        }

        body {
            margin: 0;
            padding: 0;
            background-color: var(--bg-color);
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
            color: var(--text-color);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            overflow: hidden;
        }

        header {
            margin-bottom: 20px;
            text-align: center;
        }

        header h1 {
            margin: 0;
            font-size: 2rem;
            color: #f0f6fc;
        }

        header p {
            margin: 5px 0 0;
            color: #8b949e;
        }

        .swiper-container {
            position: relative;
            width: 340px;
            height: 480px;
            perspective: 1000px;
        }

        .card-stack {
            position: relative;
            width: 100%;
            height: 100%;
        }

        .card {
            position: absolute;
            width: 100%;
            height: 100%;
            background-color: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            box-shadow: 0 8px 24px rgba(0,0,0,0.5);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            user-select: none;
            cursor: grab;
            transform-origin: center bottom;
            transition: transform 0.3s ease, opacity 0.3s ease;
        }

        .card:active {
            cursor: grabbing;
        }

        .card-header {
            padding: 16px;
            border-bottom: 1px solid var(--border-color);
            background-color: #1f242c;
        }

        .repo-name {
            font-weight: 600;
            font-size: 1.1rem;
            color: #58a6ff;
        }

        .repo-lang {
            font-size: 0.8rem;
            color: #8b949e;
            margin-top: 4px;
            display: inline-block;
            padding: 2px 8px;
            background: #21262d;
            border-radius: 12px;
            border: 1px solid var(--border-color);
        }

        .card-body {
            padding: 16px;
            flex-grow: 1;
            overflow: auto;
            font-family: ui-monospace, SFMono-Regular, SF Pro Text, Menlo, Monaco, Consolas, monospace;
            font-size: 0.85rem;
            background-color: #090d16;
            color: #e6edf3;
            white-space: pre;
        }

        /* Badge overlays for swiping feedback */
        .badge {
            position: absolute;
            top: 30px;
            padding: 10px 20px;
            border-num: 4px solid;
            border-radius: 8px;
            font-size: 2rem;
            font-weight: bold;
            text-transform: uppercase;
            opacity: 0;
            transform: rotate(-15deg);
            pointer-events: none;
            transition: opacity 0.1s ease;
        }

        .badge.smash {
            right: 30px;
            color: var(--smash-color);
            border: 4px solid var(--smash-color);
            transform: rotate(15deg);
        }

        .badge.pass {
            left: 30px;
            color: var(--pass-color);
            border: 4px solid var(--pass-color);
            transform: rotate(-15deg);
        }

        .controls {
            margin-top: 25px;
            display: flex;
            gap: 20px;
        }

        .btn {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            border: none;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            transition: transform 0.1s ease;
        }

        .btn:active {
            transform: scale(0.9);
        }

        .btn-pass {
            background-color: #21262d;
            color: var(--pass-color);
            border: