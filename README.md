<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TruePeopleSearch → GHL Importer</title>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#f5f4f1;--surface:#fff;--surface2:#f0efe9;
  --border:#e2e0d8;--border2:#ccc9be;
  --text:#1a1917;--text2:#6b6860;--text3:#9a9890;
  --accent:#1a1917;--accent-fg:#fff;
  --green:#166534;--green-bg:#f0fdf4;--green-border:#bbf7d0;
  --red:#991b1b;--red-bg:#fff1f2;--red-border:#fecdd3;
  --blue:#1e40af;--blue-bg:#eff6ff;--blue-border:#bfdbfe;
  --orange:#92400e;--orange-bg:#fffbeb;--orange-border:#fde68a;
  --r:10px;--rs:6px
}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',system-ui,sans-serif;background:var(--bg);color:var(--text);min-height:100vh;padding:2rem 1rem;font-size:15px;line-height:1.6}
.container{max-width:720px;margin:0 auto}
.header{margin-bottom:1.75rem}
.header h1{font-size:20px;font-weight:600;letter-spacing:-.02em}
.header p{font-size:13px;color:var(--text2);margin-top:3px}
.card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:1.4rem;margin-bottom:1rem}
.card-title{font-size:15px;font-weight:600;margin-bottom:1.1rem;letter-spacing:-.01em;display:flex;align-items:center;gap:8px}
.badge{background:var(--accent);color:var(--accent-fg);border-radius:20px;font-size:12px;font-weight:500;padding:2px 9px}
.instr{background:var(--surface2);border-radius:var(--rs);padding:1rem 1.1rem;margin-bottom:1.1rem}
.instr-label{font-size:11px;font-weight:600;text-transform:uppercase;letter-spacing:.06em;color:var(--text2);margin-bottom:.7rem}
.ir{display:flex;gap:10px;margin-bottom:8px;align-items:flex-start}
.ir:last-child{margin-bottom:0}
.in{width:20px;height:20px;border-radius:50%;background:var(--surface);border:1px solid var(--border2);font-size:11px;font-weight:600;display:flex;align-items:center;justify-content:center;flex-shrink:0;margin-top:2px;color:var(--text2)}
.it{font-size:13px;color:var(--text);line-height:1.55}
.it b{font-weight:600}
.it code{font-family:monospace;font-size:12px;background:var(--surface);border:1px solid var(--border);border-radius:4px;padding:1px 5px}
.info-box{border-radius:var(--rs);padding:10px 13px;font-size:13px;margin-bottom:1.1rem;display:flex;gap:8px}
.info-orange{background:var(--orange-bg);color:var(--orange);border:1px solid var(--orange-border)}
textarea{width:100%;padding:9px 12px;border:1px solid var(--border2);border-radius:var(--rs);font-family:monospace;font-size:12px;line-height:1.5;color:var(--text);background:var(--surface);outline:none;resize:vertical}
textarea:focus{border-color:var(--accent)}
.char-count{font-size:12px;color:var(--text3);margin-top:4px;text-align:right}
.btn-row{display:flex;gap:8px;margin-top:1.1rem;flex-wrap:wrap}
.btn{padding:9px 18px;border-radius:var(--rs);font-size:14px;font-weight:500;cursor:pointer;border:1.5px solid transparent;transition:all .15s;display:inline-flex;align-items:center;gap:6px}
.btn-primary{background:var(--accent);color:var(--accent-fg);border-color:var(--accent)}
.btn-primary:hover{opacity:.85}
.btn-primary:disabled{opacity:.4;cursor:not-allowed}
.btn-secondary{background:var(--surface);color:var(--text);border-color:var(--border2)}
.btn-secondary:hover{background:var(--surface2)}
.btn-green{background:var(--green-bg);color:var(--green);border-color:var(--green-border);font-weight:600}
.btn-green:hover{background:#dcfce7}
.divider{height:1px;background:var(--border);margin:1.1rem 0}
.data-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.df{background:var(--surface2);border-radius:var(--rs);padding:9px 11px}
.df.full{grid-column:1/-1}
.dk{font-size:10.5px;font-weight:600;text-transform:uppercase;letter-spacing:.06em;color:var(--text3);margin-bottom:3px}
.dv{font-size:13px;color:var(--text);line-height:1.5;word-break:break-word}
.tag{display:inline-block;background:var(--surface);border:1px solid var(--border);border-radius:20px;padding:2px 9px;font-size:12px;margin:2px 3px 2px 0}
.phone-block{background:var(--surface);border:1px solid var(--border);border-radius:var(--rs);padding:8px 10px;margin:3px 0}
.phone-num{font-size:13px;font-weight:600;color:var(--text)}
.phone-meta{font-size:11px;color:var(--text2);margin-top:2px}
.queue-list{display:flex;flex-direction:column;gap:6px;margin-bottom:1rem}
.qi{background:var(--surface2);border:1px solid var(--border);border-radius:var(--rs);padding:10px 14px;display:flex;align-items:center;justify-content:space-between}
.qi-name{font-size:14px;font-weight:500}
.qi-detail{font-size:12px;color:var(--text2);margin-top:2px}
.qi-rm{background:none;border:none;color:var(--text3);cursor:pointer;font-size:18px;padding:0 4px;line-height:1}
.qi-rm:hover{color:var(--red)}
.spinner{width:15px;height:15px;border:2px solid rgba(255,255,255,.35);border-top-color:#fff;border-radius:50%;animation:spin .7s linear infinite;display:inline-block}
@keyframes spin{to{transform:rotate(360deg)}}
.alert{border-radius:var(--rs);padding:10px 13px;font-size:13px;margin-top:10px}
.alert-success{background:var(--green-bg);color:var(--green);border:1px solid var(--green-border)}
.alert-error{background:var(--red-bg);color:var(--red);border:1px solid var(--red-border)}
.steps{display:flex;align-items:center;margin-bottom:1.5rem}
.si{display:flex;align-items:center;gap:7px;font-size:13px;color:var(--text3);white-space:nowrap}
.si.active{color:var(--text);font-weight:500}
.si.done{color:var(--green)}
.sc{width:22px;height:22px;border-radius:50%;border:1.5px solid var(--border2);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:600;flex-shrink:0;background:var(--surface)}
.si.active .sc{background:var(--accent);color:var(--accent-fg);border-color:var(--accent)}
.si.done .sc{background:var(--green-bg);color:var(--green);border-color:var(--green-border)}
.scon{flex:1;height:1px;background:var(--border);margin:0 8px}
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <h1>TruePeopleSearch → GHL Importer</h1>
    <p>Extract · Review · Download CSV · Import into GoHighLevel</p>
  </div>

  <!-- SCREEN 1: PASTE -->
  <div id="screen-paste">
    <div class="steps">
      <div class="si active"><div class="sc">1</div>Paste &amp; extract</div>
      <div class="scon"></div>
      <div class="si"><div class="sc">2</div>Review &amp; queue</div>
      <div class="scon"></div>
      <div class="si"><div class="sc">3</div>Download &amp; import</div>
    </div>
    <div class="card">
      <div class="card-title">Paste a TruePeopleSearch profile</div>
      <div class="instr">
        <div class="instr-label">How to copy the page</div>
        <div class="ir"><div class="in">1</div><div class="it">Open <b>truepeoplesearch.com</b> in another tab and open the person's full profile</div></div>
        <div class="ir"><div class="in">2</div><div class="it">Click anywhere on the page then press <b>Ctrl+A</b> (Windows) or <b>Cmd+A</b> (Mac) to select all</div></div>
        <div class="ir"><div class="in">3</div><div class="it">Press <b>Ctrl+C</b> to copy then click in the box below and press <b>Ctrl+V</b> to paste</div></div>
      </div>
      <label style="font-size:12px;font-weight:500;color:var(--text2);text-transform:uppercase;letter-spacing:.05em;display:block;margin-bottom:5px">Paste profile text here</label>
      <textarea id="raw-text" rows="11" placeholder="Paste the full TruePeopleSearch page text here..."></textarea>
      <div class="char-count" id="char-count">0 characters</div>
      <div id="paste-msg"></div>
      <div class="btn-row">
        <button class="btn btn-primary" id="extract-btn" onclick="extractData()">&#9889; Extract info</button>
      </div>
    </div>
  </div>

  <!-- SCREEN 2: REVIEW -->
  <div id="screen-review" style="display:none">
    <div class="steps">
      <div class="si done"><div class="sc">&#10003;</div>Paste &amp; extract</div>
      <div class="scon"></div>
      <div class="si active"><div class="sc">2</div>Review &amp; queue</div>
      <div class="scon"></div>
      <div class="si"><div class="sc">3</div>Download &amp; import</div>
    </div>
    <div class="card">
      <div class="card-title">Review extracted data <span id="review-name" style="color:var(--text2);font-weight:400;font-size:14px"></span></div>
      <div class="data-grid" id="data-grid"></div>
      <div class="divider"></div>
      <div class="btn-row">
        <button class="btn btn-primary" onclick="addToQueue()">+ Add to CSV queue</button>
        <button class="btn btn-secondary" onclick="goBackToPaste()">&#8592; Extract another</button>
      </div>
    </div>
  </div>

  <!-- SCREEN 3: QUEUE + DOWNLOAD -->
  <div id="screen-queue" style="display:none">
    <div class="steps">
      <div class="si done"><div class="sc">&#10003;</div>Paste &amp; extract</div>
      <div class="scon"></div>
      <div class="si done"><div class="sc">&#10003;</div>Review &amp; queue</div>
      <div class="scon"></div>
      <div class="si active"><div class="sc">3</div>Download &amp; import</div>
    </div>
    <div class="card">
      <div class="card-title">CSV queue <span class="badge" id="queue-count">0</span></div>
      <div class="queue-list" id="queue-list"></div>
      <div class="btn-row">
        <button class="btn btn-secondary" onclick="goBackToPaste()">+ Add more profiles</button>
      </div>
    </div>
    <div class="card">
      <div class="card-title">&#11015; Download &amp; import into GHL</div>
      <div class="info-orange">
        <span>&#9888;</span>
        <span><b>Before importing:</b> Create the custom fields in GHL first (Step 2 below). You only do this once.</span>
      </div>
      <div class="instr">
        <div class="instr-label">Step 1 — Download your CSV</div>
        <div class="ir"><div class="in">1</div><div class="it">Click <b>Download CSV</b> below — a file called <code>ghl-contacts.csv</code> saves to your Downloads folder</div></div>
        <div class="ir"><div class="in">2</div><div class="it">Phones are sorted most recent to oldest. Phone 1 is the most recently reported number and will be the primary dialable number in GHL</div></div>
      </div>
      <div class="instr">
        <div class="instr-label">Step 2 — Create custom fields in GHL (one-time setup)</div>
        <div class="ir"><div class="in">1</div><div class="it">In GHL go to <b>Settings</b> in the left sidebar &#8594; click <b>Custom Fields</b></div></div>
        <div class="ir"><div class="in">2</div><div class="it">Click <b>+ Add Field</b> &#8594; select <b>Text</b> &#8594; name it exactly: <code>Age</code> &#8594; Save</div></div>
        <div class="ir"><div class="in">3</div><div class="it">Click <b>+ Add Field</b> &#8594; select <b>Text</b> &#8594; name it exactly: <code>Date of Birth</code> &#8594; Save</div></div>
        <div class="ir"><div class="in">4</div><div class="it">Click <b>+ Add Field</b> &#8594; select <b>Text Area</b> &#8594; name it exactly: <code>Phone Details</code> &#8594; Save. <b>This stores every phone number with its type, carrier, and last reported date</b></div></div>
        <div class="ir"><div class="in">5</div><div class="it">Click <b>+ Add Field</b> &#8594; select <b>Text Area</b> &#8594; name it exactly: <code>Also Seen As</code> &#8594; Save</div></div>
        <div class="ir"><div class="in">6</div><div class="it">Click <b>+ Add Field</b> &#8594; select <b>Text Area</b> &#8594; name it exactly: <code>Previous Addresses</code> &#8594; Save</div></div>
        <div class="ir"><div class="in">7</div><div class="it">Click <b>+ Add Field</b> &#8594; select <b>Text Area</b> &#8594; name it exactly: <code>Possible Relatives</code> &#8594; Save</div></div>
        <div class="ir"><div class="in">8</div><div class="it">You only do this once &#8212; all fields will be available for every future import</div></div>
      </div>
      <div class="instr">
        <div class="instr-label">Step 3 — Import the CSV into GHL</div>
        <div class="ir"><div class="in">1</div><div class="it">In GHL go to <b>Contacts</b> in the left sidebar</div></div>
        <div class="ir"><div class="in">2</div><div class="it">Click the <b>Import</b> icon (arrow pointing up) near the top right</div></div>
        <div class="ir"><div class="in">3</div><div class="it">Click <b>Select File</b> and choose <code>ghl-contacts.csv</code> from your Downloads folder</div></div>
        <div class="ir"><div class="in">4</div><div class="it">On the mapping screen these auto-map: <b>First Name, Last Name, Email, Phone, Address1, City, State, Postal Code</b></div></div>
        <div class="ir"><div class="in">5</div><div class="it">Manually map the rest using the dropdown: <code>Age</code> &#8594; Age, <code>Date of Birth</code> &#8594; Date of Birth, <code>Phone Details</code> &#8594; Phone Details, <code>Also Seen As</code> &#8594; Also Seen As, <code>Previous Addresses</code> &#8594; Previous Addresses, <code>Possible Relatives</code> &#8594; Possible Relatives</div></div>
        <div class="ir"><div class="in">6</div><div class="it">Click <b>Next</b> then <b>Import</b> &#8212; contacts appear in GHL within minutes</div></div>
      </div>
      <div class="btn-row">
        <button class="btn btn-green" onclick="downloadCSV()">&#11015; Download CSV</button>
      </div>
      <div id="download-msg"></div>
    </div>
  </div>
</div>

<script>
var queue = [];
var currentParsed = null;

// Month lookup for date sorting
var MONTHS = {jan:0,feb:1,mar:2,apr:3,may:4,jun:5,jul:6,aug:7,sep:8,oct:9,nov:10,dec:11};

document.getElementById('raw-text').addEventListener('input', function(){
  document.getElementById('char-count').textContent = this.value.length.toLocaleString() + ' characters';
});

// ── Parse date string like "Mar 2021" to Date for sorting ──────────────────
function parseReportedDate(str) {
  if (!str) return new Date(0);
  var m = str.match(/([A-Za-z]{3})\s+(\d{4})/);
  if (!m) return new Date(0);
  var mo = MONTHS[m[1].toLowerCase()];
  if (mo === undefined) return new Date(0);
  return new Date(parseInt(m[2]), mo, 1);
}

// ── Parse phones with full metadata ────────────────────────────────────────
function parsePhones(text) {
  // Rejoin area codes split across lines by TruePeopleSearch link formatting
  var rejoined = text.replace(/(\(\d{3}\))\s*\n\s*/g, '$1 ');
  var lines = rejoined.split('\n').map(function(l){ return l.trim(); });
  var phones = [];

  for (var i = 0; i < lines.length; i++) {
    var line = lines[i];

    // Match a phone number on this line: (XXX) XXX-XXXX
    var phoneMatch = line.match(/(\(\d{3}\)\s*\d{3}[-.\s]\d{4}|\d{3}[-.\s]\d{3}[-.\s]\d{4})/);
    if (!phoneMatch) continue;

    var number = phoneMatch[1].replace(/\s+/g,' ').trim();
    var cleanNumber = number.replace(/\D/g,'');

    // Extract type from same line after the number
    var afterNum = line.replace(phoneMatch[1],'').replace(/^\s*[-–]\s*/,'').trim();
    var type = '';
    var typeMatch = afterNum.match(/^(Wireless|Landline|Voip|VoIP|Mobile|Cell|Work|Home|Fax)/i);
    if (typeMatch) type = typeMatch[1];

    var isPossiblePrimary = false;
    var lastReported = '';
    var lastReportedDate = new Date(0);
    var carrier = '';

    // Look ahead through next lines to find metadata
    var j = i + 1;

    // Check for "Possible Primary Phone" label on next line
    if (j < lines.length && /possible\s+primary\s+phone/i.test(lines[j])) {
      isPossiblePrimary = true;
      j++;
    }

    // Check for "Last reported [Month] [Year]" line
    if (j < lines.length) {
      var lrMatch = lines[j].match(/Last\s+reported\s+([A-Za-z]{3,9}\s+\d{4})/i);
      if (lrMatch) {
        lastReported = lrMatch[1].trim();
        lastReportedDate = parseReportedDate(lastReported);
        j++;

        // Carrier is on the very next line after Last reported
        if (j < lines.length) {
          var nextLine = lines[j].trim();
          // Make sure it is not another phone number or a known label
          if (nextLine.length > 0
            && !/^\(?\d{3}\)?/.test(nextLine)
            && !/^last\s+reported/i.test(nextLine)
            && !/^possible\s+primary/i.test(nextLine)
            && !/^(wireless|landline|voip|mobile|cell)/i.test(nextLine)) {
            carrier = nextLine;
            j++;
          }
        }
      }
    }

    // Handle duplicates — keep the version with the most data
    var existingIdx = -1;
    for (var k = 0; k < phones.length; k++) {
      if (phones[k].cleanNumber === cleanNumber) { existingIdx = k; break; }
    }

    var phoneObj = {
      number: number,
      cleanNumber: cleanNumber,
      type: type,
      carrier: carrier,
      lastReported: lastReported,
      lastReportedDate: lastReportedDate,
      isPossiblePrimary: isPossiblePrimary
    };

    if (existingIdx === -1) {
      // New number — add it
      phones.push(phoneObj);
    } else if (lastReported && !phones[existingIdx].lastReported) {
      // Found a better version with metadata — replace the empty one
      phones[existingIdx] = phoneObj;
    }

    // Advance i to where we left off
    i = j - 1;
  }

  // Sort by most recent Last Reported date first
  phones.sort(function(a, b){
    return b.lastReportedDate.getTime() - a.lastReportedDate.getTime();
  });

  return phones;
}

// ── Main parser ─────────────────────────────────────────────────────────────
function parseProfile(text) {
  var result = {
    firstName:'', lastName:'', middleName:'', fullName:'',
    age:'', dateOfBirth:'',
    emails:[], phones:[],
    address1:'', city:'', state:'', zipCode:'',
    previousAddresses:[], relatives:[], aliases:[]
  };

  var lines = text.split('\n').map(function(l){ return l.trim(); }).filter(function(l){ return l.length > 0; });

  // Emails
  var emailMatches = text.match(/[\w.+-]+@[\w-]+\.[a-z]{2,}/gi) || [];
  result.emails = emailMatches.filter(function(v,i,a){ return a.indexOf(v)===i; });

  // Age
  var ageM = text.match(/\bAge[:\s]+(\d{1,3})\b/i);
  if (ageM) result.age = ageM[1];

  // DOB — handles "Born February 1947" and "Born February 15, 1947"
  var dobM = text.match(/\bBorn\s+((January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{1,2},?\s*\d{4}|(January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{4})/i);
  if (dobM) result.dateOfBirth = dobM[1].trim();

  // Phones with metadata
  result.phones = parsePhones(text);

  // Section extractor helper
  function extractSection(startKws, stopKws) {
    var tl = text.toLowerCase();
    var start = -1;
    for (var i=0; i<startKws.length; i++) {
      var idx = tl.indexOf(startKws[i].toLowerCase());
      if (idx !== -1) { start = idx + startKws[i].length; break; }
    }
    if (start === -1) return '';
    var end = text.length;
    for (var j=0; j<stopKws.length; j++) {
      var idx2 = tl.indexOf(stopKws[j].toLowerCase(), start);
      if (idx2 !== -1 && idx2 < end) end = idx2;
    }
    return text.slice(start, end).trim();
  }

  var stopWords = ['phone number','email address','current address','associated address',
    'previous address','possible relative','also seen as','also known as',
    'background','court record','social media','neighbor','property'];

  // Name — TruePeopleSearch always shows: [Name] then Age XX / Born [Month]
  // Use FIRST Age/Born line only — relatives may share the same age further down the page
  var knownHeaders = /^(possible|also\s+seen|also\s+known|background|phone|email|current|associated|previous|registered|search|lookup|reverse|people|court|social|neighbor|property|service)/i;

  var firstAgeIdx = -1;
  for (var i = 0; i < lines.length; i++) {
    if (/\bAge\s+\d{1,3}\b/i.test(lines[i]) ||
        /\bBorn\s+(January|February|March|April|May|June|July|August|September|October|November|December)/i.test(lines[i])) {
      firstAgeIdx = i;
      break; // stop at FIRST occurrence only
    }
  }

  if (firstAgeIdx > 0) {
    // Look backwards up to 4 lines from first Age/Born for the name
    for (var k = firstAgeIdx - 1; k >= Math.max(0, firstAgeIdx - 4) && !result.fullName; k--) {
      var candidate = lines[k];
      if (!candidate || candidate.length < 3 || candidate.length > 70) continue;
      if (/^\d/.test(candidate)) continue;
      if (/,/.test(candidate)) continue;
      if (/http|www\.|\.com/i.test(candidate)) continue;
      if (knownHeaders.test(candidate)) continue;

      var totalWords = candidate.split(/\s+/);
      if (totalWords.length < 2 || totalWords.length > 5) continue;

      var nameWords = totalWords.filter(function(w) {
        return /^[A-Z][a-z]{1,}$/.test(w) || /^[A-Z]{3,}$/.test(w);
      });

      if (nameWords.length === totalWords.length) {
        var tc = nameWords.map(function(w) {
          return w.charAt(0).toUpperCase() + w.slice(1).toLowerCase();
        });
        result.fullName = tc.join(' ');
        result.firstName = tc[0] || '';
        result.lastName = tc[tc.length - 1] || '';
        if (tc.length >= 3) result.middleName = tc.slice(1, -1).join(' ');
      }
    }
  }

  // Also Seen As
  var aliasSection = extractSection(
    ['also seen as','also known as','aliases','other names'],
    stopWords.filter(function(s){ return s.indexOf('also')===-1; })
  );
  if (aliasSection) {
    var cleaned = aliasSection.replace(/([a-z])([A-Z])/g,'$1\n$2');
    cleaned.split('\n').map(function(l){ return l.trim(); }).filter(function(l){ return l.length>2; }).forEach(function(line){
      if (/^includes\s/i.test(line)) return;
      if (/\d/.test(line)) return;
      if (/search|lookup|services|reverse|privacy|terms|background/i.test(line)) return;
      line.split(',').map(function(s){ return s.trim(); }).filter(function(s){ return s.length>2; }).forEach(function(name){
        var ws = name.split(/\s+/);
        if (ws.length>=2 && ws.length<=6 && ws.every(function(w){ return /^[A-Z][a-zA-Z'-]*\.?$/.test(w); }) && result.aliases.indexOf(name)===-1)
          result.aliases.push(name);
      });
    });
  }

  // Current address
  var addrSection = extractSection(
    ['current address','current home address'],
    ['associated address','previous address','possible relative','phone number','email','also seen','also known']
  );
  if (addrSection) {
    var addrLines = addrSection.split('\n').map(function(l){ return l.trim(); }).filter(function(l){ return l; });
    var streetLine = addrLines.find ? addrLines.find(function(l){ return /^\d+\s+\w/.test(l); }) : null;
    if (!streetLine) { for(var k=0;k<addrLines.length;k++){ if(/^\d+\s+\w/.test(addrLines[k])){ streetLine=addrLines[k]; break; } } }
    if (streetLine) result.address1 = streetLine;
    var czLine = null;
    for(var k=0;k<addrLines.length;k++){ if(/[A-Z]{2}\s+\d{5}|,\s*[A-Z]{2}/.test(addrLines[k])){ czLine=addrLines[k]; break; } }
    if (czLine) {
      var zipM2 = czLine.match(/\b(\d{5}(-\d{4})?)\b/);
      if (zipM2) result.zipCode = zipM2[1];
      var stM = czLine.match(/\b([A-Z]{2})\b/);
      if (stM) result.state = stM[1];
      var cityM = czLine.replace(/\b[A-Z]{2}\b.*/,'').replace(/,/g,'').trim();
      if (cityM) result.city = cityM;
    }
  }

  // Previous addresses
  var prevSection = extractSection(
    ['associated addresses','previous addresses','past addresses','former addresses'],
    ['possible relative','phone number','email','background','court','social media']
  );
  if (prevSection) {
    var cur = '';
    prevSection.split('\n').map(function(l){ return l.trim(); }).filter(function(l){ return l; }).forEach(function(line){
      if (/^\d+\s+\w/.test(line)) { cur = line; }
      else if (/[A-Z]{2}\s+\d{5}|,\s*[A-Z]{2}/.test(line) && cur) { result.previousAddresses.push(cur+', '+line.trim()); cur=''; }
      else if (/[A-Z]{2}\s+\d{5}|,\s*[A-Z]{2}/.test(line)) { result.previousAddresses.push(line.trim()); }
    });
    if (cur && result.previousAddresses.indexOf(cur)===-1) result.previousAddresses.push(cur);
  }

  // Relatives
  var relSection = extractSection(
    ['possible relatives','known associates','associated people'],
    ['phone number','email','address','background','court','social media','neighbor']
  );
  if (relSection) {
    relSection.split('\n').map(function(l){ return l.trim(); }).filter(function(l){ return l.length>2; }).forEach(function(line){
      if (/^age\s*\d/i.test(line)||/^\d/.test(line)) return;
      if (/search|lookup|services|reverse|privacy|terms|background/i.test(line)) return;
      var ws = line.split(/\s+/);
      if (ws.length>=2 && ws.length<=6 && /^[A-Za-z]/.test(line) && result.relatives.indexOf(line)===-1)
        result.relatives.push(line);
    });
  }

  return result;
}

// ── Extract ─────────────────────────────────────────────────────────────────
function extractData() {
  var raw = document.getElementById('raw-text').value.trim();
  if (!raw) { showMsg('paste-msg','error','Please paste the profile text first.'); return; }
  if (raw.length < 100) { showMsg('paste-msg','error','Text seems too short — press Ctrl+A to select the whole page before copying.'); return; }
  var btn = document.getElementById('extract-btn');
  btn.disabled = true; btn.innerHTML = '<span class="spinner"></span> Extracting...';
  setTimeout(function(){
    currentParsed = parseProfile(raw);
    renderDataGrid(currentParsed);
    document.getElementById('review-name').textContent = currentParsed.fullName ? '— '+currentParsed.fullName : '';
    show('screen-review');
    btn.disabled = false; btn.innerHTML = '&#9889; Extract info';
  }, 250);
}

// ── Render data grid ─────────────────────────────────────────────────────────
function renderDataGrid(data) {
  var grid = document.getElementById('data-grid');
  grid.innerHTML = '';

  function addField(label, val, full) {
    if (!val || (Array.isArray(val) && !val.length)) return;
    var d = document.createElement('div'); d.className = 'df'+(full?' full':'');
    var k = document.createElement('div'); k.className='dk'; k.textContent=label;
    var v = document.createElement('div'); v.className='dv';
    if (Array.isArray(val)) { val.forEach(function(item){ var t=document.createElement('span'); t.className='tag'; t.textContent=item; v.appendChild(t); }); }
    else { v.textContent=val; }
    d.appendChild(k); d.appendChild(v); grid.appendChild(d);
  }

  addField('Full name', data.fullName);
  addField('First name', data.firstName);
  addField('Last name', data.lastName);
  addField('Middle name', data.middleName);
  addField('Age', data.age);
  addField('Date of birth', data.dateOfBirth);
  addField('Email', data.emails.length ? data.emails.join(', ') : '', true);
  addField('Address', data.address1);
  addField('City', data.city);
  addField('State', data.state);
  addField('ZIP', data.zipCode);
  addField('Also seen as', data.aliases, true);
  addField('Previous addresses', data.previousAddresses, true);
  addField('Possible relatives', data.relatives, true);

  // Phones — show each as a block
  if (data.phones.length) {
    var d = document.createElement('div'); d.className='df full';
    var k = document.createElement('div'); k.className='dk'; k.textContent='Phone numbers (sorted most recent first)';
    d.appendChild(k);
    data.phones.forEach(function(p, i){
      var pb = document.createElement('div'); pb.className='phone-block';
      var pn = document.createElement('div'); pn.className='phone-num';
      pn.textContent = (i===0?'★ ':'')+p.number+(p.type?' — '+p.type:'')+(p.isPossiblePrimary?' [Possible Primary]':'');
      var pm = document.createElement('div'); pm.className='phone-meta';
      pm.textContent = [p.carrier, p.lastReported?'Last reported '+p.lastReported:''].filter(Boolean).join(' · ');
      pb.appendChild(pn); if(pm.textContent) pb.appendChild(pm);
      d.appendChild(pb);
    });
    grid.appendChild(d);
  }
}

// ── Queue ───────────────────────────────────────────────────────────────────
function addToQueue() {
  if (!currentParsed) return;
  queue.push(JSON.parse(JSON.stringify(currentParsed)));
  renderQueue();
  show('screen-queue');
  document.getElementById('raw-text').value='';
  document.getElementById('char-count').textContent='0 characters';
  document.getElementById('paste-msg').innerHTML='';
  currentParsed=null;
}

function renderQueue() {
  var list = document.getElementById('queue-list');
  list.innerHTML='';
  document.getElementById('queue-count').textContent=queue.length;
  queue.forEach(function(p,i){
    var item=document.createElement('div'); item.className='qi';
    var info=document.createElement('div');
    var name=document.createElement('div'); name.className='qi-name'; name.textContent=p.fullName||'Unknown';
    var detail=document.createElement('div'); detail.className='qi-detail';
    var ph = p.phones.length ? p.phones[0].number : '';
    detail.textContent=[ph, p.city&&p.state?p.city+', '+p.state:''].filter(Boolean).join(' · ');
    info.appendChild(name); info.appendChild(detail);
    var btn=document.createElement('button'); btn.className='qi-rm'; btn.textContent='×';
    btn.onclick=(function(idx){ return function(){ queue.splice(idx,1); renderQueue(); if(!queue.length) show('screen-paste'); }; })(i);
    item.appendChild(info); item.appendChild(btn); list.appendChild(item);
  });
}

// ── CSV Download ─────────────────────────────────────────────────────────────
function downloadCSV() {
  if (!queue.length) { showMsg('download-msg','error','No profiles in queue.'); return; }

  // Find max phone count across all contacts
  var maxPhones = 0;
  queue.forEach(function(p){ if(p.phones.length>maxPhones) maxPhones=p.phones.length; });
  if (maxPhones===0) maxPhones=1;

  // Build headers — one column per phone number (no limit)
  var headers = ['First Name','Last Name','Email','Address1','City','State','Postal Code','Date of Birth','Age'];

  // Phone column headers: Phone 1, Phone 1 Type, Phone 1 Carrier, Phone 1 Last Reported, Phone 2...
  for (var i=1; i<=maxPhones; i++) {
    headers.push('Phone '+i);
    headers.push('Phone '+i+' Type');
    headers.push('Phone '+i+' Carrier');
    headers.push('Phone '+i+' Last Reported');
  }

  headers.push('Phone Details');
  headers.push('Also Seen As');
  headers.push('Previous Addresses');
  headers.push('Possible Relatives');
  headers.push('Source');

  var rows = queue.map(function(p){
    var cells = [
      ce(p.firstName),
      ce(p.lastName),
      ce(p.emails[0]||''),
      ce(p.address1),
      ce(p.city),
      ce(p.state),
      ce(p.zipCode),
      ce(p.dateOfBirth),
      ce(p.age)
    ];

    // Each phone in its own set of 4 columns
    for (var i=0; i<maxPhones; i++) {
      var ph = p.phones[i];
      if (ph) {
        cells.push(ce(ph.cleanNumber));  // digits only for GHL phone field
        cells.push(ce(ph.type));
        cells.push(ce(ph.carrier));
        cells.push(ce(ph.lastReported));
      } else {
        cells.push('','','','');
      }
    }

    // Phone Details — full summary, one per line
    var phoneDetails = p.phones.map(function(ph,idx){
      return [
        (idx===0?'[Primary] ':'')+ph.number,
        ph.type||'',
        ph.isPossiblePrimary?'Possible Primary':'',
        ph.carrier||'',
        ph.lastReported?'Last reported '+ph.lastReported:''
      ].filter(Boolean).join(' — ');
    }).join('\n');

    cells.push(cm(phoneDetails ? [phoneDetails] : []));
    cells.push(cm(p.aliases));
    cells.push(cm(p.previousAddresses));
    cells.push(cm(p.relatives));
    cells.push('TruePeopleSearch');

    return cells.join(',');
  });

  var csv = [headers.join(',')].concat(rows).join('\n');
  var blob = new Blob(['\uFEFF'+csv], {type:'text/csv;charset=utf-8;'});
  var url = URL.createObjectURL(blob);
  var a = document.createElement('a');
  a.href=url; a.download='ghl-contacts.csv'; a.click();
  URL.revokeObjectURL(url);
  showMsg('download-msg','success','&#10003; ghl-contacts.csv downloaded. Follow Steps 2 and 3 above to import into GHL.');
}

// CSV escape — plain value
function ce(val) {
  if (val===null||val===undefined||val==='') return '';
  var s = String(val);
  if (s.indexOf(',')!==-1||s.indexOf('"')!==-1||s.indexOf('\n')!==-1)
    return '"'+s.replace(/"/g,'""')+'"';
  return s;
}

// CSV multiline — array to quoted multiline cell
function cm(arr) {
  if (!arr||!arr.length) return '';
  var s = arr.join('\n').replace(/"/g,'""');
  return '"'+s+'"';
}

// ── Navigation ───────────────────────────────────────────────────────────────
function show(id) {
  ['screen-paste','screen-review','screen-queue'].forEach(function(s){
    document.getElementById(s).style.display = s===id?'block':'none';
  });
  if (id==='screen-paste' && queue.length>0)
    document.getElementById('screen-queue').style.display='block';
}

function goBackToPaste() {
  document.getElementById('screen-paste').style.display='block';
  document.getElementById('screen-review').style.display='none';
}

function showMsg(id,type,text) {
  document.getElementById(id).innerHTML='<div class="alert alert-'+type+'">'+text+'</div>';
}
</script>
</body>
</html>
