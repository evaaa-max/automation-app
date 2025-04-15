<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shirt Operation Dashboard</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* General Styles */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            display: flex;
        }
        .sidebar {
            width: 220px;
            background-color: black;
            padding: 20px;
            color: white;
            height: 100vh;
            position: fixed;
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            justify-content: flex-start;
        }
        .sidebar img {
            width: 80px;
            height: auto;
            margin-left: 15px;
            margin-bottom: 10px;
            display: inline-block;
        }
        .sidebar nav {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            margin-top: 20px;
        }
        .sidebar nav a {
            display: block;
            margin: 10px 0;
            color: white;
            font-weight: bold;
            text-decoration: none;
            padding: 10px 15px;
            border-radius: 5px;
            transition: background-color 0.3s ease, padding-left 0.2s ease;
            width: 100%;
        }
        .sidebar nav a:hover {
            background-color: #575757;
            padding-left: 20px;
        }
        .container {
            margin-left: 250px;
            width: 80%;
            padding: 20px;
            display: flex;
            flex-direction: column;
        }
        .header {
            text-align: center;
            margin-bottom: 20px;
        }
        .hidden {
            display: none;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table th, table td {
            padding: 10px;
            border: 1px solid #ccc;
            text-align: center;
        }
        table th {
            background-color: #f2f2f2;
        }
        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            margin-top: 10px; /* Added margin for better spacing */
        }
        button:hover {
            background-color: #45a049;
        }
        .delete-button {
            background-color: #FF0000;
            color: white;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
        }
        .delete-button:hover {
            background-color: #cc0000;
        }
        .button-container {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
        }
        .button-container button {
            margin-left: 10px;
        }
        .chart {
            width: 100%; /* Set the width of the chart */
            height: 200px; /* Set the height of the chart */
        }
        .summary-table {
            margin-top: 80px; /* Align with the graph */
            border-collapse: collapse;
            width: 100%;
        }
        .summary-table th, .summary-table td {
            border: 1px solid #ccc;
            padding: 10px;
            text-align: center;
        }
        .dashboard-container {
            display: flex;
            justify-content: space-between;
            align-items: flex-start; /* Align items to the start to match baseline */
        }
        .chart-container {
            width: 60%; /* Adjust width for the chart */
        }
        .summary-container {
            width: 35%; /* Adjust width for the summary table */
            margin-left: 20px; /* Add margin to separate from the chart */
        }
    </style>
