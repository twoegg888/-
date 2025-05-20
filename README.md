<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>주차권 계산기</title>
    <style>
        body {
            margin: 0;
            padding: 20px;
            font-family: sans-serif;
        }
        .parking-form {
            max-width: 400px;
            margin: 0 auto;
        }
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
        }
        .form-group input {
            width: 100%;
            padding: 8px;
            margin: 8px 0;
            box-sizing: border-box;
        }
        .ticket-input {
            width: 100px !important;
        }
        .total {
            font-weight: bold;
            font-size: 1.2em;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="parking-form">
        <h1>주차권 충전</h1>
        <div class="form-group">
            <label>업체명(입금자명) *</label>
            <input type="text" id="company" placeholder="심슨코퍼레이션">
        </div>
        <div class="form-group">
            <label>연락 가능한 전화번호 *</label>
            <input type="text" id="phone" placeholder="01048842588">
        </div>
        <div class="form-group">
            <label>30분 주차권 필요 수량</label><br>
            <input type="number" id="count30" class="ticket-input" min="0" value="0" onchange="updateTotal()" onkeyup="updateTotal()"> 1매 500원
        </div>
        <div class="form-group">
            <label>1시간 주차권 필요 수량</label><br>
            <input type="number" id="count60" class="ticket-input" min="0" value="0" onchange="updateTotal()" onkeyup="updateTotal()"> 1매 900원
        </div>
        <div class="form-group">
            <label>2시간 주차권 필요 수량</label><br>
            <input type="number" id="count120" class="ticket-input" min="0" value="0" onchange="updateTotal()" onkeyup="updateTotal()"> 1매 1500원
        </div>
        <hr>
        <div class="total">
            총 합계금: <span id="total">0</span>원
        </div>
    </div>

    <script>
        function updateTotal() {
            var count30 = parseInt(document.getElementById('count30').value) || 0;
            var count60 = parseInt(document.getElementById('count60').value) || 0;
            var count120 = parseInt(document.getElementById('count120').value) || 0;
            
            var total = (count30 * 500) + (count60 * 900) + (count120 * 1500);
            document.getElementById('total').textContent = total.toLocaleString();
        }

        // 페이지 로드 시 초기 계산
        window.onload = updateTotal;
    </script>
</body>
</html>
