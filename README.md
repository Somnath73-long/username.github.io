<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Homework Calendar</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #333;
            min-height: 100vh;
            padding: 15px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .container {
            width: 100%;
            max-width: 500px;
            background: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }
        
        header {
            background: #4a6fc7;
            color: white;
            text-align: center;
            padding: 20px 15px;
            position: relative;
        }
        
        h1 {
            font-size: 1.6rem;
            margin-bottom: 5px;
        }
        
        .header-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
        }
        
        .current-month {
            font-size: 1.2rem;
            font-weight: 600;
        }
        
        .nav-btn {
            background: rgba(255, 255, 255, 0.2);
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            color: white;
            font-size: 1.2rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .calendar {
            padding: 15px;
        }
        
        .weekdays {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            text-align: center;
            font-weight: 600;
            color: #6a6a6a;
            margin-bottom: 10px;
            font-size: 0.9rem;
        }
        
        .days {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
        }
        
        .day {
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            font-weight: 500;
            cursor: pointer;
            position: relative;
            transition: all 0.2s;
        }
        
        .day:hover {
            background: #f0f5ff;
        }
        
        .day.other {
            color: #b0b0b0;
        }
        
        .day.selected {
            background: #4a6fc7;
            color: white;
        }
        
        .day.has-homework::after {
            content: '';
            position: absolute;
            bottom: 4px;
            width: 6px;
            height: 6px;
            background: #ff6b6b;
            border-radius: 50%;
        }
        
        .homework-section {
            padding: 15px;
            border-top: 1px solid #eee;
            max-height: 300px;
            overflow-y: auto;
        }
        
        .section-title {
            font-size: 1.2rem;
            margin-bottom: 15px;
            color: #4a6fc7;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .homework-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        
        .homework-item {
            background: #f8f9ff;
            border-radius: 12px;
            padding: 15px;
            border-left: 4px solid #4a6fc7;
        }
        
        .homework-title {
            font-weight: 600;
            margin-bottom: 5px;
            color: #3a3a3a;
        }
        
        .homework-due {
            font-size: 0.85rem;
            color: #ff6b6b;
            margin-bottom: 8px;
        }
        
        .homework-desc {
            font-size: 0.95rem;
            color: #666;
            line-height: 1.4;
        }
        
        .add-btn {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: #4a6fc7;
            color: white;
            border: none;
            font-size: 1.8rem;
            box-shadow: 0 4px 15px rgba(74, 111, 199, 0.5);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 100;
        }
        
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            padding: 20px;
            display: none;
        }
        
        .modal-content {
            background: white;
            width: 100%;
            max-width: 500px;
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 5px 25px rgba(0, 0, 0, 0.2);
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .modal-title {
            font-size: 1.4rem;
            color: #4a6fc7;
        }
        
        .close-btn {
            background: none;
            border: none;
            font-size: 1.5rem;
            color: #999;
            cursor: pointer;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }
        
        input, textarea {
            width: 100%;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 10px;
            font-size: 1rem;
        }
        
        textarea {
            min-height: 120px;
            resize: vertical;
        }
        
        .submit-btn {
            background: #4a6fc7;
            color: white;
            border: none;
            padding: 16px;
            width: 100%;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .submit-btn:hover {
            background: #3a5bb7;
        }
        
        .empty-state {
            text-align: center;
            padding: 30px 0;
            color: #999;
        }
        
        .empty-state i {
            font-size: 3rem;
            margin-bottom: 15px;
            color: #ddd;
        }
        
        .sync-indicator {
            position: fixed;
            top: 20px;
            right: 20px;
            background: white;
            padding: 10px 15px;
            border-radius: 50px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9rem;
            z-index: 1000;
            display: none;
        }
        
        .sync-indicator .dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background: #4caf50;
        }
        
        .sync-indicator.syncing .dot {
            background: #ff9800;
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        
        @media (max-width: 400px) {
            .day {
                height: 36px;
                font-size: 0.9rem;
            }
            
            h1 {
                font-size: 1.4rem;
            }
            
            .current-month {
                font-size: 1.1rem;
            }
        }
    </style>
</head>
<body>
    <div class="sync-indicator" id="sync-indicator">
        <div class="dot"></div>
        <span>Changes saved locally</span>
    </div>
    
    <div class="container">
        <header>
            <h1>Homework Calendar</h1>
            <p>Track assignments and due dates</p>
            
            <div class="header-controls">
                <button class="nav-btn" id="prev-month"><i class="fas fa-chevron-left"></i></button>
                <div class="current-month" id="current-month">September 2023</div>
                <button class="nav-btn" id="next-month"><i class="fas fa-chevron-right"></i></button>
            </div>
        </header>
        
        <div class="calendar">
            <div class="weekdays">
                <div>Sun</div>
                <div>Mon</div>
                <div>Tue</div>
                <div>Wed</div>
                <div>Thu</div>
                <div>Fri</div>
                <div>Sat</div>
            </div>
            
            <div class="days" id="calendar-days">
                <!-- Days will be generated by JavaScript -->
            </div>
        </div>
        
        <div class="homework-section">
            <div class="section-title">
                <span id="selected-date">Today's Homework</span>
                <span id="homework-count">0 items</span>
            </div>
            
            <div class="homework-list" id="homework-list">
                <!-- Homework items will be displayed here -->
            </div>
        </div>
    </div>
    
    <button class="add-btn" id="add-btn">
        <i class="fas fa-plus"></i>
    </button>
    
    <div class="modal" id="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Add Homework</h2>
                <button class="close-btn" id="close-modal"><i class="fas fa-times"></i></button>
            </div>
            
            <form id="homework-form">
                <div class="form-group">
                    <label for="title">Title *</label>
                    <input type="text" id="title" placeholder="Math exercises" required>
                </div>
                
                <div class="form-group">
                    <label for="due-date">Due Date *</label>
                    <input type="date" id="due-date" required>
                </div>
                
                <div class="form-group">
                    <label for="description">Description</label>
                    <textarea id="description" placeholder="Chapter 5, problems 1-10"></textarea>
                </div>
                
                <button type="submit" class="submit-btn">Add Homework</button>
            </form>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Elements
            const calendarDays = document.getElementById('calendar-days');
            const currentMonthElement = document.getElementById('current-month');
            const prevMonthButton = document.getElementById('prev-month');
            const nextMonthButton = document.getElementById('next-month');
            const selectedDateElement = document.getElementById('selected-date');
            const homeworkList = document.getElementById('homework-list');
            const homeworkCount = document.getElementById('homework-count');
            const addButton = document.getElementById('add-btn');
            const modal = document.getElementById('modal');
            const closeModal = document.getElementById('close-modal');
            const homeworkForm = document.getElementById('homework-form');
            const syncIndicator = document.getElementById('sync-indicator');
            
            // Current date
            let currentDate = new Date();
            let currentMonth = currentDate.getMonth();
            let currentYear = currentDate.getFullYear();
            let selectedDate = new Date();
            
            // Homework data (stored in localStorage)
            let homeworkData = JSON.parse(localStorage.getItem('homeworkData')) || {};
            
            // Initialize the calendar
            function initCalendar() {
                updateCalendar();
                updateHomeworkList();
                
                // Event listeners
                prevMonthButton.addEventListener('click', () => {
                    currentMonth--;
                    if (currentMonth < 0) {
                        currentMonth = 11;
                        currentYear--;
                    }
                    updateCalendar();
                });
                
                nextMonthButton.addEventListener('click', () => {
                    currentMonth++;
                    if (currentMonth > 11) {
                        currentMonth = 0;
                        currentYear++;
                    }
                    updateCalendar();
                });
                
                addButton.addEventListener('click', () => {
                    // Set the due date to the selected date by default
                    document.getElementById('due-date').value = formatDate(selectedDate);
                    modal.style.display = 'flex';
                });
                
                closeModal.addEventListener('click', () => {
                    modal.style.display = 'none';
                });
                
                homeworkForm.addEventListener('submit', (e) => {
                    e.preventDefault();
                    addHomework();
                });
            }
            
            // Show sync status
            function showSyncStatus(message, isSyncing = false) {
                syncIndicator.style.display = 'flex';
                syncIndicator.querySelector('span').textContent = message;
                
                if (isSyncing) {
                    syncIndicator.classList.add('syncing');
                } else {
                    syncIndicator.classList.remove('syncing');
                }
                
                // Hide after 3 seconds
                setTimeout(() => {
                    syncIndicator.style.display = 'none';
                }, 3000);
            }
            
            // Save data to localStorage
            function saveData() {
                localStorage.setItem('homeworkData', JSON.stringify(homeworkData));
                showSyncStatus('Changes saved locally');
            }
            
            // Update the calendar display
            function updateCalendar() {
                // Update current month display
                const monthNames = ["January", "February", "March", "April", "May", "June",
                    "July", "August", "September", "October", "November", "December"
                ];
                currentMonthElement.textContent = `${monthNames[currentMonth]} ${currentYear}`;
                
                // Clear previous days
                calendarDays.innerHTML = '';
                
                // Get first day of month and total days
                const firstDay = new Date(currentYear, currentMonth, 1).getDay();
                const daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();
                
                // Previous month days
                const daysInPrevMonth = new Date(currentYear, currentMonth, 0).getDate();
                for (let i = 0; i < firstDay; i++) {
                    const dayElement = document.createElement('div');
                    dayElement.className = 'day other';
                    dayElement.textContent = daysInPrevMonth - firstDay + i + 1;
                    calendarDays.appendChild(dayElement);
                }
                
                // Current month days
                for (let i = 1; i <= daysInMonth; i++) {
                    const dayElement = document.createElement('div');
                    dayElement.className = 'day';
                    dayElement.textContent = i;
                    
                    // Check if this date has homework
                    const dateStr = formatDate(new Date(currentYear, currentMonth, i));
                    if (homeworkData[dateStr] && homeworkData[dateStr].length > 0) {
                        dayElement.classList.add('has-homework');
                    }
                    
                    // Check if this is the selected date
                    if (i === selectedDate.getDate() && 
                        currentMonth === selectedDate.getMonth() && 
                        currentYear === selectedDate.getFullYear()) {
                        dayElement.classList.add('selected');
                    }
                    
                    // Add click event
                    dayElement.addEventListener('click', () => {
                        selectedDate = new Date(currentYear, currentMonth, i);
                        updateCalendar();
                        updateHomeworkList();
                    });
                    
                    calendarDays.appendChild(dayElement);
                }
                
                // Next month days
                const daysNextMonth = 42 - (firstDay + daysInMonth); // 6 rows of 7 days
                for (let i = 1; i <= daysNextMonth; i++) {
                    const dayElement = document.createElement('div');
                    dayElement.className = 'day other';
                    dayElement.textContent = i;
                    calendarDays.appendChild(dayElement);
                }
            }
            
            // Update the homework list
            function updateHomeworkList() {
                const dateStr = formatDate(selectedDate);
                const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
                selectedDateElement.textContent = selectedDate.toLocaleDateString('en-US', options);
                
                // Clear previous homework
                homeworkList.innerHTML = '';
                
                // Check if there's homework for this date
                if (homeworkData[dateStr] && homeworkData[dateStr].length > 0) {
                    homeworkCount.textContent = `${homeworkData[dateStr].length} items`;
                    
                    // Add homework items
                    homeworkData[dateStr].forEach((homework, index) => {
                        const homeworkItem = document.createElement('div');
                        homeworkItem.className = 'homework-item';
                        homeworkItem.innerHTML = `
                            <div class="homework-title">${homework.title}</div>
                            <div class="homework-due">Due: ${new Date(homework.dueDate).toLocaleDateString()}</div>
                            <div class="homework-desc">${homework.description || 'No description'}</div>
                            <div style="text-align: right; margin-top: 10px;">
                                <button class="delete-btn" data-date="${dateStr}" data-index="${index}">Delete</button>
                            </div>
                        `;
                        homeworkList.appendChild(homeworkItem);
                    });
                    
                    // Add event listeners to delete buttons
                    document.querySelectorAll('.delete-btn').forEach(button => {
                        button.addEventListener('click', function() {
                            const date = this.getAttribute('data-date');
                            const index = parseInt(this.getAttribute('data-index'));
                            deleteHomework(date, index);
                        });
                    });
                } else {
                    homeworkCount.textContent = '0 items';
                    homeworkList.innerHTML = `
                        <div class="empty-state">
                            <i class="fas fa-tasks"></i>
                            <p>No homework for this date</p>
                        </div>
                    `;
                }
            }
            
            // Add new homework
            function addHomework() {
                const title = document.getElementById('title').value;
                const dueDate = document.getElementById('due-date').value;
                const description = document.getElementById('description').value;
                
                if (!title || !dueDate) {
                    alert('Please fill in all required fields');
                    return;
                }
                
                // Add to homework data
                if (!homeworkData[dueDate]) {
                    homeworkData[dueDate] = [];
                }
                
                homeworkData[dueDate].push({
                    title,
                    dueDate,
                    description
                });
                
                // Save data
                saveData();
                
                // Reset form and close modal
                homeworkForm.reset();
                modal.style.display = 'none';
                
                // Update calendar and homework list
                updateCalendar();
                
                // If the due date is in the current view, update the homework list
                const dueDateObj = new Date(dueDate);
                if (dueDateObj.getMonth() === currentMonth && 
                    dueDateObj.getFullYear() === currentYear) {
                    selectedDate = dueDateObj;
                    updateHomeworkList();
                }
                
                showSyncStatus('Homework added successfully');
            }
            
            // Delete homework
            function deleteHomework(date, index) {
                if (confirm('Are you sure you want to delete this homework?')) {
                    homeworkData[date].splice(index, 1);
                    
                    // Remove date key if no more homework
                    if (homeworkData[date].length === 0) {
                        delete homeworkData[date];
                    }
                    
                    // Save data
                    saveData();
                    
                    // Update UI
                    updateCalendar();
                    updateHomeworkList();
                    
                    showSyncStatus('Homework deleted');
                }
            }
            
            // Format date as YYYY-MM-DD
            function formatDate(date) {
                const year = date.getFullYear();
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                return `${year}-${month}-${day}`;
            }
            
            // Add some sample homework data
            const today = new Date();
            const tomorrow = new Date();
            tomorrow.setDate(today.getDate() + 1);
            const nextWeek = new Date();
            nextWeek.setDate(today.getDate() + 7);
            
            // Only add sample data if no data exists
            if (Object.keys(homeworkData).length === 0) {
                homeworkData[formatDate(today)] = [
                    {
                        title: "Math Exercises",
                        dueDate: formatDate(today),
                        description: "Chapter 5, problems 1-20"
                    },
                    {
                        title: "Science Report",
                        dueDate: formatDate(today),
                        description: "Write a report on photosynthesis"
                    }
                ];
                
                homeworkData[formatDate(tomorrow)] = [
                    {
                        title: "History Essay",
                        dueDate: formatDate(tomorrow),
                        description: "500-word essay on World War II"
                    }
                ];
                
                homeworkData[formatDate(nextWeek)] = [
                    {
                        title: "Literature Reading",
                        dueDate: formatDate(nextWeek),
                        description: "Read chapters 3-5 of To Kill a Mockingbird"
                    }
                ];
                
                saveData();
            }
            
            // Initialize the calendar
            initCalendar();
        });
    </script>
</body>
</html>
