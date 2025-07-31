# Babo-bear-Booking

<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>Babo Bear Booking</title>
  <link href="https://cdn.jsdelivr.net/npm/fullcalendar@6.1.11/index.global.min.css" rel="stylesheet" />
  <link href="https://fonts.googleapis.com/css2?family=Kanit&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Kanit', sans-serif;
      background: linear-gradient(to bottom, #fff3f3, #ffe9e9);
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 960px;
      margin: auto;
      padding: 20px;
    }

    .header {
      text-align: center;
      padding: 10px 0 0;
      color: #6b3e26;
    }

    .logo {
      width: 100px;
      height: auto;
      display: block;
      margin: 0 auto 10px;
    }

    .input-form {
      background-color: #fff;
      border: 2px solid #f8d3d3;
      border-radius: 15px;
      padding: 20px;
      margin-bottom: 20px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.05);
    }

    label {
      margin-top: 10px;
      font-weight: bold;
      display: block;
      color: #6b3e26;
    }

    input, select {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border-radius: 10px;
      border: 1px solid #ddd;
      background-color: #fffdfd;
      font-size: 16px;
    }

    button {
      background-color: #f4a6a6;
      color: white;
      padding: 12px 20px;
      margin-top: 20px;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      font-size: 16px;
      transition: 0.2s;
    }

    button:hover {
      background-color: #e88c8c;
    }

    #searchInput {
      width: 100%;
      padding: 12px;
      border-radius: 10px;
      border: 1px solid #ddd;
      margin-bottom: 20px;
      font-size: 16px;
      background-color: #fffafa;
    }

    #calendar {
      background-color: #fff;
      border-radius: 15px;
      padding: 10px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.08);
    }

    @media (max-width: 768px) {
      .container {
        padding: 10px;
      }

      #calendar {
        font-size: 12px;
      }

      input, select, button {
        font-size: 14px;
      }

      .logo {
        width: 80px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <img src="/mnt/data/Screenshot_2025-07-31_093858-removebg-preview.png" alt="Babo Bear Logo" class="logo" />
    <h1 class="header">📅Babo Bear Booking</h1>

    <div class="input-form">
      <label for="title">ชื่อกิจกรรม</label>
      <input type="text" id="title" placeholder="เช่น แจกชานมฟรี, ออกร้านที่ห้าง...">

      <label for="store">เลือกร้านที่ไปออก Event</label>
      <select id="store">
        <option value="Babo Bear A">Babo Bear A</option>
        <option value="Babo Bear B">Babo Bear B</option>
      </select>

      <label for="start">วันที่เริ่ม</label>
      <input type="date" id="start">

      <label for="end">วันที่สิ้นสุด</label>
      <input type="date" id="end">

      <button onclick="addEvent()">✨ เพิ่มกิจกรรม</button>
    </div>

    <input type="text" id="searchInput" placeholder="🔍 ค้นหาชื่อกิจกรรม..." oninput="filterEvents()" />

    <div id="calendar"></div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/fullcalendar@6.1.11/index.global.min.js"></script>
  <script>
    let calendar;
    let allEvents = [];

    document.addEventListener('DOMContentLoaded', function () {
      const calendarEl = document.getElementById('calendar');
      calendar = new FullCalendar.Calendar(calendarEl, {
        initialView: 'dayGridMonth',
        editable: true,
        selectable: true,
        height: 'auto',
        eventClick: function(info) {
          if (confirm(`ลบ "${info.event.title}" หรือไม่?`)) {
            info.event.remove();
            allEvents = allEvents.filter(e => e.id !== info.event.id);
          }
        },
      });
      calendar.render();
    });

    function getColorForStore(store) {
      if (store === "Babo Bear A") {
        return "#f9c5d1";  // ชมพูพาสเทล
      } else if (store === "Babo Bear B") {
        return "#d6b89c";  // น้ำตาลพาสเทล
      }
      return "#ccc";
    }

    function addEvent() {
      const title = document.getElementById('title').value.trim();
      const store = document.getElementById('store').value;
      const start = document.getElementById('start').value;
      const end = document.getElementById('end').value;

      if (title && start && end) {
        const eventId = String(Date.now());
        const eventColor = getColorForStore(store);
        const event = {
          id: eventId,
          title: `${title} (${store})`,
          start: start,
          end: new Date(new Date(end).getTime() + 86400000).toISOString().split('T')[0],
          backgroundColor: eventColor,
          borderColor: eventColor,
          textColor: "#333",
          allDay: true
        };

        calendar.addEvent(event);
        allEvents.push(event);

        document.getElementById('title').value = '';
        document.getElementById('start').value = '';
        document.getElementById('end').value = '';
      } else {
        alert('กรุณากรอกข้อมูลให้ครบ');
      }
    }

    function filterEvents() {
      const keyword = document.getElementById('searchInput').value.toLowerCase();
      calendar.removeAllEvents();

      const filtered = allEvents.filter(event => event.title.toLowerCase().includes(keyword));
      filtered.forEach(ev => calendar.addEvent(ev));
    }
  </script>
</body>
</html>
