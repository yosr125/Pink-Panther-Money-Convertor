<!DOCTYPE html>
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
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 0.5rem; /* Reduced overall padding */
            margin: 0;
        }
        .panther-card {
            background-color: var(--panther-light);
            border: 6px solid var(--panther-deep-pink); /* Slightly thinner border */
            box-shadow: 0 8px 25px rgba(233, 30, 99, 0.5), 0 0 8px rgba(0, 0, 0, 0.2);
            /* Adjust max-width for better use of space */
            max-width: 400px; 
            width: 100%;
        }
        .keypad-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 0.4rem; /* Reduced gap */
        }
        .calc-btn {
            background-color: var(--calc-bg);
            color: var(--calc-btn-text);
            font-weight: 600;
            transition: background-color 0.1s ease, transform 0.1s ease;
            padding: 0.8rem; /* Reduced button padding */
            min-height: 50px; /* Ensure a minimum touch target size */
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
<body class="p-2">

    <div id="app" class="panther-card max-w-lg w-full p-4 rounded-2xl">
        <h1 class="text-4xl text-center font-bold mb-3" style="font-family: 'Bebas Neue', sans-serif; color: var(--panther-dark);">
            <span class="text-xs block tracking-widest leading-none">THE INTERNATIONAL HEIST FUND 🕵️‍♀️</span>
            <span class="text-5xl text-pink-600">P<span class="text-gray-800">I</span>NK</span> CONVERTER 🐾
        </h1>

        <!-- Conversion Rates (Hardcoded Fictional Rates Relative to 1 EUR) -->
        <script>
            // --- 🕵️‍♀️ HARDCODED FICTIONAL EXCHANGE RATES (1 EUR = X Currency) ---
            // These rates are for demonstration purposes and are NOT real-time.
            const RATES = {
                "EUR": 1.00,  // Euro (The Base)
                "PLN": 4.35,  // Polish Złoty (Important)
                "GBP": 0.85,  // British Pound (Important)
                "SEK": 11.20, // Swedish Krona (Important)
                "DKK": 7.45,  // Danish Krone (Important)
                "USD": 1.08,  // US Dollar
                "JPY": 170.00, // Japanese Yen
                "CAD": 1.47,  // Canadian Dollar
                "AUD": 1.63,  // Australian Dollar
                "CHF": 0.98,  // Swiss Franc
                "NOK": 11.90, // Norwegian Krone
                "HUF": 400.00, // Hungarian Forint
                "CZK": 25.00, // Czech Koruna
            };
        </script>

        <!-- Display/Input Area -->
        <div class="mb-4 bg-white border border-gray-300 rounded-lg shadow-inner p-3">
            <div class="flex items-center justify-between mb-2">
                <label for="fromCurrencySelect" class="text-xs font-semibold text-gray-500">I have: 💎</label>
                <select id="fromCurrencySelect" class="bg-gray-100 border border-gray-300 rounded-md p-1 text-lg font-mono focus:ring-panther-deep-pink focus:border-panther-deep-pink transition duration-150">
                    <!-- Options populated by JS -->
                </select>
            </div>
            
            <input type="text" id="amountInput" value="0" class="w-full text-4xl font-mono text-right border-none focus:ring-0 p-0 bg-transparent tracking-tight" placeholder="0" oninput="manualInputConvert()">

            <div class="text-xs text-gray-500 mt-1 text-right">
                <span id="currentRateDisplay">...</span>
            </div>
        </div>

        <!-- Result Display -->
        <div class="text-center mb-4 p-3 bg-gray-100 rounded-lg border border-gray-300">
            <p class="text-base font-semibold text-gray-500">Converted Value (The Loot) 💰</p>
            <p id="resultDisplay" class="text-5xl font-extrabold font-mono transition duration-300 result-text-red">
                0.00 EUR
            </p>
            <p id="statusMessage" class="mt-1 text-xs text-gray-700 font-semibold italic">A small prize. Better luck next time. 💔</p>
        </div>

        <!-- Calculator Keypad and Currency Buttons -->
        <div class="keypad-grid">
            <!-- Row 1 -->
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('7')">7</button>
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('8')">8</button>
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('9')">9</button>
            <button class="calc-btn action rounded-lg text-xs bg-pink-600 hover:bg-pink-700" onclick="clearInput()">CLEAR 🗑️</button>
            
            <!-- Row 2 -->
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('4')">4</button>
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('5')">5</button>
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('6')">6</button>
            <button class="calc-btn action rounded-lg text-xs bg-pink-700 hover:bg-pink-800" onclick="backspaceInput()">DELETE ❌</button>

            <!-- Row 3 -->
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('1')">1</button>
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('2')">2</button>
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('3')">3</button>
            <button class="calc-btn action rounded-lg text-xl" onclick="appendToInput('.')">.</button>

            <!-- Row 4 -->
            <button class="calc-btn action rounded-lg text-xl col-span-2" onclick="appendToInput('0')">0</button>
            <button class="calc-btn action rounded-lg text-sm col-span-2 bg-pink-500 hover:bg-pink-600" onclick="convert()">CONVERT 🐾</button>
        </div>
    </div>

    <script>
        const amountInput = document.getElementById('amountInput');
        const fromCurrencySelect = document.getElementById('fromCurrencySelect');
        const resultDisplay = document.getElementById('resultDisplay');
        const currentRateDisplay = document.getElementById('currentRateDisplay');
        const statusMessage = document.getElementById('statusMessage');

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
                resultDisplay.className = 'text-5xl font-extrabold font-mono transition duration-300 result-text-red';
                statusMessage.textContent = "Enter an amount for the Pink Panther to check! 🕵️‍♀️";
                currentRateDisplay.textContent = `Rate: 1 ${fromCurrency} = ${(RATES['EUR'] / RATES[fromCurrency]).toFixed(4)} EUR (Fictional)`;
                return;
            }

            // Conversion Formula: EUR Value = Amount / Rate (where Rate is the 1 EUR = X value)
            const fromRate = RATES[fromCurrency];
            
            if (!fromRate) {
                resultDisplay.textContent = 'ERROR';
                resultDisplay.className = 'text-5xl font-extrabold font-mono transition duration-300 result-text-red';
                statusMessage.textContent = "Error: Currency rate missing!";
                currentRateDisplay.textContent = `Rate: N/A`;
                return;
            }

            const convertedAmount = amount / fromRate;

            // --- Apply Color Coding Rule ---
            const threshold = 100;
            if (convertedAmount > threshold) {
                resultDisplay.className = 'text-5xl font-extrabold font-mono transition duration-300 result-text-green';
                statusMessage.textContent = "Jackpot! The vault is full! 💰✨ (Above 100 EUR)";
            } else {
                resultDisplay.className = 'text-5xl font-extrabold font-mono transition duration-300 result-text-red';
                statusMessage.textContent = "A small prize. Better luck next time. 💔 (Under 100 EUR)";
            }

            // Update Display
            resultDisplay.textContent = `${convertedAmount.toFixed(2)} EUR`;
            
            // Update Rate Display
            const rateToOneEuro = (1 / fromRate).toFixed(4);
            currentRateDisplay.textContent = `Rate: 1 ${fromCurrency} = ${rateToOneEuro} EUR (Fictional)`;
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
            // Trigger conversion whenever the currency selection changes
            fromCurrencySelect.addEventListener('change', convert);
            // Initial conversion check
            convert();
        });
    </script>

</body>
</html>
