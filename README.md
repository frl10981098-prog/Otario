<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Teste Psicodélico de Percepção</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body, html {
            width: 100%;
            height: 100%;
            background-color: #000;
            color: #fff;
            font-family: 'Courier New', Courier, monospace;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        #intro {
            text-align: center;
            padding: 20px;
        }

        .btn {
            padding: 15px 40px;
            font-size: 20px;
            background-color: #8b0000;
            color: white;
            border: 2px solid #ff0000;
            cursor: pointer;
            margin-top: 20px;
            text-transform: uppercase;
            letter-spacing: 2px;
            transition: 0.3s;
        }

        .btn:hover {
            background-color: #ff0000;
            box-shadow: 0 0 15px #ff0000;
        }

        /* Container do Jumpscare */
        #jumpscare-container {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background-color: black;
            z-index: 9999;
        }

        #jumpscare-img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            filter: contrast(200%) brightness(80%);
        }

        /* Efeito de flash estroboscópico e tremor extremo */
        .scare-effects {
            animation: tremor 0.05s infinite, strobe 0.08s infinite alternate;
        }

        @keyframes tremor {
            0% { transform: translate(10px, 10px) scale(1.1) rotate(1deg); }
            25% { transform: translate(-10px, -15px) scale(1.15) rotate(-2deg); }
            50% { transform: translate(-15px, 10px) scale(1.05) rotate(2deg); }
            75% { transform: translate(15px, -10px) scale(1.2) rotate(-1deg); }
            100% { transform: translate(-10px, 10px) scale(1.1) rotate(0deg); }
        }

        @keyframes strobe {
            0% { filter: invert(0%) contrast(300%); }
            100% { filter: invert(100%) contrast(300%); }
        }
    </style>
</head>
<body>

    <div id="intro">
        <h1>Teste de Illusão de Ótica</h1>
        <p>Aproxime o rosto da tela e fixe o olhar no centro...</p>
        <button class="btn" onclick="iniciarSusto()">Iniciar Teste</button>
    </div>

    <div id="jumpscare-container">
        <!-- Substitua o link abaixo pela URL de uma imagem de horror de sua preferência -->
        <img id="jumpscare-img" src="https://i.imgur.com/2pL3vN4.jpg" alt="BOO!">
    </div>

    <script>
        function iniciarSusto() {
            document.getElementById('intro').style.display = 'none';

            // Solicita tela cheia imediatamente
            if (document.documentElement.requestFullscreen) {
                document.documentElement.requestFullscreen();
            }

            // Tempo de espera aleatório (entre 2 e 4 segundos) para aumentar a tensão
            var tempoEspera = Math.floor(Math.random() * 2000) + 2000;

            setTimeout(function() {
                var container = document.getElementById('jumpscare-container');
                
                // Ativa os efeitos visuais
                container.style.display = 'block';
                container.classList.add('scare-effects');

                // Gerador de Som Estourado (Earrape) usando Web Audio API
                var audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                
                // Criar ruído branco
                var bufferSize = audioCtx.sampleRate * 3; // 3 segundos de som
                var buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
                var data = buffer.getChannelData(0);
                for (var i = 0; i < bufferSize; i++) {
                    data[i] = Math.random() * 2 - 1;
                }

                var noise = audioCtx.createBufferSource();
                noise.buffer = buffer;

                // Oscilador de frequência agudo (grito sintético)
                var osc = audioCtx.createOscillator();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(1200, audioCtx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(400, audioCtx.currentTime + 3);

                // Nó de Ganho com saturação/distorção extrema
                var gainNode = audioCtx.createGain();
                gainNode.gain.setValueAtTime(15, audioCtx.currentTime); // Volume extremamente estourado

                // Conectar os áudios
                noise.connect(gainNode);
                osc.connect(gainNode);
                gainNode.connect(audioCtx.destination);

                // Tocar o som
                noise.start();
                osc.start();
            }, tempoEspera);
        }
    </script>
</body>
</html>

