
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Heart Rate Analysis</title>
</head>
<body>
    <h1>Heart Rate Analysis</h1>
    
    <form onsubmit="submitHeartRate(); return false;">
        <div>
            <label for="active">Active Pulse Rate:</label>
            <input type="number" id="active" name="active" required><br><br>
            <label for="rest">Resting Pulse Rate:</label>
            <input type="number" id="rest" name="rest" required><br><br>
            <label for="smoke">Do you smoke? (0 for No, 1 for Yes):</label>
            <input type="number" id="smoke" name="smoke" required><br><br>
            <label for="sex">Gender (0 for Female, 1 for Male):</label>
            <input type="number" id="sex" name="sex" required><br><br>
            <label for="exercise">Level of Exercise (1, 2, or 3):</label>
            <input type="number" id="exercise" name="exercise" required min="1" max="3"><br><br>
            <label for="hgt">Height (in inches):</label>
            <input type="number" id="hgt" name="hgt" required><br><br>
            <label for="wgt">Weight (in pounds):</label>
            <input type="number" id="wgt" name="wgt" required><br><br>
            <input type="submit" value="Submit">
        </div>
    </form>

    <div id="heartRateStats"></div>
    <canvas id="heartRateGraph" width="800" height="600"></canvas>

    <script>
        let myChart = null;

        function submitHeartRate() {
            const activePulse = document.getElementById("active").value;
            const restingPulse = document.getElementById("rest").value;
            const smoking = document.getElementById("smoke").value;
            const gender = document.getElementById("sex").value;
            const exercise = document.getElementById("exercise").value;
            const height = document.getElementById("hgt").value;
            const weight = document.getElementById("wgt").value;
            
            const data = {
                "Active": activePulse,
                "Rest": restingPulse,
                "Smoke": smoking,
                "Sex": gender,
                "Exercise": exercise,
                "Hgt": height,
                "Wgt": weight
            };

            fetch(`http://127.0.0.1:5000/api/heartrate/create`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify([data])
            })
            .then(response => {
                if (!response.ok) {
                    throw new Error('Heart rate data not found');
                }
                return response.json();
            })
            .then(data => {
                console.log('Response:', data); // Log the response to the console
                if (data && data.averageHeartRate && typeof data.averageHeartRate.gender !== 'undefined' && typeof data.averageHeartRate.exercise !== 'undefined') {
                    // Proceed with processing the data
                    console.log('Gender:', data.averageHeartRate.gender);
                    console.log('Exercise Level:', data.averageHeartRate.exercise);

                    // Display the data or perform further actions here
                    document.getElementById("heartRateStats").innerText = `Average heart rate - Gender: ${data.averageHeartRate.gender}, Exercise Level: ${data.averageHeartRate.exercise}`;

                    if (myChart) {
                        myChart.destroy();
                    }

                    generateGraph(data);
                } else {
                    throw new Error('Invalid data format');
                }
            })
            .catch(error => {
                document.getElementById("heartRateStats").innerText = error.message;
                if (myChart) {
                    myChart.destroy();
                }
            });
        }

        function generateGraph(data) {
            const chartData = {
                labels: ['Gender', 'Exercise Level'],
                datasets: [
                    {
                        label: 'Average Heart Rate',
                        data: [data.averageHeartRate.gender, data.averageHeartRate.exercise],
                        backgroundColor: [
                            'rgba(255, 99, 132, 0.2)',
                            'rgba(54, 162, 235, 0.2)'
                        ],
                        borderColor: [
                            'rgba(255, 99, 132, 1)',
                            'rgba(54, 162, 235, 1)'
                        ],
                        borderWidth: 1
                    }
                ]
            };

            const ctx = document.getElementById('heartRateGraph').getContext('2d');
            myChart = new Chart(ctx, {
                type: 'bar',
                data: chartData,
                options: {
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
        }

        // Select the node that will be observed for mutations
        const targetNode = document.getElementById('heartRateStats');

        // Options for the observer (which mutations to observe)
        const config = { childList: true };

        // Callback function to execute when mutations are observed
        const callback = function(mutationsList, observer) {
            for(const mutation of mutationsList) {
                if (mutation.type === 'childList') {
                    console.log('A child node has been added or removed.');
                    // Call your function to handle the mutation here
                }
            }
        };

        // Create an observer instance linked to the callback function
        const observer = new MutationObserver(callback);

        // Start observing the target node for configured mutations
        observer.observe(targetNode, config);
    </script>
</body>
</html>
