<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Family Trip Bakkhali Planner</title>
    <style>
        :root {
            --primary: #1e3a8a;
            --secondary: #0284c7;
            --bg: #f8fafc;
            --card-bg: #ffffff;
            --text: #1e293b;
            --border: #e2e8f0;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            background: var(--card-bg);
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid var(--border);
            padding-bottom: 20px;
            margin-bottom: 25px;
        }

        h1 {
            color: var(--primary);
            margin: 0;
            font-size: 28px;
        }

        .total-budget {
            background: #f0fdf4;
            color: #166534;
            padding: 10px 20px;
            border-radius: 8px;
            font-weight: bold;
            font-size: 20px;
            border: 1px solid #bbf7d0;
        }

        .actions-bar {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 6px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn-primary { background-color: var(--secondary); color: white; }
        .btn-primary:hover { background-color: #0369a1; }
        .btn-success { background-color: #16a34a; color: white; }
        .btn-success:hover { background-color: #15803d; }
        .btn-danger { background-color: #ef4444; color: white; padding: 4px 8px; font-size: 12px;}
        .btn-danger:hover { background-color: #dc2626; }

        .day-section {
            margin-bottom: 35px;
            border: 1px solid var(--border);
            border-radius: 8px;
            overflow: hidden;
        }

        .day-header {
            background-color: #f1f5f9;
            padding: 15px;
            font-weight: bold;
            font-size: 18px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        th, td {
            padding: 12px 15px;
            border-bottom: 1px solid var(--border);
        }

        th {
            background-color: #f8fafc;
            color: #64748b;
            font-size: 13px;
            text-transform: uppercase;
        }

        input[type="text"], input[type="number"] {
            width: 100%;
            padding: 6px 10px;
            border: 1px solid var(--border);
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 14px;
        }

        input[type="text"]:focus, input[type="number"]:focus {
            outline: 2px solid var(--secondary);
        }

        .col-time { width: 12%; }
        .col-activity { width: 55%; }
        .col-cost { width: 20%; }
        .col-action { width: 8%; text-align: center; }

        @media print {
            body { background: white; padding: 0; }
            .container { box-shadow: none; padding: 0; max-width: 100%; }
            .actions-bar, .col-action, .btn-danger, th:last-child, td:last-child { display: none !important; }
            input { border: none !important; padding: 0 !important; background: transparent !important; pointer-events: none; }
            .day-section { page-break-inside: avoid; border: 1px solid #000; margin-bottom: 20px; }
            th { border-bottom: 2px solid #000; color: #000; }
            td { border-bottom: 1px solid #ccc; }
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>Family Trip Bakkhali</h1>
        <div class="total-budget">Total Cost: ₹ <span id="grand-total">0</span></div>
    </div>

    <div class="actions-bar">
        <button class="btn btn-success" onclick="saveAndPrint()">💾 Save & Print</button>
    </div>

    <div id="itinerary-container"></div>
</div>

<script>
    // Initial data structured directly from your uploaded Excel sheet
    const defaultData = [
        {
            day: "15th August",
            title: "Towards Bakkhali",
            items: [
                { time: "04:00:00", activity: "Home to Sealdah (2 Cabs)", cost: 1000 },
                { time: "06:00:00", activity: "Sealdah Station to Namkhana Station (Train)", cost: 200 },
                { time: "07:30:00", activity: "Snacks in Train", cost: 240 },
                { time: "09:00:00", activity: "Namkhana Station to Bakkhali (Auto)", cost: 320 },
                { time: "10:00:00", activity: "Hotel check in (3 rooms)", cost: 2700 },
                { time: "10:30:00", activity: "Breakfast", cost: 400 },
                { time: "11:30:00", activity: "Sea Beach bath", cost: 0 },
                { time: "14:00:00", activity: "Lunch", cost: 1200 },
                { time: "----", activity: "Rest", cost: 0 },
                { time: "17:00:00", activity: "Sea Beach fish market", cost: 800 },
                { time: "19:00:00", activity: "Chai & tiffin", cost: 160 },
                { time: "21:00:00", activity: "Dinner", cost: 960 },
                { time: "----", activity: "Rest", cost: 0 }
            ]
        },
        {
            day: "16th August",
            title: "Towards Home",
            items: [
                { time: "10:00:00", activity: "Breakfast", cost: 400 },
                { time: "11:00:00", activity: "Bath & get ready for check out", cost: 0 },
                { time: "11:30:00", activity: "Hotel check out", cost: 0 },
                { time: "12:00:00", activity: "Sea Beach shopping market (Individual)", cost: 0 },
                { time: "13:00:00", activity: "Lunch", cost: 1200 },
                { time: "14:00:00", activity: "Bakkhali to Namkhana Station", cost: 320 },
                { time: "16:00:00", activity: "Namkhana Station To Sealdah Station (Train)", cost: 200 },
                { time: "20:00:00", activity: "Sealdah Station to Home (2 Cabs)", cost: 1000 }
            ]
        }
    ];

    // Load from LocalStorage if available, otherwise use initial data
    let tripData = JSON.parse(localStorage.getItem('bakkhaliTripData')) || defaultData;

    function renderTable() {
        const container = document.getElementById('itinerary-container');
        container.innerHTML = '';
        let grandTotal = 0;

        tripData.forEach((dayData, dayIndex) => {
            let daySubtotal = 0;
            
            const daySection = document.createElement('div');
            daySection.className = 'day-section';

            let tableRows = '';
            dayData.items.forEach((item, itemIndex) => {
                daySubtotal += Number(item.cost) || 0;
                tableRows += `
                    <tr>
                        <td class="col-time"><input type="text" value="${item.time}" onchange="updateItem(${dayIndex}, ${itemIndex}, 'time', this.value)"></td>
                        <td class="col-activity"><input type="text" value="${item.activity}" onchange="updateItem(${dayIndex}, ${itemIndex}, 'activity', this.value)"></td>
                        <td class="col-cost"><input type="number" value="${item.cost}" onchange="updateItem(${dayIndex}, ${itemIndex}, 'cost', this.value)"></td>
                        <td class="col-action"><button class="btn btn-danger" onclick="deleteItem(${dayIndex}, ${itemIndex})">✕</button></td>
                    </tr>
                `;
            });

            grandTotal += daySubtotal;

            daySection.innerHTML = `
                <div class="day-header">
                    <span>${dayData.day} - ${dayData.title}</span>
                    <span style="font-size: 15px; color: #475569;">Est: ₹ ${daySubtotal}</span>
                </div>
                <table>
                    <thead>
                        <tr>
                            <th class="col-time">Time</th>
                            <th class="col-activity">Activity / Details</th>
                            <th class="col-cost">Cost (₹)</th>
                            <th class="col-action"></th>
                        </tr>
                    </thead>
                    <tbody>
                        ${tableRows}
                        <tr>
                            <td colspan="4" style="padding: 10px 15px;">
                                <button class="btn btn-primary" style="padding: 5px 12px; font-size: 13px;" onclick="addItem(${dayIndex})">+ Add Item</button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            `;
            container.appendChild(daySection);
        });

        document.getElementById('grand-total').innerText = grandTotal;
    }

    function updateItem(dayIndex, itemIndex, field, value) {
        if (field === 'cost') {
            tripData[dayIndex].items[itemIndex][field] = Number(value) || 0;
        } else {
            tripData[dayIndex].items[itemIndex][field] = value;
        }
        saveDataSilently();
        // Recalculate totals without full re-render to keep focus if editing costs
        let grand = 0;
        tripData.forEach(d => d.items.forEach(i => grand += (Number(i.cost) || 0)));
        document.getElementById('grand-total').innerText = grand;
    }

    function addItem(dayIndex) {
        tripData[dayIndex].items.push({ time: "12:00:00", activity: "New Activity", cost: 0 });
        saveDataSilently();
        renderTable();
    }

    function deleteItem(dayIndex, itemIndex) {
        tripData[dayIndex].items.splice(itemIndex, 1);
        saveDataSilently();
        renderTable();
    }

    function saveDataSilently() {
        localStorage.setItem('bakkhaliTripData', JSON.stringify(tripData));
    }

    function saveAndPrint() {
        // Explicit save confirmation visual feedback
        localStorage.setItem('bakkhaliTripData', JSON.stringify(tripData));
        
        // Let UI settle momentarily before opening print view
        setTimeout(() => {
            window.print();
        }, 250);
    }

    // Initial load
    renderTable();
</script>

</body>
</html>
