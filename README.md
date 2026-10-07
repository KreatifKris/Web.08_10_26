
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Happy Birthday Abigail!</title>
  <style>
    :root {
      --bg-color: #fdf2f8;
      --card-bg: #ffffff;
      --primary: #ec4899;
      --primary-hover: #db2777;
      --text: #374151;
    }

    body {
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--bg-color);
      color: var(--text);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .container {
      width: 90%;
      max-width: 480px;
      background: var(--card-bg);
      padding: 2rem;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
      text-align: center;
      box-sizing: border-box;
    }

    .page { display: none; }
    .page.active {
      display: block;
      animation: fadeIn 0.4s ease-in-out;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(8px); }
      to { opacity: 1; transform: translateY(0); }
    }

    h1, h2 { color: var(--primary); margin-bottom: 1rem; }
    p { line-height: 1.5; }
    .form-group { text-align: left; margin-bottom: 1.2rem; }
    label { display: block; font-weight: 600; margin-bottom: 0.4rem; font-size: 0.95rem; }
    
    input, textarea {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid #d1d5db;
      border-radius: 8px;
      box-sizing: border-box;
      font-size: 0.95rem;
      font-family: inherit;
    }

    input:focus, textarea:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(236, 72, 153, 0.2);
    }

    .btn {
      background-color: var(--primary);
      color: white;
      border: none;
      padding: 12px 24px;
      font-size: 1rem;
      font-weight: 600;
      border-radius: 8px;
      cursor: pointer;
      width: 100%;
      transition: background-color 0.2s, opacity 0.2s;
    }

    .btn:hover { background-color: var(--primary-hover); }
    .btn:disabled { opacity: 0.6; cursor: not-allowed; }

    .profile-img {
      width: 150px;
      height: 150px;
      border-radius: 50%;
      object-fit: cover;
      border: 4px solid var(--primary);
      margin-bottom: 1rem;
      box-shadow: 0 4px 12px rgba(236, 72, 153, 0.3);
    }

    .wish-box {
      background: #fbcfe8;
      padding: 1.2rem;
      border-radius: 12px;
      margin-top: 1.2rem;
      font-style: italic;
      color: #831843;
      text-align: left;
    }

    .wish-box strong {
      display: block;
      margin-bottom: 0.3rem;
      font-style: normal;
    }
  </style>
</head>
<body>

  <div class="container">
    <!-- HALAMAN 1: PERTANYAAN -->
    <div id="page1" class="page active">
      <h1>Halo Abigail! ✨</h1>
      <p>Sebelum masuk ke ucapan utamanya, isi beberapa pertanyaan singkat ini dulu ya!</p>

      <form id="quizForm">
        <div class="form-group">
          <label for="nickname">Panggilan favoritmu:</label>
          <input type="text" id="nickname" required placeholder="Contoh: Abi / Gail">
        </div>

        <div class="form-group">
          <label for="memory">Momen paling seru tahun ini:</label>
          <input type="text" id="memory" required placeholder="Tulis cerita singkatnya...">
        </div>

        <div class="form-group">
          <label for="wish">Harapan & wish kamu di umur yang baru:</label>
          <textarea id="wish" rows="3" required placeholder="Apa harapan terbesarmu?"></textarea>
        </div>

        <button type="submit" class="btn" id="nextBtn">Next ➔</button>
      </form>
    </div>

    <!-- HALAMAN 2: UCAPAN & FOTO -->
    <div id="page2" class="page">
      <img src="abigail.jpg" alt="Abigail" class="profile-img">
      <h2>Selamat Ulang Tahun, Abigail! 🎉🎉</h2>
      <p>Semoga di usiamu yang baru ini selalu diberikan kebahagiaan, kesehatan, dan kelancaran dalam segala hal yang kamu impikan!</p>

      <div class="wish-box">
        <strong>Harapan yang Kamu Tulis:</strong>
        <p id="displayWish" style="margin: 0;">"..."</p>
      </div>
    </div>
  </div>

  <script>
    // URL Realtime Database milikmu
    const DATABASE_URL = "https://abby-5979a-default-rtdb.firebaseio.com";

    const quizForm = document.getElementById('quizForm');
    const nextBtn = document.getElementById('nextBtn');
    const page1 = document.getElementById('page1');
    const page2 = document.getElementById('page2');
    const displayWish = document.getElementById('displayWish');

    quizForm.addEventListener('submit', async (e) => {
      e.preventDefault();

      const nickname = document.getElementById('nickname').value;
      const memory = document.getElementById('memory').value;
      const wish = document.getElementById('wish').value;

      nextBtn.disabled = true;
      nextBtn.innerText = "Sabar ya...";

      // Endpoint REST API Realtime Database (menambah endpoint /responses.json)
      const endpoint = `${DATABASE_URL}/responses.json`;

      const payload = {
        name: "Abigail",
        nickname: nickname,
        memory: memory,
        wish: wish,
        submittedAt: new Date().toISOString()
      };

      try {
        const response = await fetch(endpoint, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
        });

        if (!response.ok) {
          throw new Error("Gagal menyimpan ke database.");
        }

        displayWish.innerText = `"${wish}"`;

        page1.classList.remove('active');
        page2.classList.add('active');

      } catch (error) {
        console.error("Firebase Error:", error);
        alert("Gagal terhubung ke Firebase. Pastikan Aturan (Rules) Realtime Database kamu sudah diubah menjadi true.");
        
        nextBtn.disabled = false;
        nextBtn.innerText = "Next ➔";
      }
    });
  </script>
</body>

