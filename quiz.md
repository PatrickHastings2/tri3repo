---
toc: true
comments: true
layout: post
title: team seed
description:  
courses: { compsci: {week: 7} }
type: tangibles
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>College Finder Quiz</title>
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
            color: #333;
        }
        h1 {
            color: #0056b3;
        }
        form {
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        label {
            margin-top: 10px;
            display: block;
            color: #333;
        }
        input, select {
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            padding: 8px;
            width: 100%;
        }
        button {
            background-color: #4caf50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        #result {
            margin-top: 20px;
            padding: 20px;
            background-color: #e2e2e2;
            border-radius: 4px;
            color: #333;
        }
    </style>
</head>
<body>
    <h1>College Finder Quiz</h1>
    <form onsubmit="event.preventDefault(); findCollege();">
        <label for="location">Location Preference:</label>
        <select id="location" required>
            <option value="Urban">Urban</option>
            <option value="Suburban">Suburban</option>
            <option value="Rural">Rural</option>
        </select>

        <label for="weather">Weather Preference:</label>
        <select id="weather" required>
            <option value="Sunny">Sunny</option>
            <option value="Snowy">Snowy</option>
            <option value="Mild">Mild</option>
        </select>

        <label for="publicPrivate">Public or Private:</label>
        <select id="publicPrivate" required>
            <option value="Public">Public</option>
            <option value="Private">Private</option>
        </select>

        <label for="populationSize">Population Size:</label>
        <select id="populationSize" required>
            <option value="Small">Small</option>
            <option value="Medium">Medium</option>
            <option value="Large">Large</option>
        </select>

        <label for="tuitionPreference">Tuition Preference:</label>
        <select id="tuitionPreference" required>
            <option value="Low">Low</option>
            <option value="Medium">Medium</option>
            <option value="High">High</option>
        </select>

        <label for="orientation">Academic Orientation:</label>
        <select id="orientation" required>
            <option value="STEM">STEM</option>
            <option value="Liberal Arts">Liberal Arts</option>
            <option value="General">General</option>
        </select>

        <button type="submit">Find Colleges</button>
    </form>
    <div id="result"></div>

    <script>
        function findCollege() {
            const preferences = {
                location: document.getElementById('location').value,
                weather: document.getElementById('weather').value,
                publicPrivate: document.getElementById('publicPrivate').value,
                populationSize: document.getElementById('populationSize').value,
                tuitionPreference: document.getElementById('tuitionPreference').value,
                orientation: document.getElementById('orientation').value
            };

            fetch('http://127.0.0.1:8059/api/college/find', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(preferences)
            })
            .then(response => response.json())
            .then(data => {
                if (data.error) {
                    document.getElementById('result').textContent = 'Error: ' + data.error;
                } else {
                    let collegesHtml = '<h4>Recommended Colleges:</h4><ul>';
                    data.forEach(college => {
                        collegesHtml += `<li>${college.name}</li>`;
                    });
                    collegesHtml += '</ul>';
                    document.getElementById('result').innerHTML = collegesHtml;
                }
            })
            .catch(error => {
                console.error('Error:', error);
                document.getElementById('result').textContent = 'Failed to fetch colleges.';
            });
        }
    </script>
</body>
</html>
