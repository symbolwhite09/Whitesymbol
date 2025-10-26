<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>White Symbol - Telegram Mini App</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, doc, setDoc, getDoc, updateDoc, collection, getDocs, query, orderBy, limit } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
        import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-analytics.js";

        // Your Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyDONYCroK_EpUoDAXFE8t1li5Wv9-fjPEs",
            authDomain: "white-symbol-38bf2.firebaseapp.com",
            projectId: "white-symbol-38bf2",
            storageBucket: "white-symbol-38bf2.firebasestorage.app",
            messagingSenderId: "983275942949",
            appId: "1:983275942949:web:afdd6550dbb4f4a568fbcc",
            measurementId: "G-LNTWKD4QN6"
        };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const analytics = getAnalytics(app);

        // Make Firebase available globally
        window.firebase = { app, db, analytics, getDoc, setDoc, updateDoc, doc, collection, getDocs, query, orderBy, limit };
    </script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #000;
            color: #fff;
            min-height: 100vh;
            padding-bottom: 80px;
        }
        
        /* Loading Screen Styles */
        .loading-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            transition: opacity 0.5s ease;
        }
        
        .loading-screen.hidden {
            opacity: 0;
            pointer-events: none;
        }
        
        .loading-logo {
            width: 80px;
            height: 80px;
            margin-bottom: 20px;
            background: linear-gradient(135deg, #34aadc, #0088cc);
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
        
        .loading-text {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #fff;
            text-align: center;
        }
        
        .loading-progress-container {
            width: 80%;
            max-width: 300px;
            height: 12px;
            background: #333;
            border-radius: 6px;
            overflow: hidden;
            margin-bottom: 10px;
        }
        
        .loading-progress-bar {
            height: 100%;
            background: linear-gradient(90deg, #34aadc, #0088cc);
            border-radius: 6px;
            width: 0%;
            transition: width 0.3s ease;
        }
        
        .loading-percentage {
            font-size: 1rem;
            color: #34aadc;
            font-weight: bold;
        }
        
        .loading-dots {
            display: flex;
            margin-top: 20px;
        }
        
        .loading-dot {
            width: 8px;
            height: 8px;
            background: #34aadc;
            border-radius: 50%;
            margin: 0 5px;
            animation: loadingDot 1.5s infinite ease-in-out;
        }
        
        .loading-dot:nth-child(2) {
            animation-delay: 0.2s;
        }
        
        .loading-dot:nth-child(3) {
            animation-delay: 0.4s;
        }
        
        @keyframes loadingDot {
            0%, 100% {
                transform: scale(1);
                opacity: 1;
            }
            50% {
                transform: scale(1.5);
                opacity: 0.7;
            }
        }
        
        .container {
            max-width: 100%;
            padding: 20px;
            opacity: 0;
            transition: opacity 0.5s ease;
        }
        
        .container.loaded {
            opacity: 1;
        }
        
        header {
            text-align: center;
            padding: 20px 0;
            margin-bottom: 20px;
            border-bottom: 1px solid #333;
            position: relative;
        }
        
        h1 {
            font-size: 2rem;
            margin-bottom: 10px;
            color: #fff;
        }
        
        .subtitle {
            font-size: 1rem;
            opacity: 0.7;
        }
        
        .settings-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background: transparent;
            border: none;
            color: #fff;
            font-size: 1.2rem;
            cursor: pointer;
            padding: 5px;
            border-radius: 50%;
            width: 35px;
            height: 35px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background-color 0.3s;
        }
        
        .settings-btn:hover {
            background-color: #222;
        }
        
        .content {
            background: #111;
            border-radius: 8px;
            padding: 8px;
            margin-bottom: 20px;
            border: 1px solid #222;
        }
        
        .section-title {
            font-size: 1.5rem;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            color: #fff;
        }
        
        .section-title i {
            margin-right: 10px;
            color: #34aadc;
        }
        
        .card-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-top: 20px;
        }
        
        .card {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 15px;
            text-align: center;
            border: 1px solid #333;
            transition: transform 0.2s, background-color 0.2s;
            cursor: pointer;
        }
        
        .card:active {
            transform: scale(0.98);
            background-color: #222;
        }
        
        .card i {
            font-size: 1.8rem;
            margin-bottom: 10px;
            color: #34aadc;
        }
        
        .card h3 {
            font-size: 1rem;
            margin-bottom: 5px;
            color: #fff;
        }
        
        .card p {
            font-size: 0.8rem;
            opacity: 0.7;
            color: #ccc;
        }
        
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: #111;
            display: flex;
            justify-content: space-around;
            padding: 15px 0;
            border-top: 1px solid #333;
            z-index: 100;
        }
        
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: #888;
            text-decoration: none;
            transition: all 0.3s ease;
            flex: 1;
            padding: 5px 0;
        }
        
        .nav-item.active {
            color: #34aadc;
        }
        
        .nav-item i {
            font-size: 1.3rem;
            margin-bottom: 5px;
        }
        
        .nav-item span {
            font-size: 0.7rem;
        }
        
        .section {
            display: none;
        }
        
        .section.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        .stats {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
        }
        
        .stat-item {
            text-align: center;
        }
        
        .stat-value {
            font-size: 1.8rem;
            font-weight: bold;
            margin-bottom: 5px;
            color: #34aadc;
        }
        
        .stat-label {
            font-size: 0.8rem;
            opacity: 0.7;
        }
        
        .leaderboard-list {
            list-style: none;
        }
        
        .leaderboard-item {
            display: flex;
            align-items: center;
            padding: 15px;
            background: #1a1a1a;
            border-radius: 12px;
            margin-bottom: 10px;
            border: 1px solid #333;
        }
        
        .rank {
            font-size: 1.2rem;
            font-weight: bold;
            margin-right: 15px;
            width: 30px;
            text-align: center;
            color: #34aadc;
        }
        
        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: #333;
            margin-right: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #fff;
        }
        
        .user-info {
            flex: 1;
        }
        
        .user-name {
            font-weight: bold;
            margin-bottom: 5px;
            color: #fff;
        }
        
        .user-points {
            font-size: 0.9rem;
            opacity: 0.7;
            color: #ccc;
        }
        
        .friends-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }
        
        .friend-card {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 15px;
            text-align: center;
            border: 1px solid #333;
        }
        
        .friend-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: #333;
            margin: 0 auto 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.2rem;
            color: #fff;
        }
        
        .friend-name {
            font-weight: bold;
            margin-bottom: 5px;
            color: #fff;
        }
        
        .friend-status {
            font-size: 0.8rem;
            opacity: 0.7;
        }
        
        .online {
            color: #4cd964;
        }
        
        .task-item {
            display: flex;
            align-items: center;
            padding: 15px;
            background: #1a1a1a;
            border-radius: 12px;
            margin-bottom: 10px;
            border: 1px solid #333;
            transition: all 0.3s ease;
        }
        
        .task-item.completed {
            opacity: 0.6;
            transform: scale(0.95);
        }
        
        .task-icon {
            width: 50px;
            height: 50px;
            border-radius: 12px;
            background: #000;
            margin-right: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #fff;
            overflow: hidden;
            border: 2px solid #333;
            position: relative;
        }
        
        .task-icon img {
            width: 30px;
            height: 30px;
            object-fit: contain;
            border-radius: 0;
        }
        
        .task-icon.x-logo {
            background: linear-gradient(135deg, #000000 0%, #1a1a1a 100%);
            border: 2px solid #ffd700;
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.3);
        }
        
        .task-icon.x-logo::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, #ffd700, #ffed4e, #ffd700);
            border-radius: 14px;
            z-index: -1;
            animation: goldenGlow 2s ease-in-out infinite alternate;
        }
        
        .task-icon.x-logo img {
            filter: brightness(0) invert(1);
            width: 28px;
            height: 28px;
        }
        
        .task-icon.telegram-logo {
            background: #0088cc;
        }
        
        .task-icon.telegram-logo img {
            filter: brightness(0) invert(1);
        }
        
        .task-info {
            flex: 1;
        }
        
        .task-name {
            font-weight: bold;
            margin-bottom: 5px;
            color: #fff;
        }
        
        .task-points {
            font-size: 0.9rem;
            opacity: 0.7;
            color: #ccc;
        }
        
        .task-action {
            background: #34aadc;
            color: #000;
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        
        .task-action:active {
            background-color: #2a8bb8;
        }
        
        .progress-bar {
            height: 6px;
            background: #333;
            border-radius: 3px;
            margin-top: 5px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: #34aadc;
            border-radius: 3px;
            width: 0%;
            transition: width 0.5s ease;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: #34aadc;
            color: #000;
            padding: 10px 20px;
            border-radius: 20px;
            font-weight: bold;
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .notification.show {
            opacity: 1;
        }
        
        .empty-state {
            text-align: center;
            padding: 40px 20px;
            opacity: 0.7;
        }
        
        .empty-icon {
            font-size: 3rem;
            margin-bottom: 15px;
            color: #555;
        }
        
        .empty-text {
            font-size: 1rem;
            margin-bottom: 10px;
        }
        
        .empty-subtext {
            font-size: 0.9rem;
        }
        
        .hidden {
            display: none;
        }
        
        .x-task-highlight {
            border: 2px solid #ffd700;
            animation: goldenPulse 2s infinite;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.2);
        }
        
        .telegram-task-highlight {
            border: 2px solid #0088cc;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { border-color: #0088cc; }
            50% { border-color: #34aadc; }
            100% { border-color: #0088cc; }
        }
        
        @keyframes goldenPulse {
            0% { 
                border-color: #ffd700;
                box-shadow: 0 0 20px rgba(255, 215, 0, 0.2);
            }
            50% { 
                border-color: #ffed4e;
                box-shadow: 0 0 30px rgba(255, 215, 0, 0.4);
            }
            100% { 
                border-color: #ffd700;
                box-shadow: 0 0 20px rgba(255, 215, 0, 0.2);
            }
        }
        
        @keyframes goldenGlow {
            0% {
                opacity: 0.7;
            }
            100% {
                opacity: 1;
            }
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }
        
        .modal.active {
            display: flex;
        }
        
        .modal-content {
            background: #111;
            border-radius: 15px;
            padding: 25px;
            width: 90%;
            max-width: 400px;
            border: 1px solid #333;
        }
        
        .modal-title {
            font-size: 1.5rem;
            margin-bottom: 15px;
            color: #fff;
            text-align: center;
        }
        
        .modal-text {
            margin-bottom: 20px;
            text-align: center;
            opacity: 0.8;
        }
        
        .modal-buttons {
            display: flex;
            gap: 10px;
        }
        
        .modal-btn {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 10px;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        
        .modal-btn.cancel {
            background: #333;
            color: #fff;
        }
        
        .modal-btn.delete {
            background: #e74c3c;
            color: #fff;
        }
        
        .modal-btn:active {
            opacity: 0.8;
        }
        
        .setting-option {
            padding: 15px;
            background: #1a1a1a;
            border-radius: 12px;
            margin-bottom: 10px;
            border: 1px solid #333;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .setting-label {
            font-weight: bold;
        }
        
        .setting-description {
            font-size: 0.8rem;
            opacity: 0.7;
            margin-top: 5px;
        }
        
        .delete-btn {
            background: #e74c3c;
            color: #fff;
            border: none;
            padding: 10px 15px;
            border-radius: 10px;
            font-weight: bold;
            cursor: pointer;
        }
        
        /* Switch styling */
        .switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 24px;
        }
        
        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #333;
            transition: .4s;
            border-radius: 24px;
        }
        
        .slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .slider {
            background-color: #34aadc;
        }
        
        input:checked + .slider:before {
            transform: translateX(26px);
        }
        
        .invite-link-container {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 20px;
            margin: 20px 0;
            border: 1px solid #333;
            text-align: center;
        }
        
        .invite-link {
            background: #333;
            border-radius: 10px;
            padding: 15px;
            margin: 15px 0;
            font-family: monospace;
            word-break: break-all;
            border: 1px solid #444;
        }
        
        .copy-btn {
            background: #34aadc;
            color: #000;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            font-weight: bold;
            cursor: pointer;
            margin: 10px 5px;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        
        .share-btn {
            background: #4cd964;
            color: #000;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            font-weight: bold;
            cursor: pointer;
            margin: 10px 5px;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        
        .referral-stats {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
        }
        
        .referral-stat {
            text-align: center;
        }
        
        .referral-value {
            font-size: 1.5rem;
            font-weight: bold;
            margin-bottom: 5px;
            color: #34aadc;
        }
        
        .referral-label {
            font-size: 0.8rem;
            opacity: 0.7;
        }
        
        .user-rank-info {
            background: #1a1a1a;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 20px;
            border: 1px solid #333;
            text-align: center;
        }
        
        .user-rank {
            font-size: 1.8rem;
            font-weight: bold;
            margin-bottom: 10px;
            color: #34aadc;
        }
        
        .user-rank-label {
            font-size: 1rem;
            opacity: 0.8;
        }
        
        .current-user {
            background: linear-gradient(135deg, #1a1a1a 0%, #2a2a2a 100%);
            border: 2px solid #34aadc;
        }
        
        .gold {
            color: #ffd700;
        }
        
        .silver {
            color: #c0c0c0;
        }
        
        .bronze {
            color: #cd7f32;
        }

        .loading {
            text-align: center;
            padding: 20px;
            color: #34aadc;
        }

        .task-completed-message {
            text-align: center;
            padding: 40px 20px;
            color: #4cd964;
        }

        .task-completed-message i {
            font-size: 3rem;
            margin-bottom: 15px;
        }

        .golden-text {
            color: #ffd700;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
        }
    </style>
</head>
<body>
    <!-- Loading Screen -->
    <div class="loading-screen" id="loadingScreen">
        <div class="loading-logo">WS</div>
        <div class="loading-text">White Symbol</div>
        <div class="loading-progress-container">
            <div class="loading-progress-bar" id="loadingProgressBar"></div>
        </div>
        <div class="loading-percentage" id="loadingPercentage">0%</div>
        <div class="loading-dots">
            <div class="loading-dot"></div>
            <div class="loading-dot"></div>
            <div class="loading-dot"></div>
        </div>
    </div>
    
    <div class="notification" id="notification"></div>
    
    <!-- Delete Account Modal -->
    <div class="modal" id="deleteModal">
        <div class="modal-content">
            <h3 class="modal-title">Delete Account</h3>
            <p class="modal-text">Are you sure you want to delete your account? This action cannot be undone and all your data will be permanently lost.</p>
            <div class="modal-buttons">
                <button class="modal-btn cancel" id="cancelDelete">Cancel</button>
                <button class="modal-btn delete" id="confirmDelete">Delete</button>
            </div>
        </div>
    </div>
    
    <div class="container" id="appContainer">
        <header>
            <h1>White Symbol</h1>
            <p class="subtitle">Start from zero and build your rewards</p>
            <button class="settings-btn" id="settingsBtn">
                <i class="fas fa-cog"></i>
            </button>
        </header>
        
        <!-- Home Section -->
        <section id="home" class="section active">
            <div class="content">
                <h2 class="section-title"><i class="fas fa-home"></i> Home</h2>
                <p>Welcome! Start completing tasks to earn your first points.</p>
                
                <div class="stats">
                    <div class="stat-item">
                        <div class="stat-value" id="points-value">0</div>
                        <div class="stat-label">Points</div>
                    </div>
                    <div class="stat-item">
                        <div class="stat-value" id="friends-value">0</div>
                        <div class="stat-label">Friends</div>
                    </div>
                    <div class="stat-item">
                        <div class="stat-value" id="tasks-value">0</div>
                        <div class="stat-label">Tasks</div>
                    </div>
                </div>
                
                <h3 style="margin: 20px 0 10px; color: #fff;">Quick Actions</h3>
                <div class="card-grid">
                    <div class="card" data-action="daily-reward">
                        <i class="fas fa-gift"></i>
                        <h3>Daily Reward</h3>
                        <p>Claim now</p>
                    </div>
                    <div class="card" data-action="invite-friends">
                        <i class="fas fa-share-alt"></i>
                        <h3>Invite Friends</h3>
                        <p>Get bonus</p>
                    </div>
                    <div class="card" data-action="view-tasks">
                        <i class="fas fa-tasks"></i>
                        <h3>Tasks</h3>
                        <p>Complete & earn</p>
                    </div>
                    <div class="card" data-action="profile">
                        <i class="fas fa-user"></i>
                        <h3>Profile</h3>
                        <p>View stats</p>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- Earn Section -->
        <section id="earn" class="section">
            <div class="content">
                <h2 class="section-title"><i class="fas fa-coins"></i> Earn Points</h2>
                <p>Complete tasks to earn points and rewards</p>
                
                <div id="tasks-container">
                    <!-- Tasks will be loaded here -->
                </div>

                <div id="all-tasks-completed" class="task-completed-message hidden">
                    <i class="fas fa-check-circle"></i>
                    <div class="empty-text">All tasks completed!</div>
                    <div class="empty-subtext">You've completed all available tasks. Check back later for new tasks.</div>
                </div>
            </div>
        </section>
        
        <!-- Leaderboard Section -->
        <section id="leaderboard" class="section">
            <div class="content">
                <h2 class="section-title"><i class="fas fa-trophy"></i> Leaderboard</h2>
                <p>Top performers this week</p>
                
                <!-- User's current rank information -->
                <div class="user-rank-info" id="user-rank-info">
                    <div class="user-rank" id="user-rank">-</div>
                    <div class="user-rank-label" id="user-rank-label">Your Current Rank</div>
                </div>
                
                <div id="leaderboard-container">
                    <!-- Leaderboard will be populated dynamically -->
                </div>
                
                <div id="no-leaderboard-message" class="empty-state hidden">
                    <div class="empty-icon">
                        <i class="fas fa-trophy"></i>
                    </div>
                    <div class="empty-text">No top players yet</div>
                    <div class="empty-subtext">Start earning points to appear on the leaderboard</div>
                </div>
            </div>
        </section>
        
        <!-- Friends Section -->
        <section id="friends" class="section">
            <div class="content">
                <h2 class="section-title"><i class="fas fa-user-friends"></i> Friends</h2>
                <p>Connect with friends and see their progress</p>
                
                <div class="referral-stats">
                    <div class="referral-stat">
                        <div class="referral-value" id="referrals-count">0</div>
                        <div class="referral-label">Referrals</div>
                    </div>
                    <div class="referral-stat">
                        <div class="referral-value" id="referral-earnings">0</div>
                        <div class="referral-label">Earnings</div>
                    </div>
                    <div class="referral-stat">
                        <div class="referral-value" id="referral-bonus">200</div>
                        <div class="referral-label">Bonus/Referral</div>
                    </div>
                </div>
                
                <div class="invite-link-container">
                    <h3>Your Invite Link</h3>
                    <p>Share this link with friends and earn 200 points when they join and start earning!</p>
                    
                    <div class="invite-link" id="invite-link">
                        Loading...
                    </div>
                    
                    <div>
                        <button class="copy-btn" id="copy-link-btn">
                            <i class="fas fa-copy"></i> Copy Link
                        </button>
                        <button class="share-btn" id="share-link-btn">
                            <i class="fas fa-share-alt"></i> Share
                        </button>
                    </div>
                </div>
                
                <div id="friends-container">
                    <!-- Friends will be added dynamically -->
                </div>
                
                <div id="no-friends-message" class="empty-state">
                    <div class="empty-icon">
                        <i class="fas fa-user-friends"></i>
                    </div>
                    <div class="empty-text">No friends yet</div>
                    <div class="empty-subtext">Invite friends to start earning together</div>
                </div>
            </div>
        </section>
        
        <!-- Settings Section -->
        <section id="settings" class="section">
            <div class="content">
                <h2 class="section-title"><i class="fas fa-cog"></i> Settings</h2>
                
                <div class="setting-option">
                    <div>
                        <div class="setting-label">Notifications</div>
                        <div class="setting-description">Receive notifications about new tasks and rewards</div>
                    </div>
                    <label class="switch">
                        <input type="checkbox" id="notifications-toggle" checked>
                        <span class="slider"></span>
                    </label>
                </div>
                
                <div class="setting-option">
                    <div>
                        <div class="setting-label">Sound Effects</div>
                        <div class="setting-description">Play sounds when completing tasks</div>
                    </div>
                    <label class="switch">
                        <input type="checkbox" id="sound-toggle" checked>
                        <span class="slider"></span>
                    </label>
                </div>
                
                <div class="setting-option">
                    <div>
                        <div class="setting-label">Privacy Mode</div>
                        <div class="setting-description">Hide your profile from other users</div>
                    </div>
                    <label class="switch">
                        <input type="checkbox" id="privacy-toggle">
                        <span class="slider"></span>
                    </label>
                </div>
                
                <div style="margin-top: 30px; text-align: center;">
                    <button class="delete-btn" id="deleteAccountBtn">
                        <i class="fas fa-trash"></i> Delete Account
                    </button>
                </div>
            </div>
        </section>
    </div>
    
    <!-- Bottom Navigation -->
    <div class="bottom-nav">
        <a href="#" class="nav-item active" data-section="home">
            <i class="fas fa-home"></i>
            <span>Home</span>
        </a>
        <a href="#" class="nav-item" data-section="earn">
            <i class="fas fa-coins"></i>
            <span>Earn</span>
        </a>
        <a href="#" class="nav-item" data-section="leaderboard">
            <i class="fas fa-trophy"></i>
            <span>Leaderboard</span>
        </a>
        <a href="#" class="nav-item" data-section="friends">
            <i class="fas fa-user-friends"></i>
            <span>Friends</span>
        </a>
    </div>

    <script>
        // App state
        const appState = {
            points: 0,
            friends: 0,
            completedTasks: 0,
            friendList: [],
            referrals: 0,
            referralEarnings: 0,
            userId: '',
            username: '',
            completedTasksList: [],
            settings: {
                notifications: true,
                sound: true,
                privacy: false
            }
        };

        // Available tasks
        const availableTasks = [
            {
                id: 'join_telegram_community',
                name: 'Join our TG Community',
                points: 50,
                link: 'https://t.me/WhiteSymbolCommunity',
                icon: 'telegram-logo',
                iconContent: '<i class="fab fa-telegram"></i>',
                highlight: 'telegram-task-highlight'
            },
            {
                id: 'x_community',
                name: 'Follow our X Community',
                points: 50,
                link: 'https://x.com/whitesymbolcom?t=GFgvVu6f4gqlkt-GmnXsAA&s=09',
                icon: 'x-logo',
                iconContent: '<img src="https://abs.twimg.com/responsive-web/client-web/icon-ios.b1fc727a.png" alt="X Logo">',
                highlight: 'x-task-highlight'
            },
            {
                id: 'telegram_announcement',
                name: 'See the Fast Announcement',
                points: 50,
                link: 'https://t.me/WhiteSymbolCommunity/3',
                icon: 'telegram-logo',
                iconContent: '<i class="fab fa-telegram"></i>',
                highlight: 'telegram-task-highlight'
            },
            {
                id: 'twitter_post',
                name: 'See in Twitter',
                points: 50,
                link: 'https://x.com/whitesymbolcom/status/1982009569636458569?t=Qpm1hiJ31ZdDyYbSsh8qsA&s=19',
                icon: 'x-logo',
                iconContent: '<img src="https://abs.twimg.com/responsive-web/client-web/icon-ios.b1fc727a.png" alt="X Logo">',
                highlight: 'x-task-highlight'
            },
            {
                id: 'invite_friends',
                name: 'Invite 3 Friends',
                points: 100,
                link: null, // This task has special behavior
                icon: 'share',
                iconContent: '<i class="fas fa-share-alt"></i>',
                highlight: ''
            }
        ];

        // Loading system
        let loadingProgress = 0;
        const loadingSteps = [
            { name: 'Initializing app', progress: 10 },
            { name: 'Connecting to database', progress: 25 },
            { name: 'Loading user data', progress: 50 },
            { name: 'Setting up UI', progress: 75 },
            { name: 'Finalizing', progress: 90 },
            { name: 'Ready', progress: 100 }
        ];

        function updateLoadingProgress(stepIndex) {
            if (stepIndex >= loadingSteps.length) return;
            
            const step = loadingSteps[stepIndex];
            loadingProgress = step.progress;
            
            document.getElementById('loadingProgressBar').style.width = `${loadingProgress}%`;
            document.getElementById('loadingPercentage').textContent = `${loadingProgress}%`;
            
            // Simulate loading time for each step
            setTimeout(() => {
                updateLoadingProgress(stepIndex + 1);
            }, 500);
        }

        function finishLoading() {
            setTimeout(() => {
                document.getElementById('loadingScreen').classList.add('hidden');
                document.getElementById('appContainer').classList.add('loaded');
            }, 500);
        }

        // Firebase functions
        async function getUserData(userId) {
            try {
                const userDoc = await window.firebase.getDoc(window.firebase.doc(window.firebase.db, 'users', userId));
                if (userDoc.exists()) {
                    return userDoc.data();
                }
                return null;
            } catch (error) {
                console.error('Error getting user data:', error);
                return null;
            }
        }

        async function saveUserData(userId, userData) {
            try {
                await window.firebase.setDoc(window.firebase.doc(window.firebase.db, 'users', userId), userData);
                return true;
            } catch (error) {
                console.error('Error saving user data:', error);
                return false;
            }
        }

        async function updateUserData(userId, updates) {
            try {
                await window.firebase.updateDoc(window.firebase.doc(window.firebase.db, 'users', userId), updates);
                return true;
            } catch (error) {
                console.error('Error updating user data:', error);
                return false;
            }
        }

        async function getLeaderboard() {
            try {
                const q = window.firebase.query(
                    window.firebase.collection(window.firebase.db, 'users'),
                    window.firebase.orderBy('points', 'desc'),
                    window.firebase.limit(10)
                );
                const querySnapshot = await window.firebase.getDocs(q);
                const leaderboard = [];
                querySnapshot.forEach((doc) => {
                    leaderboard.push({ id: doc.id, ...doc.data() });
                });
                return leaderboard;
            } catch (error) {
                console.error('Error getting leaderboard:', error);
                return [];
            }
        }

        // Initialize the app with Firebase
        async function initApp() {
            try {
                // Start loading progress
                updateLoadingProgress(0);
                
                // Wait for Firebase to be available
                await waitForFirebase();
                
                await loadAllData();
                await updateStatsDisplay();
                await updateFriendsDisplay();
                await updateLeaderboard();
                await updateReferralStats();
                await updateInviteLink();
                await updateSettingsToggles();
                await loadTasks();
                
                // Finish loading
                finishLoading();
                
                showNotification('App loaded successfully!');
            } catch (error) {
                console.error('Error initializing app:', error);
                showNotification('Error loading app data');
                finishLoading();
            }
        }

        function waitForFirebase() {
            return new Promise((resolve) => {
                const checkFirebase = () => {
                    if (window.firebase && window.firebase.db) {
                        resolve();
                    } else {
                        setTimeout(checkFirebase, 100);
                    }
                };
                checkFirebase();
            });
        }

        // Load all data from Firebase
        async function loadAllData() {
            // Generate or get user ID
            appState.userId = localStorage.getItem('userId') || generateUserId();
            localStorage.setItem('userId', appState.userId);
            
            // Generate username
            appState.username = localStorage.getItem('username') || generateUsername();
            localStorage.setItem('username', appState.username);
            
            // Load user data from Firebase
            const userData = await getUserData(appState.userId);
            if (userData) {
                // Existing user
                appState.points = userData.points || 0;
                appState.friends = userData.friends || 0;
                appState.completedTasks = userData.completedTasks || 0;
                appState.friendList = userData.friendList || [];
                appState.referrals = userData.referrals || 0;
                appState.referralEarnings = userData.referralEarnings || 0;
                appState.completedTasksList = userData.completedTasksList || [];
                appState.settings = userData.settings || appState.settings;
            } else {
                // New user - create in Firebase
                const newUserData = {
                    username: appState.username,
                    points: 0,
                    friends: 0,
                    completedTasks: 0,
                    friendList: [],
                    referrals: 0,
                    referralEarnings: 0,
                    completedTasksList: [],
                    settings: appState.settings,
                    createdAt: new Date().toISOString()
                };
                await saveUserData(appState.userId, newUserData);
            }
        }

        // Save all data to Firebase
        async function saveAllData() {
            const userData = {
                username: appState.username,
                points: appState.points,
                friends: appState.friends,
                completedTasks: appState.completedTasks,
                friendList: appState.friendList,
                referrals: appState.referrals,
                referralEarnings: appState.referralEarnings,
                completedTasksList: appState.completedTasksList,
                settings: appState.settings,
                lastUpdated: new Date().toISOString()
            };
            
            await saveUserData(appState.userId, userData);
        }

        // Generate a unique user ID for referral links
        function generateUserId() {
            return 'user_' + Math.random().toString(36).substring(2, 10) + Date.now().toString(36);
        }

        // Generate a Telegram username for the current user
        function generateUsername() {
            const adjectives = ['cool', 'happy', 'smart', 'quick', 'bright', 'fast', 'clever'];
            const nouns = ['panda', 'tiger', 'eagle', 'wolf', 'fox', 'bear', 'lion'];
            const adj = adjectives[Math.floor(Math.random() * adjectives.length)];
            const noun = nouns[Math.floor(Math.random() * nouns.length)];
            const num = Math.floor(Math.random() * 1000);
            return `@${adj}_${noun}${num}`;
        }

        // Navigation functionality
        document.querySelectorAll('.nav-item').forEach(item => {
            item.addEventListener('click', function(e) {
                e.preventDefault();
                
                // Remove active class from all nav items and sections
                document.querySelectorAll('.nav-item').forEach(nav => {
                    nav.classList.remove('active');
                });
                document.querySelectorAll('.section').forEach(section => {
                    section.classList.remove('active');
                });
                
                // Add active class to clicked nav item
                this.classList.add('active');
                
                // Show corresponding section
                const sectionId = this.getAttribute('data-section');
                document.getElementById(sectionId).classList.add('active');
                
                // Update leaderboard when navigating to it
                if (sectionId === 'leaderboard') {
                    updateLeaderboard();
                }
            });
        });

        // Settings button
        document.getElementById('settingsBtn').addEventListener('click', function() {
            // Navigate to settings section
            document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
            document.querySelectorAll('.section').forEach(section => section.classList.remove('active'));
            document.getElementById('settings').classList.add('active');
        });

        // Settings toggles
        document.getElementById('notifications-toggle').addEventListener('change', function() {
            appState.settings.notifications = this.checked;
            saveAllData();
            showNotification(this.checked ? 'Notifications enabled' : 'Notifications disabled');
        });

        document.getElementById('sound-toggle').addEventListener('change', function() {
            appState.settings.sound = this.checked;
            saveAllData();
            showNotification(this.checked ? 'Sound effects enabled' : 'Sound effects disabled');
        });

        document.getElementById('privacy-toggle').addEventListener('change', function() {
            appState.settings.privacy = this.checked;
            saveAllData();
            showNotification(this.checked ? 'Privacy mode enabled' : 'Privacy mode disabled');
        });

        // Card actions
        document.querySelectorAll('.card[data-action]').forEach(card => {
            card.addEventListener('click', function() {
                const action = this.getAttribute('data-action');
                
                switch(action) {
                    case 'daily-reward':
                        claimDailyReward();
                        break;
                    case 'invite-friends':
                        // Navigate to friends section
                        document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
                        document.querySelectorAll('.section').forEach(section => section.classList.remove('active'));
                        document.querySelector('.nav-item[data-section="friends"]').classList.add('active');
                        document.getElementById('friends').classList.add('active');
                        break;
                    case 'view-tasks':
                        // Navigate to earn section
                        document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
                        document.querySelectorAll('.section').forEach(section => section.classList.remove('active'));
                        document.querySelector('.nav-item[data-section="earn"]').classList.add('active');
                        document.getElementById('earn').classList.add('active');
                        break;
                    case 'profile':
                        showNotification('Profile section coming soon!');
                        break;
                }
            });
        });

        // Copy invite link button
        document.getElementById('copy-link-btn').addEventListener('click', function() {
            const inviteLink = document.getElementById('invite-link').textContent;
            navigator.clipboard.writeText(inviteLink).then(() => {
                showNotification('Invite link copied to clipboard!');
            }).catch(() => {
                // Fallback for browsers that don't support clipboard API
                const textArea = document.createElement('textarea');
                textArea.value = inviteLink;
                document.body.appendChild(textArea);
                textArea.select();
                document.execCommand('copy');
                document.body.removeChild(textArea);
                showNotification('Invite link copied to clipboard!');
            });
        });

        // Share invite link button
        document.getElementById('share-link-btn').addEventListener('click', function() {
            const inviteLink = document.getElementById('invite-link').textContent;
            
            if (navigator.share) {
                navigator.share({
                    title: 'Join me on Telegram Mini App!',
                    text: 'Earn rewards by completing simple tasks. Use my invite link to get started!',
                    url: inviteLink
                }).then(() => {
                    showNotification('Invite link shared successfully!');
                }).catch(() => {
                    showNotification('Invite link copied to clipboard!');
                });
            } else {
                // Fallback to copying if Web Share API is not supported
                navigator.clipboard.writeText(inviteLink).then(() => {
                    showNotification('Invite link copied to clipboard!');
                });
            }
        });

        // Delete account button
        document.getElementById('deleteAccountBtn').addEventListener('click', function() {
            document.getElementById('deleteModal').classList.add('active');
        });

        // Cancel delete
        document.getElementById('cancelDelete').addEventListener('click', function() {
            document.getElementById('deleteModal').classList.remove('active');
        });

        // Confirm delete
        document.getElementById('confirmDelete').addEventListener('click', function() {
            deleteAccount();
            document.getElementById('deleteModal').classList.remove('active');
        });

        // Helper functions
        async function updateStatsDisplay() {
            document.getElementById('points-value').textContent = appState.points;
            document.getElementById('friends-value').textContent = appState.friends;
            document.getElementById('tasks-value').textContent = appState.completedTasks;
        }

        async function updateReferralStats() {
            document.getElementById('referrals-count').textContent = appState.referrals;
            document.getElementById('referral-earnings').textContent = appState.referralEarnings;
        }

        async function updateInviteLink() {
            const inviteLink = `https://t.me/Whitesymbolminibot?start=ref_${appState.userId}`;
            document.getElementById('invite-link').textContent = inviteLink;
        }

        async function updateFriendsDisplay() {
            const friendsContainer = document.getElementById('friends-container');
            const noFriendsMessage = document.getElementById('no-friends-message');
            
            // Clear current friends
            friendsContainer.innerHTML = '';
            
            if (appState.friendList.length === 0) {
                noFriendsMessage.classList.remove('hidden');
            } else {
                noFriendsMessage.classList.add('hidden');
                
                // Add each friend to the container
                appState.friendList.forEach(friend => {
                    const friendElement = document.createElement('div');
                    friendElement.className = 'friend-card';
                    friendElement.innerHTML = `
                        <div class="friend-avatar">${friend.avatar}</div>
                        <div class="friend-name">${friend.name}</div>
                        <div class="friend-status ${friend.status === 'online' ? 'online' : ''}">
                            ${friend.status === 'online' ? 'Online' : '2 hours ago'}
                        </div>
                    `;
                    friendsContainer.appendChild(friendElement);
                });
            }
        }

        async function updateSettingsToggles() {
            document.getElementById('notifications-toggle').checked = appState.settings.notifications;
            document.getElementById('sound-toggle').checked = appState.settings.sound;
            document.getElementById('privacy-toggle').checked = appState.settings.privacy;
        }

        async function updateLeaderboard() {
            const leaderboardContainer = document.getElementById('leaderboard-container');
            const noLeaderboardMessage = document.getElementById('no-leaderboard-message');
            const userRankInfo = document.getElementById('user-rank-info');
            
            // Clear current leaderboard
            leaderboardContainer.innerHTML = '';

            const leaderboard = await getLeaderboard();
            
            // Find user's rank
            const userRankPosition = leaderboard.findIndex(user => user.id === appState.userId) + 1;
            const userRank = document.getElementById('user-rank');
            const userRankLabel = document.getElementById('user-rank-label');
            
            // Update user rank display
            if (userRankPosition > 0) {
                userRank.textContent = `#${userRankPosition}`;
                userRankLabel.textContent = `Your Current Rank`;
                
                // Add special styling for top 3 ranks
                if (userRankPosition === 1) {
                    userRank.classList.add('gold');
                } else if (userRankPosition === 2) {
                    userRank.classList.add('silver');
                } else if (userRankPosition === 3) {
                    userRank.classList.add('bronze');
                }
            } else {
                userRank.textContent = 'Not Ranked';
                userRankLabel.textContent = 'Earn points to get ranked!';
            }
            
            if (leaderboard.length === 0) {
                noLeaderboardMessage.classList.remove('hidden');
                leaderboardContainer.classList.add('hidden');
                userRankInfo.classList.add('hidden');
            } else {
                noLeaderboardMessage.classList.add('hidden');
                leaderboardContainer.classList.remove('hidden');
                userRankInfo.classList.remove('hidden');
                
                // Add each leaderboard item
                leaderboard.forEach((user, index) => {
                    const leaderboardItem = document.createElement('li');
                    leaderboardItem.className = 'leaderboard-item';
                    
                    // Add special class if this is the current user
                    if (user.id === appState.userId) {
                        leaderboardItem.classList.add('current-user');
                    }
                    
                    // Add rank color classes
                    let rankClass = '';
                    if (index === 0) rankClass = 'gold';
                    else if (index === 1) rankClass = 'silver';
                    else if (index === 2) rankClass = 'bronze';
                    
                    // Generate avatar from username
                    const avatar = user.username ? user.username.substring(1, 3).toUpperCase() : 'US';
                    
                    leaderboardItem.innerHTML = `
                        <div class="rank ${rankClass}">${index + 1}</div>
                        <div class="avatar">${avatar}</div>
                        <div class="user-info">
                            <div class="user-name">${user.username}</div>
                            <div class="user-points">${user.points} points</div>
                        </div>
                    `;
                    leaderboardContainer.appendChild(leaderboardItem);
                });
            }
        }

        async function loadTasks() {
            const tasksContainer = document.getElementById('tasks-container');
            const allTasksCompleted = document.getElementById('all-tasks-completed');
            
            // Filter out completed tasks
            const incompleteTasks = availableTasks.filter(task => 
                !appState.completedTasksList.includes(task.id) && 
                (task.id !== 'invite_friends' || appState.referrals < 3)
            );

            // If all tasks are completed, show message
            if (incompleteTasks.length === 0) {
                tasksContainer.classList.add('hidden');
                allTasksCompleted.classList.remove('hidden');
                return;
            } else {
                tasksContainer.classList.remove('hidden');
                allTasksCompleted.classList.add('hidden');
            }

            // Clear and load tasks
            tasksContainer.innerHTML = '';
            
            incompleteTasks.forEach(task => {
                const taskElement = document.createElement('div');
                taskElement.className = `task-item ${task.highlight}`;
                taskElement.id = `task-${task.id}`;
                
                if (task.id === 'invite_friends') {
                    // Special handling for invite friends task
                    const progressWidth = Math.min((appState.referrals / 3) * 100, 100);
                    const isCompleted = appState.referrals >= 3;
                    
                    taskElement.innerHTML = `
                        <div class="task-icon">
                            ${task.iconContent}
                        </div>
                        <div class="task-info">
                            <div class="task-name">${task.name}</div>
                            <div class="task-points">+${task.points} coins</div>
                            <div class="progress-bar">
                                <div class="progress-fill" style="width: ${progressWidth}%"></div>
                            </div>
                        </div>
                        <button class="task-action" id="${task.id}-button" 
                                style="background: ${isCompleted ? '#4cd964' : '#34aadc'}" 
                                ${isCompleted ? 'disabled' : ''}>
                            ${isCompleted ? 'Completed' : 'Invite'}
                        </button>
                    `;
                } else {
                    taskElement.innerHTML = `
                        <div class="task-icon ${task.icon}">
                            ${task.iconContent}
                        </div>
                        <div class="task-info">
                            <div class="task-name">${task.name}</div>
                            <div class="task-points">+${task.points} coins</div>
                            <div class="progress-bar">
                                <div class="progress-fill" id="${task.id}-progress"></div>
                            </div>
                        </div>
                        <button class="task-action" id="${task.id}-button">Start</button>
                    `;
                }
                
                tasksContainer.appendChild(taskElement);
            });

            // Add event listeners for task buttons
            incompleteTasks.forEach(task => {
                const button = document.getElementById(`${task.id}-button`);
                if (button) {
                    if (task.id === 'invite_friends') {
                        button.addEventListener('click', () => {
                            if (appState.referrals >= 3) {
                                showNotification('You already completed this task!');
                                return;
                            }
                            
                            // Navigate to friends section
                            document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
                            document.querySelectorAll('.section').forEach(section => section.classList.remove('active'));
                            document.querySelector('.nav-item[data-section="friends"]').classList.add('active');
                            document.getElementById('friends').classList.add('active');
                            
                            showNotification('Invite friends to complete this task!');
                        });
                    } else {
                        button.addEventListener('click', () => completeTask(task));
                    }
                }
            });
        }

        async function completeTask(task) {
            if (appState.completedTasksList.includes(task.id)) {
                showNotification('You already completed this task!');
                return;
            }

            const button = document.getElementById(`${task.id}-button`);
            const progressBar = document.getElementById(`${task.id}-progress`);
            
            // Show progress animation
            let progress = 0;
            const progressInterval = setInterval(() => {
                progress += 20;
                if (progressBar) {
                    progressBar.style.width = `${progress}%`;
                }
                
                if (progress >= 100) {
                    clearInterval(progressInterval);
                    
                    // Open link in new tab
                    if (task.link) {
                        window.open(task.link, '_blank');
                    }
                    
                    // Complete the task after a delay
                    setTimeout(async () => {
                        appState.points += task.points;
                        appState.completedTasks += 1;
                        appState.completedTasksList.push(task.id);
                        
                        await updateStatsDisplay();
                        await saveAllData();
                        await updateLeaderboard();
                        
                        // Remove the completed task from UI
                        const taskElement = document.getElementById(`task-${task.id}`);
                        if (taskElement) {
                            taskElement.classList.add('completed');
                            setTimeout(() => {
                                taskElement.remove();
                                loadTasks(); // Reload tasks to update the list
                            }, 1000);
                        }
                        
                        showNotification(`${task.name} completed! +${task.points} coins`);
                    }, 2000);
                }
            }, 300);
            
            button.textContent = 'Redirecting...';
            button.style.background = '#4cd964';
            button.disabled = true;
        }

        async function claimDailyReward() {
            // Only give reward once per day
            const lastReward = localStorage.getItem('lastRewardDate');
            const today = new Date().toDateString();
            
            if (lastReward !== today) {
                appState.points += 50;
                await updateStatsDisplay();
                await saveAllData();
                await updateLeaderboard();
                localStorage.setItem('lastRewardDate', today);
                showNotification('Daily reward claimed! +50 points');
            } else {
                showNotification('You already claimed your daily reward today!');
            }
        }

        async function deleteAccount() {
            // Reset all app state
            appState.points = 0;
            appState.friends = 0;
            appState.completedTasks = 0;
            appState.friendList = [];
            appState.referrals = 0;
            appState.referralEarnings = 0;
            appState.completedTasksList = [];
            
            // Save reset state to Firebase
            await saveAllData();
            
            // Update UI
            await updateStatsDisplay();
            await updateFriendsDisplay();
            await updateLeaderboard();
            await updateReferralStats();
            
            // Reset tasks
            await loadTasks();
            
            // Navigate to home
            document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
            document.querySelectorAll('.section').forEach(section => section.classList.remove('active'));
            document.querySelector('.nav-item[data-section="home"]').classList.add('active');
            document.getElementById('home').classList.add('active');
            
            showNotification('Account data reset successfully');
        }

        function showNotification(message) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Initialize the app when Firebase is ready
        document.addEventListener('DOMContentLoaded', function() {
            // Start the app initialization
            initApp();
        });
    </script>
</body>
</html>
