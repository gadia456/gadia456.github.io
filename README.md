# gadia456.github.io
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Tracking Link Manager - Location Tracker</title>
  <link
    href="https://fonts.googleapis.com/css2?family=Material+Icons"
    rel="stylesheet"
  />
  <style>
    /* Reset & base */
    *, *::before, *::after {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont,
        'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans',
        'Helvetica Neue', sans-serif;
      background: linear-gradient(135deg, #ffffff, #f3f4f6);
      color: #111827;
      min-height: 100vh;
      display: grid;
      grid-template-rows: 64px 1fr auto;
      grid-template-columns: 280px 1fr;
      grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
      transition: background-color 0.3s ease;
    }
    /* Dark mode support */
    body.dark {
      background: linear-gradient(135deg, #1f2937, #111827);
      color: #d1d5db;
    }

    /* Header */
    header {
      grid-area: header;
      background: rgba(255 255 255 / 0.75);
      backdrop-filter: saturate(180%) blur(12px);
      border-bottom: 1px solid #e5e7eb;
      height: 64px;
      display: flex;
      align-items: center;
      padding: 0 24px;
      position: sticky;
      top: 0;
      z-index: 10;
      box-shadow: 0 1px 10px rgb(0 0 0 / 0.05);
      transition: background-color 0.3s ease, border-color 0.3s ease;
    }
    body.dark header {
      background: rgba(17 24 39 / 0.9);
      border-color: #374151;
      box-shadow: 0 1px 10px rgb(0 0 0 / 0.6);
    }
    header .brand {
      font-weight: 700;
      font-size: clamp(1.25rem, 2vw, 1.75rem);
      background: linear-gradient(135deg, #6366f1, #06b6d4);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      user-select: none;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    header .brand .material-icons {
      font-size: 28px;
    }
    header .theme-toggle {
      margin-left: auto;
      background: none;
      border: none;
      cursor: pointer;
      font-size: 24px;
      padding: 8px;
      border-radius: 12px;
      color: inherit;
      transition: background-color 0.2s ease;
    }
    header .theme-toggle:focus-visible {
      outline: 2px solid #6366f1;
      outline-offset: 2px;
    }
    header .theme-toggle:hover {
      background-color: rgba(99 102 241 / 0.1);
    }
    body.dark header .theme-toggle:hover {
      background-color: rgba(6 182 212 / 0.2);
    }

    /* Sidebar */
    aside.sidebar {
      grid-area: sidebar;
      background: rgba(243 244 246 / 0.9);
      backdrop-filter: saturate(180%) blur(12px);
      border-right: 1px solid #e5e7eb;
      padding: 24px 16px 32px;
      display: flex;
      flex-direction: column;
      gap: 24px;
      position: sticky;
      top: 64px;
      height: calc(100vh - 64px);
      overflow-y: auto;
      transition: background-color 0.3s ease, border-color 0.3s ease;
    }
    body.dark aside.sidebar {
      background: rgba(31 41 55 / 0.9);
      border-color: #374151;
    }
    aside.sidebar nav.navigation {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }
    aside.sidebar nav.navigation button.nav-button {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px 16px;
      font-size: 1rem;
      border: none;
      background: none;
      color: inherit;
      cursor: pointer;
      border-radius: 12px;
      transition:
        background-color 0.25s ease,
        box-shadow 0.25s ease;
      user-select: none;
    }
    aside.sidebar nav.navigation button.nav-button:focus-visible {
      outline: 2px solid #6366f1;
      outline-offset: 2px;
    }
    aside.sidebar nav.navigation button.nav-button:hover {
      background: rgba(99 102 241 / 0.1);
      box-shadow: 0 4px 10px rgb(99 102 241 / 0.15);
    }
    body.dark aside.sidebar nav.navigation button.nav-button:hover {
      background: rgba(6 182 212 / 0.2);
      box-shadow: 0 4px 10px rgb(6 182 212 / 0.3);
    }
    aside.sidebar nav.navigation button.nav-button .material-icons {
      font-size: 24px;
      flex-shrink: 0;
    }

    /* Main Content */
    main.main {
      grid-area: main;
      padding: 32px 48px;
      min-height: calc(100vh - 64px);
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 32px;
      scroll-behavior: smooth;
    }
    body.dark main.main {
      color: #d1d5db;
    }

    /* Section cards */
    section.card {
      background: rgba(255 255 255 / 0.8);
      backdrop-filter: saturate(180%) blur(12px);
      border-radius: 16px;
      padding: 32px;
      box-shadow: 0 6px 15px rgba(0,0,0,0.06);
      transition: background-color 0.3s ease;
    }
    body.dark section.card {
      background: rgba(31 41 55 / 0.85);
      box-shadow: 0 6px 15px rgba(0,0,0,0.4);
    }
    section.card h2 {
      margin-top: 0;
      font-size: clamp(1.5rem, 2vw, 2rem);
      margin-bottom: 24px;
      font-weight: 700;
      color: inherit;
    }

    /* Form */
    form#linkForm {
      display: flex;
      flex-direction: column;
      gap: 16px;
      max-width: 480px;
    }
    form#linkForm label {
      font-weight: 600;
      margin-bottom: 6px;
    }
    form#linkForm input[type="text"] {
      padding: 12px 16px;
      font-size: 1rem;
      border-radius: 12px;
      border: 1px solid #d1d5db;
      transition: border-color 0.3s ease;
      background: #fff;
      color: #111827;
      font-family: inherit;
    }
    form#linkForm input[type="text"]:focus {
      outline: none;
      border-color: #6366f1;
      box-shadow: 0 0 8px rgba(99,102,241,0.4);
    }
    body.dark form#linkForm input[type="text"] {
      background: #1f2937;
      border-color: #374151;
      color: #d1d5db;
    }
    body.dark form#linkForm input[type="text"]:focus {
      border-color: #06b6d4;
      box-shadow: 0 0 8px rgba(6,182,212,0.6);
    }
    form#linkForm button {
      background: linear-gradient(135deg, #6366f1, #06b6d4);
      border: none;
      color: white;
      font-weight: 700;
      font-size: 1rem;
      padding: 14px 0;
      border-radius: 16px;
      cursor: pointer;
      box-shadow: 0 10px 20px rgba(99,102,241, 0.3);
      transition: box-shadow 0.3s ease, transform 0.3s ease;
    }
    form#linkForm button:focus-visible {
      outline: 2px solid #e0e7ff;
      outline-offset: 3px;
    }
    form#linkForm button:hover {
      box-shadow: 0 20px 40px rgba(99,102,241, 0.5);
      transform: translateY(-2px);
    }

    /* Links list */
    ul#linksList {
      list-style: none;
      padding: 0;
      margin: 0;
      display: flex;
      flex-direction: column;
      gap: 20px;
      max-width: 900px;
    }
    ul#linksList li.link-item {
      background: rgba(243 244 246 / 0.9);
      border-radius: 16px;
      padding: 20px 28px;
      box-shadow: 0 4px 12px rgb(0 0 0 / 0.05);
      display: flex;
      flex-direction: column;
      gap: 12px;
      transition: background-color 0.3s ease;
    }
    body.dark ul#linksList li.link-item {
      background: rgba(31 41 55 / 0.9);
      box-shadow: 0 4px 12px rgb(0 0 0 / 0.3);
    }
    ul#linksList li.link-item:hover {
      background: rgba(236 240 253 / 0.9);
    }
    body.dark ul#linksList li.link-item:hover {
      background: rgba(14 165 233 / 0.25);
    }
    ul#linksList li.link-item .link-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
    }
    ul#linksList li.link-item .link-url {
      font-family: monospace;
      color: #2563eb;
      word-break: break-word;
      user-select: all;
    }
    body.dark ul#linksList li.link-item .link-url {
      color: #38bdf8;
    }
    ul#linksList li.link-item .visitor-list {
      max-height: 200px;
      overflow-y: auto;
      background: rgba(255 255 255 / 0.7);
      border-radius: 12px;
      padding: 12px;
      font-size: 0.9rem;
      line-height: 1.4;
      font-family: monospace, monospace;
      transition: background-color 0.3s ease;
    }
    body.dark ul#linksList li.link-item .visitor-list {
      background: rgba(31 41 55 / 0.6);
    }
    ul#linksList li.link-item .visitor-list::-webkit-scrollbar {
      width: 8px;
    }
    ul#linksList li.link-item .visitor-list::-webkit-scrollbar-thumb {
      background: #6366f1;
      border-radius: 12px;
    }

    /* Notification toast */
    #toast {
      position: fixed;
      bottom: 24px;
      right: 24px;
      background: rgba(99 102 241 / 0.95);
      color: white;
      padding: 16px 24px;
      border-radius: 16px;
      font-size: 1rem;
      box-shadow: 0 6px 18px rgba(99,102,241,0.6);
      opacity: 0;
      pointer-events: none;
      transform: translateY(40px);
      transition: opacity 0.3s ease, transform 0.3s ease;
      z-index: 100;
      display: flex;
      align-items: center;
      gap: 12px;
    }
    #toast.show {
      opacity: 1;
      pointer-events: auto;
      transform: translateY(0);
    }
    #toast .material-icons {
      font-size: 28px;
    }

    /* Responsive */
    @media (max-width: 1023px) {
      body {
        grid-template-columns: 64px 1fr;
        grid-template-areas:
          "header header"
          "sidebar main"
          "footer footer";
      }
      aside.sidebar {
        width: 64px;
        padding: 16px 8px 32px;
        border-radius: 0 16px 16px 0;
      }
      aside.sidebar nav.navigation button.nav-button span.label {
        display: none;
      }
      main.main {
        padding: 24px;
      }
    }
    @media (max-width: 639px) {
      body {
        grid-template-rows: 64px 1fr 56px;
        grid-template-columns: 1fr;
        grid-template-areas:
          "header"
          "main"
          "footer";
      }
      aside.sidebar {
        display: none;
      }
      main.main {
        padding: 20px 16px;
      }
      footer.footer {
        height: 56px;
        position: sticky;
        bottom: 0;
        background: rgba(243 244 246 / 0.9);
        backdrop-filter: blur(12px);
        border-top: 1px solid #e5e7eb;
        display: flex;
        justify-content: space-around;
        align-items: center;
        box-shadow: 0 -1px 10px rgb(0 0 0 / 0.05);
      }
      body.dark footer.footer {
        background: rgba(31 41 55 / 0.9);
        border-color: #374151;
        box-shadow: 0 -1px 10px rgb(0 0 0 / 0.3);
      }
      footer.footer button {
        background: none;
        border: none;
        color: inherit;
        font-size: 24px;
        cursor: pointer;
        padding: 8px;
        border-radius: 12px;
      }
      footer.footer button:focus-visible {
        outline: 2px solid #6366f1;
        outline-offset: 2px;
      }
      footer.footer button:hover {
        background: rgba(99 102 241 / 0.1);
      }
      body.dark footer.footer button:hover {
        background: rgba(6 182 212 / 0.2);
      }
    }
  </style>
