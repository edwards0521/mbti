<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MBTI로 포켓몬 찾기</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: "Arial", "Noto Sans KR", sans-serif;
      background: #fff7df;
      color: #4d3b20;
    }

    .container {
      width: min(920px, 92%);
      margin: 0 auto;
      padding: 45px 0 60px;
    }

    header {
      text-align: center;
      margin-bottom: 35px;
    }

    .title {
      margin: 0;
      font-size: clamp(2.2rem, 6vw, 4rem);
      color: #6b4b20;
      letter-spacing: -2px;
    }

    .subtitle {
      margin: 12px 0 0;
      font-size: clamp(1.05rem, 2.5vw, 1.35rem);
      color: #8a6b38;
    }

    .card {
      background: #fffdf5;
      border: 3px solid #f2d889;
      border-radius: 28px;
      padding: 30px;
      box-shadow: 0 10px 25px rgba(126, 92, 30, 0.1);
    }

    .question {
      text-align: center;
      font-size: 1.6rem;
      font-weight: bold;
      margin: 0 0 22px;
    }

    .mbti-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
    }

    .mbti-button {
      border: 2px solid #e9ca70;
      background: #fff8d9;
      color: #5f481f;
      border-radius: 16px;
      padding: 17px 8px;
      font-size: 1.15rem;
      font-weight: bold;
      cursor: pointer;
      transition: 0.18s ease;
    }

    .mbti-button:hover {
      background: #ffeaa9;
      transform: translateY(-2px);
    }

    .mbti-button:focus-visible {
      outline: 4px solid #f3c84b;
      outline-offset: 2px;
    }

    .result {
      display: none;
      text-align: center;
    }

    .result.show {
      display: block;
    }

    .selected-type {
      display: inline-block;
      background: #f5d66d;
      color: #5c4319;
      border-radius: 999px;
      padding: 8px 18px;
      font-size: 1.1rem;
      font-weight: bold;
      margin-bottom: 12px;
    }

    .pokemon-emoji {
      font-size: clamp(6rem, 20vw, 10rem);
      line-height: 1;
      margin: 10px 0;
    }

    .pokemon-name {
      font-size: clamp(2rem, 6vw, 3.2rem);
      margin: 8px 0;
      color: #684718;
    }

    .reason {
      max-width: 620px;
      margin: 15px auto 25px;
      font-size: 1.15rem;
      line-height: 1.8;
      color: #695638;
    }

    .compatibility {
      background: #fff3c2;
      border-radius: 20px;
      padding: 20px;
      margin: 20px auto;
      max-width: 620px;
    }

    .compatibility-title {
      font-size: 1rem;
      color: #98742c;
      font-weight: bold;
      margin-bottom: 6px;
    }

    .compatibility-type {
      font-size: 1.5rem;
      font-weight: bold;
      color: #604719;
    }

    .restart {
      margin-top: 18px;
      border: none;
      background: #e9b83f;
      color: #fffdf5;
      border-radius: 16px;
      padding: 15px 30px;
      font-size: 1.1rem;
      font-weight: bold;
      cursor: pointer;
      box-shadow: 0 5px 0 #c59428;
    }

    .restart:hover {
      background: #dcae35;
    }

    .restart:active {
      transform: translateY(3px);
      box-shadow: 0 2px 0 #c59428;
    }

    @media (max-width: 650px) {
      .container {
        padding-top: 30px;
      }

      .card {
        padding: 22px 16px;
      }

      .mbti-grid {
        grid-template-columns: repeat(2, 1fr);
      }

      .mbti-button {
        padding: 16px 5px;
      }
    }
  </style>
</head>

