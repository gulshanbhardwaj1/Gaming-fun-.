     <h1><font size="150">Gaming & fun.com</font></h1>
    
    
    <!DOCTYPE html>
<html lang="hi">
<head><meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
  body {
    margin: 0;
    padding: 0;
  }
  .container {
    width: 100%;
    max-width: 400px;  /* मोबाइल स्क्रीन चौड़ाई */
    margin: 0 auto;
    height: 100vh;     /* पूरी स्क्रीन की ऊँचाई */
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
</style>
  <meta charset="UTF-8" />
  <title>गणित गेम्स - जोड़/घटाव और गुणा/भाग</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg,#f7fbff,#e8f7ec);
      padding: 30px;
      text-align: center;
    }
    .container {
      max-width: 760px;
      margin: 0 auto;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 6px 20px rgba(0,0,0,0.08);
    }
    h1 { color: #1f6feb; margin-bottom: 6px; }
    .menu { margin: 15px 0; }
    .menu button {
      margin: 6px;
      padding: 10px 16px;
      border-radius: 8px;
      border: none;
      cursor: pointer;
      background: #2e8b57;
      color: white;
      font-weight: bold;
    }
    .menu button.alt { background: #ff8c00; }
    .menu button.selected { box-shadow: 0 4px 10px rgba(0,0,0,0.12); transform: translateY(-2px); }
    .game {
      display: none;
      margin-top: 20px;
      text-align: center;
    }
    .game.active { display: block; }
    .card {
      background: #fafafa;
      padding: 18px;
      border-radius: 10px;
      margin: 10px auto;
      display: inline-block;
      min-width: 300px;
    }
    .question {
      font-size: 26px;
      margin-bottom: 10px;
    }
    input[type="number"] {
      padding: 8px;
      font-size: 18px;
      width: 120px;
      text-align: center;
      border-radius: 8px;
      border: 1px solid #ccc;
    }
    .controls button {
      padding: 10px 14px;
      margin: 8px;
      border-radius: 8px;
      border: none;
      cursor: pointer;
      background: #1f6feb;
      color: white;
    }
    .info { margin-top: 10px; font-weight: bold; }
    .correct { color: green; }
    .wrong { color: red; }
    .small { font-size: 14px; color: #555; margin-top: 6px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>MATHS QUIZE GAME 🎮</h1>
    <p class="small">नीचे से गेम चुनो — जोड़/घटाव या गुणा/भाग। हर सही उत्तर पर स्कोर बढ़ेगा।</p>

    <div class="menu">
      <button id="btnAddSub" class="selected">जोड़ / घटाव</button>
      <button id="btnMulDiv" class="alt">गुणा / भाग</button>
    </div>

    <!-- Addition / Subtraction Game -->
    <div id="addsub" class="game active">
      <div class="card">
        <div class="question" id="asQuestion">?</div>

        <div>
          <label><input type="radio" name="asOp" value="add" checked> जोड़ (+) </label>
          <label style="margin-left:12px;"><input type="radio" name="asOp" value="sub"> घटाव (−) </label>
        </div>

        <div style="margin-top:12px;">
          <input id="asAnswer" type="number" placeholder="अपना उत्तर">
        </div>

        <div class="controls">
          <button onclick="asCheck()">जाँचो</button>
          <button onclick="asNext()">अगला</button>
          <button onclick="asReset()">रीसेट स्कोर</button>
        </div>

        <div id="asResult" class="info"></div>
        <div id="asScore" class="small">स्कोर: 0 | सही: 0 | गलत: 0</div>
      </div>
    </div>

    <!-- Multiplication / Division Game -->
    <div id="muldiv" class="game">
      <div class="card">
        <div class="question" id="mdQuestion">?</div>

        <div>
          <label><input type="radio" name="mdOp" value="mul" checked> गुणा (×) </label>
          <label style="margin-left:12px;"><input type="radio" name="mdOp" value="div"> भाग (÷) </label>
        </div>

        <div style="margin-top:12px;">
          <input id="mdAnswer" type="number" placeholder="अपना उत्तर">
        </div>

        <div class="controls">
          <button onclick="mdCheck()">जाँचो</button>
          <button onclick="mdNext()">अगला</button>
          <button onclick="mdReset()">रीसेट स्कोर</button>
        </div>

        <div id="mdResult" class="info"></div>
        <div id="mdScore" class="small">स्कोर: 0 | सही: 0 | गलत: 0</div>
      </div>
    </div>

  </div>

  <script>
    // ---------- UI: टैब स्विच ----------
    const btnAddSub = document.getElementById('btnAddSub');
    const btnMulDiv = document.getElementById('btnMulDiv');
    const addsubDiv = document.getElementById('addsub');
    const muldivDiv = document.getElementById('muldiv');

    btnAddSub.addEventListener('click', () => {
      btnAddSub.classList.add('selected');
      btnMulDiv.classList.remove('selected');
      addsubDiv.classList.add('active');
      muldivDiv.classList.remove('active');
    });
    btnMulDiv.addEventListener('click', () => {
      btnMulDiv.classList.add('selected');
      btnAddSub.classList.remove('selected');
      muldivDiv.classList.add('active');
      addsubDiv.classList.remove('active');
    });

    // ---------- जोड़/घटाव गेम ----------
    let asCorrect = 0, asWrong = 0, asScoreVal = 0;
    let asCurrentAns = null;

    function asGenerate() {
      // छोटी संख्या: 1-50, बड़ी संख्या: 1-50
      let a = Math.floor(Math.random()*50) + 1;
      let b = Math.floor(Math.random()*50) + 1;
      let op = document.querySelector('input[name="asOp"]:checked').value;
      let qText;
      if (op === 'add') {
        asCurrentAns = a + b;
        qText = `${a} + ${b} = ?`;
      } else {
        // घटाव में हमेशा non-negative रखें — बड़े से छोटे को घटाएँ
        if (a < b) [a,b] = [b,a];
        asCurrentAns = a - b;
        qText = `${a} − ${b} = ?`;
      }
      document.getElementById('asQuestion').textContent = qText;
      document.getElementById('asAnswer').value = '';
      document.getElementById('asResult').textContent = '';
    }

    function asCheck() {
      let val = Number(document.getElementById('asAnswer').value);
      if (isNaN(val) || document.getElementById('asAnswer').value === '') {
        document.getElementById('asResult').textContent = 'कृपया कोई संख्या डालो।';
        document.getElementById('asResult').className = 'info wrong';
        return;
      }
      if (val === asCurrentAns) {
        asCorrect++;
        asScoreVal += 10; // हर सही पर 10 अंक
        document.getElementById('asResult').textContent = 'बढ़िया! सही उत्तर ✅';
        document.getElementById('asResult').className = 'info correct';
      } else {
        asWrong++;
        asScoreVal -= 2; // गलत पर -2
        document.getElementById('asResult').textContent = `गलत। सही उत्तर: ${asCurrentAns}`;
        document.getElementById('asResult').className = 'info wrong';
      }
      updateAsScore();
    }

    function asNext() { asGenerate(); }
    function asReset() {
      asCorrect = asWrong = asScoreVal = 0;
      updateAsScore();
      asGenerate();
      document.getElementById('asResult').textContent = '';
    }
    function updateAsScore() {
      document.getElementById('asScore').textContent = `स्कोर: ${asScoreVal} | सही: ${asCorrect} | गलत: ${asWrong}`;
    }

    // आरम्भ में पहली क्वेश्चन बनाओ
    asGenerate();

    // जब ऑपरेशन बदले तो नया प्रश्न बनाओ
    document.querySelectorAll('input[name="asOp"]').forEach(r => {
      r.addEventListener('change', asGenerate);
    });

    // ---------- गुणा/भाग गेम ----------
    let mdCorrect = 0, mdWrong = 0, mdScoreVal = 0;
    let mdCurrentAns = null;

    function mdGenerate() {
      let op = document.querySelector('input[name="mdOp"]:checked').value;
      if (op === 'mul') {
        // छोटे गुणन के लिए 2-12 तक
        let a = Math.floor(Math.random()*11) + 2;
        let b = Math.floor(Math.random()*11) + 2;
        mdCurrentAns = a * b;
        document.getElementById('mdQuestion').textContent = `${a} × ${b} = ?`;
      } else {
        // division: integer result के लिए पहले गुणा करो फिर भाग दो
        let a = Math.floor(Math.random()*11) + 2;
        let b = Math.floor(Math.random()*11) + 2;
        let prod = a * b;
        // सवाल रूप में prod ÷ a = ?
        mdCurrentAns = b; // prod / a = b
        document.getElementById('mdQuestion').textContent = `${prod} ÷ ${a} = ?`;
      }
      document.getElementById('mdAnswer').value = '';
      document.getElementById('mdResult').textContent = '';
    }

    function mdCheck() {
      let val = Number(document.getElementById('mdAnswer').value);
      if (isNaN(val) || document.getElementById('mdAnswer').value === '') {
        document.getElementById('mdResult').textContent = 'कृपया कोई संख्या डालो।';
        document.getElementById('mdResult').className = 'info wrong';
        return;
      }
      if (val === mdCurrentAns) {
        mdCorrect++;
        mdScoreVal += 10;
        document.getElementById('mdResult').textContent = 'शाबाश! सही उत्तर ✅';
        document.getElementById('mdResult').className = 'info correct';
      } else {
        mdWrong++;
        mdScoreVal -= 2;
        document.getElementById('mdResult').textContent = `गलत। सही उत्तर: ${mdCurrentAns}`;
        document.getElementById('mdResult').className = 'info wrong';
      }
      updateMdScore();
    }

    function mdNext() { mdGenerate(); }
    function mdReset() {
      mdCorrect = mdWrong = mdScoreVal = 0;
      updateMdScore();
      mdGenerate();
      document.getElementById('mdResult').textContent = '';
    }
    function updateMdScore() {
      document.getElementById('mdScore').textContent = `स्कोर: ${mdScoreVal} | सही: ${mdCorrect} | गलत: ${mdWrong}`;
    }

    mdGenerate();
    document.querySelectorAll('input[name="mdOp"]').forEach(r => {
      r.addEventListener('change', mdGenerate);
    });

    // कीबोर्ड से Enter दबाकर चेक करने की सुविधा
    document.getElementById('asAnswer').addEventListener('keydown', function(e){
      if (e.key === 'Enter') asCheck();
    });
    document.getElementById('mdAnswer').addEventListener('keydown', function(e){
      if (e.key === 'Enter') mdCheck();
    });

  </script>
</body>
</html>
