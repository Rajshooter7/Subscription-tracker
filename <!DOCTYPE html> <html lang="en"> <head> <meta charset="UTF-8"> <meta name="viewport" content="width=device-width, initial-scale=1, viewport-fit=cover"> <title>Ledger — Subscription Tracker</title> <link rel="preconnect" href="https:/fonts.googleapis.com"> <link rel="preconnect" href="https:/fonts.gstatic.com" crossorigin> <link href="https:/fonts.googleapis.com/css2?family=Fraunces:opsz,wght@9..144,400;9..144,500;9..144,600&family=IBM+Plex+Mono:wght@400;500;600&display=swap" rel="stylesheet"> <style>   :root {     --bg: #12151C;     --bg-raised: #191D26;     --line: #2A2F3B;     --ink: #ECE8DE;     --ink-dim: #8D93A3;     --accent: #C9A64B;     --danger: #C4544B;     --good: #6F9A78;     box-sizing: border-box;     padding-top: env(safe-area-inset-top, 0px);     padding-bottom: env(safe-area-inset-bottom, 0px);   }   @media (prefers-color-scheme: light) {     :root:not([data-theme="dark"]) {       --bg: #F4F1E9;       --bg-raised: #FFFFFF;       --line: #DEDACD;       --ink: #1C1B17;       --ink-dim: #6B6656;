<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Ledger — Subscription Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #12151C;
    --bg-raised: #191D26;
    --line: #2A2F3B;
    --ink: #ECE8DE;
    --ink-dim: #8D93A3;
    --accent: #C9A64B;
    --danger: #C4544B;
    --good: #6F9A78;
    box-sizing: border-box;
    padding-top: env(safe-area-inset-top, 0px);
    padding-bottom: env(safe-area-inset-bottom, 0px);
  }
  @media (prefers-color-scheme: light) {
    :root:not([data-theme="dark"]) {
      --bg: #F4F1E9;
      --bg-raised: #FFFFFF;
      --line: #DEDACD;
      --ink: #1C1B17;
      --ink-dim: #6B6656;
      --accent: #9A7B2A;
      --danger: #A63F35;
      --good: #4C7856;
    }
  }
  :root[data-theme="light"] {
    --bg: #F4F1E9;
    --bg-raised: #FFFFFF;
    --line: #DEDACD;
    --ink: #1C1B17;
    --ink-dim: #6B6656;
    --accent: #9A7B2A;
    --danger: #A63F35;
    --good: #4C7856;
  }
  html { scroll-padding-top: env(safe-area-inset-top, 0px); height: 100%; }
  * { box-sizing: border-box; }
  body {
    margin: 0;
    min-height: 100%;
    background: var(--bg);
    color: var(--ink);
    font-family: 'IBM Plex Mono', monospace;
    -webkit-font-smoothing: antialiased;
  }
  .wrap { max-width: 720px; margin: 0 auto; padding: 32px 20px 80px; }

  header { margin-bottom: 36px; }
  .masthead {
    display: flex; justify-content: space-between; align-items: baseline;
    border-bottom: 1px solid var(--line); padding-bottom: 18px;
  }
  h1 {
    font-family: 'Fraunces', serif; font-weight: 600; font-size: 30px;
    margin: 0; letter-spacing: -0.01em;
  }
  .date-line { font-size: 12px; color: var(--ink-dim); }

  .totals { display: grid; grid-template-columns: 1fr 1fr; gap: 1px; background: var(--line); margin-top: 20px; border: 1px solid var(--line); }
  .totals div { background: var(--bg); padding: 16px 18px; }
  .totals .label { font-size: 11px; color: var(--ink-dim); text-transform: none; }
  .totals .value { font-family: 'Fraunces', serif; font-size: 26px; margin-top: 4px; }

  .section-title {
    font-size: 12px; color: var(--ink-dim); margin: 30px 0 10px;
    display: flex; justify-content: space-between; align-items: center;
  }

  .alert-box {
    border: 1px solid var(--accent); padding: 12px 14px; margin-bottom: 20px;
    font-size: 13px; line-height: 1.5; color: var(--ink);
  }
  .alert-box b { color: var(--accent); }
  .alert-box.empty { display: none; }

  .ledger { border-top: 1px solid var(--line); }
  .row {
    display: flex; align-items: center; gap: 12px;
    padding: 13px 0; border-bottom: 1px solid var(--line);
  }
  .row .dot { width: 6px; height: 6px; border-radius: 50%; background: var(--ink-dim); flex-shrink: 0; }
  .row.due-soon .dot { background: var(--accent); }
  .row .name-block { flex: 1; min-width: 0; }
  .row .name { font-size: 14px; }
  .row .meta { font-size: 11px; color: var(--ink-dim); margin-top: 2px; }
  .row .amount { font-size: 14px; min-width: 74px; text-align: right; }
  .row .edit-btn {
    background: none; border: 1px solid var(--line); color: var(--ink-dim);
    font-family: inherit; font-size: 11px; padding: 5px 9px; cursor: pointer;
  }
  .row .edit-btn:hover { border-color: var(--ink-dim); color: var(--ink); }

  .empty-state { padding: 30px 0; color: var(--ink-dim); font-size: 13px; line-height: 1.6; }

  .add-form {
    margin-top: 28px; border: 1px solid var(--line); background: var(--bg-raised);
    padding: 18px; display: none;
  }
  .add-form.open { display: block; }
  .add-form h2 { font-family: 'Fraunces', serif; font-size: 17px; margin: 0 0 14px; font-weight: 500; }
  .field { margin-bottom: 12px; }
  .field label { display: block; font-size: 11px; color: var(--ink-dim); margin-bottom: 5px; }
  .field input, .field select {
    width: 100%; background: var(--bg); border: 1px solid var(--line); color: var(--ink);
    font-family: inherit; font-size: 14px; padding: 9px 10px;
  }
  .field input:focus, .field select:focus { outline: 2px solid var(--accent); outline-offset: -1px; }
  .field-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .form-actions { display: flex; gap: 10px; margin-top: 16px; }
  button.primary {
    background: var(--accent); color: var(--bg); border: none;
    font-family: inherit; font-size: 13px; font-weight: 600; padding: 10px 16px; cursor: pointer;
  }
  button.primary:hover { opacity: 0.88; }
  button.ghost {
    background: none; color: var(--ink-dim); border: 1px solid var(--line);
    font-family: inherit; font-size: 13px; padding: 10px 16px; cursor: pointer;
  }
  button.ghost:hover { color: var(--ink); }
  button.danger-link {
    background: none; border: none; color: var(--danger); font-family: inherit;
    font-size: 12px; cursor: pointer; padding: 0; text-decoration: underline;
  }

  .add-toggle {
    width: 100%; margin-top: 20px; background: none; border: 1px dashed var(--line);
    color: var(--ink-dim); font-family: inherit; font-size: 13px; padding: 12px; cursor: pointer;
  }
  .add-toggle:hover { border-color: var(--accent); color: var(--accent); }

  .cancelled-list { margin-top: 6px; }
  .cancelled-list .row .name { color: var(--ink-dim); text-decoration: line-through; text-decoration-color: var(--line); }
  .cancelled-list .row .amount { color: var(--good); }

  footer { margin-top: 48px; font-size: 11px; color: var(--ink-dim); border-top: 1px solid var(--line); padding-top: 16px; }
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="masthead">
      <h1>Ledger</h1>
      <div class="date-line" id="today"></div>
    </div>
    <div class="totals">
      <div>
        <div class="label">monthly total</div>
        <div class="value" id="monthlyTotal">$0</div>
      </div>
      <div>
        <div class="label">annual total</div>
        <div class="value" id="annualTotal">$0</div>
      </div>
    </div>
  </header>

  <div class="alert-box empty" id="alertBox"></div>

  <div class="section-title"><span>active subscriptions</span><span id="activeCount"></span></div>
  <div class="ledger" id="activeList"></div>
  <div class="empty-state" id="emptyState" style="display:none;">
    Nothing tracked yet. Add the first subscription below — even one you're unsure about counts.
  </div>

  <button class="add-toggle" id="addToggle">+ add a subscription</button>

  <div class="add-form" id="addForm">
    <h2 id="formTitle">New subscription</h2>
    <div class="field">
      <label for="fName">Name</label>
      <input type="text" id="fName" placeholder="e.g. Spotify">
    </div>
    <div class="field-row">
      <div class="field">
        <label for="fAmount">Amount ($)</label>
        <input type="number" id="fAmount" step="0.01" placeholder="9.99">
      </div>
      <div class="field">
        <label for="fCycle">Billing cycle</label>
        <select id="fCycle">
          <option value="monthly">Monthly</option>
          <option value="annual">Annual</option>
          <option value="weekly">Weekly</option>
        </select>
      </div>
    </div>
    <div class="field">
      <label for="fDate">Next renewal date</label>
      <input type="date" id="fDate">
    </div>
    <div class="field">
      <label for="fCategory">Category (optional)</label>
      <input type="text" id="fCategory" placeholder="e.g. Streaming, Software, Fitness">
    </div>
    <div class="form-actions">
      <button class="primary" id="saveBtn">Save</button>
      <button class="ghost" id="cancelFormBtn">Cancel</button>
    </div>
  </div>

  <div id="cancelledSection" style="display:none;">
    <div class="section-title"><span>cancelled</span><span id="savedAmount"></span></div>
    <div class="ledger cancelled-list" id="cancelledList"></div>
  </div>

  <footer>
    Stored only on this device, in this browser. Nothing is sent anywhere.
  </footer>
