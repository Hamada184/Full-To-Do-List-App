<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaskFlow - Advanced To Do List</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #667eea;
            --primary-dark: #5a67d8;
            --secondary: #764ba2;
            --success: #48bb78;
            --danger: #f56565;
            --warning: #ed8936;
            --info: #4299e1;
            --light: #f7fafc;
            --dark: #2d3748;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        body::before {
            content: '';
            position: absolute;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 1px, transparent 1px);
            background-size: 50px 50px;
            animation: float 20s linear infinite;
        }

        @keyframes float {
            0% { transform: translate(0, 0); }
            100% { transform: translate(50px, 50px); }
        }

        .container {
            background: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            box-shadow: 0 30px 90px rgba(0, 0, 0, 0.3), 0 0 0 1px rgba(255,255,255,0.3);
            width: 100%;
            max-width: 900px;
            padding: 40px;
            animation: slideIn 0.6s cubic-bezier(0.16, 1, 0.3, 1);
            position: relative;
            z-index: 1;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(40px) scale(0.95);
            }
            to {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }

        .header {
            text-align: center;
            margin-bottom: 35px;
            position: relative;
        }

        h1 {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            font-size: 2.5em;
            margin-bottom: 8px;
            font-weight: 800;
            letter-spacing: -1px;
        }

        .subtitle {
            color: #718096;
            font-size: 15px;
            font-weight: 500;
        }

        .quick-stats {
            display: flex;
            gap: 12px;
            justify-content: center;
            margin-top: 15px;
        }

        .quick-stat {
            background: var(--light);
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 13px;
            font-weight: 600;
            color: var(--dark);
        }

        .input-section {
            background: linear-gradient(135deg, #f7fafc 0%, #edf2f7 100%);
            padding: 25px;
            border-radius: 16px;
            margin-bottom: 30px;
            box-shadow: inset 0 2px 8px rgba(0,0,0,0.05);
        }

        .input-container {
            position: relative;
            margin-bottom: 15px;
        }

        #taskInput {
            width: 100%;
            padding: 18px 180px 18px 20px;
            border: 3px solid #e2e8f0;
            border-radius: 14px;
            font-size: 16px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            background: white;
            font-weight: 500;
        }

        #taskInput:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 4px rgba(102, 126, 234, 0.1), 0 4px 12px rgba(102, 126, 234, 0.15);
            transform: translateY(-2px);
        }

        .input-actions {
            position: absolute;
            right: 8px;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            gap: 6px;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 10px;
            font-size: 14px;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        #addBtn {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
        }

        #addBtn:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.4);
        }

        #voiceBtn, #aiBtn {
            padding: 12px 14px;
            font-size: 16px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        #voiceBtn {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
        }

        #aiBtn {
            background: linear-gradient(135deg, #4fd1c5 0%, #38b2ac 100%);
            color: white;
        }

        #voiceBtn.listening {
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.05); opacity: 0.8; }
        }

        .task-options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 12px;
        }

        .option-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .option-group label {
            font-size: 12px;
            font-weight: 700;
            color: var(--dark);
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .option-group select, .option-group input {
            padding: 10px;
            border: 2px solid #e2e8f0;
            border-radius: 8px;
            font-size: 14px;
            background: white;
            font-weight: 600;
        }

        .toolbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            gap: 15px;
            flex-wrap: wrap;
        }

        .filters {
            display: flex;
            gap: 8px;
            background: white;
            padding: 6px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .filter-btn {
            padding: 10px 18px;
            background: transparent;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 13px;
            font-weight: 700;
            color: #4a5568;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .filter-btn:hover {
            background: var(--light);
        }

        .filter-btn.active {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
        }

        .search-sort {
            display: flex;
            gap: 10px;
        }

        #searchInput {
            padding: 10px 15px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            font-size: 14px;
            width: 200px;
            font-weight: 500;
        }

        #searchInput:focus {
            outline: none;
            border-color: var(--primary);
        }

        #sortSelect, #viewMode {
            padding: 10px 15px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            font-size: 13px;
            cursor: pointer;
            background: white;
            font-weight: 700;
        }

        #taskList {
            list-style: none;
            max-height: 500px;
            overflow-y: auto;
            padding-right: 8px;
        }

        #taskList::-webkit-scrollbar {
            width: 8px;
        }

        #taskList::-webkit-scrollbar-track {
            background: var(--light);
            border-radius: 10px;
        }

        #taskList::-webkit-scrollbar-thumb {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            border-radius: 10px;
        }

        .task-item {
            background: white;
            padding: 20px;
            margin-bottom: 12px;
            border-radius: 16px;
            display: flex;
            align-items: flex-start;
            gap: 15px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            border: 2px solid #e2e8f0;
            position: relative;
            overflow: hidden;
            animation: taskSlideIn 0.4s cubic-bezier(0.16, 1, 0.3, 1);
        }

        @keyframes taskSlideIn {
            from {
                opacity: 0;
                transform: translateX(-30px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        .task-item::before {
            content: '';
            position: absolute;
            left: 0;
            top: 0;
            bottom: 0;
            width: 5px;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            transition: width 0.3s;
        }

        .task-item:hover {
            border-color: var(--primary);
            box-shadow: 0 8px 24px rgba(102, 126, 234, 0.15);
            transform: translateY(-3px) scale(1.01);
        }

        .task-item:hover::before {
            width: 8px;
        }

        .task-item.completed {
            background: linear-gradient(135deg, #f7fafc 0%, #edf2f7 100%);
            border-color: var(--success);
        }

        .task-item.completed::before {
            background: var(--success);
        }

        .task-item.completed .task-text {
            text-decoration: line-through;
            color: #a0aec0;
        }

        .task-item.priority-high::before {
            background: linear-gradient(135deg, #fc8181 0%, #f56565 100%);
        }

        .task-item.priority-medium::before {
            background: linear-gradient(135deg, #fbd38d 0%, #ed8936 100%);
        }

        .task-item.priority-low::before {
            background: linear-gradient(135deg, #9ae6b4 0%, #48bb78 100%);
        }

        .task-item.overdue {
            border-color: var(--danger);
            background: linear-gradient(135deg, #fff5f5 0%, #fed7d7 100%);
        }

        .checkbox-wrapper {
            position: relative;
            margin-top: 4px;
        }

        .checkbox {
            width: 24px;
            height: 24px;
            cursor: pointer;
            accent-color: var(--primary);
        }

        .task-content {
            flex: 1;
            min-width: 0;
        }

        .task-text {
            color: var(--dark);
            font-size: 16px;
            word-break: break-word;
            line-height: 1.6;
            margin-bottom: 10px;
            font-weight: 600;
        }

        .task-meta {
            display: flex;
            gap: 10px;
            align-items: center;
            flex-wrap: wrap;
        }

        .meta-badge {
            font-size: 11px;
            padding: 5px 10px;
            border-radius: 6px;
            font-weight: 700;
            display: inline-flex;
            align-items: center;
            gap: 4px;
            text-transform: uppercase;
            letter-spacing: 0.3px;
        }

        .task-date {
            background: #e6fffa;
            color: #234e52;
        }

        .task-priority {
            background: #fed7d7;
            color: #c53030;
        }

        .task-category {
            background: #e9d8fd;
            color: #553c9a;
        }

        .task-due {
            background: #feebc8;
            color: #7c2d12;
        }

        .task-tags {
            background: #bee3f8;
            color: #2c5282;
        }

        .task-actions {
            display: flex;
            gap: 8px;
        }

        .icon-btn {
            width: 36px;
            height: 36px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .edit-btn {
            background: #bee3f8;
            color: #2c5282;
        }

        .edit-btn:hover {
            background: #90cdf4;
            transform: rotate(15deg) scale(1.1);
        }

        .delete-btn {
            background: #fed7d7;
            color: #c53030;
        }

        .delete-btn:hover {
            background: #fc8181;
            color: white;
            transform: scale(1.1);
        }

        .duplicate-btn {
            background: #d6bcfa;
            color: #553c9a;
        }

        .duplicate-btn:hover {
            background: #b794f4;
            transform: scale(1.1);
        }

        .stats-grid {
            margin-top: 30px;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 15px;
        }

        .stat-card {
            background: white;
            padding: 20px;
            border-radius: 14px;
            text-align: center;
            box-shadow: 0 4px 12px rgba(0,0,0,0.08);
            transition: all 0.3s;
            border: 2px solid transparent;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 24px rgba(0,0,0,0.12);
            border-color: var(--primary);
        }

        .stat-icon {
            font-size: 28px;
            margin-bottom: 8px;
        }

        .stat-value {
            font-size: 32px;
            font-weight: 800;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            display: block;
            margin-bottom: 4px;
        }

        .stat-label {
            font-size: 12px;
            color: #718096;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .progress-section {
            margin-top: 20px;
            padding: 20px;
            background: white;
            border-radius: 14px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.08);
        }

        .progress-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 12px;
        }

        .progress-label {
            font-weight: 700;
            color: var(--dark);
            font-size: 14px;
        }

        .progress-percent {
            font-weight: 800;
            color: var(--primary);
            font-size: 16px;
        }

        .progress-bar {
            height: 12px;
            background: var(--light);
            border-radius: 10px;
            overflow: hidden;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--primary) 0%, var(--secondary) 50%, #f093fb 100%);
            transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .progress-fill::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            bottom: 0;
            right: 0;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            animation: shimmer 2s infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .action-buttons {
            margin-top: 20px;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 12px;
        }

        .action-btn {
            padding: 14px 20px;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            font-size: 13px;
            font-weight: 700;
            transition: all 0.3s;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .btn-danger {
            background: linear-gradient(135deg, #fc8181 0%, #f56565 100%);
            color: white;
        }

        .btn-success {
            background: linear-gradient(135deg, #68d391 0%, #48bb78 100%);
            color: white;
        }

        .btn-info {
            background: linear-gradient(135deg, #63b3ed 0%, #4299e1 100%);
            color: white;
        }

        .btn-warning {
            background: linear-gradient(135deg, #fbd38d 0%, #ed8936 100%);
            color: white;
        }

        .action-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.15);
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(5px);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            animation: fadeIn 0.3s;
        }

        .modal.active {
            display: flex;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .modal-content {
            background: white;
            padding: 35px;
            border-radius: 20px;
            width: 90%;
            max-width: 500px;
            max-height: 90vh;
            overflow-y: auto;
            animation: modalSlide 0.4s cubic-bezier(0.16, 1, 0.3, 1);
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
        }

        @keyframes modalSlide {
            from {
                opacity: 0;
                transform: scale(0.9) translateY(20px);
            }
            to {
                opacity: 1;
                transform: scale(1) translateY(0);
            }
        }

        .modal h2 {
            margin-bottom: 25px;
            color: var(--dark);
            font-size: 24px;
            font-weight: 800;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 700;
            color: var(--dark);
            font-size: 13px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .form-group input, .form-group select, .form-group textarea {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            font-size: 15px;
            font-weight: 500;
            transition: all 0.3s;
        }

        .form-group textarea {
            resize: vertical;
            min-height: 80px;
            font-family: inherit;
        }

        .form-group input:focus, .form-group select:focus, .form-group textarea:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .modal-actions {
            display: flex;
            gap: 12px;
            margin-top: 25px;
        }

        .modal-actions button {
            flex: 1;
            padding: 14px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 700;
            font-size: 14px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.3s;
        }

        .btn-save {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
        }

        .btn-cancel {
            background: #e2e8f0;
            color: var(--dark);
        }

        .modal-actions button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 16px rgba(0,0,0,0.15);
        }

        .empty-state {
            text-align: center;
            padding: 60px 20px;
            color: #a0aec0;
        }

        .empty-state svg {
            width: 120px;
            height: 120px;
            margin-bottom: 20px;
            opacity: 0.2;
        }

        .empty-state p {
            font-size: 18px;
            font-weight: 600;
        }

        .toast {
            position: fixed;
            bottom: 30px;
            right: 30px;
            background: white;
            padding: 16px 24px;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            display: flex;
            align-items: center;
            gap: 12px;
            z-index: 2000;
            animation: slideInRight 0.4s cubic-bezier(0.16, 1, 0.3, 1);
            border-left: 4px solid var(--success);
        }

        @keyframes slideInRight {
            from {
                transform: translateX(400px);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        .toast.hide {
            animation: slideOutRight 0.4s cubic-bezier(0.4, 0, 1, 1);
        }

        @keyframes slideOutRight {
            to {
                transform: translateX(400px);
                opacity: 0;
            }
        }

        .toast-icon {
            font-size: 24px;
        }

        .toast-message {
            font-weight: 600;
            color: var(--dark);
        }

        .view-grid #taskList {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 15px;
        }

        .view-grid .task-item {
            flex-direction: column;
        }

        @media (max-width: 768px) {
            .container {
                padding: 25px;
            }

            h1 {
                font-size: 2em;
            }

            .toolbar {
                flex-direction: column;
            }

            .search-sort {
                width: 100%;
                flex-direction: column;
            }

            #searchInput {
                width: 100%;
            }

            .input-actions {
                position: static;
                transform: none;
                margin-top: 10px;
                justify-content: flex-end;
            }

            #taskInput {
                padding-right: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>⚡ TaskFlow</h1>
            <p class="subtitle">Master your productivity with intelligent task management</p>
            <div class="quick-stats">
                <span class="quick-stat" id="quickToday">Today: 0</span>
                <span class="quick-stat" id="quickOverdue">Overdue: 0</span>
            </div>
        </div>
        
        <div class="input-section">
            <div class="input-container">
                <input type="text" id="taskInput" placeholder="What's your next mission?" />
                <div class="input-actions">
                    <button id="aiBtn" title="AI Suggestions">🤖</button>
                    <button id="voiceBtn" title="Voice Input">🎤</button>
                    <button id="addBtn" class="btn">Add</button>
                </div>
            </div>
            <div class="task-options">
                <div class="option-group">
                    <label>Priority</label>
                    <select id="priorityInput">
                        <option value="medium">Medium</option>
                        <option value="high">High</option>
                        <option value="low">Low</option>
                    </select>
                </div>
                <div class="option-group">
                    <label>Category</label>
                    <input type="text" id="categoryInput" placeholder="e.g., Work" />
                </div>
                <div class="option-group">
                    <label>Due Date</label>
                    <input type="date" id="dueDateInput" />
                </div>
                <div class="option-group">
                    <label>Tags</label>
                    <input type="text" id="tagsInput" placeholder="#urgent #important" />
                </div>
            </div>
        </div>

        <div class="toolbar">
            <div class="filters">
                <button class="filter-btn active" data-filter="all">All</button>
                <button class="filter-btn" data-filter="today">Today</button>
                <button class="filter-btn" data-filter="active">Active</button>
                <button class="filter-btn" data-filter="completed">Completed</button>
                <button class="filter-btn" data-filter="overdue">Overdue</button>
            </div>
            <div class="search-sort">
                <input type="text" id="searchInput" placeholder="🔍 Search tasks..." />
                <select id="sortSelect">
                    <option value="date">Date Added</option>
                    <option value="priority">Priority</option>
                    <option value="duedate">Due Date</option>
                    <option value="name">Name</option>
                </select>
                <select id="viewMode">
                    <option value="list">📋 List</option>
                    <option value="grid">📊 Grid</option>
                </select>
            </div>
        </div>

        <ul id="taskList"></ul>

        <div class="progress-section">
            <div class="progress-header">
                <span class="progress-label">Overall Progress</span>
                <span class="progress-percent" id="progressPercent">0%</span>
            </div>
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill"></div>
            </div>
        </div>

        <div class="stats-grid">
            <div class="stat-card">
                <div class="stat-icon">📝</div>
                <span class="stat-value" id="totalTasks">0</span>
                <span class="stat-label">Total Tasks</span>
            </div>
            <div class="stat-card">
                <div class="stat-icon">⚡</div>
                <span class="stat-value" id="activeTasks">0</span>
                <span class="stat-label">Active</span>
            </div>
            <div class="stat-card">
                <div class="stat-icon">✅</div>
                <span class="stat-value" id="completedTasks">0</span>
                <span class="stat-label">Completed</span>
            </div>
            <div class="stat-card">
                <div class="stat-icon">🎯</div>
                <span class="stat-value" id="productivityScore">0</span>
                <span class="stat-label">Score</span>
            </div>
        </div>

        <div class="action-buttons">
            <button class="action-btn btn-success" id="completeAllBtn">✓ Complete All</button>
            <button class="action-btn btn-danger" id="clearCompleted">🗑️ Clear Done</button>
            <button class="action-btn btn-info" id="exportBtn">📤 Export</button>
            <button class="action-btn btn-warning" id="importBtn">📥 Import</button>
        </div>
    </div>

    <div class="modal" id="editModal">
        <div class="modal-content">
            <h2>✏️ Edit Task</h2>
            <div class="form-group">
                <label>Task Description</label>
                <textarea id="editTaskInput"></textarea>
            </div>
            <div class="form-group">
                <label>Priority Level</label>
                <select id="editPriority">
                    <option value="low">Low</option>
                    <option value="medium">Medium</option>
                    <option value="high">High</option>
                </select>
            </div>
            <div class="form-group">
                <label>Category</label>
                <input type="text" id="editCategory" placeholder="e.g., Work, Personal, Health" />
            </div>
            <div class="form-group">
                <label>Due Date</label>
                <input type="date" id="editDueDate" />
            </div>
            <div class="form-group">
                <label>Tags</label>
                <input type="text" id="editTags" placeholder="#urgent #important" />
            </div>
            <div class="modal-actions">
                <button class="btn-save" id="saveEditBtn">Save Changes</button>
                <button class="btn-cancel" id="cancelEditBtn">Cancel</button>
            </div>
        </div>
    </div>

    <input type="file" id="importFile" accept=".json" style="display: none;" />

    <script>
        let tasks = [];
        let currentFilter = 'all';
        let currentSort = 'date';
        let currentView = 'list';
        let editingTaskId = null;
        let searchQuery = '';

        const taskInput = document.getElementById('taskInput');
        const addBtn = document.getElementById('addBtn');
        const voiceBtn = document.getElementById('voiceBtn');
        const aiBtn = document.getElementById('aiBtn');
        const priorityInput = document.getElementById('priorityInput');
        const categoryInput = document.getElementById('categoryInput');
        const dueDateInput = document.getElementById('dueDateInput');
        const tagsInput = document.getElementById('tagsInput');
        const taskList = document.getElementById('taskList');
        const filterBtns = document.querySelectorAll('.filter-btn');
        const sortSelect = document.getElementById('sortSelect');
        const viewMode = document.getElementById('viewMode');
        const searchInput = document.getElementById('searchInput');
        const clearCompletedBtn = document.getElementById('clearCompleted');
        const completeAllBtn = document.getElementById('completeAllBtn');
        const exportBtn = document.getElementById('exportBtn');
        const importBtn = document.getElementById('importBtn');
        const importFile = document.getElementById('importFile');
        const progressFill = document.getElementById('progressFill');
        const progressPercent = document.getElementById('progressPercent');
        const editModal = document.getElementById('editModal');

        const editTaskInput = document.getElementById('editTaskInput');
        const editPriority = document.getElementById('editPriority');
        const editCategory = document.getElementById('editCategory');
        const editDueDate = document.getElementById('editDueDate');
        const editTags = document.getElementById('editTags');
        const saveEditBtn = document.getElementById('saveEditBtn');
        const cancelEditBtn = document.getElementById('cancelEditBtn');

        function showToast(message, icon = '✅') {
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.innerHTML = `
                <span class="toast-icon">${icon}</span>
                <span class="toast-message">${message}</span>
            `;
            document.body.appendChild(toast);
            
            setTimeout(() => {
                toast.classList.add('hide');
                setTimeout(() => toast.remove(), 400);
            }, 3000);
        }

        function addTask() {
            const taskText = taskInput.value.trim();
            
            if (taskText === '') {
                taskInput.style.borderColor = 'var(--danger)';
                setTimeout(() => taskInput.style.borderColor = '#e2e8f0', 500);
                return;
            }

            const task = {
                id: Date.now(),
                text: taskText,
                completed: false,
                priority: priorityInput.value,
                category: categoryInput.value.trim(),
                dueDate: dueDateInput.value,
                tags: tagsInput.value.trim(),
                createdAt: new Date().toISOString()
            };

            tasks.push(task);
            taskInput.value = '';
            categoryInput.value = '';
            dueDateInput.value = '';
            tagsInput.value = '';
            priorityInput.value = 'medium';
            
            saveTasks();
            renderTasks();
            updateStats();
            showToast('Task added successfully!', '✨');
        }

        function toggleTask(id) {
            const task = tasks.find(t => t.id === id);
            tasks = tasks.map(t => 
                t.id === id ? { ...t, completed: !t.completed } : t
            );
            saveTasks();
            renderTasks();
            updateStats();
            showToast(task.completed ? 'Task marked active!' : 'Task completed!', task.completed ? '🔄' : '✅');
        }

        function deleteTask(id) {
            if (confirm('Are you sure you want to delete this task?')) {
                tasks = tasks.filter(task => task.id !== id);
                saveTasks();
                renderTasks();
                updateStats();
                showToast('Task deleted!', '🗑️');
            }
        }

        function duplicateTask(id) {
            const task = tasks.find(t => t.id === id);
            if (!task) return;

            const newTask = {
                ...task,
                id: Date.now(),
                completed: false,
                text: task.text + ' (Copy)',
                createdAt: new Date().toISOString()
            };

            tasks.push(newTask);
            saveTasks();
            renderTasks();
            updateStats();
            showToast('Task duplicated!', '📋');
        }

        function editTask(id) {
            const task = tasks.find(t => t.id === id);
            if (!task) return;

            editingTaskId = id;
            editTaskInput.value = task.text;
            editPriority.value = task.priority;
            editCategory.value = task.category || '';
            editDueDate.value = task.dueDate || '';
            editTags.value = task.tags || '';
            editModal.classList.add('active');
        }

        function saveEdit() {
            if (editingTaskId === null) return;

            tasks = tasks.map(task => 
                task.id === editingTaskId ? {
                    ...task,
                    text: editTaskInput.value.trim(),
                    priority: editPriority.value,
                    category: editCategory.value.trim(),
                    dueDate: editDueDate.value,
                    tags: editTags.value.trim()
                } : task
            );

            editModal.classList.remove('active');
            editingTaskId = null;
            saveTasks();
            renderTasks();
            showToast('Task updated!', '✏️');
        }

        function clearCompleted() {
            const count = tasks.filter(t => t.completed).length;
            if (count === 0) {
                showToast('No completed tasks to clear!', 'ℹ️');
                return;
            }
            if (confirm(`Delete ${count} completed task(s)?`)) {
                tasks = tasks.filter(task => !task.completed);
                saveTasks();
                renderTasks();
                updateStats();
                showToast(`${count} task(s) cleared!`, '🗑️');
            }
        }

        function completeAll() {
            const incompleteTasks = tasks.filter(t => !t.completed);
            if (incompleteTasks.length === 0) {
                showToast('All tasks are already completed!', 'ℹ️');
                return;
            }
            tasks = tasks.map(task => ({ ...task, completed: true }));
            saveTasks();
            renderTasks();
            updateStats();
            showToast('All tasks completed! 🎉', '✅');
        }

        function exportTasks() {
            const dataStr = JSON.stringify(tasks, null, 2);
            const dataBlob = new Blob([dataStr], { type: 'application/json' });
            const url = URL.createObjectURL(dataBlob);
            const link = document.createElement('a');
            link.href = url;
            link.download = `taskflow_${new Date().toISOString().split('T')[0]}.json`;
            link.click();
            URL.revokeObjectURL(url);
            showToast('Tasks exported successfully!', '📤');
        }

        function importTasks() {
            importFile.click();
        }

        importFile.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = (event) => {
                try {
                    const importedTasks = JSON.parse(event.target.result);
                    if (Array.isArray(importedTasks)) {
                        tasks = importedTasks;
                        saveTasks();
                        renderTasks();
                        updateStats();
                        showToast('Tasks imported successfully!', '📥');
                    } else {
                        showToast('Invalid file format!', '❌');
                    }
                } catch (error) {
                    showToast('Error importing tasks!', '❌');
                }
            };
            reader.readAsText(file);
            importFile.value = '';
        });

        function isOverdue(dueDate) {
            if (!dueDate) return false;
            const today = new Date();
            today.setHours(0, 0, 0, 0);
            const due = new Date(dueDate);
            return due < today;
        }

        function isToday(dueDate) {
            if (!dueDate) return false;
            const today = new Date();
            today.setHours(0, 0, 0, 0);
            const due = new Date(dueDate);
            due.setHours(0, 0, 0, 0);
            return due.getTime() === today.getTime();
        }

        function getFilteredTasks() {
            let filtered = tasks;
            
            if (searchQuery) {
                const query = searchQuery.toLowerCase();
                filtered = filtered.filter(task => 
                    task.text.toLowerCase().includes(query) ||
                    (task.category && task.category.toLowerCase().includes(query)) ||
                    (task.tags && task.tags.toLowerCase().includes(query))
                );
            }

            if (currentFilter === 'active') {
                filtered = filtered.filter(task => !task.completed);
            } else if (currentFilter === 'completed') {
                filtered = filtered.filter(task => task.completed);
            } else if (currentFilter === 'today') {
                filtered = filtered.filter(task => !task.completed && isToday(task.dueDate));
            } else if (currentFilter === 'overdue') {
                filtered = filtered.filter(task => !task.completed && isOverdue(task.dueDate));
            }

            if (currentSort === 'priority') {
                const priorityOrder = { high: 0, medium: 1, low: 2 };
                filtered.sort((a, b) => priorityOrder[a.priority] - priorityOrder[b.priority]);
            } else if (currentSort === 'name') {
                filtered.sort((a, b) => a.text.localeCompare(b.text));
            } else if (currentSort === 'duedate') {
                filtered.sort((a, b) => {
                    if (!a.dueDate) return 1;
                    if (!b.dueDate) return -1;
                    return new Date(a.dueDate) - new Date(b.dueDate);
                });
            } else {
                filtered.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
            }

            return filtered;
        }

        function renderTasks() {
            const container = document.getElementById('taskList').parentElement;
            if (currentView === 'grid') {
                container.classList.add('view-grid');
            } else {
                container.classList.remove('view-grid');
            }

            const filteredTasks = getFilteredTasks();
            
            if (filteredTasks.length === 0) {
                taskList.innerHTML = `
                    <div class="empty-state">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" />
                        </svg>
                        <p>No tasks found</p>
                    </div>
                `;
                return;
            }

            taskList.innerHTML = filteredTasks.map(task => {
                const date = new Date(task.createdAt).toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric' });
                const overdue = !task.completed && isOverdue(task.dueDate);
                const dueDateStr = task.dueDate ? new Date(task.dueDate).toLocaleDateString('en-US', { month: 'short', day: 'numeric' }) : '';
                
                return `
                    <li class="task-item ${task.completed ? 'completed' : ''} ${overdue ? 'overdue' : ''} priority-${task.priority}" data-id="${task.id}">
                        <div class="checkbox-wrapper">
                            <input type="checkbox" class="checkbox" ${task.completed ? 'checked' : ''} />
                        </div>
                        <div class="task-content">
                            <div class="task-text">${task.text}</div>
                            <div class="task-meta">
                                <span class="meta-badge task-date">📅 ${date}</span>
                                <span class="meta-badge task-priority">⚡ ${task.priority.toUpperCase()}</span>
                                ${task.category ? `<span class="meta-badge task-category">📂 ${task.category}</span>` : ''}
                                ${task.dueDate ? `<span class="meta-badge task-due">${overdue ? '⚠️' : '⏰'} ${dueDateStr}</span>` : ''}
                                ${task.tags ? `<span class="meta-badge task-tags">${task.tags}</span>` : ''}
                            </div>
                        </div>
                        <div class="task-actions">
                            <button class="icon-btn duplicate-btn" title="Duplicate">📋</button>
                            <button class="icon-btn edit-btn" title="Edit">✏️</button>
                            <button class="icon-btn delete-btn" title="Delete">🗑️</button>
                        </div>
                    </li>
                `;
            }).join('');

            document.querySelectorAll('.checkbox').forEach(checkbox => {
                checkbox.addEventListener('change', (e) => {
                    const id = parseInt(e.target.closest('.task-item').dataset.id);
                    toggleTask(id);
                });
            });

            document.querySelectorAll('.edit-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const id = parseInt(e.target.closest('.task-item').dataset.id);
                    editTask(id);
                });
            });

            document.querySelectorAll('.delete-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const id = parseInt(e.target.closest('.task-item').dataset.id);
                    deleteTask(id);
                });
            });

            document.querySelectorAll('.duplicate-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const id = parseInt(e.target.closest('.task-item').dataset.id);
                    duplicateTask(id);
                });
            });
        }

        function updateStats() {
            const total = tasks.length;
            const completed = tasks.filter(task => task.completed).length;
            const active = total - completed;
            const rate = total > 0 ? Math.round((completed / total) * 100) : 0;
            const todayTasks = tasks.filter(t => !t.completed && isToday(t.dueDate)).length;
            const overdueTasks = tasks.filter(t => !t.completed && isOverdue(t.dueDate)).length;
            
            const score = Math.min(100, Math.round((completed * 10) + (rate * 0.5)));
            
            document.getElementById('totalTasks').textContent = total;
            document.getElementById('activeTasks').textContent = active;
            document.getElementById('completedTasks').textContent = completed;
            document.getElementById('productivityScore').textContent = score;
            document.getElementById('progressPercent').textContent = rate + '%';
            document.getElementById('quickToday').textContent = `Today: ${todayTasks}`;
            document.getElementById('quickOverdue').textContent = `Overdue: ${overdueTasks}`;
            
            progressFill.style.width = rate + '%';
        }

        function saveTasks() {
            try {
                localStorage.setItem('taskflow_tasks', JSON.stringify(tasks));
            } catch (e) {
                console.log('Storage not available');
            }
        }

        function loadTasks() {
            try {
                const saved = localStorage.getItem('taskflow_tasks');
                if (saved) {
                    tasks = JSON.parse(saved);
                    renderTasks();
                    updateStats();
                }
            } catch (e) {
                console.log('Could not load tasks');
            }
        }

        function initVoiceInput() {
            if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
                voiceBtn.style.display = 'none';
                return;
            }

            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            const recognition = new SpeechRecognition();
            recognition.lang = 'en-US';
            recognition.continuous = false;

            voiceBtn.addEventListener('click', () => {
                recognition.start();
                voiceBtn.classList.add('listening');
                showToast('Listening...', '🎤');
            });

            recognition.onresult = (event) => {
                const transcript = event.results[0][0].transcript;
                taskInput.value = transcript;
                voiceBtn.classList.remove('listening');
                showToast('Voice input received!', '✅');
            };

            recognition.onerror = () => {
                voiceBtn.classList.remove('listening');
                showToast('Voice input failed!', '❌');
            };

            recognition.onend = () => {
                voiceBtn.classList.remove('listening');
            };
        }

        function initAI() {
            aiBtn.addEventListener('click', () => {
                const suggestions = [
                    'Review project documentation',
                    'Schedule team meeting',
                    'Update project timeline',
                    'Send follow-up emails',
                    'Prepare presentation slides',
                    'Review code changes',
                    'Plan weekly goals',
                    'Organize workspace'
                ];
                const suggestion = suggestions[Math.floor(Math.random() * suggestions.length)];
                taskInput.value = suggestion;
                showToast('AI suggestion generated!', '🤖');
            });
        }

        addBtn.addEventListener('click', addTask);
        taskInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                addTask();
            }
        });

        filterBtns.forEach(btn => {
            btn.addEventListener('click', (e) => {
                filterBtns.forEach(b => b.classList.remove('active'));
                e.target.classList.add('active');
                currentFilter = e.target.dataset.filter;
                renderTasks();
            });
        });

        sortSelect.addEventListener('change', (e) => {
            currentSort = e.target.value;
            renderTasks();
        });

        viewMode.addEventListener('change', (e) => {
            currentView = e.target.value;
            renderTasks();
        });

        searchInput.addEventListener('input', (e) => {
            searchQuery = e.target.value.trim();
            renderTasks();
        });

        clearCompletedBtn.addEventListener('click', clearCompleted);
        completeAllBtn.addEventListener('click', completeAll);
        exportBtn.addEventListener('click', exportTasks);
        importBtn.addEventListener('click', importTasks);
        saveEditBtn.addEventListener('click', saveEdit);
        cancelEditBtn.addEventListener('click', () => {
            editModal.classList.remove('active');
            editingTaskId = null;
        });

        editModal.addEventListener('click', (e) => {
            if (e.target === editModal) {
                editModal.classList.remove('active');
                editingTaskId = null;
            }
        });

        initVoiceInput();
        initAI();
        loadTasks();
        renderTasks();
        updateStats();
    </script>
</body>
</html>
