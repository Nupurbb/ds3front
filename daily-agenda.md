---
layout: default
title: Daily Agenda
permalink: /dailyAgenda
--- 

<body>
    <h1>Daily Agenda</h1>
    <div class="calendar" id="calendar"></div>
    <script>
        const daysOfWeek = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
        function createDaySection(day) {
            const calendar = document.getElementById('calendar');
            const dayDiv = document.createElement('div');
            dayDiv.classList.add('day');
            const heading = document.createElement('h2');
            heading.textContent = day;
            dayDiv.appendChild(heading);
            ['Morning', 'Noon', 'Afternoon'].forEach((timePeriod) => {
                const timePeriodDiv = document.createElement('div');
                timePeriodDiv.classList.add('time-period');
                const label = document.createElement('label');
                label.textContent = timePeriod;
                const taskInput = document.createElement('input');
                taskInput.type = 'text';
                taskInput.placeholder = 'Enter task...';
                const addButton = document.createElement('button');
                addButton.textContent = 'Add Task';
                addButton.onclick = function () {
                    addTask(day, timePeriod, taskInput);
                };
                const taskList = document.createElement('ul');
                taskList.id = `taskList${day}${timePeriod}`;
                timePeriodDiv.appendChild(label);
                timePeriodDiv.appendChild(taskInput);
                timePeriodDiv.appendChild(addButton);
                timePeriodDiv.appendChild(taskList);
                dayDiv.appendChild(timePeriodDiv);
            });
            calendar.appendChild(dayDiv);
        }
        function addTask(day, timePeriod, inputElement) {
            const taskInput = inputElement;
            const taskList = document.getElementById(`taskList${day}${timePeriod}`);
            if (taskInput.value.trim() === '') {
                alert('Please enter a task.');
                return;
            }
            const li = document.createElement('li');
            const checkbox = document.createElement('input');
            checkbox.type = 'checkbox';
            li.appendChild(checkbox);
            li.appendChild(document.createTextNode(taskInput.value));
            taskList.appendChild(li);
            taskInput.value = '';
        }
        daysOfWeek.forEach(createDaySection);
        // Add event listener to detect Enter key press
        document.querySelectorAll('.time-period input[type="text"]').forEach(input => {
            input.addEventListener('keypress', function (e) {
                if (e.key === 'Enter') {
                    const day = this.closest('.day').querySelector('h2').textContent;
                    const timePeriod = this.closest('.time-period').querySelector('label').textContent;
                    addTask(day, timePeriod, this);
                }
            });
        });
    </script>
</body>


<style>
    body {
        background-color: #FAE5DE;
        font-family: 'Arial', sans-serif;
        margin: 20px;
    }
    .calendar {
        display: flex;
        flex-direction: column;
    }
    .day {
        border: 1px solid #ccc;
        padding: 10px;
        text-align: left;
        margin-bottom: 10px;
        background-color: #f9f9f9;
        border-radius: 5px;
    }
    h2 {
        margin-bottom: 10px;
    }
    .time-period {
        margin-bottom: 15px;
    }
    .time-period label {
        font-weight: bold;
    }
    input[type="text"] {
        width: 70%;
        margin: 5px 0;
        border: 1px solid #ccc;
        box-sizing: border-box;
        padding: 8px;
    }
    button {
        width: 100%;
        padding: 8px;
        background-color: #f5aeb6;
        color: white;
        border: none;
        cursor: pointer;
        margin-top: 5px;
    }
    button:hover {
        background-color: #f27e8c;
    }
    ul {
        list-style-type: none;
        padding: 0;
        margin-top: 10px;
    }
    li {
        margin-bottom: 8px;
        display: flex;
        align-items: center;
    }
    li input[type="checkbox"] {
        margin-right: 8px;
    }
    li span {
        cursor: pointer;
    }
</style>