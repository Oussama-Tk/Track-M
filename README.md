<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SpendTrack - Daily Expense Tracker</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        
        * {
            font-family: 'Inter', sans-serif;
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }
        
        .slide-up {
            animation: slideUp 0.3s ease-out;
        }
        
        .bounce-in {
            animation: bounceIn 0.6s ease-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes slideUp {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        @keyframes bounceIn {
            0% { transform: scale(0.3); opacity: 0; }
            50% { transform: scale(1.05); }
            70% { transform: scale(0.9); }
            100% { transform: scale(1); opacity: 1; }
        }
        
        .glass {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        
        .expense-item {
            transition: all 0.3s ease;
        }
        
        .expense-item:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }
        
        .demo-badge {
            position: absolute;
            top: 10px;
            right: 10px;
            background: rgba(255, 193, 7, 0.9);
            color: #000;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 10px;
            font-weight: 600;
            z-index: 1000;
        }
    </style>
</head>
<body class="gradient-bg min-h-screen">
    <div class="demo-badge">DEMO VERSION</div>
    
    <!-- Auth Container -->
    <div id="authContainer" class="min-h-screen flex items-center justify-center p-4">
        <div class="glass rounded-2xl p-8 w-full max-w-md fade-in">
            <!-- Login Form -->
            <div id="loginForm">
                <div class="text-center mb-8">
                    <h1 class="text-3xl font-bold text-white mb-2">💰 SpendTrack</h1>
                    <p class="text-gray-200">Track your daily expenses beautifully</p>
                </div>
                
                <form class="space-y-6">
                    <div>
                        <label class="block text-sm font-medium text-gray-200 mb-2">Email</label>
                        <input type="email" class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition-all" placeholder="Enter your email">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-200 mb-2">Password</label>
                        <input type="password" class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition-all" placeholder="Enter your password">
                    </div>
                    
                    <button type="button" onclick="showDashboard()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-4 rounded-lg transition-all duration-300 transform hover:scale-105">
                        Sign In (Demo)
                    </button>
                </form>
                
                <div class="mt-6 text-center">
                    <p class="text-gray-200">Don't have an account? 
                        <button onclick="showRegister()" class="text-blue-300 hover:text-blue-200 font-medium">Sign up</button>
                    </p>
                </div>
            </div>
            
            <!-- Register Form -->
            <div id="registerForm" class="hidden">
                <div class="text-center mb-8">
                    <h1 class="text-3xl font-bold text-white mb-2">Create Account</h1>
                    <p class="text-gray-200">Start tracking your expenses today</p>
                </div>
                
                <form class="space-y-6">
                    <div>
                        <label class="block text-sm font-medium text-gray-200 mb-2">Full Name</label>
                        <input type="text" class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition-all" placeholder="Enter your full name">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-200 mb-2">Email</label>
                        <input type="email" class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition-all" placeholder="Enter your email">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-200 mb-2">Password</label>
                        <input type="password" class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition-all" placeholder="Create a password">
                    </div>
                    
                    <button type="button" onclick="showDashboard()" class="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-4 rounded-lg transition-all duration-300 transform hover:scale-105">
                        Create Account (Demo)
                    </button>
                </form>
                
                <div class="mt-6 text-center">
                    <p class="text-gray-200">Already have an account? 
                        <button onclick="showLogin()" class="text-blue-300 hover:text-blue-200 font-medium">Sign in</button>
                    </p>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Dashboard Container -->
    <div id="dashboardContainer" class="hidden min-h-screen p-4">
        <!-- Header -->
        <header class="glass rounded-2xl p-6 mb-6 fade-in">
            <div class="flex justify-between items-center">
                <div>
                    <h1 class="text-2xl font-bold text-white">💰 SpendTrack Dashboard</h1>
                    <p class="text-gray-200">Welcome back, John!</p>
                </div>
                <button onclick="logout()" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg transition-all">
                    Logout
                </button>
            </div>
        </header>
        
        <!-- Stats Cards -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
            <div class="glass rounded-xl p-6 slide-up">
                <div class="flex items-center justify-between">
                    <div>
                        <p class="text-gray-200 text-sm">Today's Spending</p>
                        <p class="text-2xl font-bold text-white" id="todaySpending">$0.00</p>
                    </div>
                    <div class="text-3xl">📊</div>
                </div>
            </div>
            
            <div class="glass rounded-xl p-6 slide-up" style="animation-delay: 0.1s">
                <div class="flex items-center justify-between">
                    <div>
                        <p class="text-gray-200 text-sm">This Week</p>
                        <p class="text-2xl font-bold text-white" id="weekSpending">$0.00</p>
                    </div>
                    <div class="text-3xl">📅</div>
                </div>
            </div>
            
            <div class="glass rounded-xl p-6 slide-up" style="animation-delay: 0.2s">
                <div class="flex items-center justify-between">
                    <div>
                        <p class="text-gray-200 text-sm">This Month</p>
                        <p class="text-2xl font-bold text-white" id="monthSpending">$0.00</p>
                    </div>
                    <div class="text-3xl">💳</div>
                </div>
            </div>
        </div>
        
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
            <!-- Add Expense Form -->
            <div class="glass rounded-xl p-6 bounce-in">
                <h2 class="text-xl font-semibold text-white mb-4">Add New Expense</h2>
                
                <form id="expenseForm" class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-200 mb-2">Amount</label>
                        <input type="number" id="amount" step="0.01" class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition-all" placeholder="0.00" required>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-200 mb-2">Category</label>
                        <select id="category" class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white focus:outline-none focus:ring-2 focus:ring-blue-400 transition-all" required>
                            <option value="">Select category</option>
                            <option value="food">🍔 Food & Dining</option>
                            <option value="transport">🚗 Transportation</option>
                            <option value="shopping">🛍️ Shopping</option>
                            <option value="entertainment">🎬 Entertainment</option>
                            <option value="bills">💡 Bills & Utilities</option>
                            <option value="health">🏥 Healthcare</option>
                            <option value="other">📝 Other</option>
                        </select>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-200 mb-2">Description</label>
                        <input type="text" id="description" class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition-all" placeholder="What did you spend on?" required>
                    </div>
                    
                    <button type="submit" class="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-4 rounded-lg transition-all duration-300 transform hover:scale-105">
                        Add Expense
                    </button>
                </form>
            </div>
            
            <!-- Recent Expenses -->
            <div class="glass rounded-xl p-6 bounce-in" style="animation-delay: 0.2s">
                <h2 class="text-xl font-semibold text-white mb-4">Recent Expenses</h2>
                
                <div id="expensesList" class="space-y-3 max-h-96 overflow-y-auto">
                    <!-- Sample expenses will be added here -->
                </div>
            </div>
        </div>
        
        <!-- Chart Section -->
        <div class="glass rounded-xl p-6 mt-6 bounce-in" style="animation-delay: 0.4s">
            <h2 class="text-xl font-semibold text-white mb-4">Spending Overview</h2>
            <div class="bg-white/10 rounded-lg p-4">
                <canvas id="spendingChart" width="400" height="200"></canvas>
            </div>
        </div>
    </div>
    
    <script>
        // Sample data for demo
        let expenses = [
            { id: 1, amount: 12.50, category: 'food', description: 'Lunch at cafe', date: new Date().toISOString().split('T')[0] },
            { id: 2, amount: 25.00, category: 'transport', description: 'Uber ride', date: new Date().toISOString().split('T')[0] },
            { id: 3, amount: 8.99, category: 'entertainment', description: 'Movie ticket', date: new Date(Date.now() - 86400000).toISOString().split('T')[0] }
        ];
        
        let chart;
        
        // Auth functions
        function showRegister() {
            document.getElementById('loginForm').classList.add('hidden');
            document.getElementById('registerForm').classList.remove('hidden');
        }
        
        function showLogin() {
            document.getElementById('registerForm').classList.add('hidden');
            document.getElementById('loginForm').classList.remove('hidden');
        }
        
        function showDashboard() {
            document.getElementById('authContainer').classList.add('hidden');
            document.getElementById('dashboardContainer').classList.remove('hidden');
            loadExpenses();
            updateStats();
            initChart();
        }
        
        function logout() {
            document.getElementById('dashboardContainer').classList.add('hidden');
            document.getElementById('authContainer').classList.remove('hidden');
        }
        
        // Expense functions
        function addExpense(amount, category, description) {
            const newExpense = {
                id: Date.now(),
                amount: parseFloat(amount),
                category: category,
                description: description,
                date: new Date().toISOString().split('T')[0]
            };
            
            expenses.unshift(newExpense);
            loadExpenses();
            updateStats();
            updateChart();
        }
        
        function deleteExpense(id) {
            expenses = expenses.filter(expense => expense.id !== id);
            loadExpenses();
            updateStats();
            updateChart();
        }
        
        function loadExpenses() {
            const expensesList = document.getElementById('expensesList');
            expensesList.innerHTML = '';
            
            expenses.slice(0, 10).forEach(expense => {
                const categoryIcons = {
                    'food': '🍔',
                    'transport': '🚗',
                    'shopping': '🛍️',
                    'entertainment': '🎬',
                    'bills': '💡',
                    'health': '🏥',
                    'other': '📝'
                };
                
                const expenseElement = document.createElement('div');
                expenseElement.className = 'expense-item bg-white/10 rounded-lg p-4 flex justify-between items-center';
                expenseElement.innerHTML = `
                    <div class="flex items-center space-x-3">
                        <span class="text-2xl">${categoryIcons[expense.category]}</span>
                        <div>
                            <p class="text-white font-medium">${expense.description}</p>
                            <p class="text-gray-300 text-sm">${expense.date}</p>
                        </div>
                    </div>
                    <div class="flex items-center space-x-2">
                        <span class="text-white font-semibold">$${expense.amount.toFixed(2)}</span>
                        <button onclick="deleteExpense(${expense.id})" class="text-red-400 hover:text-red-300 transition-colors">
                            ❌
                        </button>
                    </div>
                `;
                expensesList.appendChild(expenseElement);
            });
        }
        
        function updateStats() {
            const today = new Date().toISOString().split('T')[0];
            const weekAgo = new Date(Date.now() - 7 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];
            const monthAgo = new Date(Date.now() - 30 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];
            
            const todayTotal = expenses
                .filter(expense => expense.date === today)
                .reduce((sum, expense) => sum + expense.amount, 0);
            
            const weekTotal = expenses
                .filter(expense => expense.date >= weekAgo)
                .reduce((sum, expense) => sum + expense.amount, 0);
            
            const monthTotal = expenses
                .filter(expense => expense.date >= monthAgo)
                .reduce((sum, expense) => sum + expense.amount, 0);
            
            document.getElementById('todaySpending').textContent = `$${todayTotal.toFixed(2)}`;
            document.getElementById('weekSpending').textContent = `$${weekTotal.toFixed(2)}`;
            document.getElementById('monthSpending').textContent = `$${monthTotal.toFixed(2)}`;
        }
        
        function initChart() {
            const ctx = document.getElementById('spendingChart').getContext('2d');
            
            const categoryTotals = {};
            expenses.forEach(expense => {
                categoryTotals[expense.category] = (categoryTotals[expense.category] || 0) + expense.amount;
            });
            
            chart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: Object.keys(categoryTotals).map(cat => cat.charAt(0).toUpperCase() + cat.slice(1)),
                    datasets: [{
                        data: Object.values(categoryTotals),
                        backgroundColor: [
                            '#FF6384',
                            '#36A2EB',
                            '#FFCE56',
                            '#4BC0C0',
                            '#9966FF',
                            '#FF9F40',
                            '#FF6384'
                        ],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            labels: {
                                color: 'white'
                            }
                        }
                    }
                }
            });
        }
        
        function updateChart() {
            if (chart) {
                const categoryTotals = {};
                expenses.forEach(expense => {
                    categoryTotals[expense.category] = (categoryTotals[expense.category] || 0) + expense.amount;
                });
                
                chart.data.labels = Object.keys(categoryTotals).map(cat => cat.charAt(0).toUpperCase() + cat.slice(1));
                chart.data.datasets[0].data = Object.values(categoryTotals);
                chart.update();
            }
        }
        
        // Form submission
        document.getElementById('expenseForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const amount = document.getElementById('amount').value;
            const category = document.getElementById('category').value;
            const description = document.getElementById('description').value;
            
            if (amount && category && description) {
                addExpense(amount, category, description);
                this.reset();
                
                // Show success animation
                const button = this.querySelector('button');
                const originalText = button.textContent;
                button.textContent = 'Added! ✅';
                button.classList.add('bg-green-700');
                
                setTimeout(() => {
                    button.textContent = originalText;
                    button.classList.remove('bg-green-700');
                }, 2000);
            }
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'97f0b61a31510267',t:'MTc1Nzg2MTU4MC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
