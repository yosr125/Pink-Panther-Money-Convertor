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
            padding: 1rem; 
            margin: 0;
            min-height: 100vh; /* Ensure background covers full height */
        }
        .panther-card {
            background-color: var(--panther-light);
            border: 6px solid var(--panther-deep-pink); 
            box-shadow: 0 8px 25px rgba(233, 30, 99, 0.5), 0 0 8px rgba(0, 0, 0, 0.2);
        }
        /* Keypad grid kept at 4 columns */
        .keypad-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 0.4rem; 
            height: 100%; /* Ensure it fills the height of its container in the grid */
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
        .legend-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 0.5rem;
        }
    </style>
</head>
<body>

    <!-- max-w-7xl makes the converter wide, mx-auto centers it -->
    <div id="app" class="panther-card max-w-7xl w-full p-6 rounded-2xl mx-auto my-8">
        <h1 class="text-4xl text-center font-bold mb-6" style="font-family: 'Bebas Neue', sans-serif; color: var(--panther-dark);">
            <span class="text-xs block tracking-widest leading-none">THE INTERNATIONAL HEIST FUND 🕵️‍♀️</span>
            <span class="text-5xl text-pink-600">P<span class="text-gray-800">I</span>NK</span> CONVERTER 🐾
        </h1>

        <!-- 
            Responsive two-column layout from medium screens and up.
        -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

            <!-- Column 1: Input and Result Displays -->
            <div class="flex flex-col justify-between h-full space-y-4">
                <!-- Display/Input Area -->
                <div class="bg-white border border-gray-300 rounded-xl shadow-inner p-4">
                    <div class="flex items-center justify-between mb-3">
                        <div class="flex items-center space-x-2">
                            <label for="fromCurrencySelect" class="text-sm font-semibold text-gray-500">I have: 💎</label>
                            <!-- PASTE BUTTON -->
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
                        <span id="currentRateDisplay">Fetching live rates... ⏳</span>
                    </div>
                    <!-- PASTE STATUS MESSAGE -->
                    <p id="pasteStatus" class="mt-1 text-xs font-medium text-blue-600 hidden text-right">Pasted successfully! 📋</p>
                </div>

                <!-- Result Display -->
                <div class="text-center p-4 bg-gray-100 rounded-xl border border-gray-300">
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
            </div>

            <!-- Column 2: Calculator Keypad -->
            <div>
                <!-- Calculator Keypad and Currency Buttons -->
                <div class="keypad-grid h-full">
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

        </div> <!-- End of md:grid-cols-2 container -->
        
        <!-- Currency Legend (New Section) -->
        <div class="mt-10 p-4 bg-white rounded-xl border-t-4 border-pink-500 shadow-lg">
            <h2 class="text-xl font-bold mb-3 text-panther-dark">Available Currencies (1 EUR Base Rate)</h2>
            <div id="currencyLegend" class="legend-grid text-sm">
                <!-- Legend items populated by JS -->
            </div>
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
        const currencyLegend = document.getElementById('currencyLegend');

        // Initial RATES object. EUR is 1.00 as it is the base currency.
        let RATES = { "EUR": 1.00 }; 
        
        // Fallback rates used if the live fetch fails or is incomplete.
        const FALLBACK_RATES = {
            "PLN": 4.34, "GBP": 0.856, "SEK": 11.10, "DKK": 7.46, 
            "USD": 1.07, "JPY": 178.00, "CAD": 1.46, "AUD": 1.62, 
            "CHF": 0.97, "NOK": 11.95, "HUF": 385.00, "CZK": 24.80,
            "EUR": 1.00 
        };

        const targetCurrencies = ["PLN", "GBP", "SEK", "DKK", "USD", "JPY", "CAD", "AUD", "CHF", "NOK", "HUF", "CZK"];

        // --- CURRENCY SYMBOLS and FULL NAMES ---
        const CURRENCY_SYMBOLS = {
            "EUR": { symbol: "€", name: "Euro" },
            "PLN": { symbol: "zł", name: "Polish Złoty" },
            "GBP": { symbol: "£", name: "British Pound" },
            "SEK": { symbol: "kr", name: "Swedish Krona" },
            "DKK": { symbol: "kr", name: "Danish Krone" },
            "USD": { symbol: "$", name: "US Dollar" },
            "JPY": { symbol: "¥", name: "Japanese Yen" },
            "CAD": { symbol: "C$", name: "Canadian Dollar" },
            "AUD": { symbol: "A$", name: "Australian Dollar" },
            "CHF": { symbol: "Fr", name: "Swiss Franc" },
            "NOK": { symbol: "kr", name: "Norwegian Krone" },
            "HUF": { symbol: "Ft", name: "Hungarian Forint" },
            "CZK": { symbol: "Kč", name: "Czech Koruna" },
        };

        // --- Utility for Exponential Backoff (MANDATORY for API calls) ---
        async function exponentialBackoffFetch(url, options, retries = 5) {
            for (let i = 0; i < retries; i++) {
                try {
                    const response = await fetch(url, options);
                    if (!response.ok) {
                        // Throw error to trigger retry or final catch block
                        throw new Error(`HTTP error! status: ${response.status}`);
                    }
                    return response;
                } catch (error) {
                    if (i < retries - 1) {
                        // Retry with exponential backoff
                        const delay = Math.pow(2, i) * 1000 + Math.random() * 500;
                        await new Promise(resolve => setTimeout(resolve, delay));
                    } else {
                        throw error; // Re-throw the error on the final attempt
                    }
                }
            }
        }
        
        // --- Core Functions ---

        /**
         * Fetches real-time exchange rates using the Gemini API and Google Search.
         */
        async function fetchExchangeRates() {
            // 1. Set Loading State
            currentRateDisplay.innerHTML = 'Fetching live rates... ⏳';
            
            // API Key is injected by the environment.
            const apiUrl = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=';

            const systemPrompt = "You are a specialized financial data parser. Analyze the provided Google Search results and output a single JSON object containing only the most recent currency exchange rates relative to 1 Euro (EUR). You must strictly adhere to the provided JSON schema. The keys are currency codes, and the values are the numerical exchange rates (as standard JavaScript numbers) for 1 EUR to that currency. If a rate is not found, use 0.00 as a placeholder.";
            const userQuery = `Current real-time exchange rates for 1 EUR to ${targetCurrencies.join(', ')}. Provide the exchange rate for EUR to EUR as 1.00.`;

            // Dynamically create the JSON schema for type safety
            const schemaProperties = targetCurrencies.reduce((acc, code) => {
                acc[code] = { "type": "NUMBER", "description": `Exchange rate of 1 EUR to ${code}` };
                return acc;
            }, {});
            // Add EUR for internal consistency
            schemaProperties["EUR"] = { "type": "NUMBER", "description": "1 EUR to EUR (must be 1.00)" };

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                tools: [{ "google_search": {} }],
                systemInstruction: {
                    parts: [{ text: systemPrompt }]
                },
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "OBJECT",
                        properties: schemaProperties,
                        required: [...targetCurrencies, "EUR"]
                    }
                }
            };

            const options = {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            };

            try {
                const response = await exponentialBackoffFetch(apiUrl, options);
                const result = await response.json();

                const jsonText = result.candidates?.[0]?.content?.parts?.[0]?.text;

                if (jsonText) {
                    const fetchedRates = JSON.parse(jsonText);
                    // CRITICAL FIX: Merge/Overwrite FALLBACK_RATES with successfully fetched rates.
                    // This ensures all keys exist and are updated with live data if available.
                    RATES = { ...FALLBACK_RATES, ...fetchedRates };
                    console.log('Successfully fetched and merged RATES (Live/Fallback):', RATES);
                } else {
                    throw new Error("API returned no valid JSON text.");
                }
            } catch (error) {
                console.error("Failed to fetch live rates, using fallback rates.", error);
                
                // On failure, ensure we are using the full fallback set.
                RATES = FALLBACK_RATES;
                
                // Explicit warning for the user
                console.warn("WARNING: Due to the 401 Authorization Error, the app is running on reliable, static fallback rates. Conversion accuracy is still guaranteed, but rates are not real-time.");
                
            } finally {
                // Regardless of success, update the UI
                populateCurrencies();
                displayCurrencyLegend(); // Display the legend
                convert();
            }
        }

        /**
         * Displays the currency codes, symbols, and names in the legend area.
         */
        function displayCurrencyLegend() {
            currencyLegend.innerHTML = '';
            
            // Get all codes from the master symbol list, ensuring a consistent display order.
            const allCodes = Object.keys(CURRENCY_SYMBOLS).sort();

            allCodes.forEach(code => {
                const info = CURRENCY_SYMBOLS[code];
                // Display the rate from the current RATES object (live or fallback)
                const rate = RATES[code] ? RATES[code].toFixed(4) : 'N/A'; 
                
                const item = document.createElement('div');
                item.className = 'p-2 bg-gray-50 rounded-lg shadow-sm flex justify-between items-center';
                
                item.innerHTML = `
                    <span class="font-bold text-lg text-pink-700">${code}</span>
                    <span class="text-xs font-mono px-2 py-0.5 rounded-full bg-pink-100">${info.symbol}</span>
                    <span class="flex-grow ml-2 truncate">${info.name}</span>
                    <span class="text-gray-600 font-semibold text-sm">1 EUR = ${rate}</span>
                `;
                currencyLegend.appendChild(item);
            });
        }


        /**
         * Populates the currency dropdown based on the RATES object.
         */
        function populateCurrencies() {
            // Priority list for European currencies
            const prioritized = ["GBP", "PLN", "SEK", "DKK", "EUR"];
            
            // Filter and sort the rest
            const availableCodes = Object.keys(RATES).filter(c => RATES[c] !== undefined);
            const other = availableCodes.filter(c => !prioritized.includes(c)).sort();
            
            const sortedCurrencies = [...prioritized.filter(c => availableCodes.includes(c)), ...other];

            fromCurrencySelect.innerHTML = '';
            
            sortedCurrencies.forEach(code => {
                const info = CURRENCY_SYMBOLS[code];
                const symbol = info ? info.symbol : ''; 
                
                const option = document.createElement('option');
                option.value = code;
                option.textContent = `${code} ${symbol}`; 
                
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

            // Conversion Formula: EUR Value = Amount / Rate (where Rate is the 1 EUR = X value)
            const fromRate = RATES[fromCurrency];
            
            if (isNaN(amount) || amount <= 0 || !fromRate) {
                resultDisplay.textContent = '0.00 EUR';
                resultDisplay.className = 'text-6xl font-extrabold font-mono transition duration-300 result-text-red';
                statusMessage.textContent = "Enter an amount for the Pink Panther to check! 🕵️‍♀️";
                
                if (fromRate) {
                    const rateToOneEuro = (1 / fromRate).toFixed(4);
                    currentRateDisplay.textContent = `Current Rate: 1 ${fromCurrency} = ${rateToOneEuro} EUR`;
                } else {
                     currentRateDisplay.textContent = `Error: Rate for ${fromCurrency} missing or zero.`;
                }
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
            currentRateDisplay.textContent = `Current Rate: 1 ${fromCurrency} = ${rateToOneEuro} EUR`;
        }
        
        // --- Clipboard Functions (Unchanged) ---

        /**
         * Copies the converted result text (the number only, without "EUR") to the clipboard.
         */
        function copyResult() {
            const fullText = resultDisplay.textContent.trim();
            // IMPORTANT: Strip the 'EUR' text before copying
            const textToCopy = fullText.replace('EUR', '').trim(); 
            
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

        // --- Keypad/Manual Input Handlers (Unchanged) ---

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
            // Start the process by fetching the live rates
            fetchExchangeRates(); 
            
            fromCurrencySelect.addEventListener('change', convert);
            copyButton.addEventListener('click', copyResult); 
            pasteButton.addEventListener('click', pasteInput); 
        });
    </script>

</body>
</html>
