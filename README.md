<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pink Panther Currency Converter 🐾</title>
    
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Bebas+Neue&display=swap" rel="stylesheet">
    <style>
        :root {
            --panther-deep-pink: #E91E63; /* A deep, elegant pink */
            --panther-light-pink: #FFCCD2; /* Softer, lighter pink for background */
            --panther-dark: #333333;
            --panther-light: #F8F8F8;
            --calc-bg: #E0BBE4; /* A lighter purple/pink for calculator buttons */
            --calc-btn-text: #333333;
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--panther-light-pink); /* Lighter pink background */
            background-image: linear-gradient(135deg, #FFCCD2 0%, #FFB5C5 100%); /* Soft pink gradient */
            /* Removed centering to allow content to span full width of its container */
            padding: 1rem; 
            margin: 0;
            min-height: 100vh; /* Ensure background covers full height */
        }
        .panther-card {
            background-color: var(--panther-light);
            border: 6px solid var(--panther-deep-pink); 
            box-shadow: 0 8px 25px rgba(233, 30, 99, 0.5), 0 0 8px rgba(0, 0, 0, 0.2);
            /* Removed fixed max-width from CSS to allow Tailwind classes to control size */
        }
        .keypad-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 0.4rem; 
        }
        .calc-btn {
            background-color: var(--calc-bg);
            color: var(--calc-btn-text);
            font-weight: 600;
            transition: background-color 0.1s ease, transform 0.1s ease;
            padding: 0.8rem; 
            min-height: 50px; 
        }
        .calc-btn:hover {
            background-color: #f0cce9; 
            transform: scale(1.02);
        }
        .calc-btn.action {
            background-color: var(--panther-deep-pink); 
            color: white;
            font-family: 'Bebas Neue', sans-serif;
            letter-spacing: 1px;
        }
        .calc-btn.action:hover {
            background-color: #f04e8d; 
        }
        .result-text-red {
            color: #EF4444; 
        }
        .result-text-green {
            color: #10B981; 
        }
    </style>
