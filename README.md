<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Calculate OEE / 计算 OEE</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 30px auto; padding: 20px; }
    label, input, select, button { display: block; width: 100%; margin-bottom: 15px; padding: 10px; }
    button { width: auto; background-color: #4CAF50; color: white; border: none; cursor: pointer; }
    .result { font-size: 1.2em; font-weight: bold; margin-top: 20px; }
  </style>
</head>
<body>

  <label for="lang">Language / 语言:</label>
  <select id="lang" onchange="switchLang()">
    <option value="en">English</option>
    <option value="zh">中文</option>
  </select>

  <label for="mode">Select Product:</label>
  <select id="mode">
    <option value="hld">HLD</option>
    <option value="broadfar">Broadfar</option>
  </select>

  <h2 id="title">Calculate OEE</h2>

  <label id="label-actual">Actual Qualified Output:</label>
  <input type="number" id="actualOutput" />

  <label id="label-waste">Quality Waste:</label>
  <input type="number" id="qualityWaste" />

  <button onclick="calculateOEE()" id="calculateBtn">Calculate</button>

  <div class="result" id="result"></div>

  <script>
    const labels = {
      en: {
        title: "Calculate OEE",
        actual: "Actual Qualified Output:",
        waste: "Quality Waste:",
        calculate: "Calculate"
      },
      zh: {
        title: "计算 OEE",
        actual: "合格产量:",
        waste: "质量损耗:",
        calculate: "计算"
      }
    };

    function switchLang() {
      const lang = document.getElementById('lang').value;
      document.getElementById('title').textContent = labels[lang].title;
      document.getElementById('label-actual').textContent = labels[lang].actual;
      document.getElementById('label-waste').textContent = labels[lang].waste;
      document.getElementById('calculateBtn').textContent = labels[lang].calculate;
    }

    function calculateOEE() {
      const actual = parseFloat(document.getElementById('actualOutput').value) || 0;
      const waste = parseFloat(document.getElementById('qualityWaste').value) || 0;
      const mode = document.getElementById('mode').value;

      let uptime, scheduleTime, ratedSpeed, bandcastWidth, gsm, calendarTime;

      if (mode === 'hld') {
        uptime = 480;
        scheduleTime = 480;
        ratedSpeed = 14.5;
        bandcastWidth = 1.2;
        gsm = 0.095;
        calendarTime = 480;
      } else if (mode === 'broadfar') {
        uptime = 480;
        scheduleTime = 480;
        ratedSpeed = 10.2;
        bandcastWidth = 1.09;
        gsm = 0.220;
        calendarTime = 480;
      }

      const availability = uptime / scheduleTime;
      const performance = (actual + waste) / (ratedSpeed * uptime * bandcastWidth * gsm);
      const utilization = scheduleTime / calendarTime;
      const quality = actual / (actual + waste);
      const oee = availability * performance * quality * 100;
      const perShiftQty = ratedSpeed * bandcastWidth * gsm * uptime;

      document.getElementById('result').innerHTML =
        `Availability: ${(availability * 100).toFixed(2)}%<br>` +
        `Performance: ${(performance * 100).toFixed(2)}%<br>` +
        `Utilization: ${(utilization * 100).toFixed(2)}%<br>` +
        `Quality: ${(quality * 100).toFixed(2)}%<br>` +
        `Theoretical yield: ${perShiftQty.toFixed(2)}<br>` +
        `<strong>OEE: ${oee.toFixed(2)}%</strong>`;
    }
  </script>
</body>
</html>
