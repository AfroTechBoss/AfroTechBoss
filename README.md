<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AfroTechBoss Terminal</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'JetBrains Mono', monospace;
            background: #0a0a0a;
            color: #00ff00;
            overflow-x: hidden;
            min-height: 100vh;
        }
        
        .terminal {
            background: linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 100%);
            min-height: 100vh;
            padding: 20px;
            position: relative;
        }
        
        .terminal::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: 
                radial-gradient(ellipse at center, transparent 0%, rgba(0, 255, 0, 0.03) 100%),
                repeating-linear-gradient(
                    0deg,
                    transparent,
                    transparent 2px,
                    rgba(0, 255, 0, 0.03) 2px,
                    rgba(0, 255, 0, 0.03) 4px
                );
            pointer-events: none;
            animation: scanlines 0.1s linear infinite;
        }
        
        @keyframes scanlines {
            0% { transform: translateY(0px); }
            100% { transform: translateY(4px); }
        }
        
        .terminal-header {
            border-bottom: 2px solid #00ff00;
            padding-bottom: 20px;
            margin-bottom: 30px;
            text-align: center;
        }
        
        .terminal-title {
            font-size: 2.5rem;
            font-weight: 700;
            text-shadow: 0 0 10px #00ff00;
            animation: glow 2s ease-in-out infinite alternate;
        }
        
        @keyframes glow {
            from { text-shadow: 0 0 10px #00ff00, 0 0 20px #00ff00; }
            to { text-shadow: 0 0 20px #00ff00, 0 0 30px #00ff00; }
        }
        
        .prompt {
            color: #00ffff;
            font-weight: 700;
        }
        
        .command {
            color: #ffff00;
        }
        
        .output {
            margin: 10px 0;
            line-height: 1.6;
        }
        
        .section {
            margin: 30px 0;
            padding: 20px;
            border: 1px solid #333;
            border-radius: 5px;
            background: rgba(0, 0, 0, 0.3);
        }
        
        .ascii-art {
            color: #00ffff;
            font-size: 0.8rem;
            white-space: pre;
            margin: 20px 0;
            text-align: center;
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin: 20px 0;
        }
        
        .stat-box {
            border: 1px solid #00ff00;
            padding: 15px;
            background: rgba(0, 255, 0, 0.05);
        }
        
        .progress-bar {
            width: 100%;
            height: 20px;
            background: #333;
            margin: 10px 0;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #00ff00, #00ffff);
            animation: progress-glow 2s ease-in-out infinite alternate;
            transition: width 2s ease;
        }
        
        @keyframes progress-glow {
            from { box-shadow: 0 0 5px #00ff00; }
            to { box-shadow: 0 0 15px #00ff00, 0 0 25px #00ffff; }
        }
        
        .contact-links a {
            color: #ffff00;
            text-decoration: none;
            transition: all 0.3s ease;
        }
        
        .contact-links a:hover {
            color: #00ffff;
            text-shadow: 0 0 5px #00ffff;
        }
        
        .graph {
            height: 200px;
            margin: 20px 0;
            position: relative;
            border: 1px solid #333;
            background: rgba(0, 0, 0, 0.3);
        }
        
        .graph-bars {
            display: flex;
            align-items: end;
            height: 100%;
            padding: 20px;
            gap: 10px;
        }
        
        .bar {
            flex: 1;
            background: linear-gradient(to top, #00ff00, #00ffff);
            min-height: 10px;
            opacity: 0;
            animation: bar-grow 1s ease forwards;
        }
        
        .bar:nth-child(1) { animation-delay: 0.1s; }
        .bar:nth-child(2) { animation-delay: 0.2s; }
        .bar:nth-child(3) { animation-delay: 0.3s; }
        .bar:nth-child(4) { animation-delay: 0.4s; }
        .bar:nth-child(5) { animation-delay: 0.5s; }
        .bar:nth-child(6) { animation-delay: 0.6s; }
        .bar:nth-child(7) { animation-delay: 0.7s; }
        
        @keyframes bar-grow {
            from { opacity: 0; height: 0; }
            to { opacity: 1; }
        }
        
        .typing-effect {
            overflow: hidden;
            white-space: nowrap;
            animation: typing 3s steps(50, end), blink 0.75s step-end infinite;
        }
        
        @keyframes typing {
            from { width: 0; }
            to { width: 100%; }
        }
        
        @keyframes blink {
            from, to { border-color: transparent; }
            50% { border-color: #00ff00; }
        }
        
        .matrix-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            opacity: 0.1;
            z-index: -1;
        }
        
        @media (max-width: 768px) {
            .terminal {
                padding: 10px;
            }
            
            .terminal-title {
                font-size: 1.8rem;
            }
            
            .ascii-art {
                font-size: 0.6rem;
            }
            
            .stats-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="matrix-bg" id="matrixBg"></div>
    
    <div class="terminal">
        <div class="terminal-header">
            <div class="terminal-title">AfroTechBoss Terminal</div>
            <div class="ascii-art">
    ╔══════════════════════════════════════╗
    ║  █████╗ ███████╗██████╗  ██████╗     ║
    ║ ██╔══██╗██╔════╝██╔══██╗██╔═══██╗    ║
    ║ ███████║█████╗  ██████╔╝██║   ██║    ║
    ║ ██╔══██║██╔══╝  ██╔══██╗██║   ██║    ║
    ║ ██║  ██║██║     ██║  ██║╚██████╔╝    ║
    ║ ╚═╝  ╚═╝╚═╝     ╚═╝  ╚═╝ ╚═════╝     ║
    ║                                      ║
    ║     ████████╗███████╗ ██████╗██╗  ██╗║
    ║     ╚══██╔══╝██╔════╝██╔════╝██║  ██║║
    ║        ██║   █████╗  ██║     ███████║║
    ║        ██║   ██╔══╝  ██║     ██╔══██║║
    ║        ██║   ███████╗╚██████╗██║  ██║║
    ║        ╚═╝   ╚══════╝ ╚═════╝╚═╝  ╚═╝║
    ╚══════════════════════════════════════╝
            </div>
        </div>

        <div class="section">
            <div class="output">
                <span class="prompt">afrotechboss@github:~$</span> <span class="command">whoami</span>
            </div>
            <div class="output typing-effect" style="animation-delay: 0.5s;">
                > Full Stack Developer & Tech Enthusiast
            </div>
            <div class="output">
                > Passionate about building innovative solutions
            </div>
            <div class="output">
                > Always learning, always coding
            </div>
        </div>

        <div class="section">
            <div class="output">
                <span class="prompt">afrotechboss@github:~$</span> <span class="command">ls -la /dev/skills</span>
            </div>
            <div class="stats-grid">
                <div class="stat-box">
                    <div><strong>JavaScript/TypeScript</strong></div>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 90%;"></div>
                    </div>
                    <div>██████████ 90%</div>
                </div>
                <div class="stat-box">
                    <div><strong>React/Next.js</strong></div>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 85%;"></div>
                    </div>
                    <div>█████████░ 85%</div>
                </div>
                <div class="stat-box">
                    <div><strong>Node.js/Express</strong></div>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 80%;"></div>
                    </div>
                    <div>████████░░ 80%</div>
                </div>
                <div class="stat-box">
                    <div><strong>Python</strong></div>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 75%;"></div>
                    </div>
                    <div>███████░░░ 75%</div>
                </div>
            </div>
        </div>

        <div class="section">
            <div class="output">
                <span class="prompt">afrotechboss@github:~$</span> <span class="command">git log --oneline --graph</span>
            </div>
            <div class="graph">
                <div style="position: absolute; top: 10px; left: 10px; font-size: 0.8rem;">Contribution Activity</div>
                <div class="graph-bars">
                    <div class="bar" style="height: 60%;"></div>
                    <div class="bar" style="height: 80%;"></div>
                    <div class="bar" style="height: 45%;"></div>
                    <div class="bar" style="height: 90%;"></div>
                    <div class="bar" style="height: 70%;"></div>
                    <div class="bar" style="height: 95%;"></div>
                    <div class="bar" style="height: 85%;"></div>
                </div>
                <div style="position: absolute; bottom: 5px; left: 50%; transform: translateX(-50%); font-size: 0.7rem;">
                    Mon | Tue | Wed | Thu | Fri | Sat | Sun
                </div>
            </div>
        </div>

        <div class="section">
            <div class="output">
                <span class="prompt">afrotechboss@github:~$</span> <span class="command">cat /proc/projects</span>
            </div>
            <div class="output">
                <div style="margin: 15px 0;">
                    <strong style="color: #00ffff;">📱 Mobile Apps</strong><br>
                    └── React Native applications with cutting-edge features
                </div>
                <div style="margin: 15px 0;">
                    <strong style="color: #00ffff;">🌐 Web Platforms</strong><br>
                    └── Full-stack web applications using modern frameworks
                </div>
                <div style="margin: 15px 0;">
                    <strong style="color: #00ffff;">🤖 AI/ML Projects</strong><br>
                    └── Machine learning models and AI-powered solutions
                </div>
                <div style="margin: 15px 0;">
                    <strong style="color: #00ffff;">⚡ Performance Optimization</strong><br>
                    └── Code optimization and system performance improvements
                </div>
            </div>
        </div>

        <div class="section">
            <div class="output">
                <span class="prompt">afrotechboss@github:~$</span> <span class="command">curl -s api.github.com/users/afrotechboss/stats</span>
            </div>
            <div class="stats-grid">
                <div class="stat-box">
                    <div><strong>📊 Repositories</strong></div>
                    <div style="font-size: 2rem; color: #00ffff;">42+</div>
                    <div>Public & Private projects</div>
                </div>
                <div class="stat-box">
                    <div><strong>⭐ Total Stars</strong></div>
                    <div style="font-size: 2rem; color: #00ffff;">250+</div>
                    <div>Community appreciation</div>
                </div>
                <div class="stat-box">
                    <div><strong>🔄 Contributions</strong></div>
                    <div style="font-size: 2rem; color: #00ffff;">1000+</div>
                    <div>This year</div>
                </div>
                <div class="stat-box">
                    <div><strong>👥 Followers</strong></div>
                    <div style="font-size: 2rem; color: #00ffff;">150+</div>
                    <div>Growing community</div>
                </div>
            </div>
        </div>

        <div class="section">
            <div class="output">
                <span class="prompt">afrotechboss@github:~$</span> <span class="command">find /etc/contact -type f</span>
            </div>
            <div class="contact-links output">
                <div style="margin: 10px 0;">
                    <strong>📧 Email:</strong> 
                    <a href="mailto:afrotechboss@yahoo.com">afrotechboss@yahoo.com</a> | 
                    <a href="mailto:chidileozoemena@gmail.com">chidileozoemena@gmail.com</a>
                </div>
                <div style="margin: 10px 0;">
                    <strong>🐙 GitHub:</strong> 
                    <a href="https://github.com/AfroTechBoss" target="_blank">github.com/AfroTechBoss</a>
                </div>
                <div style="margin: 10px 0;">
                    <strong>🐦 X (Twitter):</strong> 
                    <a href="https://x.com/0xAfroTechBoss" target="_blank">@0xAfroTechBoss</a>
                </div>
                <div style="margin: 10px 0;">
                    <strong>💼 LinkedIn:</strong> 
                    <a href="https://www.linkedin.com/in/chidile-ozoemena-293688231/" target="_blank">linkedin.com/in/chidile-ozoemena</a>
                </div>
            </div>
        </div>

        <div class="section">
            <div class="output">
                <span class="prompt">afrotechboss@github:~$</span> <span class="command">echo "Ready to collaborate and build amazing things together!"</span>
            </div>
            <div class="output" style="color: #ffff00; font-size: 1.2rem; text-align: center; margin: 20px 0;">
                🚀 Ready to collaborate and build amazing things together! 🚀
            </div>
            <div class="output" style="text-align: center;">
                <span style="animation: glow 2s ease-in-out infinite alternate;">
                    > Let's connect and create something extraordinary! <
                </span>
            </div>
        </div>

        <div class="section">
            <div class="output">
                <span class="prompt">afrotechboss@github:~$</span> <span style="animation: blink 1s infinite;">█</span>
            </div>
        </div>
    </div>

    <script>
        // Matrix digital rain effect
        function createMatrixEffect() {
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            canvas.classList.add('matrix-bg');
            document.getElementById('matrixBg').appendChild(canvas);

            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;

            const chars = 'アァカサタナハマヤャラワガザダバパイィキシチニヒミリヰギジヂビピウゥクスツヌフムユュルグズブヅプエェケセテネヘメレヱゲゼデベペオォコソトノホモヨョロヲゴゾドボポヴッン0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ';
            const charArray = chars.split('');

            const fontSize = 14;
            const columns = canvas.width / fontSize;

            const drops = [];
            for (let x = 0; x < columns; x++) {
                drops[x] = 1;
            }

            function draw() {
                ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
                ctx.fillRect(0, 0, canvas.width, canvas.height);

                ctx.fillStyle = '#00ff00';
                ctx.font = fontSize + 'px monospace';

                for (let i = 0; i < drops.length; i++) {
                    const text = charArray[Math.floor(Math.random() * charArray.length)];
                    ctx.fillText(text, i * fontSize, drops[i] * fontSize);

                    if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                        drops[i] = 0;
                    }
                    drops[i]++;
                }
            }

            setInterval(draw, 35);
        }

        // Initialize effects
        document.addEventListener('DOMContentLoaded', function() {
            createMatrixEffect();
            
            // Add some interactive terminal behavior
            document.addEventListener('click', function(e) {
                // Create ripple effect
                const ripple = document.createElement('div');
                ripple.style.position = 'fixed';
                ripple.style.left = e.clientX + 'px';
                ripple.style.top = e.clientY + 'px';
                ripple.style.width = '0px';
                ripple.style.height = '0px';
                ripple.style.borderRadius = '50%';
                ripple.style.background = 'rgba(0, 255, 0, 0.3)';
                ripple.style.transform = 'translate(-50%, -50%)';
                ripple.style.pointerEvents = 'none';
                ripple.style.zIndex = '1000';
                document.body.appendChild(ripple);

                // Animate ripple
                ripple.animate([
                    { width: '0px', height: '0px', opacity: 1 },
                    { width: '100px', height: '100px', opacity: 0 }
                ], {
                    duration: 600,
                    easing: 'ease-out'
                }).onfinish = () => ripple.remove();
            });
        });

        // Responsive canvas resize
        window.addEventListener('resize', function() {
            location.reload(); // Simple way to handle resize for matrix effect
        });
    </script>
</body>
</html>
