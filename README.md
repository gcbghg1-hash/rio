!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>هدية 🎁</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&family=Great+Vibes&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Cairo', sans-serif; }
        body { background: #0c0c0c; overflow: hidden; height: 100vh; display: flex; justify-content: center; align-items: center; color: white; }
        
        /* الشاشات */
        .screen { position: absolute; top: 0; left: 0; width: 100%; height: 100%; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 20px; text-align: center; opacity: 0; pointer-events: none; transition: opacity 0.5s; overflow-y: auto; background: #fdfbf7; }
        .screen.active { opacity: 1; pointer-events: auto; }

        /* شاشة القفل */
        .lock-image-container { width: 250px; height: 200px; border-radius: 15px; overflow: hidden; margin-bottom: 20px; box-shadow: 0 8px 20px rgba(0,0,0,0.15); border: 3px solid #fff; background: #fff; display: flex; align-items: center; justify-content: center; }
        .lock-image-container img { width: 100%; height: 100%; object-fit: cover; }
        
        input { padding: 12px 20px; border-radius: 50px; border: 2px solid #e0d5c8; text-align: center; font-size: 16px; outline: none; width: 80%; max-width: 250px; margin-bottom: 15px; background: transparent; color: #333; }
        .btn { background: #b93b3b; color: white; border: none; padding: 14px 40px; border-radius: 50px; font-size: 18px; font-weight: bold; cursor: pointer; box-shadow: 0 4px 15px rgba(185,59,59,0.4); }
        
        /* مؤشر الكتابة */
        .cursor { display: inline-block; width: 3px; background-color: #b93b3b; margin-right: 3px; animation: blink 0.7s infinite; vertical-align: middle; }
        @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }

        /* شاشة المقدمة */
        .intro-text { font-size: 18px; line-height: 1.8; color: #4a3b32; max-width: 320px; min-height: 120px; margin-bottom: 20px; font-weight: bold; white-space: pre-wrap; display: inline; }
        .btn-next { background: #f2d5d5; color: #a02a2a; border: none; padding: 12px 30px; border-radius: 50px; font-size: 16px; font-weight: bold; cursor: pointer; margin-top: 20px; }
        
        /* شاشة المعرض طويلة وتسمح بالتقليب الراسي */
        #gallery { 
            display: block; 
            overflow-y: scroll; 
            -webkit-overflow-scrolling: touch;
            padding: 20px 10px 120px 10px; 
            background: #0f0f0f;
            height: 100vh;
        }
        
        /* كارت صورة طويل بأبعاد 520px لتظهر الصورة كاملة وطويلة */
        .tiktok-card { 
            position: relative;
            width: 100%; 
            max-width: 340px; 
            height: 520px; /* ارتفاع ثابت وطويل */
            margin: 0 auto 35px auto; 
            border-radius: 20px; 
            overflow: hidden; 
            box-shadow: 0 10px 25px rgba(0,0,0,0.7); 
            background: #1e1e1e;
        }
        
        .tiktok-card img { 
            width: 100%; 
            height: 100%; 
            object-fit: cover; 
            display: block; 
        }

        /* الطبقة المظلمة أسفل الصورة للنص */
        .tiktok-overlay {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            padding: 50px 20px 20px 20px;
            background: linear-gradient(transparent, rgba(0,0,0,0.9));
            color: #fff;
            text-align: right;
        }

        .caption { 
            font-size: 17px; 
            font-weight: bold; 
            line-height: 1.5; 
            text-shadow: 0 2px 4px rgba(0,0,0,0.8);
        }

        /* الرسالة النهائية */
        .final-text { font-size: 20px; line-height: 2; color: #4a3b32; font-weight: bold; max-width: 300px; white-space: pre-wrap; display: inline; }
        .lovely { font-family: 'Great Vibes', cursive; font-size: 35px; color: #d46b6b; margin-top: 15px; opacity: 0; transition: 1s; }

        /* القلوب */
        .heart { position: absolute; pointer-events: none; font-size: 20px; animation: floatUp 1.5s ease-out forwards; z-index: 999; }
        @keyframes floatUp { 0% { transform: translateY(0); opacity: 1; } 100% { transform: translateY(-150px); opacity: 0; } }
        .bg-heart { position: absolute; bottom: -50px; font-size: 18px; color: #8b5a2b; opacity: 0.5; animation: floatBg 6s linear infinite; pointer-events: none; }
        @keyframes floatBg { 0% { transform: translateY(0); opacity: 0.5; } 100% { transform: translateY(-110vh); opacity: 0; } }

        /* شريط الأغنية */
        #music-player { position: fixed; bottom: 15px; left: 50%; transform: translateX(-50%) translateY(100px); width: 90%; max-width: 340px; background: rgba(255,255,255,0.95); border-radius: 50px; padding: 10px 18px; display: flex; align-items: center; justify-content: space-between; box-shadow: 0 5px 20px rgba(0,0,0,0.3); z-index: 100; transition: 0.5s; border: 1px solid #eee; }
        #music-player.show { transform: translateX(-50%) translateY(0); }
        .music-info { font-size: 14px; font-weight: bold; color: #4a3b32; }
        .play-btn { background: #f2d5d5; border: none; width: 38px; height: 38px; border-radius: 50%; cursor: pointer; color: #a02a2a; font-size: 14px; display: flex; align-items: center; justify-content: center; }
    </style>
</head>
<body>

    <!-- شاشة القفل -->
    <div id="lock" class="screen active">
        <div class="lock-image-container">
            <img src="https://images.unsplash.com/photo-1518199266791-5375a83190b7?w=600" alt="LOVE">
        </div>
        <input type="text" id="passwordInput" placeholder="اكتب كلمة السر" autocomplete="off">
        <button class="btn" onclick="unlock()">💖 Unlock 💖</button>
    </div>

    <!-- شاشة المقدمة -->
    <div id="intro" class="screen">
        <div style="text-align: center;">
            <span class="intro-text" id="introText"></span><span id="introCursor" class="cursor">&nbsp;</span>
        </div>
        <button class="btn-next" onclick="goToGallery()">next →</button>
    </div>

    <!-- شاشة المعرض (صور طويلة بارتفاع 520px وتقليب مستمر) -->
    <div id="gallery" class="screen">
        
        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1518199266791-5375a83190b7?w=800" alt="1">
            <div class="tiktok-overlay"><div class="caption">بأحبك وبأموت فيكي.. أنتي دنيتي كلها ❤️</div></div>
        </div>

        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1529333166437-7750a6dd5a70?w=800" alt="2">
            <div class="tiktok-overlay"><div class="caption">وجودك في حياتي ينور عتمة أيامي ✨❤️</div></div>
        </div>

        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1494774157365-9e04c6720e47?w=800" alt="3">
            <div class="tiktok-overlay"><div class="caption">إيديكي في إيدي.. وعمري كله فداكي 🙈❤️</div></div>
        </div>

        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1516589178581-6cd7833ae3b2?w=800" alt="4">
            <div class="tiktok-overlay"><div class="caption">في حضنك بتبدأ حياتي وبينساني العالم كله 💖</div></div>
        </div>

        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1522098543979-ffc7f79a56c4?w=800" alt="5">
            <div class="tiktok-overlay"><div class="caption">أنتي حبي الأول والأخير والوحيد في قلبي 🌹</div></div>
        </div>

        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1515934751635-c81c6bc9a2d8?w=800" alt="6">
            <div class="tiktok-overlay"><div class="caption">ضحكتك هي السر اللي بيسعد يومي كله 😂❤️</div></div>
        </div>

        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1511632765486-a01980e01a18?w=800" alt="7">
            <div class="tiktok-overlay"><div class="caption">كل لحظة معاكي تسوى الدنيا وما فيها ☕❤️</div></div>
        </div>

        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1518895949257-7621c3c786d7?w=800" alt="8">
            <div class="tiktok-overlay"><div class="caption">بعشق التفاصيل اللي بتجمعنا سوا 😍💖</div></div>
        </div>

        <div class="tiktok-card">
            <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=800" alt="9">
            <div class="tiktok-overlay"><div class="caption">ربنا يديمك في أيامي ونفضل مع بعض دايماً 🌸❤️</div></div>
        </div>

        <div style="text-align: center; margin-top: 10px; margin-bottom: 40px;">
            <button class="btn" onclick="goToMessage()" style="width: 80%; max-width: 300px;">التالي ←</button>
        </div>
    </div>

    <!-- شاشة الرسالة النهائية -->
    <div id="message" class="screen">
        <div style="text-align: center;">
            <span class="final-text" id="finalText"></span><span id="finalCursor" class="cursor">&nbsp;</span>
        </div>
        <div class="lovely">Lovely</div>
    </div>

    <!-- شريط الأغنية -->
    <div id="music-player">
        <div class="music-info">🎵 وانت معايا ❤️</div>
        <button class="play-btn" id="playBtn" onclick="toggleMusic()">⏸</button>
    </div>

    <!-- ملف الصوت المباشر -->
    <audio id="bgMusic" loop preload="auto">
        <source src="https://files.catbox.moe/mevccy.mp3" type="audio/mpeg">
    </audio>

    <script>
        const music = document.getElementById('bgMusic');
        let isPlaying = false;

        function playAudio() {
            if (!isPlaying) {
                music.play().then(() => {
                    isPlaying = true;
                    document.getElementById('playBtn').innerHTML = '⏸';
                    document.getElementById('music-player').classList.add('show');
                }).catch(() => {});
            }
        }

        window.addEventListener('load', playAudio);
        document.addEventListener('click', playAudio, { once: true });
        document.addEventListener('touchstart', playAudio, { once: true });

        function unlock() {
            playAudio();
            if (document.getElementById('passwordInput').value.trim() === 'بحبك') {
                document.getElementById('lock').classList.remove('active');
                document.getElementById('intro').classList.add('active');
                startIntro();
            } else {
                alert('كلمة السر غلط! اكتب: بحبك');
            }
        }

        function typeWriter(element, cursorElement, text, speed, callback) {
            let i = 0;
            element.innerHTML = '';
            cursorElement.style.display = 'inline-block';
            
            function type() {
                if (i < text.length) {
                    element.innerHTML += text.charAt(i);
                    i++;
                    setTimeout(type, speed);
                } else {
                    cursorElement.style.display = 'none';
                    if (callback) callback();
                }
            }
            type();
        }

        function startIntro() {
            typeWriter(
                document.getElementById('introText'), 
                document.getElementById('introCursor'), 
                "حبيت اعمل هديه جديده وعايزين نملاه صورنا بقا بس قبل اي حاجه عايز تعرفي اني بحبك وربنا يخليكي ليا يا قلبي ويلا بقا انبهري ❤️😂", 
                60
            );
        }

        function goToGallery() {
            document.getElementById('intro').classList.remove('active');
            document.getElementById('gallery').classList.add('active');
        }

        function goToMessage() {
            document.getElementById('gallery').classList.remove('active');
            document.getElementById('message').classList.add('active');
            
            typeWriter(
                document.getElementById('finalText'), 
                document.getElementById('finalCursor'), 
                "بحبك اوي\n\nربنا يخليكي ليا\n\nوعايزين بقى صور كتير لينا متتنسيش 😂❤️", 
                80, 
                () => {
                    setTimeout(() => { document.querySelector('.lovely').style.opacity = '1'; }, 500);
                }
            );
        }

        function toggleMusic() {
            if (isPlaying) { 
                music.pause(); 
                document.getElementById('playBtn').innerHTML = '▶'; 
                isPlaying = false;
            } else { 
                music.play().then(() => {
                    document.getElementById('playBtn').innerHTML = '⏸'; 
                    isPlaying = true;
                });
            }
        }

        document.body.addEventListener('click', function(e) {
            if (e.target.tagName === 'INPUT' || e.target.tagName === 'BUTTON') return;
            const hearts = ['❤️', '💖', '💗', '💕', '🤎', '🤍'];
            const heart = document.createElement('div');
            heart.className = 'heart';
            heart.innerHTML = hearts[Math.floor(Math.random() * hearts.length)];
            heart.style.left = e.pageX + 'px';
            heart.style.top = e.pageY + 'px';
            heart.style.filter = `hue-rotate(${Math.random() * 360}deg)`;
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 1500);
        });

        setInterval(() => {
            const heart = document.createElement('div');
            heart.className = 'bg-heart';
            heart.innerHTML = '🤎';
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.animationDuration = (Math.random() * 3 + 4) + 's';
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 7000);
        }, 800);
    </script>
</body>
</html>