</div>

<script>
(function() {
  const STORAGE_KEY = 'ledger_subscriptions_v1';
  let subs = [];
  let editingId = null;

  function loadSubs() {
    try {
      const raw = localStorage.getItem(STORAGE_KEY);
      subs = raw ? JSON.parse(raw) : [];
    } catch (e) { subs = []; }
  }
  function saveSubs() {
    try { localStorage.setItem(STORAGE_KEY, JSON.stringify(subs)); }
    catch (e) { console.error('save failed', e); }
  }

  function monthlyEquivalent(sub) {
    if (sub.cycle === 'annual') return sub.amount / 12;
    if (sub.cycle === 'weekly') return sub.amount * 4.33;
    return sub.amount;
  }

  function fmt(n) {
    return '$' + n.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
  }

  function daysUntil(dateStr) {
    if (!dateStr) return null;
    const today = new Date(); today.setHours(0,0,0,0);
    const target = new Date(dateStr + 'T00:00:00');
    return Math.round((target - today) / 86400000);
  }

  function render() {
    const active = subs.filter(s => s.status !== 'cancelled');
    const cancelled = subs.filter(s => s.status === 'cancelled');

    const monthlyTotal = active.reduce((sum, s) => sum + monthlyEquivalent(s), 0);
    document.getElementById('monthlyTotal').textContent = fmt(monthlyTotal);
    document.getElementById('annualTotal').textContent = fmt(monthlyTotal * 12);

    document.getElementById('activeCount').textContent = active.length ? active.length + ' tracked' : '';

    const dueSoon = active.filter(s => {
      const d = daysUntil(s.nextRenewal);
      return d !== null && d >= 0 && d <= 3;
    }).sort((a,b) => daysUntil(a.nextRenewal) - daysUntil(b.nextRenewal));

    const alertBox = document.getElementById('alertBox');
    if (dueSoon.length) {
      alertBox.classList.remove('empty');
      alertBox.innerHTML = dueSoon.map(s => {
        const d = daysUntil(s.nextRenewal);
        const when = d === 0 ? 'today' : d === 1 ? 'tomorrow' : 'in ' + d + ' days';
        return '<div><b>' + escapeHtml(s.name) + '</b> renews ' + when + ' — ' + fmt(s.amount) + '</div>';
      }).join('');
    } else {
      alertBox.classList.add('empty');
      alertBox.innerHTML = '';
    }

    const listEl = document.getElementById('activeList');
    const emptyEl = document.getElementById('emptyState');
    if (!active.length) {
      listEl.innerHTML = '';
      emptyEl.style.display = 'block';
    } else {
      emptyEl.style.display = 'none';
      const sorted = [...active].sort((a,b) => {
        const da = daysUntil(a.nextRenewal), db = daysUntil(b.nextRenewal);
        if (da === null) return 1;
        if (db === null) return -1;
        return da - db;
      });
      listEl.innerHTML = sorted.map(s => {
        const d = daysUntil(s.nextRenewal);
        const dueSoonClass = (d !== null && d >= 0 && d <= 3) ? ' due-soon' : '';
        const cycleLabel = s.cycle === 'annual' ? '/yr' : s.cycle === 'weekly' ? '/wk' : '/mo';
        const dateLabel = s.nextRenewal ? formatDate(s.nextRenewal) : 'no date set';
        const cat = s.category ? ' · ' + escapeHtml(s.category) : '';
        return '<div class="row' + dueSoonClass + '">' +
          '<div class="dot"></div>' +
          '<div class="name-block">' +
            '<div class="name">' + escapeHtml(s.name) + '</div>' +
            '<div class="meta">renews ' + dateLabel + cat + '</div>' +
          '</div>' +
          '<div class="amount">' + fmt(s.amount) + cycleLabel + '</div>' +
          '<button class="edit-btn" data-id="' + s.id + '" data-action="edit">edit</button>' +
        '</div>';
      }).join('');
    }

    const cancelledSection = document.getElementById('cancelledSection');
    if (cancelled.length) {
      cancelledSection.style.display = 'block';
      const savedMonthly = cancelled.reduce((sum, s) => sum + monthlyEquivalent(s), 0);
      document.getElementById('savedAmount').textContent = fmt(savedMonthly) + '/mo saved';
      document.getElementById('cancelledList').innerHTML = cancelled.map(s => {
        const cycleLabel = s.cycle === 'annual' ? '/yr' : s.cycle === 'weekly' ? '/wk' : '/mo';
        return '<div class="row">' +
          '<div class="dot"></div>' +
          '<div class="name-block"><div class="name">' + escapeHtml(s.name) + '</div>' +
          '<div class="meta">cancelled</div></div>' +
          '<div class="amount">' + fmt(s.amount) + cycleLabel + '</div>' +
        '</div>';
      }).join('');
    } else {
      cancelledSection.style.display = 'none';
    }
  }

  function formatDate(dateStr) {
    const d = new Date(dateStr + 'T00:00:00');
    return d.toLocaleDateString('en-US', { month: 'short', day: 'numeric' });
  }

  function escapeHtml(str) {
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
  }

  function openForm(sub) {
    editingId = sub ? sub.id : null;
    document.getElementById('formTitle').textContent = sub ? 'Edit subscription' : 'New subscription';
    document.getElementById('fName').value = sub ? sub.name : '';
    document.getElementById('fAmount').value = sub ? sub.amount : '';
    document.getElementById('fCycle').value = sub ? sub.cycle : 'monthly';
    document.getElementById('fDate').value = sub ? sub.nextRenewal : '';
    document.getElementById('fCategory').value = sub ? (sub.category || '') : '';
    document.getElementById('addForm').classList.add('open');
    document.getElementById('addToggle').style.display = 'none';

    let cancelRow = document.getElementById('cancelRow');
    if (cancelRow) cancelRow.remove();
    if (sub) {
      const actions = document.querySelector('.form-actions');
      const row = document.createElement('div');
      row.id = 'cancelRow';
      row.style.marginTop = '12px';
      row.innerHTML = '<button class="danger-link" id="markCancelledBtn">mark as cancelled</button>';
      actions.after(row);
      document.getElementById('markCancelledBtn').addEventListener('click', () => {
        sub.status = 'cancelled';
        saveSubs();
        closeForm();
        render();
      });
    }
    document.getElementById('fName').focus();
  }

  function closeForm() {
    editingId = null;
    document.getElementById('addForm').classList.remove('open');
    document.getElementById('addToggle').style.display = 'block';
    let cancelRow = document.getElementById('cancelRow');
    if (cancelRow) cancelRow.remove();
  }

  document.getElementById('addToggle').addEventListener('click', () => openForm(null));
  document.getElementById('cancelFormBtn').addEventListener('click', closeForm);

  document.getElementById('saveBtn').addEventListener('click', () => {
    const name = document.getElementById('fName').value.trim();
    const amount = parseFloat(document.getElementById('fAmount').value);
    const cycle = document.getElementById('fCycle').value;
    const nextRenewal = document.getElementById('fDate').value;
    const category = document.getElementById('fCategory').value.trim();

    if (!name || isNaN(amount) || amount < 0) {
      alert('Enter a name and a valid amount.');
      return;
    }

    if (editingId) {
      const sub = subs.find(s => s.id === editingId);
      if (sub) {
        sub.name = name; sub.amount = amount; sub.cycle = cycle;
        sub.nextRenewal = nextRenewal; sub.category = category;
      }
    } else {
      subs.push({
        id: 'sub_' + Date.now() + '_' + Math.random().toString(36).slice(2,7),
        name, amount, cycle, nextRenewal, category, status: 'active'
      });
    }
    saveSubs();
    closeForm();
    render();
  });

  document.getElementById('activeList').addEventListener('click', (e) => {
    const btn = e.target.closest('[data-action="edit"]');
    if (!btn) return;
    const sub = subs.find(s => s.id === btn.dataset.id);
    if (sub) openForm(sub);
  });

  document.getElementById('today').textContent = new Date().toLocaleDateString('en-US', {
    weekday: 'long', month: 'long', day: 'numeric'
  });

  loadSubs();
  render();
})();
</script>
</body>
</html>