</head>
<body>

    <!-- 
        UPDATED: Changed max-w-4xl to max-w-7xl (1280px) to make the converter much wider,
        effectively filling the page on most displays while keeping it centered and stylish.
    -->
    <div id="app" class="panther-card max-w-7xl w-full p-6 rounded-2xl mx-auto my-8">
        <h1 class="text-4xl text-center font-bold mb-3" style="font-family: 'Bebas Neue', sans-serif; color: var(--panther-dark);">
            <span class="text-xs block tracking-widest leading-none">THE INTERNATIONAL HEIST FUND 🕵️‍♀️</span>
            <span class="text-5xl text-pink-600">P<span class="text-gray-800">I</span>NK</span> CONVERTER 🐾
        </h1>

        <!-- Conversion Rates (Real-Time Rates Relative to 1 EUR) -->
        <script>
            // --- 🕵️‍♀️ REAL-TIME EXCHANGE RATES (1 EUR = X Currency) ---
            // These rates are accurate as of the last available data but are not live streaming.
            const RATES = {
                "EUR": 1.00,   // Euro (The Base)
                "PLN": 4.34,   // Polish Złoty 
                "GBP": 0.856,  // British Pound 
                "SEK": 11.10,  // Swedish Krona 
                "DKK": 7.46,   // Danish Krone 
                "USD": 1.07,   // US Dollar
                "JPY": 178.00, // Japanese Yen
                "CAD": 1.46,   // Canadian Dollar
                "AUD": 1.62,   // Australian Dollar
                "CHF": 0.97,   // Swiss Franc
                "NOK": 11.95,  // Norwegian Krone
                "HUF": 385.00, // Hungarian Forint
                "CZK": 24.80,  // Czech Koruna
            };
        </script>

        <!-- Display/Input Area -->
        <div class="mb-6 bg-white border border-gray-300 rounded-xl shadow-inner p-4">
            <div class="flex items-center justify-between mb-3">
                <div class="flex items-center space-x-2">
                    <label for="fromCurrencySelect" class="text-sm font-semibold text-gray-500">I have: 💎</label>
                    <!-- NEW PASTE BUTTON -->
                    <button id="pasteButton" class="text-xs px-2 py-0.5 rounded text-white bg-pink-500 hover:bg-pink-600 transition duration-150 shadow-sm" title="Paste Value">
                        PASTE 📋
                    </button>
                </div>
                <select id="fromCurrencySelect" class="bg-gray-100 border border-gray-300 rounded-md p-2 text-xl font-mono focus:ring-panther-deep-pink focus:border-panther-deep-pink transition duration-150">
                    <!-- Options populated by JS -->
                </select>
            </div>
            
            <input type="text" id="amountInput" value="0" class="w-full text-6xl font-mono text-right border-none focus:ring-0 p-0 bg-transparent tracking-tight" placeholder="0" oninput="manualInputConvert()">

            <div class="text-sm text-gray-500 mt-2 text-right">
                <span id="currentRateDisplay">...</span>
            </div>
            <!-- NEW PASTE STATUS MESSAGE -->
            <p id="pasteStatus" class="mt-1 text-xs font-medium text-blue-600 hidden text-right">Pasted successfully! 📋</p>
        </div>

        <!-- Result Display -->
        <div class="text-center mb-6 p-4 bg-gray-100 rounded-xl border border-gray-300">
            <p class="text-lg font-semibold text-gray-500">Converted Value (The Loot) 💰</p>
            <div class="flex items-center justify-center space-x-3">
                <p id="resultDisplay" class="text-6xl font-extrabold font-mono transition duration-300 result-text-red">
                    0.00 EUR
                </p>
                <button id="copyButton" class="text-2xl p-3 rounded-full text-white bg-pink-500 hover:bg-pink-600 transition duration-150 shadow-md focus:outline-none focus:ring-2 focus:ring-pink-500 focus:ring-opacity-50" title="Copy Result">
                    <!-- Copy Icon SVG -->
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v2M9 16l3-3m0 0l3 3m-3-3v8" />
                    </svg>
                </button>
            </div>
            <p id="statusMessage" class="mt-2 text-sm text-gray-700 font-semibold italic">A small prize. Better luck next time. 💔</p>
            <p id="copyStatus" class="mt-1 text-xs font-medium text-green-600 hidden">Copied to clipboard! ✅</p>
        </div>

        <!-- Calculator Keypad and Currency Buttons -->
        <div class="keypad-grid">
            <!-- Row 1 -->
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('7')">7</button>
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('8')">8</button>
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('9')">9</button>
            <button class="calc-btn action rounded-xl text-sm bg-pink-600 hover:bg-pink-700" onclick="clearInput()">CLEAR 🗑️</button>
            
            <!-- Row 2 -->
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('4')">4</button>
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('5')">5</button>
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('6')">6</button>
            <button class="calc-btn action rounded-xl text-sm bg-pink-700 hover:bg-pink-800" onclick="backspaceInput()">DELETE ❌</button>

            <!-- Row 3 -->
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('1')">1</button>
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('2')">2</button>
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('3')">3</button>
            <button class="calc-btn action rounded-xl text-2xl" onclick="appendToInput('.')">.</button>

            <!-- Row 4 -->
            <button class="calc-btn action rounded-xl text-2xl col-span-2" onclick="appendToInput('0')">0</button>
            <button class="calc-btn action rounded-xl text-lg col-span-2 bg-pink-500 hover:bg-pink-600" onclick="convert()">CONVERT 🐾</button>
        </div>
    </div>

    <script>
        const amountInput = document.getElementById('amountInput');
        const fromCurrencySelect = document.getElementById('fromCurrencySelect');
        const resultDisplay = document.getElementById('resultDisplay');
        const currentRateDisplay = document.getElementById('currentRateDisplay');
        const statusMessage = document.getElementById('statusMessage');
        const copyButton = document.getElementById('copyButton');
        const copyStatus = document.getElementById('copyStatus');
        const pasteButton = document.getElementById('pasteButton'); 
        const pasteStatus = document.getElementById('pasteStatus'); 

        // --- Core Functions ---

        /**
         * Populates the currency dropdown based on the RATES object.
         * Prioritizes GBP, PLN, SEK, DKK.
         */
        function populateCurrencies() {
            // GBP is now the first priority!
            const prioritized = ["GBP", "PLN", "SEK", "DKK", "EUR"];
            const other = Object.keys(RATES).filter(c => !prioritized.includes(c)).sort();
            
            const sortedCurrencies = [...prioritized.filter(c => RATES.hasOwnProperty(c)), ...other];

            fromCurrencySelect.innerHTML = '';
            
            sortedCurrencies.forEach(code => {
                const option = document.createElement('option');
                option.value = code;
                option.textContent = code;
                fromCurrencySelect.appendChild(option);
            });
            
            // Set default currency to GBP
            fromCurrencySelect.value = "GBP";
        }

        /**
         * Performs the currency conversion based on current input and selection.
         */
        function convert() {
            let amount = parseFloat(amountInput.value) || 0;
            const fromCurrency = fromCurrencySelect.value;
            const toCurrency = "EUR"; // Fixed target

            if (isNaN(amount) || amount <= 0) {
                resultDisplay.textContent = '0.00 EUR';
                resultDisplay.className = 'text-6xl font-extrabold font-mono transition duration-300 result-text-red';
                statusMessage.textContent = "Enter an amount for the Pink Panther to check! 🕵️‍♀️";
                currentRateDisplay.textContent = `Rate: 1 ${fromCurrency} = ${(1 / RATES[fromCurrency]).toFixed(4)} EUR (Approx. Real-Time)`;
                return;
            }

            // Conversion Formula: EUR Value = Amount / Rate (where Rate is the 1 EUR = X value)
            const fromRate = RATES[fromCurrency];
            
            if (!fromRate) {
                resultDisplay.textContent = 'ERROR';
                resultDisplay.className = 'text-6xl font-extrabold font-mono transition duration-300 result-text-red';
                statusMessage.textContent = "Error: Currency rate missing!";
                currentRateDisplay.textContent = `Rate: N/A`;
                return;
            }

            const convertedAmount = amount / fromRate;

            // --- Apply Color Coding Rule ---
            const threshold = 100;
            if (convertedAmount > threshold) {
                resultDisplay.className = 'text-6xl font-extrabold font-mono transition duration-300 result-text-green';
                statusMessage.textContent = "Jackpot! The vault is full! 💰✨ (Above 100 EUR)";
            } else {
                resultDisplay.className = 'text-6xl font-extrabold font-mono transition duration-300 result-text-red';
                statusMessage.textContent = "A small prize. Better luck next time. 💔 (Under 100 EUR)";
            }

            // Update Display
            resultDisplay.textContent = `${convertedAmount.toFixed(2)} EUR`;
            
            // Update Rate Display
            const rateToOneEuro = (1 / fromRate).toFixed(4);
            currentRateDisplay.textContent = `Rate: 1 ${fromCurrency} = ${rateToOneEuro} EUR (Approx. Real-Time)`;
        }
        
        // --- Clipboard Functions ---

        /**
         * Copies the converted result text to the clipboard.
         */
        function copyResult() {
            const textToCopy = resultDisplay.textContent.trim();
            
            // Use modern clipboard API first
            if (navigator.clipboard) {
                navigator.clipboard.writeText(textToCopy).then(() => {
                    showCopySuccess();
                }).catch(err => {
                    console.error('Failed to copy using clipboard API: ', err);
                    // Fallback attempt (for iframe restrictions)
                    fallbackCopyTextToClipboard(textToCopy);
                });
            } else {
                // Fallback for older browsers or restricted environments
                fallbackCopyTextToClipboard(textToCopy);
            }
        }

        /**
         * Fallback for older browsers or environments without full clipboard support.
         */
        function fallbackCopyTextToClipboard(text) {
            const textArea = document.createElement("textarea");
            textArea.value = text;
            
            // Avoid scrolling to bottom
            textArea.style.position = "fixed";
            textArea.style.opacity = 0; 

            document.body.appendChild(textArea);
            textArea.focus();
            textArea.select();

            try {
                const successful = document.execCommand('copy');
                if (successful) {
                    showCopySuccess();
                } else {
                    console.error('Fallback copy failed.');
                }
            } catch (err) {
                console.error('Oops, unable to copy', err);
            }

            document.body.removeChild(textArea);
        }

        /**
         * Shows the "Copied" message temporarily.
         */
        function showCopySuccess() {
            copyStatus.classList.remove('hidden');
            setTimeout(() => {
                copyStatus.classList.add('hidden');
            }, 2000);
        }

        /**
         * Pastes clipboard content into the amount input field.
         */
        function pasteInput() {
            if (navigator.clipboard && navigator.clipboard.readText) {
                navigator.clipboard.readText().then(text => {
                    // Sanitize the pasted text to allow only digits and one decimal point
                    const sanitizedText = text.replace(/[^0-9.]/g, '');
                    amountInput.value = sanitizedText || '0';
                    manualInputConvert();
                    showPasteStatus('Pasted successfully! 📋', 'text-blue-600');
                }).catch(err => {
                    console.error('Failed to read clipboard via button: ', err);
                    showPasteStatus('Paste failed. Use CTRL/CMD+V to paste manually.', 'text-red-600');
                });
            } else {
                showPasteStatus('Paste API not supported. Use CTRL/CMD+V to paste manually.', 'text-red-600');
            }
        }
        
        /**
         * Shows a temporary status message for the paste operation.
         */
        function showPasteStatus(message, colorClass) {
            pasteStatus.textContent = message;
            pasteStatus.className = `mt-1 text-xs font-medium ${colorClass} text-right`;
            pasteStatus.classList.remove('hidden');
            setTimeout(() => {
                pasteStatus.classList.add('hidden');
            }, 3000);
        }

        // --- Keypad/Manual Input Handlers ---

        /**
         * Appends value from keypad to the input field.
         */
        function appendToInput(val) {
            let current = amountInput.value.replace(/^0+/, ''); // Remove leading zero unless it's the only character
            if (current === '') current = '0';
            
            if (val === '.') {
                if (!current.includes('.')) {
                    amountInput.value = current + '.';
                }
            } else {
                if (current === '0') {
                    amountInput.value = val;
                } else {
                    amountInput.value = current + val;
                }
            }
            manualInputConvert();
        }

        /**
         * Clears the input field.
         */
        function clearInput() {
            amountInput.value = '0';
            manualInputConvert();
        }

        /**
         * Deletes the last character in the input field.
         */
        function backspaceInput() {
            let current = amountInput.value;
            if (current.length > 1 && current !== '0') {
                amountInput.value = current.slice(0, -1);
            } else {
                amountInput.value = '0';
            }
            manualInputConvert();
        }

        /**
         * Handles direct manual input change and triggers conversion.
         */
        function manualInputConvert() {
            // Sanitize input to allow only digits and one decimal point
            amountInput.value = amountInput.value.replace(/[^0-9.]/g, '');
            // Ensure leading zero if input is empty
            if (amountInput.value === '') {
                amountInput.value = '0';
            }
            convert();
        }

        // --- Event Listeners and Initialization ---

        document.addEventListener('DOMContentLoaded', () => {
            populateCurrencies();
            fromCurrencySelect.addEventListener('change', convert);
            copyButton.addEventListener('click', copyResult); 
            pasteButton.addEventListener('click', pasteInput); 
            convert();
        });
    </script>

</body>
</html>
