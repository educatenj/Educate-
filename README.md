<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>educate@nj</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: #000;
            color: #fff;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            padding: 20px;
            margin-bottom: 30px;
        }

        h1 {
            font-size: 3rem;
            color: #fff;
            letter-spacing: 2px;
        }

        .calendar-container {
            background: #111;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 30px;
            border: 1px solid #333;
        }

        .calendar-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .calendar-nav button {
            background: #333;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 1rem;
            width: 40px;
            height: 40px;
            transition: 0.3s;
        }

        .calendar-nav button:hover {
            background: #555;
        }

        .current-month {
            font-size: 1.2rem;
            font-weight: bold;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
        }

        .calendar-day-header {
            text-align: center;
            padding: 10px 0;
            font-weight: bold;
            color: #777;
        }

        .calendar-day {
            height: 80px;
            background: #222;
            border-radius: 5px;
            padding: 5px;
            cursor: pointer;
            transition: 0.3s;
            border: 1px solid #333;
            position: relative;
        }

        .calendar-day:hover {
            background: #333;
        }

        .day-number {
            font-size: 0.9rem;
            font-weight: bold;
        }

        .has-note {
            position: absolute;
            bottom: 5px;
            right: 5px;
            width: 6px;
            height: 6px;
            background: #4285f4;
            border-radius: 50%;
        }

        .notes-container {
            background: #111;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 30px;
            border: 1px solid #333;
            display: none; /* Hidden by default */
        }

        .note-form textarea {
            width: 100%;
            height: 100px;
            padding: 10px;
            background: #222;
            color: #fff;
            border: 1px solid #333;
            border-radius: 5px;
            margin-bottom: 10px;
            resize: vertical;
        }

        .note-form button {
            background: #333;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            transition: 0.3s;
        }

        .note-form button:hover {
            background: #555;
        }

        .google-meet-section {
            text-align: center;
        }

        .meet-button {
            display: inline-block;
            background: #4285f4;
            color: white;
            text-decoration: none;
            padding: 15px 30px;
            border-radius: 5px;
            font-weight: bold;
            transition: 0.3s;
        }

        .meet-button:hover {
            background: #3367d6;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>educate@nj</h1>
        </header>
        
        <div class="calendar-container">
            <div class="calendar-controls">
                <div class="calendar-nav">
                    <button id="prev-month"><i class="fas fa-chevron-left"></i></button>
                </div>
                <div class="current-month" id="current-month"></div>
                <div class="calendar-nav">
                    <button id="next-month"><i class="fas fa-chevron-right"></i></button>
                </div>
            </div>
            <div class="calendar-grid" id="calendar-grid"></div>
        </div>
        
        <div class="notes-container" id="notes-container">
            <div class="note-form">
                <textarea id="note-text" placeholder="Add a note..."></textarea>
                <button id="save-note">Save Note</button>
            </div>
        </div>
        
        <div class="google-meet-section">
            <a href="https://meet.google.com/" class="meet-button" target="_blank">
                <i class="fab fa-google"></i> Google Meet
            </a>
        </div>
    </div>

    <script>
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
                if (!selectedDate) return;
                
                const noteText = document.getElementById('note-text').value.trim();
                if (noteText) {
                    const dateKey = formatDateKey(selectedDate);
                    if (!notes[dateKey]) notes[dateKey] = [];
                    notes[dateKey].push(noteText);
                    if (notes[dateKey].length > 20) notes[dateKey].shift(); // Keep max 20 notes
                    localStorage.setItem('calendarNotes', JSON.stringify(notes));
                    renderCalendar(currentDate);
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
                const dayNames = ['S', 'M', 'T', 'W', 'T', 'F', 'S'];
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
                    
                    // Check if day has notes
                    const dayDate = new Date(date.getFullYear(), date.getMonth(), day);
                    const dateKey = formatDateKey(dayDate);
                    
                    if (notes[dateKey] && notes[dateKey].length > 0) {
                        const noteIndicator = document.createElement('div');
                        noteIndicator.classList.add('has-note');
                        dayElement.appendChild(noteIndicator);
                    }
                    
                    // Select day on click
                    dayElement.addEventListener('click', function() {
                        selectedDate = dayDate;
                        const notesContainer = document.getElementById('notes-container');
                        notesContainer.style.display = 'block';
                        document.getElementById('note-text').value = '';
                    });
                    
                    calendarGrid.appendChild(dayElement);
                }
            }
            
            // Helper function
            function formatDateKey(date) {
                return date.toISOString().split('T')[0]; // YYYY-MM-DD
            }
        });
    </script>
</body>
</html>
