<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AttractX</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #050505;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
        }

        canvas {
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1;
        }

        .container {
            position: relative;
            z-index: 10;
            text-align: center;
            pointer-events: none; /* Allows mouse to interact with background behind text */
        }

        h1 {
            color: #ffffff;
            font-size: clamp(3rem, 12vw, 8rem);
            font-weight: 900;
            letter-spacing: -0.04em;
            margin: 0;
            line-height: 1;
        }

        h1 span {
            color: transparent;
            -webkit-text-stroke: 1px rgba(255, 255, 255, 0.4);
        }

        p {
            color: #888;
            font-size: 0.75rem;
            text-transform: uppercase;
            letter-spacing: 0.8em;
            margin-top: 20px;
            font-weight: 300;
        }
    </style>
</head>
<body>

    <canvas id="canvas"></canvas>

    <div class="container">
        <h1>Attract<span>X</span></h1>
        <p>Launching soon</p>
    </div>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');

        let particles = [];
        const mouse = { x: null, y: null, radius: 150 };

        window.addEventListener('mousemove', (event) => {
            mouse.x = event.x;
            mouse.y = event.y;
        });

        function resize() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            init();
        }

        class Particle {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.baseX = x;
                this.baseY = y;
                this.size = Math.random() * 1.5 + 0.5;
                this.density = (Math.random() * 30) + 1;
            }

            draw() {
                ctx.fillStyle = 'rgba(255, 255, 255, 0.3)';
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.closePath();
                ctx.fill();
            }

            update() {
                let dx = mouse.x - this.x;
                let dy = mouse.y - this.y;
                let distance = Math.sqrt(dx * dx + dy * dy);
                let forceDirectionX = dx / distance;
                let forceDirectionY = dy / distance;
                let maxDistance = mouse.radius;
                let force = (maxDistance - distance) / maxDistance;
                let directionX = forceDirectionX * force * this.density;
                let directionY = forceDirectionY * force * this.density;

                if (distance < mouse.radius) {
                    this.x += directionX;
                    this.y += directionY;
                } else {
                    if (this.x !== this.baseX) {
                        let dx = this.x - this.baseX;
                        this.x -= dx / 20;
                    }
                    if (this.y !== this.baseY) {
                        let dy = this.y - this.baseY;
                        this.y -= dy / 20;
                    }
                }
            }
        }

        function init() {
            particles = [];
            // Create a grid of particles
            const gap = 30;
            for (let y = 0; y < canvas.height; y += gap) {
                for (let x = 0; x < canvas.width; x += gap) {
                    particles.push(new Particle(x, y));
                }
            }
        }

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < particles.length; i++) {
                particles[i].draw();
                particles[i].update();
            }
            requestAnimationFrame(animate);
        }

        window.addEventListener('resize', resize);
        resize();
        animate();
    </script>
</body>
</html>
