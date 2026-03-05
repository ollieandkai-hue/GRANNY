<!DOCTYPE html>
<html lang="en-us">
<head>
    <meta charset="UTF-8">
    <title>Granny – Fullscreen Enabled</title>
    <style>
        body { margin: 0; overflow: hidden; background: black; }
        #gameContainer { width: 100%; height: 100%; position: relative; }
        #fullscreenBtn, #pointerLockBtn {
            position: absolute;
            top: 10px;
            left: 10px;
            z-index: 1000;
            padding: 8px 12px;
            background: #ffa600;
            color: black;
            font-weight: bold;
            border: none;
            cursor: pointer;
        }
        #pointerLockBtn { left: 130px; }
        #loadingBar {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 400px;
            height: 20px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
        }
        #progressFull {
            height: 100%;
            width: 0%;
            background: #4caf50;
            border-radius: 10px;
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <canvas id="unityCanvas" style="width:100%; height:100%;"></canvas>
        <button id="fullscreenBtn">Fullscreen</button>
        <button id="pointerLockBtn">Lock Mouse</button>
        <div id="loadingBar"><div id="progressFull"></div></div>
    </div>

    <script>
        const buildUrl = "https://cdn.jsdelivr.net/gh/gru6nny/ohd@main/Build";
        const loaderUrl = buildUrl + "/Granny.loader.js";

        const canvas = document.getElementById("unityCanvas");
        const loadingBarFull = document.getElementById("progressFull");
        const loadingBar = document.getElementById("loadingBar");

        let myGameInstance = null;

        function updateProgress(progress) {
            loadingBarFull.style.width = (progress * 100) + "%";
        }

        async function loadUnityGame() {
            const script = document.createElement("script");
            script.src = loaderUrl;
            script.onload = () => {
                createUnityInstance(canvas, {
                    dataUrl: buildUrl + "/Granny.data",
                    frameworkUrl: buildUrl + "/Granny.framework.js",
                    codeUrl: buildUrl + "/Granny.wasm",
                    streamingAssetsUrl: buildUrl + "/StreamingAssets",
                    companyName: "Anastasia Kazantseva",
                    productName: "Granny",
                    productVersion: "1.0",
                    showBanner: (msg) => console.log(msg),
                }, updateProgress).then((unityInstance) => {
                    myGameInstance = unityInstance;
                    loadingBar.style.display = "none";
                }).catch(err => console.error(err));
            };
            document.body.appendChild(script);
        }

        loadUnityGame();

        document.getElementById("fullscreenBtn").addEventListener("click", () => {
            if (canvas.requestFullscreen) canvas.requestFullscreen();
            else if (canvas.webkitRequestFullscreen) canvas.webkitRequestFullscreen();
            else if (canvas.mozRequestFullScreen) canvas.mozRequestFullScreen();
        });

        document.getElementById("pointerLockBtn").addEventListener("click", () => {
            if (canvas.requestPointerLock) canvas.requestPointerLock();
            else if (canvas.mozRequestPointerLock) canvas.mozRequestPointerLock();
            else if (canvas.webkitRequestPointerLock) canvas.webkitRequestPointerLock();
        });
    </script>
</body>
</html>
