<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Full-Width Responsive Layout</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    <style>
        /* Optional: Smooth transition for hover effects */
        .card {
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .card:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
    </style>
</head>
<body class="bg-gray-50 font-sans text-gray-800">

    <!-- 
        The key change is here: 
        We use w-full (width: 100%) and omit max-w-X classes 
        on the main content container to ensure it fills the entire width.
    -->
    <div class="min-h-screen flex flex-col">

        <!-- Header: Full Width -->
        <header class="bg-indigo-600 shadow-lg text-white p-4">
            <div class="w-full px-4 sm:px-6 lg:px-8">
                <h1 class="text-2xl font-bold">Full Width Application Header</h1>
                <p class="text-indigo-200">This navigation bar spans 100% of the screen width.</p>
            </div>
        </header>

        <!-- Main Content Area: Full Width -->
        <main class="flex-grow p-4 sm:p-6 lg:p-8">
            
            <!-- Content Wrapper: Also using w-full -->
            <div class="w-full space-y-8">
                
                <!-- Section 1: Demonstrating Full Width Text -->
                <section class="bg-white p-6 shadow-xl rounded-xl">
                    <h2 class="text-xl font-semibold text-indigo-700 mb-4">Responsive Full-Width Container</h2>
                    <p class="mb-4">
                        This content block utilizes the **w-full** utility, ensuring it stretches to fit the maximum available horizontal space on any device. 
                        Padding (`p-6`) and margins (`mb-4`) are used for spacing, but the container itself takes up 100% of the parent's width.
                    </p>
                    <div class="bg-indigo-50 border-l-4 border-indigo-500 p-4 rounded-md">
                        <p class="text-sm text-indigo-700">
                            On smaller mobile screens, the content adjusts fluidly, preventing horizontal scrolling.
                        </p>
                    </div>
                </section>
                
                <!-- Section 2: Full Width Grid Example (3 columns on desktop, 1 on mobile) -->
                <section>
                    <h2 class="text-xl font-semibold text-gray-700 mb-4">Full-Width Card Grid (Responsive)</h2>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                        
                        <!-- Card 1 -->
                        <div class="card bg-white p-6 rounded-xl shadow-lg border border-gray-100">
                            <h3 class="text-lg font-bold text-gray-900 mb-2">Service A</h3>
                            <p class="text-gray-600">This column is a third of the width on large screens, ensuring the entire row still fills the page.</p>
                        </div>
                        
                        <!-- Card 2 -->
                        <div class="card bg-white p-6 rounded-xl shadow-lg border border-gray-100">
                            <h3 class="text-lg font-bold text-gray-900 mb-2">Service B</h3>
                            <p class="text-gray-600">The grid automatically reflows to fewer columns on tablet (`md:`) and mobile screens.</p>
                        </div>

                        <!-- Card 3 -->
                        <div class="card bg-white p-6 rounded-xl shadow-lg border border-gray-100">
                            <h3 class="text-lg font-bold text-gray-900 mb-2">Service C</h3>
                            <p class="text-gray-600">Resize your browser window to see how these cards adapt to the full available width.</p>
                        </div>
                    </div>
                </section>
            </div>

        </main>

        <!-- Footer: Full Width -->
        <footer class="bg-gray-800 text-white p-4">
            <div class="w-full px-4 sm:px-6 lg:px-8 text-center text-sm text-gray-400">
                <p>&copy; 2024 Full Width App. All rights reserved.</p>
            </div>
        </footer>

    </div>
</body>
</html>
