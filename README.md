<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kinetic Typography - Có Chắc Yêu Là Đây</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: #050505;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        #lyrics {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: #ffffff;
            font-size: 3rem;
            font-weight: bold;
            text-align: center;
            text-shadow: 0 0 20px rgba(255, 255, 255, 0.5);
            pointer-events: none;
            transition: opacity 0.3s ease;
            width: 80%;
        }
        #canvas-container {
            width: 100vw;
            height: 100vh;
        }
    </style>
</head>
<body>

    <div id="lyrics"></div>
    <div id="canvas-container"></div>

    <!-- Nhúng thư viện Three.js từ CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        // --- 1. Cấu hình Scene, Camera, Renderer ---
        const container = document.getElementById('canvas-container');
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 50;

        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(window.devicePixelRatio);
        container.appendChild(renderer.domElement);

        // --- 2. Tạo hiệu ứng hạt (Particles) ---
        const particleCount = 1000;
        const geometry = new THREE.BufferGeometry();
        const positions = new Float32Array(particleCount * 3);
        const colors = new Float32Array(particleCount * 3);

        const colorOptions = [
            new THREE.Color(0xff5733),
            new THREE.Color(0x33ff57),
            new THREE.Color(0x3357ff),
            new THREE.Color(0xf3ff33),
            new THREE.Color(0xff33f3)
        ];

        for (let i = 0; i < particleCount * 3; i += 3) {
            positions[i] = (Math.random() - 0.5) * 100;
            positions[i + 1] = (Math.random() - 0.5) * 100;
            positions[i + 2] = (Math.random() - 0.5) * 100;

            const randomColor = colorOptions[Math.floor(Math.random() * colorOptions.length)];
            colors[i] = randomColor.r;
            colors[i + 1] = randomColor.g;
            colors[i + 2] = randomColor.b;
        }

        geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
        geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));

        const material = new THREE.PointsMaterial({
            size: 0.8,
            vertexColors: true,
            transparent: true,
            opacity: 0.8
        });

        const particles = new THREE.Points(geometry, material);
        scene.add(particles);

        // --- 3. Lời bài hát "Có Chắc Yêu Là Đây" (Sơn Tùng M-TP) ---
        const lyricsData = [
            { text: "Em là ai từ đâu bước đến đây...", time: 0 },
            { text: "Dịu dàng chân mây...", time: 3 },
            { text: "Tựa như ánh nắng sớm ấm áp...", time: 6 },
            { text: "Xua tan đi màn đêm...", time: 9 },
            { text: "Liệu rằng anh có đang mơ...", time: 12 },
            { text: "Hay đang sống trong giấc mơ?", time: 15 },
            { text: "Vì nụ cười em trao...", time: 18 },
            { text: "Làm anh ngẩn ngơ suốt đời...", time: 21 },
            { text: "Oh baby, oh baby...", time: 25 },
            { text: "Có chắc yêu là đây?", time: 28 },
            { text: "Chạm nhẹ vào tim anh...", time: 32 },
            { text: "Khiến anh đắm say bình yên...", time: 35 }
        ];

        const lyricsElement = document.getElementById('lyrics');
        let startTime = Date.now();

        function updateLyrics() {
            let elapsed = (Date.now() - startTime) / 1000;
            
            // Lặp lại hiệu ứng sau khi hết bài (khoảng 40 giây)
            if (elapsed > 40) {
                startTime = Date.now();
                elapsed = 0;
            }

            let currentLine = lyricsData[0].text;
            for (let i = 0; i < lyricsData.length; i++) {
                if (elapsed >= lyricsData[i].time) {
                    currentLine = lyricsData[i].text;
                }
            }
            lyricsElement.innerText = currentLine;
        }

        // --- 4. Animation Loop ---
        function animate() {
            requestAnimationFrame(animate);

            // Xoay nhẹ hệ thống hạt tạo chiều sâu 3D
            particles.rotation.x += 0.001;
            particles.rotation.y += 0.002;

            updateLyrics();

            renderer.render(scene, camera);
        }

        animate();

        // --- Xử lý khi thay đổi kích thước màn hình ---
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
