<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Barangay Mojon — Document Request System</title>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --navy:    #1B2A4A;
      --navy-lt: #243660;
      --gold:    #C8A84B;
      --gold-lt: #E2C97A;
      --cream:   #F8F6F1;
      --steel:   #4A6080;
      --border:  #D0D9E8;
      --gray:    #E8EDF4;
      --text:    #1B2A4A;
      --muted:   #6B7A99;
      --green:   #2E7D52;
      --red:     #C0392B;
    }

    body {
      font-family: 'Inter', sans-serif;
      background: var(--cream);
      color: var(--text);
      min-height: 100vh;
    }

    /* ── HEADER ── */
    header {
      background: var(--navy);
      color: #fff;
      padding: 0;
      position: sticky;
      top: 0;
      z-index: 100;
      box-shadow: 0 2px 12px rgba(0,0,0,.25);
    }
    .header-band {
      background: var(--gold);
      height: 4px;
    }
    .header-inner {
      max-width: 1100px;
      margin: 0 auto;
      padding: 18px 24px;
      display: flex;
      align-items: center;
      gap: 20px;
    }
    .seal {
      width: 64px;
      height: 64px;
      flex-shrink: 0;
      background: var(--gold);
      border-radius: 50%;
      border: 3px solid #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 26px;
      color: var(--navy);
      font-weight: 700;
      box-shadow: 0 0 0 3px var(--gold);
    }
    .header-text h1 {
      font-family: 'Playfair Display', serif;
      font-size: 1.35rem;
      letter-spacing: .02em;
      line-height: 1.2;
    }
    .header-text p {
      font-size: .78rem;
      color: var(--gold-lt);
      margin-top: 2px;
      letter-spacing: .06em;
      text-transform: uppercase;
    }

    /* ── NAV TABS ── */
    .nav-tabs {
      background: var(--navy-lt);
      display: flex;
      max-width: 1100px;
      margin: 0 auto;
      padding: 0 24px;
      gap: 4px;
    }
    .nav-tab {
      padding: 10px 20px;
      font-size: .82rem;
      font-weight: 500;
      color: rgba(255,255,255,.65);
      cursor: pointer;
      border-bottom: 3px solid transparent;
      transition: all .2s;
      user-select: none;
      letter-spacing: .03em;
    }
    .nav-tab:hover { color: #fff; }
    .nav-tab.active { color: var(--gold); border-bottom-color: var(--gold); }

    /* ── LAYOUT ── */
    .main-wrap {
      max-width: 1100px;
      margin: 36px auto;
      padding: 0 24px;
      display: grid;
      grid-template-columns: 300px 1fr;
      gap: 28px;
      align-items: start;
    }

    /* ── SIDEBAR ── */
    .sidebar { display: flex; flex-direction: column; gap: 20px; }

    .card {
      background: #fff;
      border-radius: 12px;
      border: 1px solid var(--border);
      overflow: hidden;
    }
    .card-head {
      background: var(--navy);
      color: #fff;
      padding: 14px 18px;
      font-family: 'Playfair Display', serif;
      font-size: .95rem;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .card-head span { font-size: 1rem; }
    .card-body { padding: 16px 18px; }

    .office-info p {
      font-size: .82rem;
      color: var(--steel);
      line-height: 1.8;
    }
    .office-info strong { color: var(--navy); font-weight: 600; }

    .doc-list { list-style: none; }
    .doc-list li {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 9px 0;
      border-bottom: 1px solid var(--gray);
      font-size: .82rem;
      color: var(--steel);
      cursor: pointer;
      transition: color .15s;
    }
    .doc-list li:last-child { border-bottom: none; }
    .doc-list li:hover { color: var(--navy); }
    .doc-list li .dot {
      width: 8px; height: 8px;
      border-radius: 50%;
      background: var(--gold);
      flex-shrink: 0;
    }

    .fee-table { width: 100%; border-collapse: collapse; font-size: .8rem; }
    .fee-table th {
      background: var(--gray);
      color: var(--navy);
      padding: 8px 10px;
      text-align: left;
      font-weight: 600;
    }
    .fee-table td {
      padding: 8px 10px;
      border-bottom: 1px solid var(--gray);
      color: var(--steel);
    }
    .fee-table tr:last-child td { border-bottom: none; }
    .fee-table .gold { color: var(--gold); font-weight: 600; }

    /* ── MAIN FORM AREA ── */
    .content-area { display: flex; flex-direction: column; gap: 24px; }

    /* Tab Panels */
    .tab-panel { display: none; }
    .tab-panel.active { display: block; }

    .section-title {
      font-family: 'Playfair Display', serif;
      font-size: 1.4rem;
      color: var(--navy);
      margin-bottom: 6px;
    }
    .section-sub {
      font-size: .83rem;
      color: var(--muted);
      margin-bottom: 22px;
    }

    /* Form styles */
    .form-card {
      background: #fff;
      border-radius: 12px;
      border: 1px solid var(--border);
      padding: 28px 28px;
    }

    .form-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 18px;
      margin-bottom: 18px;
    }
    .form-row.single { grid-template-columns: 1fr; }
    .form-row.triple { grid-template-columns: 1fr 1fr 1fr; }

    .field { display: flex; flex-direction: column; gap: 5px; }
    .field label {
      font-size: .78rem;
      font-weight: 600;
      color: var(--navy);
      letter-spacing: .04em;
      text-transform: uppercase;
    }
    .field label .req { color: var(--red); margin-left: 2px; }
    .field input,
    .field select,
    .field textarea {
      border: 1.5px solid var(--border);
      border-radius: 8px;
      padding: 10px 13px;
      font-family: 'Inter', sans-serif;
      font-size: .88rem;
      color: var(--text);
      background: var(--cream);
      transition: border-color .2s, box-shadow .2s;
      outline: none;
      appearance: none;
    }
    .field input:focus,
    .field select:focus,
    .field textarea:focus {
      border-color: var(--navy);
      box-shadow: 0 0 0 3px rgba(27,42,74,.1);
      background: #fff;
    }
    .field textarea { resize: vertical; min-height: 90px; }
    .field select {
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%234A6080' stroke-width='1.5' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
      background-repeat: no-repeat;
      background-position: right 12px center;
      padding-right: 36px;
    }

    .divider {
      border: none;
      border-top: 1px solid var(--gray);
      margin: 22px 0;
    }
    .subsection-label {
      font-size: .75rem;
      font-weight: 700;
      letter-spacing: .08em;
      text-transform: uppercase;
      color: var(--gold);
      margin-bottom: 14px;
    }

    /* Doc type selector */
    .doc-type-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 12px;
      margin-bottom: 20px;
    }
    .doc-type-btn {
      border: 2px solid var(--border);
      border-radius: 10px;
      padding: 14px 10px;
      text-align: center;
      cursor: pointer;
      transition: all .2s;
      background: var(--cream);
      user-select: none;
    }
    .doc-type-btn .icon { font-size: 1.5rem; margin-bottom: 6px; }
    .doc-type-btn .label { font-size: .75rem; font-weight: 600; color: var(--navy); line-height: 1.3; }
    .doc-type-btn .fee { font-size: .7rem; color: var(--muted); margin-top: 3px; }
    .doc-type-btn:hover { border-color: var(--steel); background: #fff; }
    .doc-type-btn.selected {
      border-color: var(--navy);
      background: #fff;
      box-shadow: 0 0 0 3px rgba(27,42,74,.1);
    }
    .doc-type-btn.selected .label { color: var(--navy); }

    /* Purpose checkboxes */
    .purpose-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
      margin-bottom: 16px;
    }
    .purpose-item {
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: .82rem;
      color: var(--steel);
      cursor: pointer;
      padding: 8px 10px;
      border: 1.5px solid var(--border);
      border-radius: 7px;
      transition: all .2s;
    }
    .purpose-item:hover { border-color: var(--steel); color: var(--navy); }
    .purpose-item input[type="checkbox"] { accent-color: var(--navy); width: 15px; height: 15px; }
    .purpose-item.checked { border-color: var(--navy); background: var(--gray); color: var(--navy); }

    /* CTA button */
    .btn-primary {
      background: var(--navy);
      color: #fff;
      border: none;
      border-radius: 9px;
      padding: 13px 32px;
      font-family: 'Inter', sans-serif;
      font-size: .9rem;
      font-weight: 600;
      cursor: pointer;
      transition: background .2s, transform .1s;
      display: inline-flex;
      align-items: center;
      gap: 8px;
      letter-spacing: .02em;
    }
    .btn-primary:hover { background: var(--navy-lt); }
    .btn-primary:active { transform: scale(.98); }
    .btn-gold {
      background: var(--gold);
      color: var(--navy);
    }
    .btn-gold:hover { background: var(--gold-lt); }

    .btn-outline {
      background: transparent;
      color: var(--navy);
      border: 1.5px solid var(--navy);
      border-radius: 9px;
      padding: 12px 24px;
      font-family: 'Inter', sans-serif;
      font-size: .88rem;
      font-weight: 500;
      cursor: pointer;
      transition: all .2s;
    }
    .btn-outline:hover { background: var(--gray); }

    .form-actions {
      display: flex;
      justify-content: flex-end;
      gap: 12px;
      margin-top: 26px;
      padding-top: 20px;
      border-top: 1px solid var(--gray);
    }

    /* ── MODAL / OVERLAY ── */
    .modal-overlay {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,.55);
      z-index: 200;
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      pointer-events: none;
      transition: opacity .3s;
    }
    .modal-overlay.open { opacity: 1; pointer-events: all; }
    .modal {
      background: #fff;
      border-radius: 16px;
      padding: 40px 36px;
      max-width: 460px;
      width: 90%;
      text-align: center;
      transform: scale(.92);
      transition: transform .3s;
      position: relative;
    }
    .modal-overlay.open .modal { transform: scale(1); }

    .stamp-anim {
      font-size: 64px;
      animation: stampIn .5s cubic-bezier(.2,1.4,.5,1) forwards;
      transform-origin: center;
    }
    @keyframes stampIn {
      0%   { transform: scale(3) rotate(-15deg); opacity: 0; }
      100% { transform: scale(1) rotate(0deg);   opacity: 1; }
    }
    .modal h2 {
      font-family: 'Playfair Display', serif;
      font-size: 1.4rem;
      color: var(--navy);
      margin: 16px 0 8px;
    }
    .modal p { font-size: .88rem; color: var(--muted); line-height: 1.6; }
    .ref-box {
      background: var(--gray);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 12px 16px;
      margin: 18px 0;
      font-size: 1rem;
      font-weight: 700;
      color: var(--navy);
      letter-spacing: .08em;
    }
    .modal-close {
      margin-top: 8px;
    }

    /* ── TRACKER ── */
    .tracker-form {
      display: flex;
      gap: 12px;
      margin-bottom: 24px;
      flex-wrap: wrap;
    }
    .tracker-form input {
      flex: 1;
      min-width: 180px;
      border: 1.5px solid var(--border);
      border-radius: 8px;
      padding: 11px 14px;
      font-family: 'Inter', sans-serif;
      font-size: .88rem;
      outline: none;
    }
    .tracker-form input:focus {
      border-color: var(--navy);
      box-shadow: 0 0 0 3px rgba(27,42,74,.1);
    }

    .status-timeline {
      position: relative;
      padding-left: 28px;
    }
    .status-timeline::before {
      content: '';
      position: absolute;
      left: 9px; top: 0; bottom: 0;
      width: 2px;
      background: var(--gray);
    }
    .timeline-item {
      position: relative;
      padding: 0 0 24px 20px;
    }
    .timeline-item:last-child { padding-bottom: 0; }
    .timeline-dot {
      position: absolute;
      left: -19px;
      top: 2px;
      width: 18px; height: 18px;
      border-radius: 50%;
      background: var(--gray);
      border: 2px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: .6rem;
    }
    .timeline-dot.done { background: var(--green); border-color: var(--green); color: #fff; }
    .timeline-dot.current { background: var(--gold); border-color: var(--gold); color: var(--navy); animation: pulse 1.5s infinite; }
    @keyframes pulse {
      0%,100% { box-shadow: 0 0 0 0 rgba(200,168,75,.5); }
      50%      { box-shadow: 0 0 0 7px rgba(200,168,75,0); }
    }
    .timeline-title { font-weight: 600; font-size: .88rem; color: var(--navy); }
    .timeline-sub { font-size: .78rem; color: var(--muted); margin-top: 2px; }

    /* ── RECORDS TABLE ── */
    .records-table { width: 100%; border-collapse: collapse; font-size: .83rem; }
    .records-table th {
      background: var(--navy);
      color: #fff;
      padding: 11px 14px;
      text-align: left;
      font-weight: 500;
      letter-spacing: .03em;
    }
    .records-table td {
      padding: 10px 14px;
      border-bottom: 1px solid var(--gray);
      color: var(--steel);
    }
    .records-table tr:last-child td { border-bottom: none; }
    .records-table tr:hover td { background: var(--cream); }
    .badge {
      display: inline-block;
      padding: 3px 9px;
      border-radius: 20px;
      font-size: .72rem;
      font-weight: 600;
      letter-spacing: .04em;
    }
    .badge-pending  { background: #FEF3C7; color: #92400E; }
    .badge-ready    { background: #D1FAE5; color: #065F46; }
    .badge-released { background: #DBEAFE; color: #1E3A8A; }
    .badge-processing { background: #EDE9FE; color: #5B21B6; }

    /* ── INFO PANEL ── */
    .info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
    .info-item {
      background: #fff;
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 18px;
    }
    .info-item .icon { font-size: 1.6rem; margin-bottom: 8px; }
    .info-item h4 { font-size: .88rem; font-weight: 700; color: var(--navy); margin-bottom: 4px; }
    .info-item p { font-size: .78rem; color: var(--muted); line-height: 1.6; }

    /* ── NOTICE BAR ── */
    .notice {
      background: #FEF9EC;
      border: 1px solid #F3D97A;
      border-radius: 9px;
      padding: 12px 16px;
      font-size: .82rem;
      color: #7A5C00;
      display: flex;
      gap: 10px;
      align-items: flex-start;
      margin-bottom: 20px;
    }
    .notice span { flex-shrink: 0; font-size: 1rem; }

    /* Footer */
    footer {
      background: var(--navy);
      color: rgba(255,255,255,.55);
      text-align: center;
      padding: 20px;
      font-size: .77rem;
      margin-top: 48px;
      letter-spacing: .04em;
    }
    footer strong { color: var(--gold); }

    /* ── RESPONSIVE ── */
    @media (max-width: 768px) {
      .main-wrap { grid-template-columns: 1fr; }
      .sidebar { order: 2; }
      .content-area { order: 1; }
      .form-row { grid-template-columns: 1fr; }
      .form-row.triple { grid-template-columns: 1fr 1fr; }
      .doc-type-grid { grid-template-columns: repeat(2, 1fr); }
      .purpose-grid { grid-template-columns: 1fr; }
      .info-grid { grid-template-columns: 1fr; }
    }
    @media (max-width: 480px) {
      .header-inner { padding: 12px 16px; gap: 14px; }
      .seal { width: 50px; height: 50px; font-size: 20px; }
      .header-text h1 { font-size: 1.05rem; }
      .form-card { padding: 18px 16px; }
      .doc-type-grid { grid-template-columns: 1fr 1fr; }
    }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { animation: none !important; transition: none !important; }
    }
  </style>
</head>
<body>

<!-- HEADER -->
<header>
  <div class="header-band"></div>
  <div class="header-inner">
    <div class="seal">🏛</div>
    <div class="header-text">
      <h1>Barangay Mojon</h1>
      <p>Malolos City, Bulacan &nbsp;·&nbsp; Document Request System</p>
    </div>
  </div>
  <div class="nav-tabs">
    <div class="nav-tab active" onclick="switchTab('request')">📋 Request</div>
    <div class="nav-tab" onclick="switchTab('track')">🔍 Track</div>
    <div class="nav-tab" onclick="switchTab('records')">📁 Records</div>
    <div class="nav-tab" onclick="switchTab('info')">ℹ️ Information</div>
  </div>
</header>

<!-- MAIN -->
<div class="main-wrap">

  <!-- SIDEBAR -->
  <aside class="sidebar">

    <div class="card">
      <div class="card-head"><span>📍</span> Barangay Hall</div>
      <div class="card-body office-info">
        <p>
          <strong>Barangay Mojon</strong><br>
          Malolos City, Bulacan<br><br>
          <strong>Office Hours</strong><br>
          Mon – Fri: 8:00 AM – 5:00 PM<br>
          Saturday: 8:00 AM – 12:00 PM<br><br>
          <strong>Contact</strong><br>
          (044) 919-0000<br>
          brgy.mojon@malolos.gov.ph
        </p>
      </div>
    </div>

    <div class="card">
      <div class="card-head"><span>📄</span> Available Documents</div>
      <div class="card-body">
        <ul class="doc-list">
          <li><span class="dot"></span>Barangay Clearance</li>
          <li><span class="dot"></span>Certificate of Residency</li>
          <li><span class="dot"></span>Certificate of Indigency</li>
          <li><span class="dot"></span>Business Clearance</li>
          <li><span class="dot"></span>Certificate of Good Moral</li>
          <li><span class="dot"></span>Barangay ID</li>
          <li><span class="dot"></span>Solo Parent Certificate</li>
          <li><span class="dot"></span>Certificate of No Income</li>
        </ul>
      </div>
    </div>

    <div class="card">
      <div class="card-head"><span>💰</span> Processing Fees</div>
      <div class="card-body">
        <table class="fee-table">
          <thead>
            <tr><th>Document</th><th>Fee</th></tr>
          </thead>
          <tbody>
            <tr><td>Barangay Clearance</td><td class="gold">₱50</td></tr>
            <tr><td>Cert. of Residency</td><td class="gold">₱30</td></tr>
            <tr><td>Cert. of Indigency</td><td class="gold">Free</td></tr>
            <tr><td>Business Clearance</td><td class="gold">₱100</td></tr>
            <tr><td>Cert. Good Moral</td><td class="gold">₱30</td></tr>
            <tr><td>Barangay ID</td><td class="gold">₱50</td></tr>
            <tr><td>Solo Parent Cert.</td><td class="gold">Free</td></tr>
            <tr><td>Cert. No Income</td><td class="gold">Free</td></tr>
          </tbody>
        </table>
      </div>
    </div>

  </aside>

  <!-- CONTENT AREA -->
  <main class="content-area">

    <!-- ═══ TAB: REQUEST ═══ -->
    <div class="tab-panel active" id="tab-request">
      <h2 class="section-title">Document Request Form</h2>
      <p class="section-sub">Fill in the information below to request an official Barangay document. All fields marked with <span style="color:var(--red)">*</span> are required.</p>

      <div class="notice">
        <span>⚠️</span>
        Please bring a valid government-issued ID and the original documents when claiming. Processing time is 1–3 working days.
      </div>

      <div class="form-card">
        <!-- DOCUMENT TYPE -->
        <div class="subsection-label">Select Document Type</div>
        <div class="doc-type-grid" id="docTypeGrid">
          <div class="doc-type-btn selected" onclick="selectDoc(this,'Barangay Clearance')">
            <div class="icon">📋</div>
            <div class="label">Barangay Clearance</div>
            <div class="fee">₱50</div>
          </div>
          <div class="doc-type-btn" onclick="selectDoc(this,'Certificate of Residency')">
            <div class="icon">🏡</div>
            <div class="label">Certificate of Residency</div>
            <div class="fee">₱30</div>
          </div>
          <div class="doc-type-btn" onclick="selectDoc(this,'Certificate of Indigency')">
            <div class="icon">🤝</div>
            <div class="label">Certificate of Indigency</div>
            <div class="fee">Free</div>
          </div>
          <div class="doc-type-btn" onclick="selectDoc(this,'Business Clearance')">
            <div class="icon">🏪</div>
            <div class="label">Business Clearance</div>
            <div class="fee">₱100</div>
          </div>
          <div class="doc-type-btn" onclick="selectDoc(this,'Certificate of Good Moral')">
            <div class="icon">⭐</div>
            <div class="label">Certificate of Good Moral</div>
            <div class="fee">₱30</div>
          </div>
          <div class="doc-type-btn" onclick="selectDoc(this,'Barangay ID')">
            <div class="icon">🪪</div>
            <div class="label">Barangay ID</div>
            <div class="fee">₱50</div>
          </div>
          <div class="doc-type-btn" onclick="selectDoc(this,'Solo Parent Certificate')">
            <div class="icon">👨‍👧</div>
            <div class="label">Solo Parent Certificate</div>
            <div class="fee">Free</div>
          </div>
          <div class="doc-type-btn" onclick="selectDoc(this,'Certificate of No Income')">
            <div class="icon">📝</div>
            <div class="label">Certificate of No Income</div>
            <div class="fee">Free</div>
          </div>
          <div class="doc-type-btn" onclick="selectDoc(this,'Other Document')">
            <div class="icon">📦</div>
            <div class="label">Other Document</div>
            <div class="fee">Varies</div>
          </div>
        </div>
        <input type="hidden" id="selectedDoc" value="Barangay Clearance" />

        <hr class="divider" />

        <!-- PERSONAL INFO -->
        <div class="subsection-label">Personal Information</div>
        <div class="form-row triple">
          <div class="field">
            <label>Last Name <span class="req">*</span></label>
            <input type="text" id="lastName" placeholder="Dela Cruz" />
          </div>
          <div class="field">
            <label>First Name <span class="req">*</span></label>
            <input type="text" id="firstName" placeholder="Juan" />
          </div>
          <div class="field">
            <label>Middle Name</label>
            <input type="text" id="middleName" placeholder="Santos" />
          </div>
        </div>
        <div class="form-row">
          <div class="field">
            <label>Date of Birth <span class="req">*</span></label>
            <input type="date" id="dob" />
          </div>
          <div class="field">
            <label>Gender <span class="req">*</span></label>
            <select id="gender">
              <option value="">— Select —</option>
              <option>Male</option>
              <option>Female</option>
              <option>Prefer not to say</option>
            </select>
          </div>
        </div>
        <div class="form-row">
          <div class="field">
            <label>Civil Status <span class="req">*</span></label>
            <select id="civilStatus">
              <option value="">— Select —</option>
              <option>Single</option>
              <option>Married</option>
              <option>Widowed</option>
              <option>Separated</option>
            </select>
          </div>
          <div class="field">
            <label>Citizenship</label>
            <input type="text" id="citizenship" value="Filipino" />
          </div>
        </div>

        <hr class="divider" />

        <!-- CONTACT & ADDRESS -->
        <div class="subsection-label">Address & Contact</div>
        <div class="form-row single">
          <div class="field">
            <label>Complete Address in Barangay Mojon <span class="req">*</span></label>
            <input type="text" id="address" placeholder="House No., Street, Purok/Zone, Barangay Mojon, Malolos City, Bulacan" />
          </div>
        </div>
        <div class="form-row">
          <div class="field">
            <label>Years of Residency <span class="req">*</span></label>
            <input type="number" id="yearsResident" placeholder="e.g. 5" min="0" />
          </div>
          <div class="field">
            <label>Mobile Number <span class="req">*</span></label>
            <input type="tel" id="mobile" placeholder="09XX-XXX-XXXX" />
          </div>
        </div>
        <div class="form-row">
          <div class="field">
            <label>Email Address</label>
            <input type="email" id="email" placeholder="you@email.com" />
          </div>
          <div class="field">
            <label>Valid ID Type <span class="req">*</span></label>
            <select id="validId">
              <option value="">— Select ID —</option>
              <option>PhilSys / National ID</option>
              <option>Driver's License</option>
              <option>Passport</option>
              <option>SSS / GSIS Card</option>
              <option>PhilHealth Card</option>
              <option>Voter's ID</option>
              <option>Postal ID</option>
              <option>Barangay Certification</option>
            </select>
          </div>
        </div>

        <hr class="divider" />

        <!-- PURPOSE -->
        <div class="subsection-label">Purpose of Request</div>
        <div class="purpose-grid">
          <label class="purpose-item" id="pur1">
            <input type="checkbox" onchange="togglePurpose('pur1')" /> Employment / Job Application
          </label>
          <label class="purpose-item" id="pur2">
            <input type="checkbox" onchange="togglePurpose('pur2')" /> Scholarship / School Requirements
          </label>
          <label class="purpose-item" id="pur3">
            <input type="checkbox" onchange="togglePurpose('pur3')" /> Business Permit / DTI
          </label>
          <label class="purpose-item" id="pur4">
            <input type="checkbox" onchange="togglePurpose('pur4')" /> Bank / Loan Requirements
          </label>
          <label class="purpose-item" id="pur5">
            <input type="checkbox" onchange="togglePurpose('pur5')" /> Travel / Visa Application
          </label>
          <label class="purpose-item" id="pur6">
            <input type="checkbox" onchange="togglePurpose('pur6')" /> Local Government Requirements
          </label>
        </div>
        <div class="field">
          <label>Other / Specify Purpose</label>
          <input type="text" id="otherPurpose" placeholder="Describe the purpose if not listed above…" />
        </div>

        <hr class="divider" />

        <!-- COPIES & NOTES -->
        <div class="form-row">
          <div class="field">
            <label>No. of Copies <span class="req">*</span></label>
            <select id="copies">
              <option>1</option>
              <option>2</option>
              <option>3</option>
              <option>4</option>
              <option>5</option>
            </select>
          </div>
          <div class="field">
            <label>Preferred Release Date</label>
            <input type="date" id="releaseDate" />
          </div>
        </div>
        <div class="form-row single">
          <div class="field">
            <label>Additional Notes / Remarks</label>
            <textarea id="remarks" placeholder="Any special instructions or additional information…"></textarea>
          </div>
        </div>

        <div class="form-actions">
          <button class="btn-outline" onclick="resetForm()">🗑 Clear Form</button>
          <button class="btn-primary btn-gold" onclick="submitRequest()">✅ Submit Request</button>
        </div>
      </div>
    </div>

    <!-- ═══ TAB: TRACK ═══ -->
    <div class="tab-panel" id="tab-track">
      <h2 class="section-title">Track Your Request</h2>
      <p class="section-sub">Enter your Reference Number to check the status of your document request.</p>

      <div class="form-card">
        <div class="tracker-form">
          <input type="text" id="trackRef" placeholder="Enter Reference No. (e.g. MJN-2026-001)" />
          <button class="btn-primary" onclick="trackRequest()">🔍 Track</button>
        </div>
        <div id="trackResult" style="display:none;">
          <hr class="divider" />
          <div class="subsection-label">Request Status</div>
          <div class="status-timeline">
            <div class="timeline-item">
              <div class="timeline-dot done">✓</div>
              <div class="timeline-title">Request Submitted</div>
              <div class="timeline-sub">June 28, 2026 · 9:15 AM</div>
            </div>
            <div class="timeline-item">
              <div class="timeline-dot done">✓</div>
              <div class="timeline-title">Under Review</div>
              <div class="timeline-sub">June 28, 2026 · 10:00 AM — Information verified</div>
            </div>
            <div class="timeline-item">
              <div class="timeline-dot current">●</div>
              <div class="timeline-title">Document Processing</div>
              <div class="timeline-sub">In progress — estimated release: June 30, 2026</div>
            </div>
            <div class="timeline-item">
              <div class="timeline-dot"></div>
              <div class="timeline-title">Ready for Claiming</div>
              <div class="timeline-sub">Pending</div>
            </div>
            <div class="timeline-item">
              <div class="timeline-dot"></div>
              <div class="timeline-title">Released</div>
              <div class="timeline-sub">Pending</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ═══ TAB: RECORDS ═══ -->
    <div class="tab-panel" id="tab-records">
      <h2 class="section-title">Request Records</h2>
      <p class="section-sub">Recent document requests logged in the system.</p>
      <div class="form-card" style="padding:0; overflow:hidden;">
        <table class="records-table">
          <thead>
            <tr>
              <th>Ref. No.</th>
              <th>Resident Name</th>
              <th>Document</th>
              <th>Date Filed</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody id="recordsBody">
            <tr>
              <td>MJN-2026-001</td>
              <td>Maria Santos</td>
              <td>Barangay Clearance</td>
              <td>June 25, 2026</td>
              <td><span class="badge badge-released">Released</span></td>
            </tr>
            <tr>
              <td>MJN-2026-002</td>
              <td>Ricardo Reyes</td>
              <td>Certificate of Residency</td>
              <td>June 26, 2026</td>
              <td><span class="badge badge-ready">Ready</span></td>
            </tr>
            <tr>
              <td>MJN-2026-003</td>
              <td>Luz Fernandez</td>
              <td>Certificate of Indigency</td>
              <td>June 27, 2026</td>
              <td><span class="badge badge-processing">Processing</span></td>
            </tr>
            <tr>
              <td>MJN-2026-004</td>
              <td>Jose Bautista</td>
              <td>Business Clearance</td>
              <td>June 28, 2026</td>
              <td><span class="badge badge-pending">Pending</span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- ═══ TAB: INFO ═══ -->
    <div class="tab-panel" id="tab-info">
      <h2 class="section-title">Information & Guidelines</h2>
      <p class="section-sub">Everything you need to know about requesting documents at Barangay Mojon.</p>

      <div class="info-grid">
        <div class="info-item">
          <div class="icon">🪪</div>
          <h4>Valid IDs Accepted</h4>
          <p>PhilSys / National ID, Driver's License, Passport, SSS/GSIS Card, PhilHealth Card, Voter's ID, Postal ID.</p>
        </div>
        <div class="info-item">
          <div class="icon">🕐</div>
          <h4>Processing Time</h4>
          <p>Standard documents are released within 1–3 working days. Same-day release may be available for urgent requests (₱20 rush fee applies).</p>
        </div>
        <div class="info-item">
          <div class="icon">📌</div>
          <h4>Requirements</h4>
          <p>Present one valid government-issued ID. Residents must have resided in Barangay Mojon for at least 6 months.</p>
        </div>
        <div class="info-item">
          <div class="icon">💳</div>
          <h4>Payment</h4>
          <p>Fees are paid at the barangay cashier upon claiming. Free documents (Indigency, No Income) require submission of supporting documents.</p>
        </div>
        <div class="info-item">
          <div class="icon">📞</div>
          <h4>Contact & Assistance</h4>
          <p>For questions, call (044) 919-0000 or visit the Barangay Hall during office hours. Email: brgy.mojon@malolos.gov.ph</p>
        </div>
        <div class="info-item">
          <div class="icon">🏛</div>
          <h4>About Barangay Mojon</h4>
          <p>Barangay Mojon is located in Malolos City, Bulacan. The barangay hall provides civil services to all registered residents.</p>
        </div>
      </div>
    </div>

  </main>
</div>

<!-- FOOTER -->
<footer>
  <strong>Barangay Mojon</strong> &nbsp;·&nbsp; Malolos City, Bulacan &nbsp;·&nbsp; Document Request System &nbsp;·&nbsp; © 2026
</footer>

<!-- SUCCESS MODAL -->
<div class="modal-overlay" id="successModal">
  <div class="modal">
    <div class="stamp-anim" id="stampEmoji">🏛</div>
    <h2>Request Submitted!</h2>
    <p>Your document request has been successfully filed at <strong>Barangay Mojon</strong>. Please save your reference number.</p>
    <div class="ref-box" id="refNumber">MJN-2026-005</div>
    <p style="font-size:.8rem; color:var(--muted);">Present this reference number and a valid ID when claiming your document at the Barangay Hall.</p>
    <div class="modal-close">
      <button class="btn-primary" onclick="closeModal()">✓ Done</button>
    </div>
  </div>
</div>

<script>
  // ── TAB SWITCHING ──
  function switchTab(name) {
    document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
    document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
    document.getElementById('tab-' + name).classList.add('active');
    event.target.classList.add('active');
  }

  // ── DOC TYPE SELECTION ──
  function selectDoc(el, name) {
    document.querySelectorAll('.doc-type-btn').forEach(b => b.classList.remove('selected'));
    el.classList.add('selected');
    document.getElementById('selectedDoc').value = name;
  }

  // ── PURPOSE TOGGLE ──
  function togglePurpose(id) {
    const el = document.getElementById(id);
    el.classList.toggle('checked', el.querySelector('input').checked);
  }

  // ── FORM RESET ──
  function resetForm() {
    ['lastName','firstName','middleName','dob','gender','civilStatus','citizenship',
     'address','yearsResident','mobile','email','validId','otherPurpose','copies',
     'releaseDate','remarks'].forEach(id => {
      const el = document.getElementById(id);
      if (el) el.value = id === 'citizenship' ? 'Filipino' : id === 'copies' ? '1' : '';
    });
    document.querySelectorAll('.purpose-item').forEach(p => {
      p.classList.remove('checked');
      p.querySelector('input').checked = false;
    });
    document.getElementById('citizenship').value = 'Filipino';
    selectDoc(document.querySelector('.doc-type-btn'), 'Barangay Clearance');
  }

  // ── FORM SUBMIT ──
  let reqCounter = 5;
  function submitRequest() {
    const required = { lastName:'Last Name', firstName:'First Name', dob:'Date of Birth',
                       gender:'Gender', civilStatus:'Civil Status', address:'Address',
                       yearsResident:'Years of Residency', mobile:'Mobile Number', validId:'Valid ID' };
    for (const [id, label] of Object.entries(required)) {
      if (!document.getElementById(id).value.trim()) {
        alert('⚠️  Please fill in: ' + label);
        document.getElementById(id).focus();
        return;
      }
    }

    const ref = 'MJN-2026-0' + String(reqCounter).padStart(2,'0');
    reqCounter++;
    document.getElementById('refNumber').textContent = ref;

    // Add to records table
    const doc  = document.getElementById('selectedDoc').value;
    const name = document.getElementById('lastName').value + ', ' + document.getElementById('firstName').value;
    const today = new Date().toLocaleDateString('en-PH',{year:'numeric',month:'long',day:'numeric'});
    const row = `<tr>
      <td>${ref}</td>
      <td>${name}</td>
      <td>${doc}</td>
      <td>${today}</td>
      <td><span class="badge badge-pending">Pending</span></td>
    </tr>`;
    document.getElementById('recordsBody').insertAdjacentHTML('afterbegin', row);

    // Show modal
    document.getElementById('successModal').classList.add('open');
    // re-trigger stamp animation
    const stamp = document.getElementById('stampEmoji');
    stamp.style.animation = 'none';
    requestAnimationFrame(() => { stamp.style.animation = ''; });
  }

  function closeModal() {
    document.getElementById('successModal').classList.remove('open');
    resetForm();
  }

  // Close modal on overlay click
  document.getElementById('successModal').addEventListener('click', function(e) {
    if (e.target === this) closeModal();
  });

  // ── TRACKER ──
  function trackRequest() {
    const val = document.getElementById('trackRef').value.trim();
    if (!val) { alert('Please enter a Reference Number.'); return; }
    document.getElementById('trackResult').style.display = 'block';
  }

  // ── SET MIN DATE for release ──
  const today = new Date().toISOString().split('T')[0];
  document.getElementById('releaseDate').min = today;
</script>
</body>
</html>