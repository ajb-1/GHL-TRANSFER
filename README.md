<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TruePeopleSearch → GHL Importer</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --bg: #f5f4f1; --surface: #ffffff; --surface2: #f0efe9;
    --border: #e2e0d8; --border2: #ccc9be;
    --text: #1a1917; --text2: #6b6860; --text3: #9a9890;
    --accent: #1a1917; --accent-fg: #ffffff;
    --green: #166534; --green-bg: #f0fdf4; --green-border: #bbf7d0;
    --red: #991b1b; --red-bg: #fff1f2; --red-border: #fecdd3;
    --blue: #1e40af; --blue-bg: #eff6ff; --blue-border: #bfdbfe;
    --orange: #92400e; --orange-bg: #fffbeb; --orange-border: #fde68a;
    --radius: 10px; --radius-sm: 6px;
  }
  body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; padding: 2rem 1rem; font-size: 15px; line-height: 1.6; }
  .container { max-width: 700px; margin: 0 auto; }
  .header { margin-bottom: 1.75rem; }
  .header h1 { font-size: 20px; font-weight: 600; letter-spacing: -0.02em; }
  .header p { font-size: 13px; color: var(--text2); margin-top: 3px; }
  .card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 1.4rem; margin-bottom: 1rem; }
  .card-title { font-size: 15px; font-weight: 600; margin-bottom: 1.1rem; letter-spacing: -0.01em; display: flex; align-items: center; gap: 8px; }
  .badge { background: var(--accent); color: var(--accent-fg); border-radius: 20px; font-size: 12px; font-weight: 500; padding: 2px 9px; }
  .badge-green { background: var(--green-bg); color: var(--green); border: 1px solid var(--green-border); }
  .instructions { background: var(--surface2); border-radius: var(--radius-sm); padding: 1rem 1.1rem; margin-bottom: 1.1rem; }
  .instructions-label { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; color: var(--text2); margin-bottom: 0.7rem; }
  .inst-row { display: flex; gap: 10px; margin-bottom: 8px; align-items: flex-start; }
  .inst-row:last-child { margin-bottom: 0; }
  .inst-n { width: 20px; height: 20px; border-radius: 50%; background: var(--surface); border: 1px solid var(--border2); font-size: 11px; font-weight: 600; display: flex; align-items: center; justify-content: center; flex-shrink: 0; margin-top: 2px; color: var(--text2); }
  .inst-text { font-size: 13px; color: var(--text); line-height: 1.55; }
  .inst-text b { font-weight: 600; }
  .inst-text code { font-family: monospace; font-size: 12px; background: var(--surface); border: 1px solid var(--border); border-radius: 4px; padding: 1px 5px; }
  .info-box { border-radius: var(--radius-sm); padding: 10px 13px; font-size: 13px; margin-bottom: 1.1rem; display: flex; gap: 8px; }
  .info-blue { background: var(--blue-bg); color: var(--blue); border: 1px solid var(--blue-border); }
  .info-orange { background: var(--orange-bg); color: var(--orange); border: 1px solid var(--orange-border); }
  .info-green { background: var(--green-bg); color: var(--green); border: 1px solid var(--green-border); }
  label.field-label { display: block; font-size: 12px; font-weight: 500; color: var(--text2); margin-bottom: 5px; text-transform: uppercase; letter-spacing: 0.05em; }
  textarea { width: 100%; padding: 9px 12px; border: 1px solid var(--border2); border-radius: var(--radius-sm); font-family: monospace; font-size: 12px; line-height: 1.5; color: var(--text); background: var(--surface); outline: none; resize: vertical; }
  textarea:focus { border-color: var(--accent); }
  .char-count { font-size: 12px; color: var(--text3); margin-top: 4px; text-align: right; }
  .btn-row { display: flex; gap: 8px; margin-top: 1.1rem; flex-wrap: wrap; }
  .btn { padding: 9px 18px; border-radius: var(--radius-sm); font-size: 14px; font-weight: 500; cursor: pointer; border: 1.5px solid transparent; transition: all 0.15s; display: inline-flex; align-items: center; gap: 6px; }
  .btn-primary { background: var(--accent); color: var(--accent-fg); border-color: var(--accent); }
  .btn-primary:hover { opacity: 0.85; }
  .btn-primary:disabled { opacity: 0.4; cursor: not-allowed; }
  .btn-secondary { background: var(--surface); color: var(--text); border-color: var(--border2); }
  .btn-secondary:hover { background: var(--surface2); }
  .btn-green { background: var(--green-bg); color: var(--green); border-color: var(--green-border); font-weight: 600; }
  .btn-green:hover { background: #dcfce7; }
  .divider { height: 1px; background: var(--border); margin: 1.1rem 0; }
  .data-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
  .data-field { background: var(--surface2); border-radius: var(--radius-sm); padding: 9px 11px; }
  .data-field.full { grid-column: 1 / -1; }
  .data-key { font-size: 10.5px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; color: var(--text3); margin-bottom: 3px; }
  .data-val { font-size: 13px; color: var(--text); line-height: 1.5; word-break: break-word; }
  .tag { display: inline-block; background: var(--surface); border: 1px solid var(--border); border-radius: 20px; padding: 2px 9px; font-size: 12px; margin: 2px 3px 2px 0; }
  .queue-list { display: flex; flex-direction: column; gap: 6px; margin-bottom: 1rem; }
  .queue-item { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 10px 14px; display: flex; align-items: center; justify-content: space-between; }
  .queue-item-name { font-size: 14px; font-weight: 500; }
  .queue-item-detail { font-size: 12px; color: var(--text2); margin-top: 2px; }
  .queue-remove { background: none; border: none; color: var(--text3); cursor: pointer; font-size: 18px; padding: 0 4px; line-height: 1; }
  .queue-remove:hover { color: var(--red); }
  .spinner { width: 15px; height: 15px; border: 2px solid rgba(255,255,255,0.35); border-top-color: white; border-radius: 50%; animation: spin 0.7s linear infinite; display: inline-block; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .alert { border-radius: var(--radius-sm); padding: 10px 13px; font-size: 13px; margin-top: 10px; }
  .alert-success { background: var(--green-bg); color: var(--green); border: 1px solid var(--green-border); }
  .alert-error { background: var(--red-bg); color: var(--red); border: 1px solid var(--red-border); }
  .steps { display: flex; align-items: center; margin-bottom: 1.5rem; }
  .step-item { display: flex; align-items: center; gap: 7px; font-size: 13px; color: var(--text3); white-space: nowrap; }
  .step-item.active { color: var(--text); font-weight: 500; }
  .step-item.done { color: var(--green); }
  .step-circle { width: 22px; height: 22px; border-radius: 50%; border: 1.5px solid var(--border2); display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 600; flex-shrink: 0; background: var(--surface); }
  .step-item.active .step-circle { background: var(--accent); color: var(--accent-fg); border-color: var(--accent); }
  .step-item.done .step-circle { background: var(--green-bg); color: var(--green); border-color: var(--green-border); }
  .step-connector { flex: 1; height: 1px; background: var(--border); margin: 0 8px; }
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <h1>TruePeopleSearch → GHL Importer</h1>
    <p>Extract contact data · Build a CSV · Import directly into GoHighLevel</p>
  </div>

  <!-- SCREEN 1: PASTE -->
  <div id="screen-paste">
    <div class="steps">
      <div class="step-item active"><div class="step-circle">1</div>Paste &amp; extract</div>
      <div class="step-connector"></div>
      <div class="step-item"><div class="step-circle">2</div>Review &amp; queue</div>
      <div class="step-connector"></div>
      <div class="step-item"><div class="step-circle">3</div>Download &amp; import</div>
    </div>
    <div class="card">
      <div class="card-title">Paste a TruePeopleSearch profile</div>
      <div class="instructions">
        <div class="instructions-label">How to copy the page</div>
        <div class="inst-row"><div class="inst-n">1</div><div class="inst-text">Open <b>truepeoplesearch.com</b> in another tab and open the person's full profile</div></div>
        <div class="inst-row"><div class="inst-n">2</div><div class="inst-text">Click anywhere on the page then press <b>Ctrl+A</b> (Windows) or <b>Cmd+A</b> (Mac) to select all</div></div>
        <div class="inst-row"><div class="inst-n">3</div><div class="inst-text">Press <b>Ctrl+C</b> (Windows) or <b>Cmd+C</b> (Mac) to copy</div></div>
        <div class="inst-row"><div class="inst-n">4</div><div class="inst-text">Click in the box below and press <b>Ctrl+V</b> (Windows) or <b>Cmd+V</b> (Mac) to paste</div></div>
      </div>
      <label class="field-label" for="raw-text">Paste profile text here</label>
      <textarea id="raw-text" rows="11" placeholder="Paste the full TruePeopleSearch page text here..."></textarea>
      <div class="char-count" id="char-count">0 characters</div>
      <div id="paste-msg"></div>
      <div class="btn-row">
        <button class="btn btn-primary" id="extract-btn" onclick="extractData()">⚡ Extract info</button>
      </div>
    </div>
  </div>

  <!-- SCREEN 2: REVIEW -->
  <div id="screen-review" style="display:none">
    <div class="steps">
      <div class="step-item done"><div class="step-circle">✓</div>Paste &amp; extract</div>
      <div class="step-connector"></div>
      <div class="step-item active"><div class="step-circle">2</div>Review &amp; queue</div>
      <div class="step-connector"></div>
      <div class="step-item"><div class="step-circle">3</div>Download &amp; import</div>
    </div>
    <div class="card">
      <div class="card-title">Review extracted data <span id="review-name" style="color:var(--text2);font-weight:400;font-size:14px;"></span></div>
      <div class="data-grid" id="data-grid"></div>
      <div class="divider"></div>
      <div class="btn-row">
        <button class="btn btn-primary" onclick="addToQueue()">+ Add to CSV queue</button>
        <button class="btn btn-secondary" onclick="goBackToPaste()">← Extract another</button>
      </div>
    </div>
  </div>

  <!-- SCREEN 3: QUEUE + DOWNLOAD -->
  <div id="screen-queue" style="display:none">
    <div class="steps">
      <div class="step-item done"><div class="step-circle">✓</div>Paste &amp; extract</div>
      <div class="step-connector"></div>
      <div class="step-item done"><div class="step-circle">✓</div>Review &amp; queue</div>
      <div class="step-connector"></div>
      <div class="step-item active"><div class="step-circle">3</div>Download &amp; import</div>
    </div>

    <div class="card">
      <div class="card-title">CSV queue <span class="badge" id="queue-count">0</span></div>
      <div class="queue-list" id="queue-list"></div>
      <div class="btn-row">
        <button class="btn btn-secondary" onclick="goBackToPaste()">+ Add more profiles</button>
      </div>
    </div>

    <div class="card">
      <div class="card-title">⬇ Download &amp; import into GHL</div>
      <div class="info-orange" style="margin-bottom:1rem;">
        <span>⚠</span>
        <span><b>Before importing:</b> You need to create 3 custom fields in GHL first so relatives, previous addresses, and aliases import correctly. See step 2 below.</span>
      </div>
      <div class="instructions">
        <div class="instructions-label">Step 1 — Download your CSV</div>
        <div class="inst-row"><div class="inst-n">1</div><div class="inst-text">Click <b>Download CSV</b> below — a file called <code>ghl-contacts.csv</code> will save to your Downloads folder</div></div>
        <div class="inst-row"><div class="inst-n">2</div><div class="inst-text">This file contains all <span id="dl-count">0</span> people you queued, formatted exactly for GHL import</div></div>
      </div>
      <div class="instructions" style="margin-top:10px;">
        <div class="instructions-label">Step 2 — Create custom fields in GHL (one-time setup)</div>
        <div class="inst-row"><div class="inst-n">1</div><div class="inst-text">In GHL go to <b>Settings</b> in the left sidebar → click <b>Custom Fields</b></div></div>
        <div class="inst-row"><div class="inst-n">2</div><div class="inst-text">Click <b>+ Add Field</b> → select <b>Text</b> → name it exactly: <code>Age</code> → Save</div></div>
        <div class="inst-row"><div class="inst-n">3</div><div class="inst-text">Click <b>+ Add Field</b> → select <b>Phone</b> → name it exactly: <code>Phone 2</code> → Save</div></div>
        <div class="inst-row"><div class="inst-n">4</div><div class="inst-text">Click <b>+ Add Field</b> → select <b>Phone</b> → name it exactly: <code>Phone 3</code> → Save. Repeat for <code>Phone 4</code>, <code>Phone 5</code> etc. if needed</div></div>
        <div class="inst-row"><div class="inst-n">5</div><div class="inst-text">Click <b>+ Add Field</b> → select <b>Text Area</b> → name it exactly: <code>Also Seen As</code> → Save</div></div>
        <div class="inst-row"><div class="inst-n">6</div><div class="inst-text">Click <b>+ Add Field</b> → select <b>Text Area</b> → name it exactly: <code>Previous Addresses</code> → Save</div></div>
        <div class="inst-row"><div class="inst-n">7</div><div class="inst-text">Click <b>+ Add Field</b> → select <b>Text Area</b> → name it exactly: <code>Possible Relatives</code> → Save</div></div>
        <div class="inst-row"><div class="inst-n">8</div><div class="inst-text">Click <b>+ Add Field</b> → select <b>Text</b> → name it exactly: <code>Date of Birth</code> → Save</div></div>
        <div class="inst-row"><div class="inst-n">9</div><div class="inst-text">You only do this once — all fields will be available for every future import</div></div>
      </div>
      <div class="instructions" style="margin-top:10px;">
        <div class="instructions-label">Step 3 — Import the CSV into GHL</div>
        <div class="inst-row"><div class="inst-n">1</div><div class="inst-text">In GHL go to <b>Contacts</b> in the left sidebar</div></div>
        <div class="inst-row"><div class="inst-n">2</div><div class="inst-text">Click the <b>Import</b> icon (arrow pointing up) near the top right</div></div>
        <div class="inst-row"><div class="inst-n">3</div><div class="inst-text">Click <b>Select File</b> and choose <code>ghl-contacts.csv</code> from your Downloads folder</div></div>
        <div class="inst-row"><div class="inst-n">4</div><div class="inst-text">On the field mapping screen, these map <b>automatically</b>: First Name, Last Name, Email, Phone, Address1, City, State, Postal Code</div></div>
        <div class="inst-row"><div class="inst-n">5</div><div class="inst-text">For the remaining columns, use the dropdown to manually map each one to the custom field you created: <code>Phone 2</code> → Phone 2, <code>Phone 3</code> → Phone 3, <code>Age</code> → Age, <code>Date of Birth</code> → Date of Birth, <code>Also Seen As</code> → Also Seen As, <code>Previous Addresses</code> → Previous Addresses, <code>Possible Relatives</code> → Possible Relatives</div></div>
        <div class="inst-row"><div class="inst-n">6</div><div class="inst-text">Click <b>Next</b> → then <b>Import</b> — contacts appear in GHL within a few minutes</div></div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" id="download-btn" onclick="downloadCSV()">⬇ Download CSV</button>
      </div>
      <div id="download-msg"></div>
    </div>
  </div>
</div>

<script>
let queue = [];
let currentParsed = null;

// ── Character count ──────────────────────────────────────────────────────────
document.getElementById('raw-text').addEventListener('input', function() {
  document.getElementById('char-count').textContent = this.value.length.toLocaleString() + ' characters';
});

// ── Parser ───────────────────────────────────────────────────────────────────
function parseProfile(text) {
  const result = {
    firstName: '', lastName: '', middleName: '', fullName: '',
    age: '', dateOfBirth: '', emails: [], phones: [],
    address1: '', city: '', state: '', zipCode: '',
    previousAddresses: [], relatives: [], aliases: []
  };
  const lines = text.split('\n').map(l => l.trim()).filter(l => l.length > 0);

  // Pre-process: rejoin phone numbers split across lines
  // TruePeopleSearch makes area codes clickable links so they land on separate lines when copied
  // e.g. "(317) \n708-5412" needs to become "(317) 708-5412"
  const rejoined = text.replace(/(\(\d{3}\))\s*\n\s*(\d{3}[-.\s]\d{4})/g, '$1 $2');

  // Phones — run against rejoined text
  const phoneRegex = /(\(?\d{3}\)?[\s.\-]\d{3}[\s.\-]\d{4})/g;
  result.phones = [...new Set(rejoined.match(phoneRegex) || [])].map(p => p.trim());

  // Emails
  const emailRegex = /[\w.+-]+@[\w-]+\.[a-z]{2,}/gi;
  result.emails = [...new Set(text.match(emailRegex) || [])];

  // Age
  const ageM = text.match(/\bAge[:\s]+(\d{1,3})\b/i);
  if (ageM) result.age = ageM[1];

  // DOB — handles "Born February 1947" (month+year) and "Born February 15, 1947" (full date)
  const dobM = text.match(/\bBorn\s+((January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{1,2},?\s*\d{4}|(January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{4})/i);
  if (dobM) result.dateOfBirth = dobM[1].trim();

  // Section extractor
  function extractSection(startKws, stopKws) {
    const tl = text.toLowerCase();
    let start = -1;
    for (const kw of startKws) { const i = tl.indexOf(kw.toLowerCase()); if (i !== -1) { start = i + kw.length; break; } }
    if (start === -1) return '';
    let end = text.length;
    for (const kw of stopKws) { const i = tl.indexOf(kw.toLowerCase(), start); if (i !== -1 && i < end) end = i; }
    return text.slice(start, end).trim();
  }
  const stopWords = ['phone number','email address','current address','associated address','previous address','possible relative','also seen as','also known as','background','court record','social media','neighbor','property'];

  // Name
  for (let i = 0; i < lines.length; i++) {
    const line = lines[i];
    if (/^(home|search|people|background|menu|skip|sign|log|find|true)/i.test(line)) continue;
    if (/^\d/.test(line)) continue;
    if (line.length < 4 || line.length > 60) continue;
    if (/age\s+\d|phone|address|email|relative|known|seen/i.test(line)) continue;
    const words = line.split(/\s+/).filter(w => /^[A-Z][a-z]+$|^[A-Z]+$/.test(w));
    if (words.length >= 2 && words.length <= 4) {
      result.fullName = line;
      result.firstName = words[0] || '';
      result.lastName = words[words.length - 1] || '';
      if (words.length >= 3) result.middleName = words.slice(1, -1).join(' ');
      break;
    }
  }

  // Aliases (Also Seen As)
  const aliasSection = extractSection(['also seen as','also known as','aliases','other names'], stopWords.filter(s => !s.includes('also')));
  if (aliasSection) {
    const cleaned = aliasSection.replace(/([a-z])([A-Z])/g, '$1\n$2');
    cleaned.split('\n').map(l => l.trim()).filter(l => l.length > 2).forEach(line => {
      if (/^includes\s/i.test(line)) return;
      if (/\d/.test(line)) return;
      if (/search|lookup|services|reverse|privacy|terms|background/i.test(line)) return;
      line.split(',').map(s => s.trim()).filter(s => s.length > 2).forEach(name => {
        const words = name.split(/\s+/);
        if (words.length >= 2 && words.length <= 6 && words.every(w => /^[A-Z][a-zA-Z'-]*\.?$/.test(w)) && !result.aliases.includes(name)) result.aliases.push(name);
      });
    });
  }

  // Current address
  const addrSection = extractSection(['current address','current home address'], ['associated address','previous address','possible relative','phone number','email','also seen','also known']);
  if (addrSection) {
    const addrLines = addrSection.split('\n').map(l => l.trim()).filter(l => l);
    const streetLine = addrLines.find(l => /^\d+\s+\w/.test(l));
    if (streetLine) result.address1 = streetLine;
    const cityStateZip = addrLines.find(l => /[A-Z]{2}\s+\d{5}/.test(l) || /,\s*[A-Z]{2}/.test(l));
    if (cityStateZip) {
      const zipM = cityStateZip.match(/\b(\d{5}(-\d{4})?)\b/);
      if (zipM) result.zipCode = zipM[1];
      const stateM = cityStateZip.match(/\b([A-Z]{2})\b/);
      if (stateM) result.state = stateM[1];
      const cityM = cityStateZip.replace(/\b[A-Z]{2}\b.*/, '').replace(/,/g, '').trim();
      if (cityM) result.city = cityM;
    }
  }

  // Previous addresses
  const prevSection = extractSection(['associated addresses','previous addresses','past addresses','former addresses'], ['possible relative','phone number','email','background','court','social media']);
  if (prevSection) {
    let cur = '';
    prevSection.split('\n').map(l => l.trim()).filter(l => l).forEach(line => {
      if (/^\d+\s+\w/.test(line)) { cur = line; }
      else if (/[A-Z]{2}\s+\d{5}|,\s*[A-Z]{2}/.test(line) && cur) { result.previousAddresses.push(cur + ', ' + line.trim()); cur = ''; }
      else if (/[A-Z]{2}\s+\d{5}|,\s*[A-Z]{2}/.test(line)) { result.previousAddresses.push(line.trim()); }
    });
    if (cur && !result.previousAddresses.includes(cur)) result.previousAddresses.push(cur);
  }

  // Relatives
  const relSection = extractSection(['possible relatives','known associates','associated people'], ['phone number','email','address','background','court','social media','neighbor']);
  if (relSection) {
    relSection.split('\n').map(l => l.trim()).filter(l => l.length > 2).forEach(line => {
      if (/^age\s*\d/i.test(line) || /^\d/.test(line)) return;
      if (/search|lookup|services|reverse|privacy|terms|background/i.test(line)) return;
      const words = line.split(/\s+/);
      if (words.length >= 2 && words.length <= 6 && /^[A-Za-z]/.test(line) && !result.relatives.includes(line)) result.relatives.push(line);
    });
  }

  return result;
}

// ── Extract ──────────────────────────────────────────────────────────────────
function extractData() {
  const raw = document.getElementById('raw-text').value.trim();
  if (!raw) { showMsg('paste-msg','error','Please paste the profile text first.'); return; }
  if (raw.length < 100) { showMsg('paste-msg','error','Text seems too short — make sure you pressed Ctrl+A to select the whole page before copying.'); return; }
  const btn = document.getElementById('extract-btn');
  btn.disabled = true; btn.innerHTML = '<span class="spinner"></span> Extracting...';
  setTimeout(() => {
    currentParsed = parseProfile(raw);
    renderDataGrid(currentParsed);
    document.getElementById('review-name').textContent = currentParsed.fullName ? '— ' + currentParsed.fullName : '';
    show('screen-review');
    btn.disabled = false; btn.innerHTML = '⚡ Extract info';
  }, 250);
}

// ── Render data grid ─────────────────────────────────────────────────────────
function renderDataGrid(data) {
  const grid = document.getElementById('data-grid');
  grid.innerHTML = '';
  const fields = [
    {key:'fullName',label:'Full name'},{key:'firstName',label:'First name'},{key:'lastName',label:'Last name'},
    {key:'middleName',label:'Middle name'},{key:'age',label:'Age'},{key:'dateOfBirth',label:'Date of birth'},
    {key:'phones',label:'Phone numbers',full:true,list:true},{key:'emails',label:'Emails',full:true,list:true},
    {key:'address1',label:'Address'},{key:'city',label:'City'},{key:'state',label:'State'},{key:'zipCode',label:'ZIP'},
    {key:'aliases',label:'Also seen as',full:true,list:true},
    {key:'previousAddresses',label:'Previous addresses',full:true,list:true},
    {key:'relatives',label:'Possible relatives',full:true,list:true}
  ];
  fields.forEach(f => {
    const val = data[f.key];
    if (!val || (Array.isArray(val) && !val.length) || val === '') return;
    const div = document.createElement('div');
    div.className = 'data-field' + (f.full ? ' full' : '');
    const key = document.createElement('div'); key.className = 'data-key'; key.textContent = f.label;
    const valEl = document.createElement('div'); valEl.className = 'data-val';
    if (f.list && Array.isArray(val)) { val.forEach(v => { const t = document.createElement('span'); t.className = 'tag'; t.textContent = v; valEl.appendChild(t); }); }
    else { valEl.textContent = val; }
    div.appendChild(key); div.appendChild(valEl); grid.appendChild(div);
  });
}

// ── Queue ────────────────────────────────────────────────────────────────────
function addToQueue() {
  if (!currentParsed) return;
  queue.push({ ...currentParsed });
  renderQueue();
  show('screen-queue');
  document.getElementById('raw-text').value = '';
  document.getElementById('char-count').textContent = '0 characters';
  document.getElementById('paste-msg').innerHTML = '';
  currentParsed = null;
}

function renderQueue() {
  const list = document.getElementById('queue-list');
  list.innerHTML = '';
  document.getElementById('queue-count').textContent = queue.length;
  document.getElementById('dl-count').textContent = queue.length;
  queue.forEach((p, i) => {
    const item = document.createElement('div'); item.className = 'queue-item';
    const info = document.createElement('div');
    const name = document.createElement('div'); name.className = 'queue-item-name'; name.textContent = p.fullName || 'Unknown name';
    const detail = document.createElement('div'); detail.className = 'queue-item-detail';
    detail.textContent = [p.phones[0], p.city && p.state ? p.city + ', ' + p.state : ''].filter(Boolean).join(' · ');
    info.appendChild(name); info.appendChild(detail);
    const btn = document.createElement('button'); btn.className = 'queue-remove'; btn.textContent = '×';
    btn.onclick = () => { queue.splice(i, 1); renderQueue(); if (queue.length === 0) show('screen-paste'); };
    item.appendChild(info); item.appendChild(btn); list.appendChild(item);
  });
}

// ── CSV Download ─────────────────────────────────────────────────────────────
function downloadCSV() {
  if (!queue.length) { showMsg('download-msg','error','No profiles in queue yet.'); return; }

  // Find max phones across all queued contacts
  const maxPhones = Math.max(...queue.map(p => p.phones.length), 1);

  // Build phone column headers: Phone, Phone 2, Phone 3...
  const phoneHeaders = ['Phone'];
  for (let i = 2; i <= maxPhones; i++) phoneHeaders.push('Phone ' + i);

  // Full headers — each phone gets its own column
  const headers = [
    'First Name', 'Last Name', 'Email',
    ...phoneHeaders,
    'Address1', 'City', 'State', 'Postal Code',
    'Date of Birth', 'Age',
    'Also Seen As',
    'Previous Addresses',
    'Possible Relatives',
    'Source'
  ];

  const rows = queue.map(p => {
    // Each phone in its own cell
    const phoneCells = [];
    for (let i = 0; i < maxPhones; i++) {
      phoneCells.push(csvEscape(p.phones[i] || ''));
    }

    // Lists: each item on its own line inside the cell — clean and readable in GHL
    const alsoSeenAs   = p.aliases.length           ? csvMultiline(p.aliases)           : '';
    const prevAddrs    = p.previousAddresses.length  ? csvMultiline(p.previousAddresses)  : '';
    const relatives    = p.relatives.length          ? csvMultiline(p.relatives)          : '';

    return [
      csvEscape(p.firstName),
      csvEscape(p.lastName),
      csvEscape(p.emails[0] || ''),
      ...phoneCells,
      csvEscape(p.address1),
      csvEscape(p.city),
      csvEscape(p.state),
      csvEscape(p.zipCode),
      csvEscape(p.dateOfBirth),
      csvEscape(p.age),
      alsoSeenAs,
      prevAddrs,
      relatives,
      'TruePeopleSearch'
    ].join(',');
  });

  const csv = [headers.join(','), ...rows].join('\n');
  const blob = new Blob(['\uFEFF' + csv], { type: 'text/csv;charset=utf-8;' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = 'ghl-contacts.csv'; a.click();
  URL.revokeObjectURL(url);
  showMsg('download-msg','success','✓ ghl-contacts.csv downloaded. Follow the steps above to import into GHL.');
}

  const csv = [headers.join(','), ...rows].join('\n');
  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = 'ghl-contacts.csv'; a.click();
  URL.revokeObjectURL(url);
  showMsg('download-msg','success','✓ ghl-contacts.csv downloaded to your Downloads folder. Now follow Step 2 and Step 3 above to import into GHL.');
}

function csvEscape(val) {
  if (!val) return '';
  const str = String(val);
  if (str.includes(',') || str.includes('"') || str.includes('\n')) return '"' + str.replace(/"/g, '""') + '"';
  return str;
}

// Puts each item on its own line inside a quoted CSV cell — clean in GHL text area fields
function csvMultiline(arr) {
  if (!arr || !arr.length) return '';
  return '"' + arr.join('\n').replace(/"/g, '""') + '"';
}

// ── Helpers ───────────────────────────────────────────────────────────────────
function show(id) {
  ['screen-paste','screen-review','screen-queue'].forEach(s => {
    document.getElementById(s).style.display = s === id ? 'block' : 'none';
  });
}
function goBackToPaste() { show('screen-paste'); if (queue.length > 0) { document.getElementById('screen-queue').style.display = 'block'; } }
function showMsg(id, type, text) { document.getElementById(id).innerHTML = '<div class="alert alert-' + type + '">' + text + '</div>'; }
</script>
</body>
</html>
