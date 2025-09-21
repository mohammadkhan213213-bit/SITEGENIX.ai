# SITEGENIX.ai
In this website I only provide ai tools
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Free AI Voice Generator</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        h1 {
            color: #2c3e50;
            text-align: center;
        }
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        textarea {
            width: 100%;
            height: 150px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            resize: vertical;
            font-size: 16px;
        }
        .controls {
            margin: 15px 0;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        select, input[type="range"] {
            width: 100%;
            margin-bottom: 15px;
        }
        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
        button {
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            flex: 1;
        }
        #generateBtn {
            background-color: #3498db;
            color: white;
        }
        #stopBtn {
            background-color: #e74c3c;
            color: white;
        }
        .value-display {
            display: inline-block;
            width: 30px;
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>AI Voice Generator</h1>
        <textarea id="textInput" placeholder="Enter your text here..."></textarea>
        
        <div class="controls">
            <label for="voiceSelect">Select Voice:</label>
            <select id="voiceSelect"></select>
            
            <label for="speed">Speed: <span id="speedValue" class="value-display">1</span></label>
            <input type="range" id="speed" min="0.5" max="2" step="0.1" value="1">
            
            <label for="pitch">Pitch: <span id="pitchValue" class="value-display">1</span></label>
            <input type="range" id="pitch" min="0.5" max="2" step="0.1" value="1">
        </div>
        
        <div class="button-group">
            <button id="generateBtn">Generate Speech</button>
            <button id="stopBtn">Stop</button>
        </div>
    </div>

    <script>
        // DOM Elements
        const textInput = document.getElementById('textInput');
        const voiceSelect = document.getElementById('voiceSelect');
        const speedControl = document.getElementById('speed');
        const pitchControl = document.getElementById('pitch');
        const speedValue = document.getElementById('speedValue');
        const pitchValue = document.getElementById('pitchValue');
        const generateBtn = document.getElementById('generateBtn');
        const stopBtn = document.getElementById('stopBtn');

        // Initialize voices
        function loadVoices() {
            // Get available voices
            const voices = window.speechSynthesis.getVoices();
            
            // Clear existing options
            voiceSelect.innerHTML = '';
            
            // Add each voice as an option
            voices.forEach(voice => {
                const option = document.createElement('option');
                option.value = voice.name;
                option.textContent = `${voice.name} (${voice.lang})`;
                voiceSelect.appendChild(option);
            });
            
            // Select the first voice by default if available
            if (voices.length > 0) {
                voiceSelect.value = voices[0].name;
            }
        }

        // Event listeners for range inputs
        speedControl.addEventListener('input', () => {
            speedValue.textContent = speedControl.value;
        });

        pitchControl.addEventListener('input', () => {
            pitchValue.textContent = pitchControl.value;
        });

        // Generate speech
        generateBtn.addEventListener('click', () => {
            const text = textInput.value.trim();
            
            if (!text) {
                alert('Please enter some text first!');
                return;
            }
            
            // Stop any currently playing speech
            window.speechSynthesis.cancel();
            
            // Create a new speech utterance
            const utterance = new SpeechSynthesisUtterance(text);
            
            // Set voice
            const selectedVoiceName = voiceSelect.value;
            const voices = window.speechSynthesis.getVoices();
            const selectedVoice = voices.find(voice => voice.name === selectedVoiceName);
            
            if (selectedVoice) {
                utterance.voice = selectedVoice;
            }
            
            // Set speech properties
            utterance.rate = parseFloat(speedControl.value);
            utterance.pitch = parseFloat(pitchControl.value);
            
            // Speak the text
            window.speechSynthesis.speak(utterance);
        });

        // Stop speech
        stopBtn.addEventListener('click', () => {
