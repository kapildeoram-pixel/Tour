<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Family Trip Bakkhali - Planner & Budget</title>
    <!-- html2pdf.js for converting HTML to PDF -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    
    <style>
        :root {
            /* Light, cheerful travel palette */
            --primary-color: #2c3e50;
            --accent-blue: #3498db;
            --accent-orange: #ff7f50;
            --bg-color: #f0f7f4;
            --card-bg: #ffffff;
            --text-color: #34495e;
            
            /* Day themes */
            --day1-theme: #e8f4fd;
            --day1-border: #3498db;
            --day2-theme: #fef5e7;
            --day2-border: #f39c12;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            background-image: linear-gradient(135deg, #f0f7f4 0%, #e8f4fd 100%);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            min-height: 100vh;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            background: var(--card-bg);
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 8px 30px rgba(0,0,0,0.06);
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px dashed #dcdde1;
            padding-bottom: 20px;
            margin-bottom: 25px;
        }

        h1 {
            margin: 0;
            color: var(--primary-color);
            font-size: 32px;
            letter-spacing: -0.5px;
        }
        
        h1 span {
            color: var(--accent-orange);
        }

        .total-budget {
            font-size: 24px;
            font-weight: bold;
            color: #27ae60;
            background: #e8f8f5;
            padding: 10px 20px;
            border-radius: 30px;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.02);
            border: 2px solid #a3e4d7;
        }

        .btn-group {
            margin-bottom: 25px;
            display: flex;
            gap: 10px;
        }

        .btn {
            padding: 14px 28px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            font-size: 15px;
            transition: all 0.2s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
        }

        .btn-save-print { 
            background: linear-gradient(135deg, #2ecc71 0%, #27ae60 100%); 
            color: white; 
        }

        .btn-send {
            background: linear-gradient(135deg, #3498db 0%, #2980b9 100%);
            color: white;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
        }
        
        .btn:active {
            transform: translateY(0);
        }

        .day-section {
            margin-bottom: 40px;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.02);
            border: 1px solid #e1e8ed;
        }

        .day-section.day1 { border-top: 4px solid var(--day1-border); }
        .day-section.day2 { border-top: 4px solid var(--day2-border); }

        .day-title {
            font-size: 20px;
            font-weight: 600;
            color: var(--primary-color);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .day1 .day-title { background-color: var(--day1-theme); }
        .day2 .day-title { background-color: var(--day2-theme); }

        .subtotal-badge {
            font-size: 15px;
            font-weight: 600;
            background: rgba(255,255,255,0.7);
            padding: 4px 12px;
            border-radius: 20px;
            color: #555;
            border: 1px solid rgba(0,0,0,0.05);
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: #ffffff;
        }

        th, td {
            padding: 14px 20px;
            text-align: left;
            border-bottom: 1px solid #f1f2f6;
        }

        th {
            background-color: #fafbfc;
            color: #7f8c8d;
            font-weight: 600;
            font-size: 13px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        td[contenteditable="true"] {
            outline: none;
            transition: all 0.2s;
        }

        td[contenteditable="true"]:focus {
            background-color: #fff9db !important;
            box-shadow: inset 0 0 0 2px #f1c40f;
        }
        
        tbody tr:nth-child(even) {
            background-color: #fcfdfe;
        }

        .cost-col {
            width: 150px;
            text-align: right;
            font-weight: 500;
        }
        th.cost-col { text-align: right; }
        
        td.cost-input {
            color: #2c3e50;
        }

        @media print {
            body {
                background: white;
                color: black;
                padding: 0;
            }
            .container {
                box-shadow: none;
                padding: 0;
                max-width: 100%;
            }
            .btn-group {
                display: none;
            }
            td[contenteditable="true"] {
                background-color: transparent !important;
            }
            .day-title {
                -webkit-print-color-adjust: exact;
                print-color-adjust: exact;
            }
            .day1 .day-title { background-color: #e8f4fd !important; }
            .day2 .day-title { background-color: #fef5e7 !important; }
        }
    </style>
</head>
<body>

<div class="container" id="planner-content">
    <div class="header">
        <h1>Family Trip <span>Bakkhali</span> 🏖️</h1>
        <div class="total-budget">Total Cost: ₹<span id="grand-total">11,100</span></div>
    </div>

    <!-- Buttons -->
    <div class="btn-group" data-html2canvas-ignore="true">
        <button class="btn btn-save-print" onclick="saveAndPrint()">💾 Save Changes & Print</button>
        <button class="btn btn-send" id="send-btn" onclick="openGmailAndDownloadPDF()">📧 Send PDF to kapildeoram1998@gmail.com</button>
    </div>

    <!-- DAY 1 -->
    <div class="day-section day1" id="day1-section">
        <div class="day-title">
            <span>📅 15th August - Towards Bakkhali</span>
            <span class="subtotal-badge">Subtotal: ₹<span id="day1-total">7,980</span></span>
        </div>
        <table>
            <thead>
                <tr>
                    <th style="width: 130px;">Time</th>
                    <th>Activity Description</th>
                    <th class="cost-col">Cost (₹)</th>
                </tr>
            </thead>
            <tbody id="day1-body">
                <tr><td contenteditable="true">04:00 AM</td><td contenteditable="true">Home to Sealdah (2 Cabs)</td><td class="cost-col cost-input" contenteditable="true">1000</td></tr>
                <tr><td contenteditable="true">06:00 AM</td><td contenteditable="true">Sealdah Station to Namkhana Station (Train)</td><td class="cost-col cost-input" contenteditable="true">200</td></tr>
                <tr><td contenteditable="true">07:30 AM</td><td contenteditable="true">Snacks in Train</td><td class="cost-col cost-input" contenteditable="true">240</td></tr>
                <tr><td contenteditable="true">09:00 AM</td><td contenteditable="true">Namkhana Station to Bakkhali (Auto)</td><td class="cost-col cost-input" contenteditable="true">320</td></tr>
                <tr><td contenteditable="true">10:00 AM</td><td contenteditable="true">Hotel check in (3 rooms)</td><td class="cost-col cost-input" contenteditable="true">2700</td></tr>
                <tr><td contenteditable="true">10:30 AM</td><td contenteditable="true">Breakfast</td><td class="cost-col cost-input" contenteditable="true">400</td></tr>
                <tr><td contenteditable="true">11:30 AM</td><td contenteditable="true">Sea Beach bath</td><td class="cost-col cost-input" contenteditable="true">0</td></tr>
                <tr><td contenteditable="true">02:00 PM</td><td contenteditable="true">Lunch</td><td class="cost-col cost-input" contenteditable="true">1200</td></tr>
                <tr><td contenteditable="true">----</td><td contenteditable="true">Rest</td><td class="cost-col cost-input" contenteditable="true">0</td></tr>
                <tr><td contenteditable="true">05:00 PM</td><td contenteditable="true">Sea Beach fish market</td><td class="cost-col cost-input" contenteditable="true">800</td></tr>
                <tr><td contenteditable="true">07:00 PM</td><td contenteditable="true">Chai & tiffin</td><td class="cost-col cost-input" contenteditable="true">160</td></tr>
                <tr><td contenteditable="true">09:00 PM</td><td contenteditable="true">Dinner</td><td class="cost-col cost-input" contenteditable="true">960</td></tr>
                <tr><td contenteditable="true">----</td><td contenteditable="true">Rest</td><td class="cost-col cost-input" contenteditable="true">0</td></tr>
            </tbody>
        </table>
    </div>

    <!-- DAY 2 -->
    <div class="day-section day2" id="day2-section">
        <div class="day-title">
            <span>📅 16th August - Towards Home</span>
            <span class="subtotal-badge">Subtotal: ₹<span id="day2-total">3,120</span></span>
        </div>
        <table>
            <thead>
                <tr>
                    <th style="width: 130px;">Time</th>
                    <th>Activity Description</th>
                    <th class="cost-col">Cost (₹)</th>
                </tr>
            </thead>
            <tbody id="day2-body">
                <tr><td contenteditable="true">10:00 AM</td><td contenteditable="true">Breakfast</td><td class="cost-col cost-input" contenteditable="true">400</td></tr>
                <tr><td contenteditable="true">11:00 AM</td><td contenteditable="true">Bath & get ready for check out</td><td class="cost-col cost-input" contenteditable="true">0</td></tr>
                <tr><td contenteditable="true">11:30 AM</td><td contenteditable="true">Hotel check out</td><td class="cost-col cost-input" contenteditable="true">0</td></tr>
                <tr><td contenteditable="true">12:00 PM</td><td contenteditable="true">Sea Beach shopping market (Individual)</td><td class="cost-col cost-input" contenteditable="true">0</td></tr>
                <tr><td contenteditable="true">01:00 PM</td><td contenteditable="true">Lunch</td><td class="cost-col cost-input" contenteditable="true">1200</td></tr>
                <tr><td contenteditable="true">02:00 PM</td><td contenteditable="true">Bakkhali to Namkhana Station</td><td class="cost-col cost-input" contenteditable="true">320</td></tr>
                <tr><td contenteditable="true">04:00 PM</td><td contenteditable="true">Namkhana Station To Sealdah Station (Train)</td><td class="cost-col cost-input" contenteditable="true">200</td></tr>
                <tr><td contenteditable="true">08:00 PM</td><td contenteditable="true">Sealdah Station to Home (2 Cabs)</td><td class="cost-col cost-input" contenteditable="true">1000</td></tr>
            </tbody>
        </table>
    </div>
</div>

<script>
    function updateTotals() {
        let day1Total = 0;
        let day2Total = 0;

        document.querySelectorAll('#day1-body .cost-input').forEach(cell => {
            let val = parseFloat(cell.innerText.replace(/[^\d.-]/g, '')) || 0;
            day1Total += val;
        });

        document.querySelectorAll('#day2-body .cost-input').forEach(cell => {
            let val = parseFloat(cell.innerText.replace(/[^\d.-]/g, '')) || 0;
            day2Total += val;
        });

        document.getElementById('day1-total').innerText = day1Total.toLocaleString('en-IN');
        document.getElementById('day2-total').innerText = day2Total.toLocaleString('en-IN');
        document.getElementById('grand-total').innerText = (day1Total + day2Total).toLocaleString('en-IN');
    }

    document.querySelectorAll('.cost-input').forEach(cell => {
        cell.addEventListener('input', updateTotals);
    });

    function getTableData() {
        const data = {
            day1: [],
            day2: []
        };

        document.querySelectorAll('#day1-body tr').forEach(row => {
            const cells = row.querySelectorAll('td');
            data.day1.push({ time: cells[0].innerText, desc: cells[1].innerText, cost: cells[2].innerText });
        });

        document.querySelectorAll('#day2-body tr').forEach(row => {
            const cells = row.querySelectorAll('td');
            data.day2.push({ time: cells[0].innerText, desc: cells[1].innerText, cost: cells[2].innerText });
        });

        return data;
    }

    function saveAndPrint() {
        const data = getTableData();
        localStorage.setItem('bakkhali_trip_12h_data', JSON.stringify(data));
        
        setTimeout(() => {
            window.print();
        }, 150);
    }

    function openGmailAndDownloadPDF() {
        // Save table edits locally first
        const data = getTableData();
        localStorage.setItem('bakkhali_trip_12h_data', JSON.stringify(data));

        const element = document.getElementById('planner-content');
        const opt = {
            margin:       10,
            filename:     'Bakkhali_Trip_Budget.pdf',
            image:        { type: 'jpeg', quality: 0.98 },
            html2canvas:  { scale: 2, useCORS: true },
            jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
        };

        // 1. Generate and instantly trigger download for the updated PDF
        html2pdf().set(opt).from(element).save();

        // 2. Open a popup compose window directly inside Gmail addressed to your recipient
        const emailTo = "kapildeoram1998@gmail.com";
        const subject = encodeURIComponent("Updated Family Trip Bakkhali Budget");
        const body = encodeURIComponent("Hello,\n\nPlease find the downloaded trip planner PDF attached to this message.");
        
        const gmailUrl = `https://mail.google.com/mail/?view=cm&fs=1&to=${emailTo}&su=${subject}&body=${body}`;
        
        window.open(gmailUrl, '_blank');
    }

    function loadData() {
        const storedData = localStorage.getItem('bakkhali_trip_12h_data');
        if (!storedData) return;

        const data = JSON.parse(storedData);
        
        const buildTable = (bodyId, rowsData) => {
            const tbody = document.getElementById(bodyId);
            tbody.innerHTML = '';
            rowsData.forEach(item => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td contenteditable="true">${item.time}</td>
                    <td contenteditable="true">${item.desc}</td>
                    <td class="cost-col cost-input" contenteditable="true">${item.cost}</td>
                `;
                tbody.appendChild(tr);
            });
        };

        if(data.day1) buildTable('day1-body', data.day1);
        if(data.day2) buildTable('day2-body', data.day2);
        
        document.querySelectorAll('.cost-input').forEach(cell => {
            cell.addEventListener('input', updateTotals);
        });
        updateTotals();
    }

    window.onload = loadData;
</script>

</body>
</html>