<body>
  <main class="container">
    <header>
      <h1 class="title">⚡ MBTI로 포켓몬 찾기</h1>
      <p class="subtitle">나와 어울리는 포켓몬은 누구일까?</p>
    </header>

    <section class="card">
      <div id="selection">
        <h2 class="question">나의 MBTI를 골라주세요!</h2>

        <div class="mbti-grid">
          <button class="mbti-button" data-type="ISTJ">ISTJ</button>
          <button class="mbti-button" data-type="ISFJ">ISFJ</button>
          <button class="mbti-button" data-type="INFJ">INFJ</button>
          <button class="mbti-button" data-type="INTJ">INTJ</button>

          <button class="mbti-button" data-type="ISTP">ISTP</button>
          <button class="mbti-button" data-type="ISFP">ISFP</button>
          <button class="mbti-button" data-type="INFP">INFP</button>
          <button class="mbti-button" data-type="INTP">INTP</button>

          <button class="mbti-button" data-type="ESTP">ESTP</button>
          <button class="mbti-button" data-type="ESFP">ESFP</button>
          <button class="mbti-button" data-type="ENFP">ENFP</button>
          <button class="mbti-button" data-type="ENTP">ENTP</button>

          <button class="mbti-button" data-type="ESTJ">ESTJ</button>
          <button class="mbti-button" data-type="ESFJ">ESFJ</button>
          <button class="mbti-button" data-type="ENFJ">ENFJ</button>
          <button class="mbti-button" data-type="ENTJ">ENTJ</button>
        </div>
      </div>

      <div id="result" class="result">
        <div id="selectedType" class="selected-type"></div>

        <div id="pokemonEmoji" class="pokemon-emoji"></div>

        <h2 id="pokemonName" class="pokemon-name"></h2>

        <p id="reason" class="reason"></p>

        <div class="compatibility">
          <div class="compatibility-title">✨ 잘 맞는 다른 유형</div>
          <div id="compatibilityType" class="compatibility-type"></div>
        </div>

        <button id="restartButton" class="restart">
          ↩ 다시 고르기
        </button>
      </div>
    </section>
  </main>

  <script>
    const pokemonData = {
      ISTJ: {
        name: "꼬부기",
        emoji: "🐢",
        reason:
          "차분하고 책임감이 강한 ISTJ처럼 꼬부기는 꾸준하고 안정적인 느낌이 있어요.<br>천천히 가더라도 자기 페이스를 지키는 모습이 잘 어울려요.",
        compatible: "ESFP"
      },

      ISFJ: {
        name: "해피너스",
        emoji: "🥚",
        reason:
          "따뜻하고 다른 사람을 잘 챙기는 ISFJ와 해피너스는 정말 잘 어울려요.<br>주변 사람들에게 편안함과 든든함을 주는 점이 닮았어요.",
        compatible: "ENTP"
      },

      INFJ: {
        name: "가디안",
        emoji: "🧚",
        reason:
          "깊은 생각과 따뜻한 마음을 가진 INFJ에게 가디안의 신비로운 분위기가 잘 맞아요.<br>소중한 사람을 지켜주려는 모습도 닮았어요.",
        compatible: "ESTP"
      },

      INTJ: {
        name: "메타그로스",
        emoji: "🤖",
        reason:
          "논리적이고 전략적인 INTJ와 메타그로스의 뛰어난 분석력은 찰떡궁합이에요.<br>복잡한 상황에서도 차분하게 해결책을 찾는 모습이 닮았어요.",
        compatible: "ENFP"
      },

      ISTP: {
        name: "리자몽",
        emoji: "🔥",
        reason:
          "독립적이고 행동력이 좋은 ISTP에게 리자몽의 자유로운 에너지가 잘 어울려요.<br>필요할 때 직접 움직여 문제를 해결하는 모습이 멋져요.",
        compatible: "ENFJ"
      },

      ISFP: {
        name: "이브이",
        emoji: "🦊",
        reason:
          "부드럽고 자유로운 ISFP와 다양한 가능성을 가진 이브이는 닮은 점이 많아요.<br>자신만의 취향과 분위기를 소중히 여기는 점도 잘 어울려요.",
        compatible: "ENTJ"
      },

      INFP: {
        name: "님피아",
        emoji: "🎀",
        reason:
          "감성이 풍부하고 상상력이 뛰어난 INFP에게 님피아의 다정한 분위기가 잘 맞아요.<br>자신이 중요하게 생각하는 것을 소중히 지키는 모습도 닮았어요.",
        compatible: "ESTJ"
      },

      INTP: {
        name: "폴리곤2",
        emoji: "💻",
        reason:
          "호기심 많고 분석하는 것을 좋아하는 INTP와 폴리곤2의 독특한 느낌이 잘 어울려요.<br>새로운 것을 탐구하고 자신만의 방식으로 생각하는 점이 닮았어요.",
        compatible: "ESFJ"
      },

      ESTP: {
        name: "루카리오",
        emoji: "🥋",
        reason:
          "빠른 판단과 행동력을 가진 ESTP에게 루카리오의 당당한 에너지가 잘 맞아요.<br>직접 경험하면서 배우고 도전하는 것을 좋아하는 점도 닮았어요.",
        compatible: "INFJ"
      },

      ESFP: {
        name: "피카츄",
        emoji: "⚡",
        reason:
          "밝고 활발하며 주변을 즐겁게 만드는 ESFP에게 피카츄만큼 잘 어울리는 포켓몬은 없죠!<br>친근하고 긍정적인 에너지로 모두에게 사랑받는 모습이 닮았어요.",
        compatible: "ISTJ"
      },

      ENFP: {
        name: "파치리스",
        emoji: "🐿️",
        reason:
          "호기심 많고 에너지가 넘치는 ENFP에게 파치리스의 통통 튀는 매력이 잘 어울려요.<br>새로운 것을 좋아하고 주변에 즐거움을 전하는 모습이 닮았어요.",
        compatible: "INTJ"
      },

      ENTP: {
        name: "팬텀",
        emoji: "👻",
        reason:
          "재치 있고 장난기 많으며 새로운 아이디어를 좋아하는 ENTP와 팬텀은 잘 맞아요.<br>예상하기 어려운 행동으로 분위기를 재미있게 만드는 점이 닮았어요.",
        compatible: "ISFJ"
      },

      ESTJ: {
        name: "거북왕",
        emoji: "🐢",
        reason:
          "목표를 향해 확실하게 나아가는 ESTJ에게 든든한 거북왕의 모습이 잘 어울려요.<br>책임감 있고 팀을 이끌어가는 강한 추진력이 닮았어요.",
        compatible: "INFP"
      },

      ESFJ: {
        name: "마릴",
        emoji: "💧",
        reason:
          "사람들과 함께하는 것을 좋아하고 친절한 ESFJ와 마릴의 귀여운 분위기가 잘 맞아요.<br>주변 사람들과 즐겁게 어울리는 따뜻한 모습이 닮았어요.",
        compatible: "INTP"
      },

      ENFJ: {
        name: "리자몽",
        emoji: "🔥",
        reason:
          "사람들을 이끌고 응원하는 ENFJ에게 강하고 든든한 리자몽이 잘 어울려요.<br>자신뿐 아니라 주변 사람의 성장까지 생각하는 모습이 멋져요.",
        compatible: "ISTP"
      },

      ENTJ: {
        name: "망나뇽",
        emoji: "🐉",
        reason:
          "목표가 뚜렷하고 추진력이 강한 ENTJ에게 망나뇽의 강력한 존재감이 잘 맞아요.<br>어려운 상황에서도 앞으로 나아가는 자신감이 닮았어요.",
        compatible: "ISFP"
      }
    };

    const buttons = document.querySelectorAll(".mbti-button");
    const selection = document.getElementById("selection");
    const result = document.getElementById("result");

    const selectedType = document.getElementById("selectedType");
    const pokemonEmoji = document.getElementById("pokemonEmoji");
    const pokemonName = document.getElementById("pokemonName");
    const reason = document.getElementById("reason");
    const compatibilityType = document.getElementById("compatibilityType");
    const restartButton = document.getElementById("restartButton");

    function showResult(type) {
      const data = pokemonData[type];

      selectedType.textContent = `나의 MBTI · ${type}`;
      pokemonEmoji.textContent = data.emoji;
      pokemonName.textContent = data.name;
      reason.innerHTML = data.reason;
      compatibilityType.textContent = data.compatible;

      selection.style.display = "none";
      result.classList.add("show");

      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    }

    buttons.forEach((button) => {
      button.addEventListener("click", () => {
        showResult(button.dataset.type);
      });
    });

    restartButton.addEventListener("click", () => {
      result.classList.remove("show");
      selection.style.display = "block";

      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    });
  </script>
</body>
</html>
