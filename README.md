<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>educate@nj - Educational Calendar</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #1a2a6c);
            color: #333;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            padding: 30px 20px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            margin-bottom: 30px;
            animation: fadeIn 1s ease-out;
        }

        h1 {
            font-size: 3.5rem;
            color: #1a2a6c;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
        }

        .tagline {
            font-size: 1.2rem;
            color: #b21f1f;
            font-weight: 600;
        }

        .main-content {
            display: flex;
            gap: 30px;
            margin-bottom: 40px;
            flex-wrap: wrap;
        }

        .calendar-container {
            flex: 2;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            min-width: 300px;
            animation: slideInLeft 1s ease-out;
        }

        .notes-container {
            flex: 1;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            min-width: 300px;
            animation: slideInRight 1s ease-out;
        }

        .section-title {
            font-size: 1.8rem;
            color: #1a2a6c;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 3px solid #b21f1f;
        }

        .calendar-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .calendar-nav button {
            background: #1a2a6c;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 1.2rem;
            transition: all 0.3s;
            width: 40px;
            height: 40px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }

        .calendar-nav button:hover {
            background: #b21f1f;
            transform: scale(1.1);
        }

        .current-month {
            font-size: 1.5rem;
            font-weight: bold;
            color: #1a2a6c;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
        }

        .calendar-day-header {
            text-align: center;
            padding: 12px 0;
            font-weight: bold;
            color: #b21f1f;
            background: rgba(178, 31, 31, 0.1);
            border-radius: 8px;
        }

        .calendar-day {
            height: 90px;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 8px;
            padding: 8px;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
            border: 2px solid rgba(26, 42, 108, 0.1);
        }

        .calendar-day:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            border-color: #b21f1f;
        }

        .day-number {
            font-size: 1.2rem;
            font-weight: bold;
            color: #1a2a6c;
        }

        .has-note {
            position: absolute;
            bottom: 5px;
            right: 5px;
            width: 8px;
            height: 8px;
            background: #b21f1f;
            border-radius: 50%;
        }

        .notes-list {
            max-height: 300px;
            overflow-y: auto;
            padding-right: 10px;
        }

        .note-item {
            background: white;
            border-left: 4px solid #1a2a6c;
            padding: 12px;
            margin-bottom: 15px;
            border-radius: 0 8px 8px 0;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
        }

        .note-date {
            font-weight: bold;
            color: #b21f1f;
            margin-bottom: 5px;
        }

        .note-text {
            color: #333;
            line-height: 1.4;
        }

        .note-form textarea {
            width: 100%;
            height: 100px;
            padding: 12px;
            border: 2px solid #1a2a6c;
            border-radius: 8px;
            margin-bottom: 15px;
            resize: vertical;
            font-size: 1rem;
        }

        .note-form button {
            background: #1a2a6c;
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: bold;
            transition: all 0.3s;
            width: 100%;
        }

        .note-form button:hover {
            background: #b21f1f;
            transform: translateY(-3px);
        }

        .google-meet-section {
            text-align: center;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            animation: fadeIn 1.5s ease-out;
        }

        .meet-button {
            display: inline-block;
            background: linear-gradient(to right, #1a73e8, #4285f4);
            color: white;
            text-decoration: none;
            font-size: 1.5rem;
            font-weight: bold;
            padding: 20px 50px;
            border-radius: 50px;
            margin: 20px 0;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            position: relative;
            overflow: hidden;
        }

        .meet-button:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
            background: linear-gradient(to right, #4285f4, #1a73e8);
        }

        .meet-button i {
            margin-right: 15px;
            font-size: 1.8rem;
        }

        .meet-description {
            font-size: 1.1rem;
            max-width: 700px;
            margin: 0 auto 25px;
            line-height: 1.6;
        }

        footer {
            text-align: center;
            color: white;
            padding: 20px;
            margin-top: 30px;
            font-size: 1rem;
        }

        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes slideInLeft {
            from { transform: translateX(-50px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        @keyframes slideInRight {
            from { transform: translateX(50px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .main-content {
                flex-direction: column;
            }
            
            h1 {
                font-size: 2.5rem;
            }
            
            .calendar-day {
                height: 70px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>educate@nj</h1>
            <p class="tagline">Your Comprehensive Educational Calendar and Note-Taking Platform</p>
        </header>
        
        <div class="main-content">
            <div class="calendar-container">
                <h2 class="section-title">Educational Calendar</h2>
                <div class="calendar-controls">
                    <div class="calendar-nav">
                        <button id="prev-month"><i class="fas fa-chevron-left"></i></button>
                    </div>
                    <div class="current-month" id="current-month">September 2023</div>
                    <div class="calendar-nav">
                        <button id="next-month"><i class="fas fa-chevron-right"></i></button>
                    </div>
                </div>
                <div class="calendar-grid" id="calendar-grid">
                    <!-- Calendar will be generated by JavaScript -->
                </div>
            </div>
            
            <div class="notes-container">
                <h2 class="section-title">My Notes</h2>
                <div class="notes-list" id="notes-list">
                    <!-- Notes will be displayed here -->
                </div>
                <div class="note-form">
                    <textarea id="note-text" placeholder="Add a note for the selected date..."></textarea>
                    <button id="save-note"><i class="fas fa-save"></i> Save Note</button>
                </div>
            </div>
        </div>
        
        <div class="google-meet-section">
            <h2 class="section-title">Join Our Virtual Classroom</h2>
            <p class="meet-description">
                Click the button below to join our live educational sessions via Google Meet. 
                Our virtual classrooms provide interactive learning experiences with qualified instructors 
                and collaborative tools for an engaging educational environment.
            </p>
            <a href="https://meet.google.com/" class="meet-button" target="_blank">
                <i class="fab fa-google"></i> Join Google Meet
            </a>
            <p>Available Monday-Friday from 9:00 AM to 5:00 PM</p>
        </div>
        
        <footer>
            <p>&copy; 2023 educate@nj - All Rights Reserved | Empowering Education Through Technology</p>
        </footer>
    </div>

    <script>
        // Calendar functionality
        document.addEventListener('DOMContentLoaded', function() {
            let currentDate = new Date();
            let selectedDate = null;
            let notes = JSON.parse(localStorage.getItem('calendarNotes')) || {};
            
            // Initialize calendar
            renderCalendar(currentDate);
            
            // Navigation buttons
            document.getElementById('prev-month').addEventListener('click', function() {
                currentDate.setMonth(currentDate.getMonth() - 1);
                renderCalendar(currentDate);
            });
            
            document.getElementById('next-month').addEventListener('click', function() {
                currentDate.setMonth(currentDate.getMonth() + 1);
                renderCalendar(currentDate);
            });
            
            // Save note button
            document.getElementById('save-note').addEventListener('click', function() {
                if (!selectedDate) {
                    alert('Please select a date first!');
                    return;
                }
                
                const noteText = document.getElementById('note-text').value.trim();
                if (noteText) {
                    const dateKey = formatDateKey(selectedDate);
                    notes[dateKey] = noteText;
                    localStorage.setItem('calendarNotes', JSON.stringify(notes));
                    renderCalendar(currentDate);
                    renderNotes();
                    document.getElementById('note-text').value = '';
                }
            });
            
            // Render calendar
            function renderCalendar(date) {
                const calendarGrid = document.getElementById('calendar-grid');
                const monthYearDisplay = document.getElementById('current-month');
                
                // Clear existing calendar
                calendarGrid.innerHTML = '';
                
                // Set month/year display
                const monthNames = ['January', 'February', 'March', 'April', 'May', 'June', 
                                    'July', 'August', 'September', 'October', 'November', 'December'];
                monthYearDisplay.textContent = `${monthNames[date.getMonth()]} ${date.getFullYear()}`;
                
                // Add day headers
                const dayNames = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
                dayNames.forEach(day => {
                    const dayHeader = document.createElement('div');
                    dayHeader.classList.add('calendar-day-header');
                    dayHeader.textContent = day;
                    calendarGrid.appendChild(dayHeader);
                });
                
                // Get first day of month and days in month
                const firstDay = new Date(date.getFullYear(), date.getMonth(), 1).getDay();
                const daysInMonth = new Date(date.getFullYear(), date.getMonth() + 1, 0).getDate();
                
                // Add empty cells for days before the first day
                for (let i = 0; i < firstDay; i++) {
                    const emptyDay = document.createElement('div');
                    emptyDay.classList.add('calendar-day', 'empty');
                    calendarGrid.appendChild(emptyDay);
                }
                
                // Add days of the month
                for (let day = 1; day <= daysInMonth; day++) {
                    const dayElement = document.createElement('div');
                    dayElement.classList.add('calendar-day');
                    
                    const dayNumber = document.createElement('div');
                    dayNumber.classList.add('day-number');
                    dayNumber.textContent = day;
                    dayElement.appendChild(dayNumber);
                    
                    // Check if day has a note
                    const dayDate = new Date(date.getFullYear(), date.getMonth(), day);
                    const dateKey = formatDateKey(dayDate);
                    
                    if (notes[dateKey]) {
                        const noteIndicator = document.createElement('div');
                        noteIndicator.classList.add('has-note');
                        dayElement.appendChild(noteIndicator);
                    }
                    
                    // Highlight current day
                    const today = new Date();
                    if (dayDate.toDateString() === today.toDateString()) {
                        dayElement.style.backgroundColor = 'rgba(178, 31, 31, 0.1)';
                        dayElement.style.border = '2px solid #b21f1f';
                    }
                    
                    // Select day on click
                    dayElement.addEventListener('click', function() {
                        selectedDate = dayDate;
                        document.getElementById('note-text').value = notes[dateKey] || '';
                        
                        // Remove any existing selection
                        document.querySelectorAll('.calendar-day').forEach(el => {
                            el.style.boxShadow = 'none';
                        });
                        
                        // Highlight selected day
                        this.style.boxShadow = '0 0 0 3px #1a2a6c';
                    });
                    
                    calendarGrid.appendChild(dayElement);
                }
                
                // Render existing notes
                renderNotes();
            }
            
            // Render notes list
            function renderNotes() {
                const notesList = document.getElementById('notes-list');
                notesList.innerHTML = '';
                
                // Get sorted dates
                const sortedDates = Object.keys(notes).sort().reverse();
                
                if (sortedDates.length === 0) {
                    notesList.innerHTML = '<p>No notes yet. Select a date and add your first note!</p>';
                    return;
                }
                
                // Display up to 5 most recent notes
                const displayNotes = sortedDates.slice(0, 5);
                
                displayNotes.forEach(dateKey => {
                    const noteItem = document.createElement('div');
                    noteItem.classList.add('note-item');
                    
                    const noteDate = document.createElement('div');
                    noteDate.classList.add('note-date');
                    noteDate.textContent = formatDisplayDate(dateKey);
                    
                    const noteText = document.createElement('div');
                    noteText.classList.add('note-text');
                    noteText.textContent = notes[dateKey];
                    
                    noteItem.appendChild(noteDate);
                    noteItem.appendChild(noteText);
                    notesList.appendChild(noteItem);
                });
                
                if (sortedDates.length > 5) {
                    const moreItem = document.createElement('div');
                    moreItem.classList.add('note-item');
                    moreItem.textContent = `+${sortedDates.length - 5} more notes...`;
                    moreItem.style.textAlign = 'center';
                    moreItem.style.fontStyle = 'italic';
                    notesList.appendChild(moreItem);
                }
            }
            
            // Helper functions
            function formatDateKey(date) {
                return date.toISOString().split('T')[0]; // YYYY-MM-DD
            }
            
            function formatDisplayDate(dateString) {
                const date = new Date(dateString);
                return date.toLocaleDateString('en-US', { 
                    weekday: 'long', 
                    year: 'numeric', 
                    month: 'long', 
                    day: 'numeric' 
                });
            }
        });
    </script>
</body>
</html>
