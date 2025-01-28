# Site-estudos-
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cronômetro de Estudo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f0f0f0;
        }

        .container {
            text-align: center;
            margin-top: 50px;
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        #timer {
            font-size: 80px;
            font-weight: bold;
            margin: 20px 0;
            color: #2c3e50;
        }

        .buttons button {
            font-size: 18px;
            padding: 10px 20px;
            margin: 5px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            color: white;
        }

        #startBtn {
            background-color: #27ae60;
        }

        #pauseBtn {
            background-color: #f1c40f;
        }

        #resetBtn {
            background-color: #e74c3c;
        }

        #status {
            font-size: 24px;
            margin-top: 20px;
            color: #34495e;
        }

        .notification {
            margin-top: 20px;
            padding: 10px;
            background-color: #3498db;
            color: white;
            border-radius: 5px;
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Cronômetro de Estudo</h1>
        <div id="timer">40:00</div>
        <div class="buttons">
            <button id="startBtn">Iniciar</button>
            <button id="pauseBtn">Pausar</button>
            <button id="resetBtn">Resetar</button>
        </div>
        <div id="status">Pronto para começar!</div>
        <div class="notification" id="notification">Hora do intervalo!</div>
    </div>

    <script>
        const timerDisplay = document.getElementById('timer');
        const startBtn = document.getElementById('startBtn');
        const pauseBtn = document.getElementById('pauseBtn');
        const resetBtn = document.getElementById('resetBtn');
        const statusElement = document.getElementById('status');
        const notification = document.getElementById('notification');

        let timeLeft = 40 * 60; // 40 minutos em segundos
        let timerId = null;
        let isPaused = true;
        let isStudyTime = true;

        function updateDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            timerDisplay.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        function startTimer() {
            if (isPaused) {
                isPaused = false;
                timerId = setInterval(() => {
                    timeLeft--;
                    updateDisplay();
                    
                    if (timeLeft === 0) {
                        clearInterval(timerId);
                        if (isStudyTime) {
                            // Tempo de estudo terminou, iniciar intervalo
                            notification.style.display = 'block';
                            notification.textContent = "Hora do intervalo de 10 minutos!";
                            statusElement.textContent = "Intervalo";
                            timeLeft = 10 * 60; // 10 minutos
                            isStudyTime = false;
                        } else {
                            // Intervalo terminou, reiniciar estudo
                            notification.style.display = 'block';
                            notification.textContent = "Hora de voltar aos estudos!";
                            statusElement.textContent = "Estudando";
                            timeLeft = 40 * 60;
                            isStudyTime = true;
                        }
                        playSound();
                        startTimer();
                    }
                }, 1000);
            }
        }

        function playSound() {
            const audio = new Audio('https://assets.mixkit.co/active_storage/sfx/2869/2869-preview.mp3');
            audio.play();
        }

        startBtn.addEventListener('click', () => {
            statusElement.textContent = "Estudando";
            startTimer();
        });

        pauseBtn.addEventListener('click', () => {
            clearInterval(timerId);
            isPaused = true;
            statusElement.textContent = "Pausado";
        });

        resetBtn.addEventListener('click', () => {
            clearInterval(timerId);
            isPaused = true;
            isStudyTime = true;
            timeLeft = 40 * 60;
            updateDisplay();
            statusElement.textContent = "Pronto para começar!";
            notification.style.display = 'none';
        });

        updateDisplay();
    </script>
</body>
</html>
