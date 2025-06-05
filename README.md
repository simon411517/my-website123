<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <title>會考成績預估排名</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      background-color: #f3ece7;
      font-family: Arial, sans-serif;
      color: #5e4637;
      padding: 20px;
      text-align: center;
    }
    h1 {
      color: #8b6e5c;
      margin-bottom: 30px;
    }
    select, button {
      font-size: 18px;
      margin: 10px;
      padding: 12px 20px;
      border: 2px solid #d8c1b0;
      border-radius: 8px;
      width: 90%;
      max-width: 300px;
      box-sizing: border-box;
    }
    button {
      background-color: #d8c1b0;
      color: white;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: #c2a692;
    }
    #result {
      margin-top: 30px;
      font-size: 20px;
      font-weight: bold;
    }
    .card {
      background: #fff;
      border-radius: 16px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      padding: 30px;
      margin-top: 30px;
      display: inline-block;
      width: 90%;
      max-width: 400px;
    }
    @media (max-width: 600px) {
      select, button {
        font-size: 16px;
        padding: 10px;
      }
      .card {
        padding: 20px;
      }
    }
  </style>
</head>
<body>
  <h1>會考成績預估排名系統</h1>

  <div>
    <select id="chinese">
      <option value="">國文</option>
      <option value="A">A</option>
      <option value="B">B</option>
      <option value="C">C</option>
    </select>

    <select id="math">
      <option value="">數學</option>
      <option value="A">A</option>
      <option value="B">B</option>
      <option value="C">C</option>
    </select>

    <select id="english">
      <option value="">英文</option>
      <option value="A">A</option>
      <option value="B">B</option>
      <option value="C">C</option>
    </select>

    <select id="science">
      <option value="">理化</option>
      <option value="A">A</option>
      <option value="B">B</option>
      <option value="C">C</option>
    </select>

    <select id="social">
      <option value="">社會</option>
      <option value="A">A</option>
      <option value="B">B</option>
      <option value="C">C</option>
    </select>
  </div>

  <button onclick="calculateRank()">計算排名</button>

  <div id="result"></div>

  <script>
    const data = {
      "5A0B0C": 17569,
      "4A1B0C": 10355,
      "4A0B1C": 31,
      "3A2B0C": 10313,
      "3A1B1C": 90,
      "3A0B2C": 0,
      "2A3B0C": 13281,
      "2A2B1C": 338,
      "2A1B2C": 17,
      "2A0B3C": 0,
      "1A4B0C": 20787,
      "1A3B1C": 2008,
      "1A2B2C": 277,
      "1A1B3C": 62,
      "1A0B4C": 43
    };

    const rankingOrder = [
      "5A0B0C",
      "4A1B0C",
      "4A0B1C",
      "3A2B0C",
      "3A1B1C",
      "3A0B2C",
      "2A3B0C",
      "2A2B1C",
      "2A1B2C",
      "2A0B3C",
      "1A4B0C",
      "1A3B1C",
      "1A2B2C",
      "1A1B3C",
      "1A0B4C"
    ];

    const totalStudents = 74681;

    function calculateRank() {
      const scores = [
        document.getElementById('chinese').value,
        document.getElementById('math').value,
        document.getElementById('english').value,
        document.getElementById('science').value,
        document.getElementById('social').value
      ];

      if (scores.includes("")) {
        document.getElementById('result').innerHTML = `<div class="card">請完整選擇每一科成績</div>`;
        return;
      }

      let countA = scores.filter(s => s === 'A').length;
      let countB = scores.filter(s => s === 'B').length;
      let countC = scores.filter(s => s === 'C').length;

      const category = `${countA}A${countB}B${countC}C`;

      if (data[category] === undefined || data[category] === 0) {
        document.getElementById('result').innerHTML = `<div class="card">查無資料，可能無人考到此成績組合。</div>`;
        return;
      }

      let betterCount = 0;
      for (let cat of rankingOrder) {
        if (cat === category) break;
        betterCount += data[cat];
      }

      let rank = betterCount + 1;
      let percentile = (rank / totalStudents * 100).toFixed(2);

      document.getElementById('result').innerHTML = 
        `<div class="card">
          你的成績類別是：<b>${category}</b><br><br>
          預估排名：約第 <b>${rank} / ${totalStudents}</b> 名<br><br>
          預估超越約 <b>${(100 - percentile).toFixed(2)}%</b> 的考生
        </div>`;

      document.getElementById('chinese').value = "";
      document.getElementById('math').value = "";
      document.getElementById('english').value = "";
      document.getElementById('science').value = "";
      document.getElementById('social').value = "";
    }
  </script>
</body>
</html>
