# student-attendence
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Enhanced Class Attendance</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f8ff;
      padding: 20px;
    }
    h1, h2 {
      text-align: center;
      color: #1a237e;
    }
    .student, .add-form {
      background-color: #ffffff;
      border-left: 5px solid #2196f3;
      border-radius: 8px;
      padding: 15px;
      margin: 10px 0;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    .student-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .info {
      font-size: 14px;
      color: #444;
      margin: 5px 0;
    }
    .buttons button {
      margin-left: 5px;
      padding: 6px 12px;
      border: none;
      border-radius: 4px;
      color: white;
      cursor: pointer;
      font-weight: bold;
    }
    .buttons button:first-child {
      background-color: #4caf50;
    }
    .buttons button:last-child {
      background-color: #f44336;
    }
    .reason {
      margin-top: 8px;
      display: none;
      padding: 5px;
      width: 100%;
      border: 1px solid #ccc;
      border-radius: 4px;
      background-color: #fff9c4;
    }
    #submitBtn, #exportBtn {
      margin: 20px auto;
      display: block;
      padding: 10px 20px;
      font-size: 16px;
      background-color: #3f51b5;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    #submitBtn:hover, #exportBtn:hover {
      background-color: #303f9f;
    }
    #result, #history {
      margin-top: 20px;
      background-color: #e3f2fd;
      padding: 15px;
      border-radius: 8px;
    }
    .add-form input {
      padding: 7px;
      margin: 5px;
      width: calc(24% - 12px);
      border: 1px solid #bbb;
      border-radius: 4px;
    }
    .add-form button {
      padding: 7px 14px;
      background-color: #009688;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-weight: bold;
    }
    .add-form button:hover {
      background-color: #00796b;
    }
    #chartContainer {
      margin-top: 30px;
      background: white;
      border-radius: 8px;
      padding: 20px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
  </style>