</head>
<body>
    <!-- Sidebar -->
    <div class="sidebar">
        <img src="https://media-exp1.licdn.com/dms/image/C4D0BAQEVycySNP4tcg/company-logo_200_200/0/1605605250350?e=2159024400&v=beta&t=QO27YUpaujujF9nFOgoqJrRfrb9gzHNqMyun-TOeSpE" alt="Company Logo">
        <nav>
            <a href="#" onclick="showSection('operations')">Shirt Operations</a>
            <a href="#" onclick="showSection('aggregated')">Aggregated Data</a>
            <a href="#" onclick="showSection('dashboard')">Dashboard</a>
        </nav>
    </div>

    <!-- Main Content -->
    <div class="container">
        <!-- Shirt Operations Section -->
        <div id="operations" class="content">
            <div>
                <select id="sectionDropdown" onchange="updateOperationsDropdown()">
                    <option value="">Select Section</option>
                </select>
            </div>
            <table id="operationsTable">
                <thead>
                    <tr>
                        <th>Section</th>
                        <th>Operation</th>
                        <th>Automation Level</th>
                        <th>SMV</th>
                        <th>Grade</th>
                        <th>Output</th>
                        <th>M/c Required @ 90%</th>
                        <th>No. of Operators</th>
                        <th>Wages</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td class="sectionCell">-</td>
                        <td>
                            <select class="operationDropdown" onchange="updateAutomationDropdown(this)">
                                <option value="">Select Operation</option>
                            </select>
                        </td>
                        <td>
                            <select class="automationDropdown" onchange="updateTableData(this)">
                                <option value="">Select Automation Level</option>
                            </select>
                        </td>
                        <td class="smvCell">-</td>
                        <td class="gradeCell">-</td>
                        <td class="outputCell">-</td>
                        <td class="mchCell">-</td>
                        <td class="numOperatorsCell">-</td>
                        <td class="wagesCell">-</td>
                        <td><button class="delete-button" onclick="deleteRow(this)">Delete</button></td>
                    </tr>
                </tbody>
            </table>
            <div class="button-container">
                <button onclick="addRow()">Add Row</button>
                <button onclick="saveData()">Save Data</button>
            </div>
        </div>

        <!-- Aggregated Data Section -->
        <div id="aggregated" class="content hidden">
            <table id="aggregatedTable">
                <thead>
                    <tr>
                        <th>S. No.</th>
                        <th>Section</th>
                        <th>Operation</th>
                        <th>Automation Level</th>
                        <th>SMV</th>
                        <th>Grade</th>
                        <th>Output</th>
                        <th>M/c Required @ 90%</th>
                        <th>No. of Operators</th>
                        <th>Wages</th>
                        <th>Delete</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
            <button onclick="exportToExcel()" style="margin-top: 10px;">Export to Excel</button> <!-- Adjusted margin -->
        </div>

        <!-- Dashboard Section -->
        <div id="dashboard" class="content hidden">
            <div class="dashboard-container">
                <div class="chart-container" id="dashboardCharts"></div>
                <div class="summary-container" id="dataAnalysis"></div>
            </div>
        </div>
    </div>

    <script>
        const salaryMapping = {
            "A*": 10991,
            "A+": 10660,
            "A": 10660,
            "B": 10398,
            "C": 10398,
            "D": 10131,
            "E": 10131
        };

        let sheetData = [];
        let savedData = JSON.parse(localStorage.getItem("savedShirtData")) || [];

        function fetchData() {
            const apiUrl = "https://script.google.com/macros/s/AKfycbx000BkphDpUgSs8iKBjb7YNTqlarZwiUCib4vsLsHGjjEKrJYZba_VN9IK6T3sF4oZ/exec";
            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    sheetData = data;
                    populateSectionsDropdown();
                })
                .catch(error => {
                    console.error("Error fetching data:", error);
                    alert("Error fetching data. Please try again later.");
                });
        }

        function populateSectionsDropdown() {
            const sectionDropdown = document.getElementById("sectionDropdown");
            const sections = [...new Set(sheetData.map(row => row["Section"]))];

            sectionDropdown.innerHTML = '<option value="">Select Section</option>';
            sections.forEach(section => {
                const option = document.createElement("option");
                option.value = section;
                option.textContent = section;
                sectionDropdown.appendChild(option);
            });
        }

        function updateOperationsDropdown() {
            const section = document.getElementById("sectionDropdown").value;
            const operationDropdowns = document.querySelectorAll(".operationDropdown");

            operationDropdowns.forEach(operationDropdown => {
                operationDropdown.innerHTML = '<option value="">Select Operation</option>';
                const operations = [...new Set(sheetData.filter(row => row["Section"] === section).map(row => row["Operation"]))];

                operations.forEach(operation => {
                    const option = document.createElement("option");
                    option.value = operation;
                    option.textContent = operation;
                    operationDropdown.appendChild(option);
                });

                // Update the section in the first row of the table
                const firstRow = document.querySelector("#operationsTable tbody tr");
                firstRow.querySelector('.sectionCell').textContent = section;
            });
        }

        function updateAutomationDropdown(operationDropdown) {
            const section = operationDropdown.closest('tr').querySelector('.sectionCell').textContent;
            const operation = operationDropdown.value;
            const automationDropdown = operationDropdown.closest('tr').querySelector('.automationDropdown');
            automationDropdown.innerHTML = '<option value="">Select Automation Level</option>';

            const automationLevels = [...new Set(sheetData.filter(row => row["Section"] === section && row["Operation"] === operation).map(row => row["Automation Level"]))];

            automationLevels.forEach(level => {
                const option = document.createElement("option");
                option.value = level;
                option.textContent = level;
                automationDropdown.appendChild(option);
            });
        }

        function updateTableData(automationDropdown) {
            const row = automationDropdown.closest('tr');
            const section = row.querySelector('.sectionCell').textContent;
            const operation = row.querySelector('.operationDropdown').value;
            const automationLevel = automationDropdown.value;

            const selectedRow = sheetData.find(rowData => rowData["Section"] === section && rowData["Operation"] === operation && rowData["Automation Level"] === automationLevel);

            if (selectedRow) {
                row.querySelector('.smvCell').textContent = selectedRow["SMV"];
                row.querySelector('.gradeCell').textContent = selectedRow["Grade"];
                row.querySelector('.outputCell').textContent = 225;
                row.querySelector('.mchCell').textContent = (225 / ((60 / selectedRow["SMV"]) * 0.9)).toFixed(2);
                row.querySelector('.numOperatorsCell').textContent = parseFloat(selectedRow["No. of Operator"]).toFixed(2);
                row.querySelector('.wagesCell').textContent = (salaryMapping[selectedRow["Grade"]] * parseFloat(selectedRow["No. of Operator"])).toFixed(2);
            } else {
                row.querySelector('.smvCell').textContent = "-";
                row.querySelector('.gradeCell').textContent = "-";
                row.querySelector('.outputCell').textContent = "-";
                row.querySelector('.mchCell').textContent = "-";
                row.querySelector('.numOperatorsCell').textContent = "-";
                row.querySelector('.wagesCell').textContent = "-";
            }
        }

        function addRow() {
            const tableBody = document.querySelector("#operationsTable tbody");
            const newRow = document.createElement("tr");
            newRow.innerHTML = `
                <td class="sectionCell">${document.getElementById("sectionDropdown").value}</td>
                <td>
                    <select class="operationDropdown" onchange="updateAutomationDropdown(this)">
                        <option value="">Select Operation</option>
                    </select>
                </td>
                <td>
                    <select class="automationDropdown" onchange="updateTableData(this)">
                        <option value="">Select Automation Level</option>
                    </select>
                </td>
                <td class="smvCell">-</td>
                <td class="gradeCell">-</td>
                <td class="outputCell">-</td>
                <td class="mchCell">-</td>
                <td class="numOperatorsCell">-</td>
                <td class="wagesCell">-</td>
                <td><button class="delete-button" onclick="deleteRow(this)">Delete</button></td>
            `;
            tableBody.appendChild(newRow);

            // Populate operations for the new row based on the selected section
            const section = document.getElementById("sectionDropdown").value;
            populateOperationsForRow(newRow, section);
        }

        function populateOperationsForRow(row, section) {
            const operationDropdown = row.querySelector(".operationDropdown");
            operationDropdown.innerHTML = '<option value="">Select Operation</option>';
            const operations = [...new Set(sheetData.filter(row => row["Section"] === section).map(row => row["Operation"]))];

            operations.forEach(operation => {
                const option = document.createElement("option");
                option.value = operation;
                option.textContent = operation;
                operationDropdown.appendChild(option);
            });
        }

        function deleteRow(button) {
            const row = button.closest('tr');
            row.remove();
        }

        function saveData() {
            const rows = Array.from(document.querySelectorAll("#operationsTable tbody tr"));
            const dataToSave = rows.map((row) => ({
                "Section": row.querySelector('.sectionCell').textContent,
                "Operation": row.querySelector('.operationDropdown') ? row.querySelector('.operationDropdown').value : row.querySelector('span').textContent,
                "Automation Level": row.querySelector('.automationDropdown') ? row.querySelector('.automationDropdown').value : row.querySelector('span').textContent,
                "SMV": row.querySelector('.smvCell').textContent,
                "Grade": row.querySelector('.gradeCell').textContent,
                "Output": row.querySelector('.outputCell').textContent,
                "M/c Required @ 90%": row.querySelector('.mchCell').textContent,
                "No. of Operators": row.querySelector('.numOperatorsCell').textContent,
                "Wages": row.querySelector('.wagesCell').textContent,
            }));

            savedData = [...savedData, ...dataToSave];
            localStorage.setItem("savedShirtData", JSON.stringify(savedData));
            populateAggregatedTable();
            renderDashboard();
            alert("Data saved successfully!");

            // Clear the table for new entries
            document.querySelector("#operationsTable tbody").innerHTML = '';
            addRow(); // Add a new empty row
        }

        function populateAggregatedTable() {
            const tableBody = document.querySelector("#aggregatedTable tbody");
            tableBody.innerHTML = savedData.map((row, index) =>
                `<tr>
                    <td>${index + 1}</td>
                    <td>${row["Section"]}</td>
                    <td>${row["Operation"]}</td>
                    <td>${row["Automation Level"]}</td>
                    <td>${row["SMV"]}</td>
                    <td>${row["Grade"]}</td>
                    <td>${row["Output"]}</td>
                    <td>${row["M/c Required @ 90%"]}</td>
                    <td>${row["No. of Operators"]}</td>
                    <td>${row["Wages"]}</td>
                    <td><button class="delete-button" onclick="deleteAggregatedRow(${index})">Delete</button></td>
                </tr>`).join("");
        }

        function deleteAggregatedRow(index) {
            savedData.splice(index, 1); // Remove the item from savedData
            localStorage.setItem("savedShirtData", JSON.stringify(savedData)); // Update localStorage
            populateAggregatedTable(); // Refresh the aggregated table
        }

        function exportToExcel() {
            const table = document.getElementById("aggregatedTable");
            const rows = Array.from(table.rows);
            let csvContent = rows.map(row => Array.from(row.cells).map(cell => cell.textContent).join(",")).join("\n");
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            link.href = URL.createObjectURL(blob);
            link.setAttribute("download", "aggregated_data.csv");
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }

        function showSection(sectionId) {
            document.querySelectorAll(".content").forEach(content => content.classList.add("hidden"));
            document.getElementById(sectionId).classList.remove("hidden");
            if (sectionId === "dashboard") {
                renderDashboard();
            }
        }

        function renderDashboard() {
            const container = document.getElementById("dashboardCharts");
            const analysisContainer = document.getElementById("dataAnalysis");
            container.innerHTML = "";
            analysisContainer.innerHTML = "";

            const grades = ["A*", "A+", "A", "B", "C", "D", "E"];
            const automationLevels = ["Manual", "Semi", "Automatic", "Advanced"];
            const sections = [...new Set(savedData.map(data => data["Section"]))];

            sections.forEach(section => {
                const bellCurveData = automationLevels.map(level => {
                    return grades.map(grade => {
                        const count = savedData
                            .filter(data =>
                                data["Section"] === section &&
                                data["Automation Level"] === level &&
                                data["Grade"] === grade
                            )
                            .reduce((sum, row) => sum + parseFloat(row["No. of Operators"]), 0);
                        return count;
                    });
                });

                const chart = document.createElement("canvas");
                chart.setAttribute("class", "chart");
                container.appendChild(chart);
                new Chart(chart, {
                    type: 'line',
                    data: {
                        labels: grades,
                        datasets: automationLevels.map((level, i) => ({
                            label: level,
                            data: bellCurveData[i],
                            fill: false,
                            borderColor: `rgba(${i * 80}, 99, 132, 1)`, 
                            tension: 0.1,
                            pointStyle: 'circle',
                            pointRadius: 5,
                            pointBackgroundColor: 'rgba(0, 0, 0, 1)',
                        }))
                    },
                    options: {
                        responsive: true,
                        plugins: {
                            title: {
                                display: true,
                                text: `Output for ${section} Section`
                            },
                            tooltip: {
                                callbacks: {
                                    title: function(tooltipItem) {
                                        return grades[tooltipItem[0].dataIndex]; 
                                    },
                                    label: function(tooltipItem) {
                                        return `Operators: ${tooltipItem.raw}`;
                                    }
                                }
                            }
                        }
                    }
                });

                // Create summary table
                const summaryTable = document.createElement("table");
                summaryTable.className = "summary-table";
                summaryTable.innerHTML = `
                    <thead>
                        <tr>
                            <th>Automation Level</th>
                            <th>Total No. of Operators</th>
                            <th>Total Wages</th>
                        </tr>
                    </thead>
                    <tbody>
                        ${automationLevels.map(level => {
                            const operatorsCount = savedData
                                .filter(data => data["Section"] === section && data["Automation Level"] === level)
                                .reduce((sum, row) => sum + parseFloat(row["No. of Operators"]), 0);
                            const totalWages = savedData
                                .filter(data => data["Section"] === section && data["Automation Level"] === level)
                                .reduce((sum, row) => sum + parseFloat(row["Wages"]), 0);

                            return `
                                <tr>
                                    <td>${level}</td>
                                    <td>${operatorsCount}</td>
                                    <td>${totalWages.toFixed(2)}</td>
                                </tr>
                            `;
                        }).join("")}
                    </tbody>
                `;
                analysisContainer.appendChild(summaryTable);
            });
        }

        // Initial Data Fetch
        fetchData();
    </script>
</body>
</html>


















