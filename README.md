<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FitBeast Mode - Your Fitness Journey</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#5D5CDE',
                        'primary-dark': '#4A49C7',
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .gradient-bg { background: linear-gradient(135deg, #5D5CDE 0%, #7C4DFF 100%); }
        .glass-effect { backdrop-filter: blur(10px); background: rgba(255, 255, 255, 0.1); }
        .progress-ring { transition: stroke-dashoffset 0.5s ease-in-out; }
        .workout-card { transition: all 0.3s ease; }
        .workout-card:hover { transform: translateY(-2px); }
        .nav-btn { 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            padding: 8px; 
            border-radius: 8px; 
            transition: all 0.2s; 
            color: #6B7280; 
        }
        .nav-btn.active { 
            color: #5D5CDE; 
            background-color: rgba(93, 92, 222, 0.1); 
        }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .loading-dot {
            animation: bounce 1.4s ease-in-out infinite both;
        }
        .loading-dot:nth-child(1) { animation-delay: -0.32s; }
        .loading-dot:nth-child(2) { animation-delay: -0.16s; }
        @keyframes bounce {
            0%, 80%, 100% { transform: scale(0); }
            40% { transform: scale(1); }
        }
        .fade-in { animation: fadeIn 0.5s ease-in; }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="bg-white dark:bg-gray-900 transition-colors duration-300">
    <!-- Dark Mode Script -->
    <script>
        if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
            document.documentElement.classList.add('dark');
        }
        window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
            if (event.matches) {
                document.documentElement.classList.add('dark');
            } else {
                document.documentElement.classList.remove('dark');
            }
        });
    </script>

    <!-- Loading Screen -->
    <div id="loadingScreen" class="fixed inset-0 gradient-bg flex items-center justify-center z-50">
        <div class="text-center text-white">
            <div class="text-6xl mb-6">💪</div>
            <h1 class="text-3xl font-bold mb-4">FitBeast Mode</h1>
            <p class="text-lg opacity-80 mb-8">Getting your fitness journey ready...</p>
            <div class="flex justify-center space-x-2">
                <div class="loading-dot w-3 h-3 bg-white rounded-full"></div>
                <div class="loading-dot w-3 h-3 bg-white rounded-full"></div>
                <div class="loading-dot w-3 h-3 bg-white rounded-full"></div>
            </div>
        </div>
    </div>

    <!-- Login/Signup Screen -->
    <div id="authScreen" class="fixed inset-0 gradient-bg flex items-center justify-center z-40 hidden">
        <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl max-w-sm w-full mx-4 p-8">
            <div class="text-center mb-8">
                <div class="text-4xl mb-4">💪</div>
                <h2 class="text-2xl font-bold text-gray-800 dark:text-white mb-2">Welcome to FitBeast</h2>
                <p class="text-gray-600 dark:text-gray-400">Your 30-day transformation starts here!</p>
            </div>

            <!-- Login Form -->
            <div id="loginForm">
                <div class="space-y-4">
                    <input type="email" id="loginEmail" placeholder="Email address" 
                           class="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                    <input type="password" id="loginPassword" placeholder="Password" 
                           class="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                    <button id="loginBtn" class="w-full bg-primary hover:bg-primary-dark text-white font-semibold py-4 px-6 rounded-xl transition-colors duration-200">
                        Sign In
                    </button>
                </div>
                <div class="text-center mt-6">
                    <button id="showSignup" class="text-primary hover:text-primary-dark text-sm">
                        Don't have an account? Sign up here
                    </button>
                </div>
            </div>

            <!-- Signup Form -->
            <div id="signupForm" class="hidden">
                <div class="space-y-4">
                    <input type="text" id="signupName" placeholder="Full name" 
                           class="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                    <input type="email" id="signupEmail" placeholder="Email address" 
                           class="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                    <input type="password" id="signupPassword" placeholder="Create password" 
                           class="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                    <select id="fitnessLevel" class="w-full p-4 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                        <option value="">Select fitness level</option>
                        <option value="beginner">Beginner (New to exercise)</option>
                        <option value="intermediate">Intermediate (Some experience)</option>
                        <option value="advanced">Advanced (Regular exerciser)</option>
                    </select>
                    <button id="signupBtn" class="w-full bg-primary hover:bg-primary-dark text-white font-semibold py-4 px-6 rounded-xl transition-colors duration-200">
                        Create Account
                    </button>
                </div>
                <div class="text-center mt-6">
                    <button id="showLogin" class="text-primary hover:text-primary-dark text-sm">
                        Already have an account? Sign in
                    </button>
                </div>
            </div>
        </div>
    </div>

    <div id="app" class="min-h-screen hidden">
        <!-- Header -->
        <header class="gradient-bg text-white p-4 sticky top-0 z-50">
            <div class="max-w-md mx-auto flex items-center justify-between">
                <button id="menuBtn" class="flex items-center justify-center w-10 h-10 rounded-lg hover:bg-white/10 transition-colors duration-200">
                    <div class="grid grid-cols-2 gap-1">
                        <div class="w-2 h-2 bg-white rounded-sm"></div>
                        <div class="w-2 h-2 bg-white rounded-sm"></div>
                        <div class="w-2 h-2 bg-white rounded-sm"></div>
                        <div class="w-2 h-2 bg-white rounded-sm"></div>
                    </div>
                </button>
                <div class="flex items-center space-x-3">
                    <div class="text-2xl">💪</div>
                    <h1 class="text-xl font-bold">FitBeast Mode</h1>
                </div>
                <div class="flex items-center space-x-3">
                    <div class="text-center">
                        <div class="text-lg font-bold" id="currentDay">Day 1</div>
                        <div class="text-xs opacity-80">of 30</div>
                    </div>
                </div>
            </div>
        </header>

        <!-- App Menu Dropdown -->
        <div id="appMenu" class="fixed top-16 left-4 right-4 max-w-md mx-auto bg-white dark:bg-gray-800 rounded-2xl shadow-xl border border-gray-200 dark:border-gray-700 z-40 hidden">
            <div class="p-4">
                <div class="space-y-3">
                    <button class="w-full flex items-center space-x-3 p-3 text-left hover:bg-gray-50 dark:hover:bg-gray-700 rounded-xl transition-colors duration-200" onclick="switchToProfile()">
                        <i class="fas fa-user text-gray-500"></i>
                        <span class="text-gray-800 dark:text-white">My Profile</span>
                    </button>
                    <button class="w-full flex items-center space-x-3 p-3 text-left hover:bg-gray-50 dark:hover:bg-gray-700 rounded-xl transition-colors duration-200" onclick="switchToSettings()">
                        <i class="fas fa-cog text-gray-500"></i>
                        <span class="text-gray-800 dark:text-white">Settings</span>
                    </button>
                    <button class="w-full flex items-center space-x-3 p-3 text-left hover:bg-gray-50 dark:hover:bg-gray-700 rounded-xl transition-colors duration-200" onclick="showStats()">
                        <i class="fas fa-chart-line text-gray-500"></i>
                        <span class="text-gray-800 dark:text-white">Stats & Analytics</span>
                    </button>
                    <button class="w-full flex items-center space-x-3 p-3 text-left hover:bg-gray-50 dark:hover:bg-gray-700 rounded-xl transition-colors duration-200" onclick="shareApp()">
                        <i class="fas fa-share text-gray-500"></i>
                        <span class="text-gray-800 dark:text-white">Share FitBeast</span>
                    </button>
                    <hr class="border-gray-200 dark:border-gray-700">
                    <button class="w-full flex items-center space-x-3 p-3 text-left hover:bg-gray-50 dark:hover:bg-gray-700 rounded-xl transition-colors duration-200" onclick="showHelp()">
                        <i class="fas fa-question-circle text-gray-500"></i>
                        <span class="text-gray-800 dark:text-white">Help & Support</span>
                    </button>
                    <button class="w-full flex items-center space-x-3 p-3 text-left hover:bg-red-50 dark:hover:bg-red-900/20 rounded-xl transition-colors duration-200 text-red-600" onclick="logout()">
                        <i class="fas fa-sign-out-alt"></i>
                        <span>Sign Out</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- Bottom Navigation -->
        <nav class="fixed bottom-0 left-0 right-0 bg-white dark:bg-gray-800 border-t border-gray-200 dark:border-gray-700 z-40">
            <div class="max-w-md mx-auto px-4 py-2">
                <div class="grid grid-cols-5 gap-1">
                    <button class="nav-btn active" data-tab="home">
                        <i class="fas fa-home text-lg mb-1"></i>
                        <span class="text-xs">Home</span>
                    </button>
                    <button class="nav-btn" data-tab="workouts">
                        <i class="fas fa-dumbbell text-lg mb-1"></i>
                        <span class="text-xs">Workouts</span>
                    </button>
                    <button class="nav-btn" data-tab="nutrition">
                        <i class="fas fa-apple-alt text-lg mb-1"></i>
                        <span class="text-xs">Nutrition</span>
                    </button>
                    <button class="nav-btn" data-tab="chat">
                        <i class="fas fa-robot text-lg mb-1"></i>
                        <span class="text-xs">AI Coach</span>
                    </button>
                    <button class="nav-btn" data-tab="profile">
                        <i class="fas fa-user text-lg mb-1"></i>
                        <span class="text-xs">Profile</span>
                    </button>
                </div>
            </div>
        </nav>

        <!-- Main Content -->
        <main class="max-w-md mx-auto p-4 pb-20">
            <!-- Home Tab -->
            <div id="homeTab" class="tab-content active fade-in">
                <!-- Welcome Message -->
                <div class="bg-gradient-to-r from-primary to-purple-500 text-white rounded-2xl p-6 mb-6">
                    <h2 class="text-xl font-bold mb-2">Hey <span id="welcomeName">Fitness Warrior</span>! 👋</h2>
                    <p class="opacity-90">Ready to crush today's workout? You're doing amazing!</p>
                </div>

                <!-- Progress Overview -->
                <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6 mb-6 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-32 h-32 bg-primary/10 rounded-full -mr-16 -mt-16"></div>
                    <div class="relative">
                        <h2 class="text-2xl font-bold text-gray-800 dark:text-white mb-4">Your Progress</h2>
                        <div class="flex items-center justify-between mb-4">
                            <div class="flex-1">
                                <div class="flex items-center justify-between mb-2">
                                    <span class="text-sm text-gray-600 dark:text-gray-400">Overall Progress</span>
                                    <span class="text-sm font-semibold text-primary" id="progressPercent">0%</span>
                                </div>
                                <div class="bg-gray-200 dark:bg-gray-700 rounded-full h-3">
                                    <div class="bg-gradient-to-r from-primary to-purple-500 h-3 rounded-full transition-all duration-500" 
                                         id="progressBar" style="width: 0%"></div>
                                </div>
                            </div>
                            <div class="ml-6">
                                <svg class="w-16 h-16 transform -rotate-90" viewBox="0 0 36 36">
                                    <path class="text-gray-200 dark:text-gray-700" stroke="currentColor" stroke-width="3" 
                                          fill="transparent" d="m18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831"/>
                                    <path class="text-primary progress-ring" stroke="currentColor" stroke-width="3" 
                                          fill="transparent" stroke-dasharray="0, 100" stroke-linecap="round"
                                          d="m18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" 
                                          id="progressRing"/>
                                </svg>
                            </div>
                        </div>
                        <div class="grid grid-cols-3 gap-4 text-center">
                            <div>
                                <div class="text-lg font-bold text-gray-800 dark:text-white" id="completedWorkouts">0</div>
                                <div class="text-xs text-gray-600 dark:text-gray-400">Completed</div>
                            </div>
                            <div>
                                <div class="text-lg font-bold text-gray-800 dark:text-white" id="currentStreak">0</div>
                                <div class="text-xs text-gray-600 dark:text-gray-400">Day Streak</div>
                            </div>
                            <div>
                                <div class="text-lg font-bold text-gray-800 dark:text-white" id="totalMinutes">0</div>
                                <div class="text-xs text-gray-600 dark:text-gray-400">Minutes</div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Today's Workout -->
                <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6 mb-6">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="text-xl font-bold text-gray-800 dark:text-white">Today's Challenge</h3>
                        <span class="bg-green-100 dark:bg-green-900 text-green-800 dark:text-green-200 px-3 py-1 rounded-full text-sm font-medium" 
                              id="workoutDifficulty">Beginner</span>
                    </div>
                    
                    <div class="space-y-4" id="todayWorkout">
                        <!-- Workout content will be populated by JavaScript -->
                    </div>
                    
                    <button class="w-full bg-primary hover:bg-primary-dark text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200 mt-4" 
                            id="startWorkoutBtn">
                        <i class="fas fa-play mr-2"></i>Let's Do This!
                    </button>
                </div>

                <!-- Weekly Overview -->
                <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6 mb-6">
                    <h3 class="text-xl font-bold text-gray-800 dark:text-white mb-4">This Week</h3>
                    <div class="grid grid-cols-7 gap-2" id="weekProgress">
                        <!-- Week days will be populated by JavaScript -->
                    </div>
                </div>

                <!-- Achievements -->
                <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                    <h3 class="text-xl font-bold text-gray-800 dark:text-white mb-4">Achievements</h3>
                    <div class="grid grid-cols-2 gap-4" id="achievements">
                        <!-- Achievements will be populated by JavaScript -->
                    </div>
                </div>
            </div>

            <!-- Workouts Tab -->
            <div id="workoutsTab" class="tab-content hidden">
                <div class="space-y-4">
                    <!-- Today's Detailed Workout -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-xl font-bold text-gray-800 dark:text-white mb-4">📋 Today's Full Workout</h3>
                        <div id="detailedWorkout" class="space-y-4">
                            <!-- Detailed workout will be populated -->
                        </div>
                        <div class="mt-6 space-y-3">
                            <button class="w-full bg-primary hover:bg-primary-dark text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200" onclick="startGuidedWorkout()">
                                <i class="fas fa-play mr-2"></i>Start Guided Workout
                            </button>
                            <button class="w-full bg-green-500 hover:bg-green-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200" onclick="openWorkoutModal()">
                                <i class="fas fa-list mr-2"></i>Quick View Exercises
                            </button>
                        </div>
                    </div>

                    <!-- Form Check Videos -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-xl font-bold text-gray-800 dark:text-white mb-4">📹 Perfect Your Form</h3>
                        <p class="text-gray-600 dark:text-gray-400 mb-4">Not sure about your technique? Get instant demo videos!</p>
                        <div class="space-y-3">
                            <select id="videoWorkoutType" class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                                <option value="pushups">Perfect Push-ups Tutorial</option>
                                <option value="squats">Proper Squat Form</option>
                                <option value="plank">Rock-Solid Plank Technique</option>
                                <option value="jumping_jacks">Jumping Jacks Demo</option>
                                <option value="mountain_climbers">Mountain Climbers Mastery</option>
                            </select>
                            <button id="generateVideoBtn" class="w-full bg-red-500 hover:bg-red-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                                <i class="fas fa-video mr-2"></i>Get Form Video
                            </button>
                        </div>
                        <div id="videoContainer" class="mt-4 hidden">
                            <div class="bg-gray-100 dark:bg-gray-700 rounded-xl p-4">
                                <video id="workoutVideo" class="w-full rounded-lg" controls>
                                    Your browser does not support the video tag.
                                </video>
                            </div>
                        </div>
                        <div id="videoLoading" class="mt-4 hidden text-center">
                            <div class="inline-flex items-center px-4 py-2 bg-blue-500 text-white rounded-lg">
                                <i class="fas fa-spinner fa-spin mr-2"></i>
                                Creating your form video...
                            </div>
                        </div>
                    </div>

                    <!-- 30-Day Program Overview -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-xl font-bold text-gray-800 dark:text-white mb-4">📚 Your 30-Day Journey</h3>
                        <div class="space-y-2" id="workoutLibrary">
                            <!-- Workout library will be populated -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Nutrition Tab -->
            <div id="nutritionTab" class="tab-content hidden">
                <div class="space-y-4">
                    <!-- Meal Planner -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-xl font-bold text-gray-800 dark:text-white mb-4">🍎 What Should I Eat?</h3>
                        <p class="text-gray-600 dark:text-gray-400 mb-4">Honestly, figuring out meals is hard. Let me help you plan something that actually tastes good!</p>
                        <div class="space-y-3">
                            <select id="dietGoal" class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                                <option value="weight_loss">Weight Loss</option>
                                <option value="muscle_gain">Muscle Gain</option>
                                <option value="maintenance">Maintenance</option>
                                <option value="endurance">Endurance Training</option>
                            </select>
                            <select id="dietType" class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                                <option value="balanced">Balanced Diet</option>
                                <option value="vegetarian">Vegetarian</option>
                                <option value="vegan">Vegan</option>
                                <option value="keto">Ketogenic</option>
                                <option value="paleo">Paleo</option>
                            </select>
                            <button id="generateMealPlanBtn" class="w-full bg-green-500 hover:bg-green-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                                <i class="fas fa-utensils mr-2"></i>Generate Meal Plan
                            </button>
                        </div>
                        <div id="mealPlanContainer" class="mt-4 hidden">
                            <!-- Meal plan will be displayed here -->
                        </div>
                        <div id="mealPlanLoading" class="mt-4 hidden text-center">
                            <div class="inline-flex items-center px-4 py-2 bg-green-500 text-white rounded-lg">
                                <i class="fas fa-spinner fa-spin mr-2"></i>
                                Creating your meal plan...
                            </div>
                        </div>
                    </div>

                    <!-- Quick Nutrition Tips -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-xl font-bold text-gray-800 dark:text-white mb-4">💡 Random Tips I Found</h3>
                        <div id="nutritionTips" class="space-y-3">
                            <div class="p-4 bg-blue-50 dark:bg-blue-900/20 rounded-xl border-l-4 border-blue-500">
                                <p class="text-sm text-gray-700 dark:text-gray-300">💧 Yeah, I know everyone says "drink water" but seriously... you're probably not drinking enough. Your muscles will thank you!</p>
                            </div>
                            <div class="p-4 bg-green-50 dark:bg-green-900/20 rounded-xl border-l-4 border-green-500">
                                <p class="text-sm text-gray-700 dark:text-gray-300">🥗 Pro tip: If you're hungry an hour after eating, you probably didn't get enough protein. Add some eggs or chicken!</p>
                            </div>
                            <div class="p-4 bg-orange-50 dark:bg-orange-900/20 rounded-xl border-l-4 border-orange-500">
                                <p class="text-sm text-gray-700 dark:text-gray-300">🍌 Bananas before workouts = instant energy. It's like nature's pre-workout drink but way cheaper!</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- AI Chat Tab -->
            <div id="chatTab" class="tab-content hidden">
                <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg h-96 flex flex-col">
                    <div class="gradient-bg text-white p-4 rounded-t-2xl flex items-center">
                        <div class="text-xl mr-3">🤖</div>
                        <div>
                            <h3 class="font-bold">Coach Mike</h3>
                            <p class="text-sm opacity-80">Your friendly neighborhood fitness buddy!</p>
                        </div>
                    </div>
                    <div id="chatMessages" class="flex-1 p-4 overflow-y-auto space-y-3">
                        <div class="bg-gray-100 dark:bg-gray-700 rounded-lg p-3 max-w-[80%]">
                            <p class="text-sm text-gray-800 dark:text-white">Hey there! 👋 I'm Mike, and I'm here to help you figure out this whole fitness thing. Don't worry, I won't judge you for eating pizza last night... we've all been there! 😅</p>
                        </div>
                    </div>
                    <div class="p-4 border-t border-gray-200 dark:border-gray-700">
                        <div class="flex space-x-2">
                            <input type="text" id="chatInput" placeholder="Ask about diet, nutrition, workouts..." 
                                   class="flex-1 p-3 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                            <button id="sendChatBtn" class="bg-primary hover:bg-primary-dark text-white p-3 rounded-xl transition-colors duration-200">
                                <i class="fas fa-paper-plane"></i>
                            </button>
                        </div>
                        <div class="flex flex-wrap gap-2 mt-3">
                            <button class="quick-question text-xs bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-300 px-3 py-1 rounded-full" data-question="What should I eat before a workout?">
                                Pre-workout meals
                            </button>
                            <button class="quick-question text-xs bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-300 px-3 py-1 rounded-full" data-question="How much protein do I need daily?">
                                Daily protein
                            </button>
                            <button class="quick-question text-xs bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-300 px-3 py-1 rounded-full" data-question="Best foods for muscle recovery?">
                                Recovery foods
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Profile Tab -->
            <div id="profileTab" class="tab-content hidden fade-in">
                <div class="space-y-6">
                    <!-- Profile Header -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <div class="text-center">
                            <div class="w-24 h-24 bg-primary rounded-full flex items-center justify-center text-white text-3xl font-bold mx-auto mb-4" id="profileAvatar">
                                <!-- User initials will go here -->
                            </div>
                            <h2 class="text-2xl font-bold text-gray-800 dark:text-white mb-2" id="profileName">Loading...</h2>
                            <p class="text-gray-600 dark:text-gray-400 mb-4" id="profileEmail">email@example.com</p>
                            <div class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-green-100 dark:bg-green-900 text-green-800 dark:text-green-200">
                                <i class="fas fa-dumbbell mr-2"></i>
                                <span id="profileLevel">Beginner</span>
                            </div>
                        </div>
                    </div>

                    <!-- Stats Cards -->
                    <div class="grid grid-cols-2 gap-4">
                        <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6 text-center">
                            <div class="text-3xl font-bold text-primary mb-2" id="profileDaysCompleted">0</div>
                            <div class="text-sm text-gray-600 dark:text-gray-400">Days Completed</div>
                        </div>
                        <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6 text-center">
                            <div class="text-3xl font-bold text-primary mb-2" id="profileTotalMinutes">0</div>
                            <div class="text-sm text-gray-600 dark:text-gray-400">Total Minutes</div>
                        </div>
                    </div>

                    <!-- Fitness Journey -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-xl font-bold text-gray-800 dark:text-white mb-4">Your Fitness Journey</h3>
                        <div class="space-y-4">
                            <div class="flex items-center justify-between p-3 bg-gray-50 dark:bg-gray-700 rounded-lg">
                                <div class="flex items-center space-x-3">
                                    <div class="w-10 h-10 bg-green-500 rounded-full flex items-center justify-center text-white">
                                        <i class="fas fa-play"></i>
                                    </div>
                                    <div>
                                        <div class="font-semibold text-gray-800 dark:text-white">Started Challenge</div>
                                        <div class="text-sm text-gray-600 dark:text-gray-400" id="startDate">Today</div>
                                    </div>
                                </div>
                                <i class="fas fa-check text-green-500"></i>
                            </div>
                        </div>
                    </div>

                    <!-- Edit Profile Button -->
                    <button onclick="editProfile()" class="w-full bg-primary hover:bg-primary-dark text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                        <i class="fas fa-edit mr-2"></i>Edit Profile
                    </button>
                </div>
            </div>

            <!-- Settings Tab -->
            <div id="settingsTab" class="tab-content hidden fade-in">
                <div class="space-y-6">
                    <h2 class="text-2xl font-bold text-gray-800 dark:text-white">Settings</h2>

                    <!-- Notifications -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">Notifications</h3>
                        <div class="space-y-4">
                            <div class="flex items-center justify-between">
                                <div>
                                    <div class="font-medium text-gray-800 dark:text-white">Daily Reminders</div>
                                    <div class="text-sm text-gray-600 dark:text-gray-400">Get reminded to workout</div>
                                </div>
                                <label class="relative inline-flex items-center cursor-pointer">
                                    <input type="checkbox" id="dailyReminders" class="sr-only peer" checked>
                                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-primary"></div>
                                </label>
                            </div>
                            <div class="flex items-center justify-between">
                                <div>
                                    <div class="font-medium text-gray-800 dark:text-white">Sound Effects</div>
                                    <div class="text-sm text-gray-600 dark:text-gray-400">Button clicks and success sounds</div>
                                </div>
                                <label class="relative inline-flex items-center cursor-pointer">
                                    <input type="checkbox" id="soundEffects" class="sr-only peer" checked>
                                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-primary"></div>
                                </label>
                            </div>
                        </div>
                    </div>

                    <!-- Workout Preferences -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">Workout Preferences</h3>
                        <div class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Difficulty Level</label>
                                <select id="difficultyLevel" class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-xl text-base bg-white dark:bg-gray-700 text-gray-800 dark:text-white">
                                    <option value="beginner">Beginner</option>
                                    <option value="intermediate">Intermediate</option>
                                    <option value="advanced">Advanced</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <!-- Save Button -->
                    <button onclick="saveSettings()" class="w-full bg-primary hover:bg-primary-dark text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                        <i class="fas fa-save mr-2"></i>Save Settings
                    </button>

                    <!-- Data Management -->
                    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6">
                        <h3 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">Data Management</h3>
                        <div class="space-y-3">
                            <button onclick="exportData()" class="w-full bg-blue-500 hover:bg-blue-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                                <i class="fas fa-download mr-2"></i>Export My Data
                            </button>
                            <button onclick="resetProgress()" class="w-full bg-red-500 hover:bg-red-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                                <i class="fas fa-redo mr-2"></i>Reset Progress
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </main>

        <!-- Workout Modal -->
        <div id="workoutModal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden items-center justify-center p-4">
            <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl max-w-md w-full max-h-[90vh] overflow-y-auto">
                <div class="gradient-bg text-white p-6 rounded-t-2xl">
                    <div class="flex items-center justify-between">
                        <h3 class="text-xl font-bold" id="modalWorkoutTitle">Day 1 Workout</h3>
                        <button id="closeModal" class="text-white hover:text-gray-200">
                            <i class="fas fa-times text-xl"></i>
                        </button>
                    </div>
                    <div class="mt-2">
                        <div class="text-lg font-semibold" id="modalTimer">25:00</div>
                        <div class="text-sm opacity-80">Estimated Time</div>
                    </div>
                </div>
                <div class="p-6">
                    <div id="exerciseList" class="space-y-4">
                        <!-- Exercise list will be populated by JavaScript -->
                    </div>
                    <div class="mt-6 space-y-3">
                        <button id="startTimerBtn" class="w-full bg-primary hover:bg-primary-dark text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                            <i class="fas fa-play mr-2"></i>Start Timer
                        </button>
                        <button id="completeWorkoutBtn" class="w-full bg-green-500 hover:bg-green-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                            <i class="fas fa-check mr-2"></i>Mark as Complete
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Timer Modal -->
        <div id="timerModal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden items-center justify-center p-4">
            <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl max-w-sm w-full p-8 text-center">
                <div class="mb-6">
                    <div class="text-4xl font-bold text-primary mb-2" id="timerDisplay">25:00</div>
                    <div class="text-gray-600 dark:text-gray-400" id="currentExercise">Get Ready!</div>
                </div>
                <div class="space-y-4">
                    <button id="pauseTimerBtn" class="bg-yellow-500 hover:bg-yellow-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200 mr-2">
                        <i class="fas fa-pause mr-2"></i>Pause
                    </button>
                    <button id="stopTimerBtn" class="bg-red-500 hover:bg-red-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                        <i class="fas fa-stop mr-2"></i>Stop
                    </button>
                </div>
            </div>
        </div>

        <!-- Success Modal -->
        <div id="successModal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden items-center justify-center p-4">
            <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl max-w-sm w-full p-8 text-center">
                <div class="text-6xl mb-4">🎉</div>
                <h3 class="text-2xl font-bold text-gray-800 dark:text-white mb-2">BOOM! You Did It!</h3>
                <p class="text-gray-600 dark:text-gray-400 mb-6">Seriously, that was awesome! Your future self is already thanking you.</p>
                <button id="closeSuccessModal" class="bg-primary hover:bg-primary-dark text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
                    Continue
                </button>
            </div>
        </div>
    </div>

    <script>
        // Enhanced App State with User Management
        let appState = {
            user: null,
            currentDay: 1,
            completedWorkouts: 0,
            currentStreak: 0,
            totalMinutes: 0,
            completedDays: [],
            settings: {
                dailyReminders: true,
                achievementAlerts: true,
                workoutTime: 'morning',
                difficultyLevel: 'beginner',
                soundEffects: true,
                autoSave: true
            }
        };

        // Enhanced Workout Program with Detailed Instructions
        const workoutProgram = {
            1: {
                title: "Foundation Day",
                difficulty: "Beginner",
                duration: 25,
                description: "Welcome to your fitness journey! Today we're building the foundation with basic movements.",
                warmup: "2 minutes of gentle arm circles and marching in place",
                cooldown: "5 minutes of stretching focusing on major muscle groups",
                exercises: [
                    { 
                        name: "Jumping Jacks", 
                        duration: "3 sets", 
                        reps: "30 seconds each", 
                        rest: "30 seconds between sets",
                        instructions: "Start standing with feet together, arms at sides. Jump feet apart while raising arms overhead. Jump back to start. Keep a steady rhythm!",
                        tips: "Land softly on the balls of your feet. If too challenging, step side to side instead of jumping.",
                        calories: "~15 calories"
                    },
                    { 
                        name: "Modified Push-ups", 
                        duration: "3 sets", 
                        reps: "8-12 reps", 
                        rest: "45 seconds between sets",
                        instructions: "Start on knees and hands, body straight from knees to head. Lower chest toward floor, push back up.",
                        tips: "Keep core tight, don't let hips sag. Focus on controlled movement over speed.",
                        calories: "~20 calories"
                    },
                    { 
                        name: "Bodyweight Squats", 
                        duration: "3 sets", 
                        reps: "10-15 reps", 
                        rest: "45 seconds between sets",
                        instructions: "Stand with feet shoulder-width apart. Lower down like sitting in a chair, keeping chest up. Push through heels to stand.",
                        tips: "Don't let knees cave inward. Pretend you're sitting back into a chair.",
                        calories: "~25 calories"
                    }
                ]
            }
        };

        // Sound Effects System
        let audioContext;
        
        function initAudioContext() {
            try {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
            } catch (e) {
                console.log('Audio not supported');
            }
        }
        
        function playSound(frequency, duration, type = 'sine') {
            if (!appState.settings.soundEffects || !audioContext) return;
            
            try {
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.frequency.value = frequency;
                oscillator.type = type;
                
                gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + duration);
                
                oscillator.start();
                oscillator.stop(audioContext.currentTime + duration);
            } catch (e) {
                console.log('Sound play failed');
            }
        }

        function playSuccessSound() {
            if (!appState.settings.soundEffects) return;
            playSound(523, 0.2);
            setTimeout(() => playSound(659, 0.2), 100);
            setTimeout(() => playSound(784, 0.3), 200);
        }

        function playClickSound() {
            if (!appState.settings.soundEffects) return;
            playSound(800, 0.1, 'square');
        }

        function playNotificationSound() {
            if (!appState.settings.soundEffects) return;
            playSound(440, 0.15);
            setTimeout(() => playSound(554, 0.15), 150);
        }

        // Initialize App with Loading Screen
        function initApp() {
            initAudioContext();
            showLoadingScreen();
            setTimeout(() => {
                checkUserLogin();
            }, 2000);
        }

        function showLoadingScreen() {
            const loadingScreen = document.getElementById('loadingScreen');
            if (loadingScreen) {
                loadingScreen.classList.remove('hidden');
            }
        }

        function hideLoadingScreen() {
            const loadingScreen = document.getElementById('loadingScreen');
            if (loadingScreen) {
                loadingScreen.classList.add('hidden');
            }
        }

        function checkUserLogin() {
            try {
                const savedUser = localStorage.getItem('fitbeast_user');
                const savedState = localStorage.getItem('fitbeast_state');
                
                if (savedUser && savedState) {
                    appState.user = JSON.parse(savedUser);
                    const savedAppState = JSON.parse(savedState);
                    appState = { ...appState, ...savedAppState };
                    hideLoadingScreen();
                    showApp();
                    updateUI();
                } else {
                    hideLoadingScreen();
                    showAuthScreen();
                }
            } catch (e) {
                console.error('Error checking login:', e);
                hideLoadingScreen();
                showAuthScreen();
            }
        }

        function showAuthScreen() {
            const authScreen = document.getElementById('authScreen');
            if (authScreen) {
                authScreen.classList.remove('hidden');
                setupAuthListeners();
            }
        }

        function hideAuthScreen() {
            const authScreen = document.getElementById('authScreen');
            if (authScreen) {
                authScreen.classList.add('hidden');
            }
        }

        function showApp() {
            const app = document.getElementById('app');
            if (app) {
                app.classList.remove('hidden');
                setupEventListeners();
                updateProgress();
                updateTodayWorkout();
                updateWeekProgress();
                updateAchievements();
                updateProfile();
                populateDetailedWorkout();
            }
        }

        function setupAuthListeners() {
            const showSignup = document.getElementById('showSignup');
            const showLogin = document.getElementById('showLogin');
            const loginBtn = document.getElementById('loginBtn');
            const signupBtn = document.getElementById('signupBtn');

            if (showSignup) {
                showSignup.addEventListener('click', () => {
                    document.getElementById('loginForm').classList.add('hidden');
                    document.getElementById('signupForm').classList.remove('hidden');
                });
            }

            if (showLogin) {
                showLogin.addEventListener('click', () => {
                    document.getElementById('signupForm').classList.add('hidden');
                    document.getElementById('loginForm').classList.remove('hidden');
                });
            }

            if (loginBtn) loginBtn.addEventListener('click', handleLogin);
            if (signupBtn) signupBtn.addEventListener('click', handleSignup);
        }

        function handleLogin() {
            const email = document.getElementById('loginEmail')?.value;
            const password = document.getElementById('loginPassword')?.value;

            if (!email || !password) {
                showCustomAlert('Please fill in all fields');
                return;
            }

            playClickSound();
            
            try {
                const savedUsers = JSON.parse(localStorage.getItem('fitbeast_users') || '{}');
                
                if (savedUsers[email] && savedUsers[email].password === password) {
                    appState.user = savedUsers[email];
                    localStorage.setItem('fitbeast_user', JSON.stringify(appState.user));
                    
                    // Load user's progress
                    const userProgress = localStorage.getItem(`fitbeast_progress_${email}`);
                    if (userProgress) {
                        const progress = JSON.parse(userProgress);
                        appState = { ...appState, ...progress };
                    }
                    
                    hideAuthScreen();
                    showApp();
                    showCustomAlert(`Welcome back, ${appState.user.name}! Ready to continue your fitness journey? 💪`);
                } else {
                    showCustomAlert('Invalid email or password. Try again!');
                }
            } catch (e) {
                console.error('Login error:', e);
                showCustomAlert('Login failed. Please try again.');
            }
        }

        function handleSignup() {
            const name = document.getElementById('signupName')?.value;
            const email = document.getElementById('signupEmail')?.value;
            const password = document.getElementById('signupPassword')?.value;
            const fitnessLevel = document.getElementById('fitnessLevel')?.value;

            if (!name || !email || !password || !fitnessLevel) {
                showCustomAlert('Please fill in all fields');
                return;
            }

            playClickSound();
            
            try {
                // Create new user
                const newUser = {
                    name,
                    email,
                    fitnessLevel,
                    password,
                    joinDate: new Date().toISOString()
                };

                // Save to users database
                const savedUsers = JSON.parse(localStorage.getItem('fitbeast_users') || '{}');
                savedUsers[email] = newUser;
                localStorage.setItem('fitbeast_users', JSON.stringify(savedUsers));

                // Set current user
                appState.user = newUser;
                appState.settings.difficultyLevel = fitnessLevel;
                localStorage.setItem('fitbeast_user', JSON.stringify(appState.user));
                
                hideAuthScreen();
                showApp();
                showCustomAlert(`Welcome to FitBeast Mode, ${name}! 🎉 Let's start your transformation journey!`);
            } catch (e) {
                console.error('Signup error:', e);
                showCustomAlert('Signup failed. Please try again.');
            }
        }

        function logout() {
            playClickSound();
            showConfirmDialog('Are you sure you want to sign out?', () => {
                localStorage.removeItem('fitbeast_user');
                saveProgress();
                location.reload();
            });
        }

        function saveProgress() {
            if (appState.user) {
                try {
                    const progressData = {
                        currentDay: appState.currentDay,
                        completedWorkouts: appState.completedWorkouts,
                        currentStreak: appState.currentStreak,
                        totalMinutes: appState.totalMinutes,
                        completedDays: appState.completedDays,
                        settings: appState.settings,
                        lastSaved: new Date().toISOString()
                    };
                    localStorage.setItem(`fitbeast_progress_${appState.user.email}`, JSON.stringify(progressData));
                    localStorage.setItem('fitbeast_state', JSON.stringify(progressData));
                } catch (e) {
                    console.error('Save progress error:', e);
                }
            }
        }

        function updateUI() {
            if (appState.user) {
                const welcomeName = document.getElementById('welcomeName');
                if (welcomeName) {
                    welcomeName.textContent = appState.user.name.split(' ')[0];
                }
            }
        }

        function updateProfile() {
            if (appState.user) {
                const initials = appState.user.name.split(' ').map(n => n[0]).join('').toUpperCase();
                
                const profileAvatar = document.getElementById('profileAvatar');
                const profileName = document.getElementById('profileName');
                const profileEmail = document.getElementById('profileEmail');
                const profileLevel = document.getElementById('profileLevel');
                const profileDaysCompleted = document.getElementById('profileDaysCompleted');
                const profileTotalMinutes = document.getElementById('profileTotalMinutes');
                const startDate = document.getElementById('startDate');

                if (profileAvatar) profileAvatar.textContent = initials;
                if (profileName) profileName.textContent = appState.user.name;
                if (profileEmail) profileEmail.textContent = appState.user.email;
                if (profileLevel) profileLevel.textContent = appState.user.fitnessLevel.charAt(0).toUpperCase() + appState.user.fitnessLevel.slice(1);
                if (profileDaysCompleted) profileDaysCompleted.textContent = appState.completedWorkouts;
                if (profileTotalMinutes) profileTotalMinutes.textContent = appState.totalMinutes;
                
                // Set start date
                if (startDate) {
                    const joinDate = new Date(appState.user.joinDate);
                    startDate.textContent = joinDate.toLocaleDateString();
                }
            }
        }

        function populateDetailedWorkout() {
            const today = workoutProgram[appState.currentDay] || workoutProgram[1];
            const container = document.getElementById('detailedWorkout');
            
            if (container && today) {
                container.innerHTML = `
                    <div class="bg-blue-50 dark:bg-blue-900/20 rounded-xl p-4 mb-4">
                        <h4 class="font-semibold text-blue-800 dark:text-blue-200 mb-2">${today.title}</h4>
                        <p class="text-blue-700 dark:text-blue-300 text-sm">${today.description}</p>
                    </div>
                    
                    <div class="space-y-4">
                        <div class="p-4 bg-yellow-50 dark:bg-yellow-900/20 rounded-xl border-l-4 border-yellow-500">
                            <h5 class="font-semibold text-yellow-800 dark:text-yellow-200 mb-2">🔥 Warm-up (2 mins)</h5>
                            <p class="text-yellow-700 dark:text-yellow-300 text-sm">${today.warmup}</p>
                        </div>
                        
                        ${today.exercises.map((exercise, index) => `
                            <div class="border border-gray-200 dark:border-gray-700 rounded-xl p-4">
                                <div class="flex items-start space-x-3">
                                    <div class="w-8 h-8 bg-primary text-white rounded-full flex items-center justify-center text-sm font-semibold">
                                        ${index + 1}
                                    </div>
                                    <div class="flex-1">
                                        <h5 class="font-semibold text-gray-800 dark:text-white mb-2">${exercise.name}</h5>
                                        <div class="grid grid-cols-2 gap-4 mb-3 text-sm">
                                            <div>
                                                <span class="text-gray-600 dark:text-gray-400">Duration:</span>
                                                <span class="font-medium text-gray-800 dark:text-white ml-1">${exercise.duration}</span>
                                            </div>
                                            <div>
                                                <span class="text-gray-600 dark:text-gray-400">Reps:</span>
                                                <span class="font-medium text-gray-800 dark:text-white ml-1">${exercise.reps}</span>
                                            </div>
                                        </div>
                                        <div class="space-y-2">
                                            <div class="p-3 bg-gray-50 dark:bg-gray-700 rounded-lg">
                                                <h6 class="font-medium text-gray-800 dark:text-white text-sm mb-1">How to do it:</h6>
                                                <p class="text-gray-600 dark:text-gray-400 text-sm">${exercise.instructions}</p>
                                            </div>
                                            <div class="p-3 bg-green-50 dark:bg-green-900/20 rounded-lg">
                                                <h6 class="font-medium text-green-800 dark:text-green-200 text-sm mb-1">💡 Pro Tips:</h6>
                                                <p class="text-green-700 dark:text-green-300 text-sm">${exercise.tips}</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        `).join('')}
                        
                        <div class="p-4 bg-purple-50 dark:bg-purple-900/20 rounded-xl border-l-4 border-purple-500">
                            <h5 class="font-semibold text-purple-800 dark:text-purple-200 mb-2">🧘 Cool-down (5 mins)</h5>
                            <p class="text-purple-700 dark:text-purple-300 text-sm">${today.cooldown}</p>
                        </div>
                    </div>
                `;
            }
        }

        // Update Progress Function
        function updateProgress() {
            const progress = (appState.completedWorkouts / 30) * 100;
            const progressRing = document.getElementById('progressRing');
            const circumference = 2 * Math.PI * 15.9155;
            const offset = circumference - (progress / 100 * circumference);
            
            const currentDay = document.getElementById('currentDay');
            const progressPercent = document.getElementById('progressPercent');
            const progressBar = document.getElementById('progressBar');
            const completedWorkouts = document.getElementById('completedWorkouts');
            const currentStreak = document.getElementById('currentStreak');
            const totalMinutes = document.getElementById('totalMinutes');

            if (currentDay) currentDay.textContent = `Day ${appState.currentDay}`;
            if (progressPercent) progressPercent.textContent = `${Math.round(progress)}%`;
            if (progressBar) progressBar.style.width = `${progress}%`;
            if (completedWorkouts) completedWorkouts.textContent = appState.completedWorkouts;
            if (currentStreak) currentStreak.textContent = appState.currentStreak;
            if (totalMinutes) totalMinutes.textContent = appState.totalMinutes;
            
            if (progressRing) {
                progressRing.style.strokeDasharray = `${circumference} ${circumference}`;
                progressRing.style.strokeDashoffset = offset;
            }
        }

        function updateTodayWorkout() {
            const today = workoutProgram[appState.currentDay] || workoutProgram[1];
            const workoutDifficulty = document.getElementById('workoutDifficulty');
            const workoutContainer = document.getElementById('todayWorkout');

            if (workoutDifficulty) workoutDifficulty.textContent = today.difficulty;
            
            if (workoutContainer) {
                workoutContainer.innerHTML = `
                    <div class="bg-gray-50 dark:bg-gray-700 rounded-xl p-4">
                        <h4 class="font-semibold text-gray-800 dark:text-white mb-2">${today.title}</h4>
                        <div class="flex items-center text-gray-600 dark:text-gray-400 text-sm space-x-4">
                            <span><i class="fas fa-clock mr-1"></i>${today.duration} min</span>
                            <span><i class="fas fa-fire mr-1"></i>${today.exercises.length} exercises</span>
                        </div>
                    </div>
                    <div class="space-y-2">
                        ${today.exercises.slice(0, 3).map(exercise => `
                            <div class="flex items-center justify-between p-3 bg-gray-50 dark:bg-gray-700 rounded-lg">
                                <span class="text-gray-800 dark:text-white font-medium">${exercise.name}</span>
                                <span class="text-gray-600 dark:text-gray-400 text-sm">${exercise.reps}</span>
                            </div>
                        `).join('')}
                        ${today.exercises.length > 3 ? `
                            <div class="text-center text-gray-500 dark:text-gray-400 text-sm py-2">
                                +${today.exercises.length - 3} more exercises
                            </div>
                        ` : ''}
                    </div>
                `;
            }
        }

        function updateWeekProgress() {
            const weekContainer = document.getElementById('weekProgress');
            const days = ['S', 'M', 'T', 'W', 'T', 'F', 'S'];
            
            if (weekContainer) {
                weekContainer.innerHTML = days.map((day, index) => {
                    const dayNumber = index + 1;
                    const isCompleted = appState.completedDays.includes(dayNumber);
                    const isCurrent = dayNumber === appState.currentDay;
                    
                    return `
                        <div class="text-center">
                            <div class="text-xs text-gray-500 dark:text-gray-400 mb-1">${day}</div>
                            <div class="w-8 h-8 rounded-full flex items-center justify-center text-xs font-semibold
                                        ${isCompleted ? 'bg-primary text-white' : 
                                          isCurrent ? 'bg-primary/20 text-primary border-2 border-primary' : 
                                          'bg-gray-200 dark:bg-gray-700 text-gray-500'}">
                                ${isCompleted ? '✓' : dayNumber}
                            </div>
                        </div>
                    `;
                }).join('');
            }
        }

        function updateAchievements() {
            const achievements = [
                { id: 1, name: "First Step", description: "Complete your first workout", icon: "🏃", unlocked: appState.completedWorkouts >= 1 },
                { id: 2, name: "Week Warrior", description: "Complete 7 consecutive days", icon: "🔥", unlocked: appState.currentStreak >= 7 },
                { id: 3, name: "Halfway Hero", description: "Reach day 15", icon: "💪", unlocked: appState.currentDay >= 15 },
                { id: 4, name: "Transformation", description: "Complete all 30 days", icon: "🏆", unlocked: appState.completedWorkouts >= 30 }
            ];

            const achievementContainer = document.getElementById('achievements');
            if (achievementContainer) {
                achievementContainer.innerHTML = achievements.map(achievement => `
                    <div class="p-4 rounded-xl border-2 transition-all ${achievement.unlocked ? 
                        'border-primary bg-primary/5' : 'border-gray-200 dark:border-gray-700 bg-gray-50 dark:bg-gray-700/50'}">
                        <div class="text-2xl mb-2">${achievement.icon}</div>
                        <h4 class="font-semibold text-gray-800 dark:text-white text-sm mb-1">${achievement.name}</h4>
                        <p class="text-gray-600 dark:text-gray-400 text-xs">${achievement.description}</p>
                    </div>
                `).join('');
            }
        }

        // Navigation System
        function setupNavigation() {
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    playClickSound();
                    const tab = e.currentTarget.dataset.tab;
                    switchTab(tab);
                });
            });
        }

        function switchTab(activeTab) {
            // Update nav buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.toggle('active', btn.dataset.tab === activeTab);
            });

            // Update tab content
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.toggle('active', content.id === activeTab + 'Tab');
            });

            // Initialize tab-specific content
            if (activeTab === 'workouts') {
                populateWorkoutLibrary();
                populateDetailedWorkout();
            } else if (activeTab === 'profile') {
                updateProfile();
            } else if (activeTab === 'settings') {
                loadSettings();
            }
        }

        function populateWorkoutLibrary() {
            const library = document.getElementById('workoutLibrary');
            if (library) {
                const totalDays = 30;
                library.innerHTML = '';
                
                for (let day = 1; day <= Math.min(totalDays, 7); day++) {
                    const workout = workoutProgram[day] || { title: `Day ${day} Workout`, duration: 25 + (day * 2), difficulty: 'Beginner' };
                    const isCompleted = appState.completedDays.includes(day);
                    const isCurrent = day === appState.currentDay;
                    const isLocked = day > appState.currentDay + 1;

                    const workoutEl = document.createElement('div');
                    workoutEl.className = `p-4 rounded-xl border transition-all cursor-pointer ${
                        isCompleted ? 'border-green-500 bg-green-50 dark:bg-green-900/20' :
                        isCurrent ? 'border-primary bg-primary/5' :
                        isLocked ? 'border-gray-200 dark:border-gray-700 bg-gray-100 dark:bg-gray-800 opacity-50' :
                        'border-gray-200 dark:border-gray-700 hover:border-primary'
                    }`;

                    workoutEl.innerHTML = `
                        <div class="flex items-center justify-between">
                            <div>
                                <h4 class="font-semibold text-gray-800 dark:text-white">Day ${day}: ${workout.title}</h4>
                                <p class="text-sm text-gray-600 dark:text-gray-400">${workout.duration} min • ${workout.difficulty}</p>
                            </div>
                            <div class="text-xl">
                                ${isCompleted ? '✅' : isCurrent ? '🎯' : isLocked ? '🔒' : '⭐'}
                            </div>
                        </div>
                    `;

                    if (!isLocked) {
                        workoutEl.addEventListener('click', () => {
                            playClickSound();
                            appState.currentDay = day;
                            updateTodayWorkout();
                            switchTab('home');
                        });
                    }

                    library.appendChild(workoutEl);
                }
            }
        }

        // Settings Management
        function switchToSettings() {
            playClickSound();
            const appMenu = document.getElementById('appMenu');
            if (appMenu) appMenu.classList.add('hidden');
            switchTab('settings');
            loadSettings();
        }

        function switchToProfile() {
            playClickSound();
            const appMenu = document.getElementById('appMenu');
            if (appMenu) appMenu.classList.add('hidden');
            switchTab('profile');
        }

        function loadSettings() {
            const dailyReminders = document.getElementById('dailyReminders');
            const soundEffects = document.getElementById('soundEffects');
            const difficultyLevel = document.getElementById('difficultyLevel');

            if (dailyReminders) dailyReminders.checked = appState.settings.dailyReminders;
            if (soundEffects) soundEffects.checked = appState.settings.soundEffects;
            if (difficultyLevel) difficultyLevel.value = appState.settings.difficultyLevel;
        }

        function saveSettings() {
            playClickSound();
            
            const dailyReminders = document.getElementById('dailyReminders');
            const soundEffects = document.getElementById('soundEffects');
            const difficultyLevel = document.getElementById('difficultyLevel');

            appState.settings = {
                ...appState.settings,
                dailyReminders: dailyReminders ? dailyReminders.checked : true,
                soundEffects: soundEffects ? soundEffects.checked : true,
                difficultyLevel: difficultyLevel ? difficultyLevel.value : 'beginner'
            };
            
            saveProgress();
            showCustomAlert('Settings saved successfully! 🎉');
        }

        function editProfile() {
            playClickSound();
            showCustomAlert('Profile editing coming soon! For now, your profile looks great! 😎');
        }

        function exportData() {
            playClickSound();
            try {
                const data = {
                    user: appState.user,
                    progress: {
                        currentDay: appState.currentDay,
                        completedWorkouts: appState.completedWorkouts,
                        currentStreak: appState.currentStreak,
                        totalMinutes: appState.totalMinutes,
                        completedDays: appState.completedDays
                    },
                    settings: appState.settings,
                    exportDate: new Date().toISOString()
                };
                
                const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `fitbeast-data-${appState.user.name.replace(/\s+/g, '-').toLowerCase()}.json`;
                a.click();
                URL.revokeObjectURL(url);
                
                showCustomAlert('Your fitness data has been exported! 📊');
            } catch (e) {
                console.error('Export error:', e);
                showCustomAlert('Export failed. Please try again.');
            }
        }

        function resetProgress() {
            playClickSound();
            showConfirmDialog(
                'Are you sure you want to reset all your progress? This cannot be undone!',
                () => {
                    appState.currentDay = 1;
                    appState.completedWorkouts = 0;
                    appState.currentStreak = 0;
                    appState.totalMinutes = 0;
                    appState.completedDays = [];
                    saveProgress();
                    updateProgress();
                    updateTodayWorkout();
                    updateWeekProgress();
                    updateAchievements();
                    updateProfile();
                    showCustomAlert('Progress reset complete. Ready for a fresh start! 💪');
                }
            );
        }

        // Event Listeners Setup
        function setupEventListeners() {
            setupNavigation();
            setupAppMenu();
            setupWorkoutButtons();
            setupAIChat();
            setupMealPlanning();
            setupVideoGeneration();
        }

        function setupAppMenu() {
            const menuBtn = document.getElementById('menuBtn');
            const appMenu = document.getElementById('appMenu');
            
            if (menuBtn && appMenu) {
                menuBtn.addEventListener('click', () => {
                    playClickSound();
                    appMenu.classList.toggle('hidden');
                });

                document.addEventListener('click', (e) => {
                    if (!menuBtn.contains(e.target) && !appMenu.contains(e.target)) {
                        appMenu.classList.add('hidden');
                    }
                });
            }
        }

        function setupWorkoutButtons() {
            const startBtn = document.getElementById('startWorkoutBtn');
            if (startBtn) {
                startBtn.addEventListener('click', () => {
                    playClickSound();
                    openWorkoutModal();
                });
            }

            const closeModal = document.getElementById('closeModal');
            if (closeModal) {
                closeModal.addEventListener('click', closeWorkoutModal);
            }

            const startTimerBtn = document.getElementById('startTimerBtn');
            if (startTimerBtn) {
                startTimerBtn.addEventListener('click', () => {
                    playClickSound();
                    startTimer();
                });
            }

            const completeWorkoutBtn = document.getElementById('completeWorkoutBtn');
            if (completeWorkoutBtn) {
                completeWorkoutBtn.addEventListener('click', () => {
                    playClickSound();
                    completeWorkout();
                });
            }

            const pauseTimerBtn = document.getElementById('pauseTimerBtn');
            if (pauseTimerBtn) {
                pauseTimerBtn.addEventListener('click', () => {
                    playClickSound();
                    pauseTimer();
                });
            }

            const stopTimerBtn = document.getElementById('stopTimerBtn');
            if (stopTimerBtn) {
                stopTimerBtn.addEventListener('click', () => {
                    playClickSound();
                    stopTimer();
                });
            }

            const closeSuccessModal = document.getElementById('closeSuccessModal');
            if (closeSuccessModal) {
                closeSuccessModal.addEventListener('click', () => {
                    playSuccessSound();
                    closeSuccessModal();
                });
            }
        }

        function setupAIChat() {
            const chatInput = document.getElementById('chatInput');
            const sendBtn = document.getElementById('sendChatBtn');

            if (sendBtn) {
                sendBtn.addEventListener('click', () => {
                    playClickSound();
                    sendChatMessage();
                });
            }

            if (chatInput) {
                chatInput.addEventListener('keypress', (e) => {
                    if (e.key === 'Enter') {
                        sendChatMessage();
                    }
                });
            }

            document.querySelectorAll('.quick-question').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    playClickSound();
                    const question = e.target.dataset.question;
                    if (chatInput) chatInput.value = question;
                    sendChatMessage();
                });
            });
        }

        function setupMealPlanning() {
            const generateMealPlanBtn = document.getElementById('generateMealPlanBtn');
            if (generateMealPlanBtn) {
                generateMealPlanBtn.addEventListener('click', async () => {
                    playClickSound();
                    await generateMealPlan();
                });
            }
        }

        function setupVideoGeneration() {
            const generateVideoBtn = document.getElementById('generateVideoBtn');
            if (generateVideoBtn) {
                generateVideoBtn.addEventListener('click', async () => {
                    playClickSound();
                    const workoutType = document.getElementById('videoWorkoutType')?.value;
                    await generateWorkoutVideo(workoutType);
                });
            }
        }

        // Modal and Timer Functions
        function openWorkoutModal() {
            const today = workoutProgram[appState.currentDay] || workoutProgram[1];
            const modalTitle = document.getElementById('modalWorkoutTitle');
            const modalTimer = document.getElementById('modalTimer');
            const exerciseList = document.getElementById('exerciseList');
            const modal = document.getElementById('workoutModal');

            if (modalTitle) modalTitle.textContent = `Day ${appState.currentDay}: ${today.title}`;
            if (modalTimer) modalTimer.textContent = `${today.duration}:00`;
            
            if (exerciseList) {
                exerciseList.innerHTML = today.exercises.map((exercise, index) => `
                    <div class="flex items-center space-x-4 p-3 bg-gray-50 dark:bg-gray-700 rounded-lg">
                        <div class="w-8 h-8 bg-primary text-white rounded-full flex items-center justify-center text-sm font-semibold">
                            ${index + 1}
                        </div>
                        <div class="flex-1">
                            <div class="font-medium text-gray-800 dark:text-white">${exercise.name}</div>
                            <div class="text-sm text-gray-600 dark:text-gray-400">${exercise.duration} • ${exercise.reps}</div>
                        </div>
                    </div>
                `).join('');
            }
            
            if (modal) {
                modal.classList.remove('hidden');
                modal.classList.add('flex');
            }
        }

        function closeWorkoutModal() {
            const modal = document.getElementById('workoutModal');
            if (modal) {
                modal.classList.add('hidden');
                modal.classList.remove('flex');
            }
        }

        let timer;
        let timeLeft;
        let isPaused = false;

        function startTimer() {
            const today = workoutProgram[appState.currentDay] || workoutProgram[1];
            timeLeft = today.duration * 60;
            
            closeWorkoutModal();
            const timerModal = document.getElementById('timerModal');
            if (timerModal) {
                timerModal.classList.remove('hidden');
                timerModal.classList.add('flex');
            }
            
            const currentExercise = document.getElementById('currentExercise');
            if (currentExercise) {
                currentExercise.textContent = today.exercises[0].name;
            }
            
            timer = setInterval(() => {
                if (!isPaused) {
                    timeLeft--;
                    updateTimerDisplay();
                    
                    if (timeLeft <= 0) {
                        clearInterval(timer);
                        completeWorkout();
                    }
                }
            }, 1000);
        }

        function updateTimerDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            const timerDisplay = document.getElementById('timerDisplay');
            if (timerDisplay) {
                timerDisplay.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            }
        }

        function pauseTimer() {
            isPaused = !isPaused;
            const btn = document.getElementById('pauseTimerBtn');
            if (btn) {
                btn.innerHTML = isPaused ? '<i class="fas fa-play mr-2"></i>Resume' : '<i class="fas fa-pause mr-2"></i>Pause';
            }
        }

        function stopTimer() {
            clearInterval(timer);
            const timerModal = document.getElementById('timerModal');
            if (timerModal) {
                timerModal.classList.add('hidden');
                timerModal.classList.remove('flex');
            }
        }

        function completeWorkout() {
            clearInterval(timer);
            const timerModal = document.getElementById('timerModal');
            if (timerModal) {
                timerModal.classList.add('hidden');
                timerModal.classList.remove('flex');
            }
            
            // Update app state
            if (!appState.completedDays.includes(appState.currentDay)) {
                appState.completedDays.push(appState.currentDay);
                appState.completedWorkouts++;
                appState.currentStreak++;
                const today = workoutProgram[appState.currentDay] || workoutProgram[1];
                appState.totalMinutes += today.duration;
            }
            
            // Show success modal
            const successModal = document.getElementById('successModal');
            if (successModal) {
                successModal.classList.remove('hidden');
                successModal.classList.add('flex');
            }
            playSuccessSound();
            
            // Update UI and save progress
            updateProgress();
            updateWeekProgress();
            updateAchievements();
            updateProfile();
            saveProgress();
        }

        function closeSuccessModal() {
            const successModal = document.getElementById('successModal');
            if (successModal) {
                successModal.classList.add('hidden');
                successModal.classList.remove('flex');
            }
            
            // Advance to next day if current day is completed
            if (appState.currentDay < 30) {
                appState.currentDay++;
                updateTodayWorkout();
                updateProgress();
                populateDetailedWorkout();
                saveProgress();
            }
        }

        // AI Chat Functions
        async function sendChatMessage() {
            const chatInput = document.getElementById('chatInput');
            const chatMessages = document.getElementById('chatMessages');
            const message = chatInput?.value.trim();

            if (!message) return;

            // Add user message
            addChatMessage(message, 'user');
            if (chatInput) chatInput.value = '';

            // Add typing indicator
            const typingEl = addChatMessage('🤔 Thinking...', 'bot', true);

            // Simulate AI response (fallback since we can't use real Poe API here)
            setTimeout(() => {
                if (typingEl) typingEl.remove();
                const responses = [
                    "For pre-workout meals, aim for easily digestible carbs 30-60 minutes before exercise. Try a banana with a small amount of nut butter or oatmeal with berries.",
                    "Daily protein needs vary, but generally aim for 0.8-1.2g per kg of body weight for general fitness, or 1.6-2.2g per kg for muscle building goals.",
                    "For muscle recovery, focus on protein-rich foods like lean meats, fish, eggs, Greek yogurt, and plant-based options like quinoa and legumes within 2 hours post-workout.",
                    "Stay hydrated! Drink water before, during, and after your workouts. Your body needs extra fluids when you're active.",
                    "Don't skip rest days! Your muscles grow and recover when you're resting, not just when you're working out."
                ];
                const randomResponse = responses[Math.floor(Math.random() * responses.length)];
                addChatMessage(randomResponse, 'bot');
                playNotificationSound();
            }, 1500);
        }

        function addChatMessage(content, sender, isTyping = false) {
            const chatMessages = document.getElementById('chatMessages');
            if (!chatMessages) return null;

            const messageEl = document.createElement('div');
            messageEl.className = `${sender === 'user' ? 'ml-auto bg-primary text-white' : 'bg-gray-100 dark:bg-gray-700 text-gray-800 dark:text-white'} 
                                   rounded-lg p-3 max-w-[80%] ${isTyping ? 'opacity-70' : ''}`;
            
            messageEl.textContent = content;
            chatMessages.appendChild(messageEl);
            chatMessages.scrollTop = chatMessages.scrollHeight;
            
            return messageEl;
        }

        // Meal Planning
        async function generateMealPlan() {
            const loadingEl = document.getElementById('mealPlanLoading');
            const containerEl = document.getElementById('mealPlanContainer');
            const goal = document.getElementById('dietGoal')?.value;
            const dietType = document.getElementById('dietType')?.value;

            if (loadingEl) loadingEl.classList.remove('hidden');
            if (containerEl) containerEl.classList.add('hidden');

            // Simulate meal plan generation
            setTimeout(() => {
                if (loadingEl) loadingEl.classList.add('hidden');
                if (containerEl) {
                    containerEl.innerHTML = `
                        <div class="bg-gray-50 dark:bg-gray-700 rounded-xl p-4">
                            <h4 class="font-semibold text-gray-800 dark:text-white mb-3">Your ${goal} Meal Plan (${dietType})</h4>
                            <div class="space-y-3">
                                <div class="p-3 bg-white dark:bg-gray-600 rounded-lg">
                                    <h5 class="font-medium">🍳 Breakfast</h5>
                                    <p class="text-sm text-gray-600 dark:text-gray-300">Oatmeal with berries and nuts (350 cal)</p>
                                </div>
                                <div class="p-3 bg-white dark:bg-gray-600 rounded-lg">
                                    <h5 class="font-medium">🥗 Lunch</h5>
                                    <p class="text-sm text-gray-600 dark:text-gray-300">Grilled chicken salad (450 cal)</p>
                                </div>
                                <div class="p-3 bg-white dark:bg-gray-600 rounded-lg">
                                    <h5 class="font-medium">🍽️ Dinner</h5>
                                    <p class="text-sm text-gray-600 dark:text-gray-300">Salmon with quinoa and vegetables (500 cal)</p>
                                </div>
                            </div>
                        </div>
                    `;
                    containerEl.classList.remove('hidden');
                }
                playSuccessSound();
            }, 1500);
        }

        // Video Generation
        async function generateWorkoutVideo(workoutType) {
            const loadingEl = document.getElementById('videoLoading');
            const containerEl = document.getElementById('videoContainer');

            if (loadingEl) loadingEl.classList.remove('hidden');
            if (containerEl) containerEl.classList.add('hidden');

            // Simulate video generation
            setTimeout(() => {
                if (loadingEl) loadingEl.classList.add('hidden');
                showCustomAlert('Video generation requires Poe integration. This is a demo version - in the full app, you\'d get real workout videos! 💪');
            }, 2000);
        }

        // Utility Functions
        function startGuidedWorkout() {
            playClickSound();
            showCustomAlert('🎯 Guided workout mode coming soon! For now, use the Quick View to see all exercises and start your workout. You\'ve got this! 💪');
        }

        function showStats() {
            playClickSound();
            const appMenu = document.getElementById('appMenu');
            if (appMenu) appMenu.classList.add('hidden');
            
            const stats = `📊 Your FitBeast Stats:
            
• Days Completed: ${appState.completedWorkouts}/30
• Current Streak: ${appState.currentStreak} days  
• Total Minutes: ${appState.totalMinutes}
• Success Rate: ${Math.round((appState.completedWorkouts / Math.max(appState.currentDay, 1)) * 100)}%

Keep crushing it! 💪`;
            showCustomAlert(stats);
        }

        function shareApp() {
            playClickSound();
            const appMenu = document.getElementById('appMenu');
            if (appMenu) appMenu.classList.add('hidden');
            
            if (navigator.share) {
                navigator.share({
                    title: 'FitBeast Mode - 30 Day Challenge',
                    text: 'Join me on this epic fitness journey! 💪',
                    url: window.location.href
                });
            } else {
                showCustomAlert('Share this app with friends: "Hey! I\'m crushing my fitness goals with FitBeast Mode! 💪🔥"');
            }
        }

        function showHelp() {
            playClickSound();
            const appMenu = document.getElementById('appMenu');
            if (appMenu) appMenu.classList.add('hidden');
            
            const helpText = `🤝 Need Help?

• Tap the Home icon to return to your dashboard
• Complete workouts daily to build your streak  
• Use the AI Coach for nutrition advice
• Check the Workouts tab for detailed instructions
• Visit Settings to customize your experience
• Your progress auto-saves every 30 seconds!

You've got this! 🚀`;
            showCustomAlert(helpText);
        }

        // Custom Modal System
        function showCustomAlert(message) {
            const modal = document.createElement('div');
            modal.className = 'fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50';
            modal.innerHTML = `
                <div class="bg-white dark:bg-gray-800 p-6 rounded-xl shadow-xl max-w-sm w-full mx-4">
                    <p class="text-gray-700 dark:text-gray-300 mb-4">${message}</p>
                    <div class="flex justify-end">
                        <button class="px-4 py-2 bg-primary text-white hover:bg-primary-dark rounded-lg" onclick="this.closest('.fixed').remove()">Got it!</button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);
            playNotificationSound();
        }

        function showConfirmDialog(message, onConfirm) {
            const modal = document.createElement('div');
            modal.className = 'fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50';
            modal.innerHTML = `
                <div class="bg-white dark:bg-gray-800 p-6 rounded-xl shadow-xl max-w-sm w-full mx-4">
                    <p class="text-gray-700 dark:text-gray-300 mb-6">${message}</p>
                    <div class="flex justify-end space-x-3">
                        <button class="px-4 py-2 text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg" onclick="this.closest('.fixed').remove()">Cancel</button>
                        <button class="px-4 py-2 bg-red-500 text-white hover:bg-red-600 rounded-lg" onclick="this.closest('.fixed').remove(); onConfirm()">Confirm</button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);
            playNotificationSound();
        }

        // Auto-save progress every 30 seconds
        setInterval(() => {
            if (appState.settings.autoSave && appState.user) {
                saveProgress();
            }
        }, 30000);

        // Initialize the app when DOM is loaded
        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
