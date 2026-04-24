<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>AI Safety Observatory</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; line-height: 1.6; max-width: 900px; margin: auto; padding: 40px; background: #f4f7f6; }
        h1 { color: #2c3e50; border-bottom: 2px solid #3498db; padding-bottom: 10px; }
        .form-section { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); margin-bottom: 30px; }
        input, textarea, select { width: 100%; padding: 10px; margin: 10px 0; border: 1px solid #ddd; border-radius: 4px; box-sizing: border-box; }
        button { background: #3498db; color: white; border: none; padding: 10px 20px; border-radius: 4px; cursor: pointer; }
        button:hover { background: #2980b9; }
        .incident-card { background: white; padding: 15px; margin-bottom: 15px; border-left: 5px solid #e74c3c; border-radius: 4px; }
        .severity-High { border-left-color: #e67e22; }
        .severity-Critical { border-left-color: #c0392b; }
    </style>
</head>
<body>

    <h1>AI Safety Observatory Portal</h1>

    <div class="form-section">
        <h3>Report New AI Incident</h3>
        <input type="text" id="title" placeholder="Incident Title (e.g., Algorithmic Bias in Hiring)">
        <input type="text" id="system" placeholder="AI System Involved">
        <select id="severity">
            <option value="Low">Low Severity</option>
            <option value="Medium" selected>Medium Severity</option>
            <option value="High">High Severity</option>
            <option value="Critical">Critical Severity</option>
        </select>
        <textarea id="desc" placeholder="Describe the incident and its impact..." rows="4"></textarea>
        <button onclick="submitIncident()">Submit to Observatory</button>
    </div>

    <h2>Documented Incidents</h2>
    <div id="list"></div>

    <script>
        async function loadIncidents() {
            const res = await fetch('/api/incidents');
            const data = await res.json();
            document.getElementById('list').innerHTML = data.map(i => `
                <div class="incident-card severity-${i.severity}">
                    <strong>${i.title}</strong> [${i.severity}]<br>
                    <small>System: ${i.systemInvolved}</small>
                    <p>${i.description}</p>
                </div>
            `).join('');
        }

        async function submitIncident() {
            const body = {
                title: document.getElementById('title').value,
                systemInvolved: document.getElementById('system').value,
                severity: document.getElementById('severity').value,
                description: document.getElementById('desc').value
            };
            await fetch('/api/incidents', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(body)
            });
            location.reload();
        }

        loadIncidents();
    </script>
</body>
</html>
