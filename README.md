<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AttractX</title>
    <style>
        :root {
            --bg: #000;
            --fg: #fff;
            --accent: rgba(255, 255, 255, 0.4);
        }

        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            background-color: var(--bg);
            font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* --- THE DESIGNED BACKGROUND --- */
        .bg-layer {
            position: absolute;
            width: 150vw;
            height: 150vh;
            background: conic-gradient(
                from 0deg at 50% 50%, 
                #000 0deg 90deg, 
                #080808 90deg 180deg, 
                #000 180deg 270deg, 
                #080808 270deg 360deg
            );
            animation: rotate 25s linear infinite;
            z-index: 1;
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        /* --- CONTENT WRAPPER --- */
        .content {
            position: relative;
            z-index: 10;
            text-align: center;
        }

        h1 {
            color: #fff;
            font-size: clamp(3rem, 15vw, 10rem);
            font-weight: 800;
            letter-spacing: -0.05em;
            margin: 0;
            text-transform: uppercase;
            mix-blend-mode: difference;
        }

        .subtitle {
            color: #fff;
            font-size: 0.8rem;
            letter-spacing: 1.2em;
            text-transform: uppercase;
            margin-top: 10px;
            opacity: 0.5;
            padding-left: 1.2em;
            mix-blend-mode: difference;
        }

        /* --- MINIMALIST SUBSCRIPTION FORM --- */
        .subscribe-form {
            margin-top: 60px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            transition: all 0.5s ease;
        }

        .input-group {
            display: flex;
            border-bottom: 1px solid var(--accent);
            padding: 5px;
            transition: border-color 0.4s;
        }

        .input-group:focus-within {
            border-bottom: 1px solid #fff;
        }

        input[type="email"] {
            background: transparent;
            border: none;
            color: #fff;
            padding: 10px;
            width: 260px;
            font-family: inherit;
            outline: none;
            font-size: 0.85rem;
            text-align: center;
            letter-spacing: 0.1em;
        }

        button {
            background: transparent;
            border: 1px solid #fff;
            color: #fff;
            padding: 12px 35px;
            font-size: 0.65rem;
            text-transform: uppercase;
            letter-spacing: 0.3em;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            margin-top: 10px;
        }

        button:hover {
            background: #fff;
            color: #000;
            transform: translateY(-2px);
        }

        /* Success Message */
        #status-msg {
            display: none;
            color: #fff;
            font-size: 0.7rem;
            letter-spacing: 0.3em;
            text-transform: uppercase;
            margin-top: 30px;
            animation: fadeInUp 0.8s forwards;
        }

        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- DECORATIVE ELEMENTS --- */
        .cursor-follower {
            position: fixed;
            width: 500px;
            height: 500px;
            background: radial-gradient(circle, rgba(255,255,255,0.08) 0%, transparent 70%);
            border-radius: 50%;
            pointer-events: none;
            z-index: 5;
            transform: translate(-50%, -50%);
        }

        .frame {
            position: fixed;
            top: 40px; bottom: 40px; left: 40px; right: 40px;
            border: 1px solid rgba(255,255,255,0.03);
            pointer-events: none;
            z-index: 20;
        }

    </style>
</head>
<body>

    <div class="bg-layer" id="bg"></div>
    <div class="cursor-follower" id="cursor"></div>
    <div class="frame"></div>

    <div class="content">
        <h1 id="title">AttractX</h1>
        <p class="subtitle">Launching soon</p>

        <!-- Formspree Integration -->
        <form id="attractx-form" class="subscribe-form" action="https://formspree.io/f/xqearooe" method="POST">
            <div class="input-group" id="input-area">
                <input type="email" name="email" placeholder="YOUR EMAIL ADDRESS" required>
            </div>
            <button type="submit" id="sub-btn">Subscribe</button>
        </form>
        
        <p id="status-msg">We will attract you soon.</p>
    </div>

    <script>
        // Background & Cursor Interaction
        const cursor = document.getElementById('cursor');
        const bg = document.getElementById('bg');
        const title = document.getElementById('title');

        document.addEventListener('mousemove', (e) => {
            cursor.style.left = e.clientX + 'px';
            cursor.style.top = e.clientY + 'px';

            const moveX = (e.clientX - window.innerWidth / 2) * 0.03;
            const moveY = (e.clientY - window.innerHeight / 2) * 0.03;
            bg.style.transform = `translate(${moveX}px, ${moveY}px)`;
            
            // Subtle letter spacing attraction
            const dist = Math.abs(e.clientX - window.innerWidth / 2) / (window.innerWidth / 2);
            title.style.letterSpacing = `${-0.05 + (dist * 0.08)}em`;
        });

        // AJAX Form Submission (No redirect)
        const form = document.getElementById("attractx-form");
        const status = document.getElementById("status-msg");
        const inputArea = document.getElementById("input-area");
        const btn = document.getElementById("sub-btn");

        async function handleSubmit(event) {
            event.preventDefault();
            btn.innerHTML = "SENDING...";
            const data = new FormData(event.target);
            
            fetch(event.target.action, {
                method: form.method,
                body: data,
                headers: { 'Accept': 'application/json' }
            }).then(response => {
                if (response.ok) {
                    inputArea.style.display = "none";
                    btn.style.display = "none";
                    status.style.display = "block";
                    form.reset();
                } else {
                    btn.innerHTML = "ERROR. TRY AGAIN";
                }
            }).catch(error => {
                btn.innerHTML = "ERROR. TRY AGAIN";
            });
        }
        form.addEventListener("submit", handleSubmit);
    </script>
</body>
</html>