</head>
<body>

  <img src="https://images.shiksha.com/mediadata/images/1656234662phpzyOTx2.jpeg" alt="Class Logo" style="display: block; margin: 0 auto; max-height: 200px;" />
  <h1>Class Attendance</h1>

  <div style="text-align:center; margin-bottom: 20px;">
    <label for="attendanceDate"><strong>Select Date: </strong></label>
    <input type="date" id="attendanceDate" />
  </div>

  <div class="add-form">
    <h2>Add New Student</h2>
    <input type="text" id="name" placeholder="Name" />
    <input type="text" id="roll" placeholder="Roll No" />
    <input type="text" id="phone" placeholder="Phone" />
    <input type="email" id="email" placeholder="Email" />
    <button onclick="addStudent()">Add Student</button>
  </div>

  <div id="studentList"></div>
  <button id="submitBtn">Submit Attendance</button>
  <button id="exportBtn">Download CSV</button>
  <div id="result"></div>
  <div id="history"><h2>Attendance History</h2></div>

  <div id="chartContainer">
    <h2 style="text-align:center">Monthly Attendance Summary</h2>
    <canvas id="attendanceChart"></canvas>
  </div>

  <script>
    const students = [
      { name: "Manju", phone: "9876543210", roll: "1", email: "manju@example.com" },
      { name: "Sanju", phone: "9876543211", roll: "2", email: "sanju@example.com" },
      { name: "Vani", phone: "6305464985", roll: "3", email: "vani@example.com" },
      { name: "Harika", phone: "9876543213", roll: "4", email: "harika@example.com" },
      { name: "Joshana", phone: "9876543214", roll: "5", email: "joshana@example.com" }
    ];

    const attendance = {};
    const studentListDiv = document.getElementById("studentList");

    function renderStudentList() {
      studentListDiv.innerHTML = "";
      students.forEach(({ name, roll, email }, index) => {
        const div = document.createElement("div");
        div.className = "student";
        div.innerHTML = `
          <div class="student-top">
            <span><strong>${name}</strong></span>
            <div class="buttons">
              <button onclick="mark('${name}', 'Present', ${index})">Present</button>
              <button onclick="mark('${name}', 'Absent', ${index})">Absent</button>
            </div>
          </div>
          <div class="info">Roll No: ${roll}</div>
          <div class="info">Email: ${email}</div>
          <input class="reason" type="text" id="reason-${index}" placeholder="Enter reason for absence" />
        `;
        studentListDiv.appendChild(div);
      });
    }
    
    function mark(name, status, index) {
      const reasonInput = document.getElementById(`reason-${index}`);
      if (status === "Absent") {
        reasonInput.style.display = "block";
      } else {
        reasonInput.style.display = "none";
        reasonInput.value = "";
      }
      attendance[name] = { status, reason: "" };
    }
    
    function addStudent() {
      const name = document.getElementById("name").value.trim();
      const roll = document.getElementById("roll").value.trim();
      const phone = document.getElementById("phone").value.trim();
      const email = document.getElementById("email").value.trim();
      if (!name || !roll || !phone || !email) {
        alert("Please fill in all fields to add a student.");
        return;
      }
      students.push({ name, roll, phone, email });
      renderStudentList();
      document.getElementById("name").value = "";
      document.getElementById("roll").value = "";
      document.getElementById("phone").value = "";
      document.getElementById("email").value = "";
    }
    
    function saveAttendanceToHistory(date, data) {
      const history = JSON.parse(localStorage.getItem("attendanceHistory") || "[]");
      history.push({ date, data });
      localStorage.setItem("attendanceHistory", JSON.stringify(history));
    }

    function loadAttendanceHistory() {
      const historyDiv = document.getElementById("history");
      historyDiv.innerHTML = "<h2>Attendance History</h2>";
      const history = JSON.parse(localStorage.getItem("attendanceHistory") || "[]");
      if (history.length === 0) {
        historyDiv.innerHTML += "<p>No attendance history yet.</p>";
        return;
      }
      history.forEach(entry => {
        const block = document.createElement("div");
        block.style.marginTop = "10px";
        let html = `<strong>Date: ${entry.date}</strong><ul>`;
        entry.data.forEach(student => {
          html += `<li>${student.roll} - ${student.name}: <strong>${student.status}</strong>`;
          if (student.status === "Absent") {
            html += ` - Reason: ${student.reason}`;
          }
          html += `</li>`;
        });
        html += `</ul>`;
        block.innerHTML = html;
        historyDiv.appendChild(block);
      });
    }

    function exportCSV() {
      let csv = "Date,Roll No,Name,Status,Reason\n";
      const history = JSON.parse(localStorage.getItem("attendanceHistory") || "[]");
      history.forEach(({ date, data }) => {
        data.forEach(s => {
          csv += `${date},${s.roll},${s.name},${s.status},${s.reason || ""}\n`;
        });
      });
      const blob = new Blob([csv], { type: "text/csv" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "attendance.csv";
      a.click();
    }

    function drawChart() {
      const ctx = document.getElementById('attendanceChart').getContext('2d');
      const history = JSON.parse(localStorage.getItem("attendanceHistory") || "[]");
      const counts = {};

      students.forEach(s => counts[s.name] = { Present: 0, Absent: 0 });
      history.forEach(({ data }) => {
        data.forEach(({ name, status }) => {
          if (counts[name]) counts[name][status]++;
        });
      });

      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: students.map(s => s.name),
          datasets: [
            {
              label: 'Present',
              data: students.map(s => counts[s.name].Present),
              backgroundColor: 'green'
            },
            {
              label: 'Absent',
              data: students.map(s => counts[s.name].Absent),
              backgroundColor: 'red'
            }
          ]
        }
      });
    }

    document.getElementById("submitBtn").onclick = () => {
      const date = document.getElementById("attendanceDate").value;
      if (!date) {
        alert("Please select a date for attendance.");
        return;
      }
      let resultHtml = "<h3>Attendance Result:</h3><ul>";
      const record = [];
      students.forEach(({ name, phone, roll }, index) => {
        const entry = attendance[name] || { status: "Not Marked", reason: "" };
        if (entry.status === "Absent") {
          const reasonInput = document.getElementById(`reason-${index}`);
          entry.reason = reasonInput.value.trim() || "No reason provided";
        }
        resultHtml += `<li>${roll} - ${name}: <strong>${entry.status}</strong>`;
        if (entry.status === "Absent") {
          resultHtml += ` - Reason: ${entry.reason}`;
          console.log(`\u{1F4E9} SMS to ${phone}: ${name} (Roll No: ${roll}) is absent. Reason: ${entry.reason}`);
        }
        resultHtml += `</li>`;
        record.push({ name, roll, status: entry.status, reason: entry.reason });
      });
      resultHtml += "</ul>";
      document.getElementById("result").innerHTML = resultHtml;
      saveAttendanceToHistory(date, record);
      loadAttendanceHistory();
      drawChart();
    };

    document.getElementById("exportBtn").onclick = exportCSV;

    // Init
    renderStudentList();
    loadAttendanceHistory();
    drawChart();
    
  </script>
</body>
</html>
