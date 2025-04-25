# index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Crypto Scanner</title>
  <style>
    body {
      background-color: #111;
      color: #fff;
      font-family: 'Segoe UI', sans-serif;
      padding: 20px;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
      color: #00ffe7;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 30px;
    }
    th, td {
      padding: 10px;
      text-align: left;
      border-bottom: 1px solid #333;
    }
    th {
      background-color: #222;
    }
    .triggered {
      color: #00ff87;
    }
    .waiting {
      color: #ffa500;
    }
  </style>
</head>
<body>
  <h1>Live Crypto Scanner</h1>

  <h2>Triggered Trades</h2>
  <table id="triggeredTable">
    <thead>
      <tr>
        <th>Symbol</th>
        <th>Timeframe</th>
        <th>Direction</th>
        <th>Entry</th>
        <th>Stoploss</th>
        <th>Target</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <h2>Waiting List</h2>
  <table id="waitingTable">
    <thead>
      <tr>
        <th>Symbol</th>
        <th>Timeframe</th>
        <th>Status</th>
        <th>Remarks</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    async function fetchData() {
      try {
        const triggeredRes = await fetch("https://your-backend.onrender.com/api/triggered");
        const triggered = await triggeredRes.json();

        const waitingRes = await fetch("https://your-backend.onrender.com/api/waiting");
        const waiting = await waitingRes.json();

        const triggeredBody = document.querySelector("#triggeredTable tbody");
        triggeredBody.innerHTML = "";
        triggered.forEach(row => {
          triggeredBody.innerHTML += `
            <tr class="triggered">
              <td>${row.symbol}</td>
              <td>${row.timeframe}</td>
              <td>${row.direction}</td>
              <td>${row.entry}</td>
              <td>${row.stoploss}</td>
              <td>${row.target}</td>
            </tr>
          `;
        });

        const waitingBody = document.querySelector("#waitingTable tbody");
        waitingBody.innerHTML = "";
        waiting.forEach(row => {
          waitingBody.innerHTML += `
            <tr class="waiting">
              <td>${row.symbol}</td>
              <td>${row.timeframe}</td>
              <td>${row.status}</td>
              <td>${row.remarks}</td>
            </tr>
          `;
        });

      } catch (error) {
        console.error("Error fetching data", error);
      }
    }

    fetchData();
    setInterval(fetchData, 60000); // auto refresh every 60 seconds
  </script>
</body>
</html>
