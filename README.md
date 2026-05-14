<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>쿠키 주식 시뮬레이션</title>
    <style>
        body { font-family: 'Malgun Gothic', sans-serif; background: #f0f2f5; margin: 0; padding: 20px; }
        #game-container { max-width: 1000px; margin: 0 auto; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        .header { display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #333; padding-bottom: 10px; }
        .stock-list { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0; }
        .stock-item { border: 1px solid #ddd; padding: 15px; border-radius: 8px; background: #fafafa; }
        .info-section { margin-top: 20px; padding: 15px; background: #eef1f7; border-radius: 8px; }
        .news-box { height: 100px; border: 1px solid #ccc; margin-top: 10px; padding: 10px; background: #fff; overflow-y: auto; }
        .btn { padding: 8px 15px; cursor: pointer; border: none; border-radius: 4px; transition: 0.2s; }
        .btn-buy { background: #e74c3c; color: white; }
        .btn-sell { background: #3498db; color: white; }
        .btn-next { background: #2ecc71; color: white; width: 100%; font-size: 1.2em; margin-top: 20px; }
        .info-btn { background: #f39c12; color: white; margin-bottom: 5px; display: block; width: 100%; text-align: left; }
    </style>
</head>
<body>

<div id="game-container">
    <div class="header">
        <h1>🍪 쿠키 주식 시장</h1>
        <div>
            <strong>현재 회차: <span id="turn">1</span> / 7</strong><br>
            <strong>보유 자산: <span id="money">100,000</span>원</strong>
        </div>
    </div>

    <div class="stock-list">
        <!-- 종목: 뮤직엔터 -->
        <div class="stock-item">
            <h3>뮤직엔터 (c1)</h3>
            <p>현재가: <span id="c1-price">10,000</span>원</p>
            <p>보유수량: <span id="c1-count">0</span></p>
            <button class="btn btn-buy" onclick="buyStock('c1')">매수</button>
            <button class="btn btn-sell" onclick="sellStock('c1')">매도</button>
        </div>
        <!-- 종목: 포춘시스터즈 -->
        <div class="stock-item">
            <h3>포춘시스터즈 (c2)</h3>
            <p>현재가: <span id="c2-price">10,000</span>원</p>
            <p>보유수량: <span id="c2-count">0</span></p>
            <button class="btn btn-buy" onclick="buyStock('c2')">매수</button>
            <button class="btn btn-sell" onclick="sellStock('c2')">매도</button>
        </div>
        <!-- 종목: 연금전자 -->
        <div class="stock-item">
            <h3>연금전자 (c3)</h3>
            <p>현재가: <span id="c3-price">10,000</span>원</p>
            <p>보유수량: <span id="c3-count">0</span></p>
            <button class="btn btn-buy" onclick="buyStock('c3')">매수</button>
            <button class="btn btn-sell" onclick="sellStock('c3')">매도</button>
        </div>
        <!-- 종목: 마카로즈 -->
        <div class="stock-item">
            <h3>마카로즈 (c4)</h3>
            <p>현재가: <span id="c4-price">10,000</span>원</p>
            <p>보유수량: <span id="c4-count">0</span></p>
            <button class="btn btn-buy" onclick="buyStock('c4')">매수</button>
            <button class="btn btn-sell" onclick="sellStock('c4')">매도</button>
        </div>
    </div>

    <div class="info-section">
        <h3>📂 획득한 기밀 정보 (GitHub Data)</h3>
        <div id="info-list">
            <!-- 회차별로 해금될 정보 버튼들 -->
        </div>
    </div>

    <div class="news-box" id="news-box">
        [시스템] 게임이 시작되었습니다. 정보를 구매하고 시장의 흐름을 읽으세요.
    </div>

    <button class="btn btn-next" onclick="nextTurn()">다음 회차로 넘기기</button>
</div>

<script>
    let turn = 1;
    let money = 100000;
    let stocks = { c1: 10000, c2: 10000, c3: 10000, c4: 10000 };
    let myStocks = { c1: 0, c2: 0, c3: 0, c4: 0 };

    // 깃허브 정보 파일 리스트
    const infoFiles = [
        "HBS 기사 셈플.pdf",
        "직비모 캡쳐 파일.pdf",
        "포춘쿠키런 카페 유저들 대화.pdf",
        "쿠키팝 팬들 커뮤니티.pdf",
        "제목.pdf",
        "마카로즈 기사 (폐기하세요).pdf",
        "쿠스패치 단독 기사.pdf"
    ];

    function updateUI() {
        document.getElementById('turn').innerText = turn;
        document.getElementById('money').innerText = money.toLocaleString();
        for(let key in stocks) {
            document.getElementById(`${key}-price`).innerText = stocks[key].toLocaleString();
            document.getElementById(`${key}-count`).innerText = myStocks[key];
        }
    }

    function buyStock(c) {
        if (money >= stocks[c]) {
            money -= stocks[c];
            myStocks[c]++;
            updateUI();
        } else {
            alert("잔액이 부족합니다!");
        }
    }

    function sellStock(c) {
        if (myStocks[c] > 0) {
            money += stocks[c];
            myStocks[c]--;
            updateUI();
        }
    }

    function openInfo(index) {
        const githubPath = `https://github.com/가람계정/저장소/raw/main/resources/data/${infoFiles[index]}`;
        window.open(githubPath, '_blank');
    }

    function nextTurn() {
        if (turn >= 7) {
            alert("최종 결과: " + (money + (myStocks.c1 * stocks.c1) + (myStocks.c2 * stocks.c2)).toLocaleString() + "원!");
            return;
        }

        // 여기에 가람님이 설계하신 r1~r28 로직을 반영 (예시로 주가 변동만 추가)
        turn++;
        stocks.c1 += Math.floor(Math.random() * 2000) - 1000; // 시나리오 반영 구간
        stocks.c2 += Math.floor(Math.random() * 2000) - 1000;
        
        // 정보 해금
        const infoList = document.getElementById('info-list');
        const newBtn = document.createElement('button');
        newBtn.className = "btn info-btn";
        newBtn.innerText = `📄 정보 ${turn}: ${infoFiles[turn-1]}`;
        newBtn.onclick = () => openInfo(turn-1);
        infoList.appendChild(newBtn);

        document.getElementById('news-box').innerText = `[${turn}회차 뉴스] 시장 상황이 변동되었습니다. 신규 정보를 확인하세요.`;
        updateUI();
    }

    // 초기 세팅
    updateUI();
</script>

</body>
</html>
