---
layout: default
title: Mood Tracker
permalink: /moodTracker
--- 

<style>
    body {
    background-color: #fae5de;
    }
    th, td {
        border: 1px solid #dddddd;
        text-align: center;
        padding: 8px;
    }

    td {
        cursor: pointer;
    }

    #mood-buttons {
        display: flex;
        margin-top: 20px;
    }

    .mood {
        background-color: transparent;
        border: 2px solid #ccc;
        border-radius: 50%;
        font-size: 24px;
        width: 60px;
        height: 60px;
        margin: 0 10px;
        cursor: pointer;
    }

    .happy { color: #ffd700; }
    .neutral { color: #a9a9a9; }
    .sad { color: #87CEEB; }
    .mad { color: #ff6347; }
    
    body {
        background-color: #FAE5DE;
        font-family: 'Arial', sans-serif;
        margin: 20px;
    }
</style>

<body>
<!-- emoji mood buttons -->
<div>
    <h2>How did you feel today?</h2>
    <div id="mood" name="mood">
        <button id="happy" class="mood happy" onclick="todayMood('😊')">😊</button>
        <button id="neutral" class="mood neutral" onclick="todayMood('😐')">😐</button>
        <button id="sad" class="mood sad" onclick="todayMood('😢')">😢</button>
        <button id="mad" class="mood mad" onclick="todayMood('😡')">😡</button>
    </div>
</div>
<p> </p>
<!-- calendar table -->
<table id="calendar">
    <thead>
        <tr>
            <th>Sun</th>
            <th>Mon</th>
            <th>Tue</th>
            <th>Wed</th>
            <th>Thu</th>
            <th>Fri</th>
            <th>Sat</th>
        </tr>
    </thead>
    <tbody id="calendarBody">
    </tbody>
</table>
</body>

<script>
    // Function to create the calendar grid
    function createCalendar(year, month) {
        const calendarBody = document.getElementById("calendarBody");
        calendarBody.innerHTML = '';

        const firstDay = new Date(year, month, 1);
        const lastDay = new Date(year, month + 1, 0);

        let currentDate = new Date(firstDay);
        while (currentDate <= lastDay) {
            const row = document.createElement("tr");
            for (let i = 0; i < 7; i++) {
                const cell = document.createElement("td");
                if (i === currentDate.getDay()) {
                    cell.textContent = currentDate.getDate();
                    row.appendChild(cell);
                    currentDate.setDate(currentDate.getDate() + 1);
                } else {
                    row.appendChild(document.createElement("td"));
                }
            }
            calendarBody.appendChild(row);
        }
    }

    // Function to update the calendar with the selected mood
    function todayMood(selectedEmoji) {
        const currentDate = new Date();
        const dayOfMonth = currentDate.getDate();

        const cells = document.querySelectorAll("#calendarBody td");
        
        cells.forEach((cell) => {
            if (cell.textContent == dayOfMonth.toString()) {
                cell.innerHTML = selectedEmoji;
            }
        });

        return selectedEmoji;
    }


    // Initialize the calendar with the current month
    const currentDate = new Date();
    createCalendar(currentDate.getFullYear(), currentDate.getMonth());
</script>