</head>
<body>
  <header role="banner" aria-label="Main header">
    <div class="brand" aria-label="App brand">
      <span class="material-icons" aria-hidden="true">link</span>
      TrackLink
    </div>
    <button aria-label="Toggle dark/light theme" class="theme-toggle" id="themeToggle" title="Toggle dark/light theme" type="button">
      <span class="material-icons" id="themeIcon">dark_mode</span>
    </button>
  </header>
  <aside class="sidebar" role="navigation" aria-label="Primary navigation">
    <nav class="navigation">
      <button class="nav-button" id="navCreateLink" aria-current="page" aria-label="Create new tracking link" title="Create new tracking link" type="button">
        <span class="material-icons" aria-hidden="true">add_link</span>
        <span class="label">Create Link</span>
      </button>
      <button class="nav-button" id="navLinksList" aria-label="View existing tracking links" title="View existing tracking links" type="button">
        <span class="material-icons" aria-hidden="true">list</span>
        <span class="label">Links</span>
      </button>
    </nav>
  </aside>
  <main class="main" role="main" tabindex="-1">
    <!-- Dynamic content loaded here -->
    <section class="card" id="createLinkSection" hidden>
      <h2>Create Tracking Link</h2>
      <form id="linkForm" novalidate>
        <label for="targetUrl">Target URL</label>
        <input
          id="targetUrl"
          name="targetUrl"
          type="url"
          placeholder="https://example.com"
          autocomplete="url"
          required
          aria-describedby="targetUrlError"
          spellcheck="false"
        />
        <div id="targetUrlError" role="alert" style="color:#dc2626; font-size:0.875rem; display:none;"></div>
        <button type="submit" aria-live="polite">Generate Tracking Link</button>
      </form>
    </section>
    <section class="card" id="linksListSection" hidden>
      <h2>Tracking Links</h2>
      <ul id="linksList" aria-live="polite" aria-relevant="additions removals"></ul>
      <p id="noLinksMessage" style="color:#6b7280;">No tracking links created yet.</p>
    </section>
  </main>
  <footer class="footer" role="contentinfo" aria-label="Footer">
    <p style="text-align:center; font-size: 0.875rem; padding: 8px 16px; margin: 0;">
      &copy; 2024 TrackLink. All rights reserved.
    </p>
  </footer>
  <div id="toast" role="alert" aria-live="assertive" aria-atomic="true" hidden>
    <span class="material-icons" aria-hidden="true">check_circle</span>
    <span id="toastMessage"></span>
  </div>
  <script>
    (() => {
      const STORAGE_KEY = 'tracklink_data';

      // Utilities
      function generateId() {
        // Simple UUID v4 generator for demo
        return 'xxxxxxxxyxxxxyxxxyxx'.replace(/[xy]/g, c => {
          const r = Math.random() * 16 | 0;
          return (c === 'x' ? r : (r & 0x3 | 0x8)).toString(16);
        });
      }
      function sanitizeUrl(url) {
        try {
          const u = new URL(url);
          return u.toString();
        } catch {
          return null;
        }
      }
      function showToast(message, duration = 3500) {
        const toast = document.getElementById('toast');
        const toastMessage = document.getElementById('toastMessage');
        toastMessage.textContent = message;
        toast.removeAttribute('hidden');
        toast.classList.add('show');
        setTimeout(() => {
          toast.classList.remove('show');
          setTimeout(() => toast.setAttribute('hidden', ''), 300);
        }, duration);
      }

      // Load data
      function loadData() {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return { links: {}, visitors: {} };
        try {
          return JSON.parse(raw);
        } catch {
          localStorage.removeItem(STORAGE_KEY);
          return { links: {}, visitors: {} };
        }
      }
      function saveData(data) {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
      }

      // Elements
      const linkForm = document.getElementById('linkForm');
      const createLinkSection = document.getElementById('createLinkSection');
      const linksListSection = document.getElementById('linksListSection');
      const linksList = document.getElementById('linksList');
      const noLinksMessage = document.getElementById('noLinksMessage');
      const navCreateLink = document.getElementById('navCreateLink');
      const navLinksList = document.getElementById('navLinksList');
      const targetUrlInput = document.getElementById('targetUrl');
      const targetUrlError = document.getElementById('targetUrlError');
      const themeToggle = document.getElementById('themeToggle');
      const themeIcon = document.getElementById('themeIcon');

      // Application data state
      let appData = loadData();
      // appData structure:
      // { links: { id: { id, targetUrl, shortUrl, createdAt } }, visitors: { linkId: [visitor objects] } }

      // Config
      // Tracking link base URL for simulation, this page supports param tracking
      const BASE_URL = window.location.origin + window.location.pathname;
      // We will generate links as BASE_URL + "?track=ID"

      // Render UI views
      function renderLinksList() {
        linksList.innerHTML = '';
        const linkIds = Object.keys(appData.links);
        if (linkIds.length === 0) {
          noLinksMessage.style.display = 'block';
          return;
        }
        noLinksMessage.style.display = 'none';

        linkIds.forEach((id) => {
          const link = appData.links[id];
          const visitors = appData.visitors[id] || [];
          const li = document.createElement('li');
          li.className = 'link-item';
          li.setAttribute('tabindex', '0');
          li.setAttribute('aria-label', `Tracking link to ${link.targetUrl} with ${visitors.length} visitors`);
          const createdDate = new Date(link.createdAt).toLocaleString();
          // Build visitor list HTML
          const visitorItems = visitors.length
            ? visitors
                .map((v) => {
                  const time = new Date(v.timestamp).toLocaleTimeString();
                  const location = v.location ? 
                    \`\${v.location.city ? v.location.city + ', ' : ''}\${v.location.regionName || ''}\` : 'Unknown location';
                  return \`<div><strong>\${time}</strong> - IP: \${v.ip} - \${location}</div>\`;
                })
                .join('')
            : '<div>No visitors yet</div>';

          li.innerHTML = \`
            <div class="link-header">
              <div>
                <strong>Tracking Link:</strong>
                <a href="\${link.shortUrl}" target="_blank" rel="noopener noreferrer nofollow" class="link-url">\${link.shortUrl}</a>
              </div>
              <div>Created: \${createdDate}</div>
            </div>
            <div>
              <strong>Target URL:</strong> <a href="\${link.targetUrl}" target="_blank" rel="noopener noreferrer nofollow">\${link.targetUrl}</a>
            </div>
            <div>
              <strong>Visitors (\${visitors.length}):</strong>
              <div class="visitor-list" tabindex="0" aria-live="polite" aria-label="Visitor list">\${visitorItems}</div>
            </div>
          \`;
          linksList.appendChild(li);
        });
      }

      // Navigation handlers
      function showCreateLinkView() {
        createLinkSection.hidden = false;
        linksListSection.hidden = true;
        navCreateLink.setAttribute('aria-current', 'page');
        navLinksList.removeAttribute('aria-current');
        targetUrlInput.focus();
        linksList.setAttribute('tabindex', '-1');
      }
      function showLinksListView() {
        createLinkSection.hidden = true;
        linksListSection.hidden = false;
        navLinksList.setAttribute('aria-current', 'page');
        navCreateLink.removeAttribute('aria-current');
        linksList.focus();
      }

      navCreateLink.addEventListener('click', showCreateLinkView);
      navLinksList.addEventListener('click', () => {
        renderLinksList();
        showLinksListView();
      });

      // Form validation and submission
      linkForm.addEventListener('submit', (e) => {
        e.preventDefault();
        targetUrlError.style.display = 'none';
        const rawUrl = targetUrlInput.value.trim();
        const validUrl = sanitizeUrl(rawUrl);

        if (!rawUrl) {
          targetUrlError.textContent = 'Target URL is required.';
          targetUrlError.style.display = 'block';
          targetUrlInput.focus();
          return;
        }
        if (!validUrl) {
          targetUrlError.textContent = 'Please enter a valid URL starting with http or https.';
          targetUrlError.style.display = 'block';
          targetUrlInput.focus();
          return;
        }

        // Create link data
        const id = generateId();
        const shortUrl = \`\${BASE_URL}?track=\${encodeURIComponent(id)}\`;
        const createdAt = new Date().toISOString();

        appData.links[id] = { id, targetUrl: validUrl, shortUrl, createdAt };
        appData.visitors[id] = [];

        saveData(appData);
        targetUrlInput.value = '';
        showToast('Tracking link created successfully');
        renderLinksList();
        showLinksListView();
      });

      // Theme toggle
      function setTheme(dark) {
        if (dark) {
          document.body.classList.add('dark');
          localStorage.setItem('theme', 'dark');
          themeIcon.textContent = 'light_mode';
          themeToggle.setAttribute('aria-label', 'Switch to light theme');
          themeToggle.title = 'Switch to light theme';
        } else {
          document.body.classList.remove('dark');
          localStorage.setItem('theme', 'light');
          themeIcon.textContent = 'dark_mode';
          themeToggle.setAttribute('aria-label', 'Switch to dark theme');
          themeToggle.title = 'Switch to dark theme';
        }
      }
      themeToggle.addEventListener('click', () => {
        const dark = document.body.classList.contains('dark');
        setTheme(!dark);
      });
      // On load, set theme from localStorage or system preference
      (() => {
        const stored = localStorage.getItem('theme');
        if (stored === 'dark') setTheme(true);
        else if (stored === 'light') setTheme(false);
        else {
          const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
          setTheme(prefersDark);
        }
      })();

      // Detect if URL has tracking parameter to process visitor logging
      function detectAndProcessVisitor() {
        const params = new URLSearchParams(window.location.search);
        const trackId = params.get('track');
        if (trackId && appData.links[trackId]) {
          // It's a tracking link visit - capture visitor info
          // Redirect after logging
          captureVisitor(trackId).then(() => {
            window.location.href = appData.links[trackId].targetUrl;
          }).catch(() => {
            // Proceed redirect anyway
            window.location.href = appData.links[trackId].targetUrl;
          });
          return true; // Don't show app UI
        }
        return false;
      }

      // Visitor capture function using free IP geolocation
      // Using https://ip-api.com/json/?fields=status,message,city,regionName,country,lat,lon,query
      async function captureVisitor(trackId) {
        try {
          // Fetch IP Geo location
          const res = await fetch('https://ip-api.com/json/?fields=status,message,city,regionName,country,query');
          const data = await res.json();
          if (data.status === 'fail') {
            throw new Error(data.message || 'Failed to get location');
          }

          const visitor = {
            ip: data.query || 'Unknown',
            location: {
              city: data.city || '',
              regionName: data.regionName || '',
              country: data.country || ''
            },
            timestamp: new Date().toISOString()
          };
          appData.visitors[trackId] = appData.visitors[trackId] || [];
          appData.visitors[trackId].push(visitor);
          // Save updated visitors
          saveData(appData);
          return visitor;
        } catch (e) {
          console.error('Visitor capture error:', e);
          return null;
        }
      }

      // On app load
      if (!detectAndProcessVisitor()) {
        // Show UI normally
        renderLinksList();
        showCreateLinkView(); // Default to create link view
      }
    })();
  </script>
</body>
</html>
