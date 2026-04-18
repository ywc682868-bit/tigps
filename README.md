<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TIGPS 研究計畫平台</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#ffffff;--bg2:#f7f6f3;--bg3:#efede8;
  --text:#1a1a1a;--text2:#6b6b6b;--text3:#9b9b9b;
  --border:#e3e2de;--border2:#d1cfc9;
  --blue-bg:#e8f0f9;--blue-text:#2a5db0;
  --green-bg:#e3f5e1;--green-text:#2a6e32;
  --red-bg:#fce8e6;--red-text:#b03030;
  --orange-bg:#fef3e2;--orange-text:#9a5c00;
  --purple-bg:#f0ecfd;--purple-text:#5b3da8;
  --pink-bg:#fde8f0;--pink-text:#a0306a;
  --teal-bg:#e2f4f1;--teal-text:#1a6b60;
  --yellow-bg:#fef9e2;--yellow-text:#7a5c00;
  --accent:#2a5db0;
  --radius:6px;--radius-lg:10px;
}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI','Noto Sans TC',sans-serif;background:var(--bg);color:var(--text);font-size:13px;line-height:1.5}

/* Layout */
.app{display:flex;height:100vh;overflow:hidden}
.sidebar{width:220px;min-width:220px;border-right:1px solid var(--border);background:var(--bg2);display:flex;flex-direction:column;overflow:hidden}
.main{flex:1;display:flex;flex-direction:column;overflow:hidden;min-width:0}

/* Sidebar */
.sb-logo{padding:16px 18px 10px;display:flex;align-items:center;gap:8px;border-bottom:1px solid var(--border)}
.sb-logo-icon{width:26px;height:26px;background:var(--accent);border-radius:6px;display:flex;align-items:center;justify-content:center;color:#fff;font-size:11px;font-weight:700;flex-shrink:0}
.sb-logo-text{font-size:14px;font-weight:600;color:var(--text)}
.sb-section{padding:10px 10px 4px;font-size:10px;font-weight:600;color:var(--text3);letter-spacing:0.06em;text-transform:uppercase}
.sb-nav{flex:1;overflow-y:auto;padding:0 8px 12px}
.sb-item{display:flex;align-items:center;gap:8px;padding:6px 10px;border-radius:var(--radius);cursor:pointer;color:var(--text2);font-size:13px;margin-bottom:1px;user-select:none}
.sb-item:hover{background:var(--bg3);color:var(--text)}
.sb-item.active{background:var(--border);color:var(--text);font-weight:500}
.sb-icon{width:16px;height:16px;flex-shrink:0;opacity:0.7}
.sb-team{padding:12px 10px;border-top:1px solid var(--border)}
.sb-team-title{font-size:10px;font-weight:600;color:var(--text3);letter-spacing:0.06em;text-transform:uppercase;margin-bottom:10px}
.team-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.member-cell{display:flex;flex-direction:column;align-items:center;gap:5px;cursor:default}
.avatar{width:38px;height:38px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:600;flex-shrink:0}
.avatar-sm{width:20px;height:20px;font-size:9px;font-weight:600;border-radius:50%;display:flex;align-items:center;justify-content:center;flex-shrink:0}
.member-name{font-size:10px;color:var(--text2);text-align:center;line-height:1.3}

/* Topbar */
.topbar{display:flex;align-items:center;gap:10px;padding:10px 20px;border-bottom:1px solid var(--border);flex-shrink:0;min-height:48px}
.topbar-title{font-size:16px;font-weight:600;flex:1;color:var(--text)}
.topbar-actions{display:flex;align-items:center;gap:6px}
.btn{padding:5px 12px;font-size:12px;border-radius:var(--radius);border:1px solid var(--border2);background:var(--bg);cursor:pointer;color:var(--text);display:flex;align-items:center;gap:5px;line-height:1.4}
.btn:hover{background:var(--bg2)}
.btn-primary{background:var(--accent);color:#fff;border-color:var(--accent)}
.btn-primary:hover{background:#1e4d9e}
.add-round{width:28px;height:28px;border-radius:50%;border:1px solid var(--border2);background:var(--bg);cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:17px;color:var(--text2);line-height:1}
.add-round:hover{background:var(--bg2);color:var(--text)}

/* Content */
.content{flex:1;overflow:hidden;display:flex;flex-direction:column}
.view{display:none;flex:1;overflow:hidden}
.view.active{display:flex;flex-direction:column}

/* === CALENDAR VIEW === */
.cal-wrap{display:flex;flex:1;overflow:hidden}
.cal-left{width:220px;min-width:220px;border-right:1px solid var(--border);padding:16px;overflow-y:auto;background:var(--bg2)}
.cal-center{flex:1;display:flex;flex-direction:column;overflow-y:auto;min-height:0}
.cal-right{width:200px;min-width:200px;border-left:1px solid var(--border);padding:14px;overflow-y:auto;background:var(--bg2)}

.cal-side-section{margin-bottom:18px}
.cal-side-title{font-size:11px;font-weight:700;padding:4px 8px;border-radius:4px;margin-bottom:8px;display:inline-block}
.cal-side-title.current{background:var(--blue-bg);color:var(--blue-text)}
.cal-side-title.next{background:var(--orange-bg);color:var(--orange-text)}
.cal-side-title.deadline{background:var(--red-bg);color:var(--red-text)}
.cal-side-title.meeting{background:var(--teal-bg);color:var(--teal-text)}
.cal-side-item{font-size:12px;padding:5px 8px;border-radius:4px;margin-bottom:3px;color:var(--text2);cursor:pointer;display:flex;align-items:flex-start;gap:5px}
.cal-side-item:hover{background:var(--border);color:var(--text)}
.cal-side-dot{width:6px;height:6px;border-radius:50%;flex-shrink:0;margin-top:4px}

.cal-nav{display:flex;align-items:center;gap:12px;padding:12px 20px;border-bottom:1px solid var(--border);flex-shrink:0}
.cal-nav-title{font-size:15px;font-weight:600;min-width:80px}
.cal-nav-year{font-size:12px;color:var(--text3);margin-left:4px}
.arrow-btn{width:26px;height:26px;border-radius:50%;border:1px solid var(--border);background:transparent;cursor:pointer;display:flex;align-items:center;justify-content:center;color:var(--text2);font-size:14px}
.arrow-btn:hover{background:var(--bg3);color:var(--text)}
.cal-today-btn{padding:3px 10px;font-size:12px;border-radius:var(--radius);border:1px solid var(--border2);background:var(--bg);cursor:pointer;color:var(--text2)}
.cal-today-btn:hover{background:var(--bg2)}

.cal-grid{padding:0 12px 12px;display:flex;flex-direction:column}
.cal-dow-row{display:grid;grid-template-columns:repeat(7,minmax(0,1fr));position:sticky;top:0;background:var(--bg);z-index:2;border-bottom:1px solid var(--border);padding:6px 0}
.cal-dow{font-size:11px;color:var(--text3);text-align:center;font-weight:500}
.cal-body{display:grid;grid-template-columns:repeat(7,minmax(0,1fr));flex:1}
.cal-cell{min-height:100px;border-right:1px solid var(--border);border-bottom:1px solid var(--border);padding:5px;cursor:pointer;vertical-align:top}
.cal-cell:hover{background:var(--bg2)}
.cal-cell.empty{cursor:default;background:transparent}
.cal-cell.empty:hover{background:transparent}
.cal-cell.other-month{background:var(--bg2);opacity:0.6}
.cal-date-num{font-size:11px;color:var(--text2);margin-bottom:3px;font-weight:500;width:20px;height:20px;display:flex;align-items:center;justify-content:center;border-radius:50%}
.cal-date-num.today{background:var(--accent);color:#fff;font-weight:600}
.cal-chip{font-size:10px;padding:2px 5px;border-radius:3px;margin-bottom:2px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;display:flex;align-items:center;gap:3px;cursor:pointer}
.cal-chip:hover{opacity:0.8}
.cal-more{font-size:10px;color:var(--text3);padding:1px 4px}

/* Status & priority badges */
.status{display:inline-flex;align-items:center;gap:4px;padding:2px 7px;border-radius:10px;font-size:11px;font-weight:500}
.st-notstarted{background:var(--bg3);color:var(--text2)}
.st-inprogress{background:var(--blue-bg);color:var(--blue-text)}
.st-done{background:var(--green-bg);color:var(--green-text)}
.st-blocked{background:var(--red-bg);color:var(--red-text)}
.priority{display:inline-flex;gap:2px;font-size:11px}
.p1{color:#e84b4b}.p2{color:#f59e0b}.p3{color:#6b7280}

/* Type tags */
.tag{display:inline-flex;padding:1px 7px;border-radius:3px;font-size:11px;font-weight:500}
.tag-questionnaire{background:var(--purple-bg);color:var(--purple-text)}
.tag-data{background:var(--blue-bg);color:var(--blue-text)}
.tag-admin{background:var(--orange-bg);color:var(--orange-text)}
.tag-meeting{background:var(--teal-bg);color:var(--teal-text)}
.tag-platform{background:var(--pink-bg);color:var(--pink-text)}

/* Questionnaire type tags */
.qtag{display:inline-flex;padding:1px 6px;border-radius:3px;font-size:10px;font-weight:500}
.qt-student{background:#e3f0fd;color:#1a5fa0}
.qt-sibling{background:#fde8e8;color:#9a2525}
.qt-parent{background:#e3f5e1;color:#2a6e32}
.qt-teacher{background:#fde8f5;color:#8a2060}
.qt-subject{background:#fff3e0;color:#8a5000}
.qt-school{background:#e8f0e8;color:#2a5030}

/* === TABLE VIEW === */
.table-wrap{flex:1;overflow:auto;padding:0}
.data-table{width:100%;border-collapse:collapse;font-size:12px}
.data-table th{padding:8px 12px;text-align:left;font-size:11px;font-weight:600;color:var(--text2);border-bottom:1px solid var(--border);background:var(--bg2);position:sticky;top:0;z-index:2;white-space:nowrap}
.data-table td{padding:7px 12px;border-bottom:1px solid var(--border);vertical-align:middle}
.data-table tr:hover td{background:var(--bg2)}
.data-table tr.done-row td{opacity:0.5}
.sort-btn{background:none;border:none;cursor:pointer;color:var(--text2);font-size:11px;font-weight:600;display:flex;align-items:center;gap:3px;padding:0}
.sort-btn:hover{color:var(--text)}
.check-cell{width:32px;text-align:center}
.check{width:14px;height:14px;border-radius:3px;border:1.5px solid var(--border2);cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:9px;flex-shrink:0;background:transparent}
.check.done{background:var(--accent);border-color:var(--accent);color:#fff}
.priority-cell{text-align:center}
.avatars-stack{display:flex;align-items:center}
.avatars-stack .avatar-sm{margin-left:-5px;border:1.5px solid var(--bg)}
.avatars-stack .avatar-sm:first-child{margin-left:0}
.task-title-cell{font-size:13px;font-weight:500;cursor:pointer;max-width:280px}
.task-title-cell.done{text-decoration:line-through;color:var(--text3)}
.task-title-cell:hover{color:var(--accent)}

/* === DATA TRACKER VIEW === */
.data-tracker{flex:1;overflow:auto;padding:16px}
.tracker-toolbar{display:flex;align-items:center;gap:8px;margin-bottom:14px;flex-wrap:wrap}
.filter-chip{padding:3px 10px;font-size:12px;border-radius:12px;border:1px solid var(--border2);background:var(--bg);cursor:pointer;color:var(--text2)}
.filter-chip:hover{background:var(--bg2)}
.filter-chip.active{border-color:var(--accent);background:var(--blue-bg);color:var(--blue-text)}
.tracker-table{width:100%;border-collapse:collapse;font-size:12px}
.tracker-table th{padding:7px 10px;text-align:left;font-size:11px;font-weight:600;color:var(--text2);border-bottom:1px solid var(--border);background:var(--bg2);white-space:nowrap;position:sticky;top:0;z-index:2}
.tracker-table td{padding:6px 10px;border-bottom:1px solid var(--border);vertical-align:middle}
.tracker-table tr:hover td{background:var(--bg2)}
.tracker-table tr.done-row td:not(:first-child):not(:nth-child(2)){opacity:0.5}
.item-type-col{width:70px}

/* Progress view */
.progress-view{flex:1;overflow-y:auto;padding:20px}
.stat-grid{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:12px;margin-bottom:24px}
.stat-card{background:var(--bg2);border-radius:var(--radius-lg);padding:14px 16px;border:1px solid var(--border)}
.stat-val{font-size:24px;font-weight:700;margin-bottom:3px}
.stat-lbl{font-size:11px;color:var(--text2)}
.prog-section{margin-bottom:24px}
.prog-section-title{font-size:13px;font-weight:600;margin-bottom:12px;color:var(--text)}
.prog-row{margin-bottom:10px}
.prog-label-row{display:flex;justify-content:space-between;font-size:12px;color:var(--text2);margin-bottom:4px}
.prog-bar{height:7px;background:var(--bg3);border-radius:4px;overflow:hidden}
.prog-fill{height:100%;border-radius:4px;background:var(--accent)}
.prog-fill.cat-q{background:#5b3da8}
.prog-fill.cat-d{background:#2a5db0}
.prog-fill.cat-a{background:#9a5c00}
.prog-fill.cat-m{background:#1a6b60}

/* Modal */
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.4);z-index:100;align-items:center;justify-content:center}
.modal-overlay.open{display:flex}
.modal{background:var(--bg);border-radius:var(--radius-lg);border:1px solid var(--border);width:520px;max-height:88vh;overflow-y:auto;padding:24px;box-shadow:0 8px 32px rgba(0,0,0,0.12)}
.modal-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:20px}
.modal-title{font-size:16px;font-weight:600}
.close-btn{width:26px;height:26px;border-radius:50%;border:1px solid var(--border);background:transparent;cursor:pointer;color:var(--text2);font-size:14px;display:flex;align-items:center;justify-content:center}
.close-btn:hover{background:var(--bg2)}
.fg{margin-bottom:14px}
.flabel{font-size:11px;font-weight:600;color:var(--text2);margin-bottom:5px;display:block;text-transform:uppercase;letter-spacing:0.04em}
input[type=text],input[type=date],select,textarea{width:100%;padding:7px 10px;font-size:13px;border:1px solid var(--border2);border-radius:var(--radius);background:var(--bg);color:var(--text);outline:none;font-family:inherit}
input:focus,select:focus,textarea:focus{border-color:var(--accent)}
textarea{resize:vertical;min-height:60px}
.two-col{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.chip-row{display:flex;gap:5px;flex-wrap:wrap}
.chip{padding:4px 10px;font-size:12px;border-radius:12px;border:1px solid var(--border2);background:transparent;cursor:pointer;color:var(--text2)}
.chip:hover{background:var(--bg2)}
.chip.on{border-color:var(--accent);background:var(--blue-bg);color:var(--blue-text)}
.chip.on-student{background:#e3f0fd;color:#1a5fa0;border-color:#90bde8}
.chip.on-sibling{background:#fde8e8;color:#9a2525;border-color:#f0a0a0}
.chip.on-parent{background:#e3f5e1;color:#2a6e32;border-color:#80c880}
.chip.on-teacher{background:#fde8f5;color:#8a2060;border-color:#e080b8}
.chip.on-subject{background:#fff3e0;color:#8a5000;border-color:#f0c060}
.chip.on-school{background:#e8f0e8;color:#2a5030;border-color:#80a880}
.mem-chip{display:flex;align-items:center;gap:5px;padding:4px 10px;font-size:12px;border-radius:12px;border:1px solid var(--border2);cursor:pointer;color:var(--text2)}
.mem-chip:hover{background:var(--bg2)}
.mem-chip.sel{border-color:var(--accent);background:var(--blue-bg);color:var(--blue-text)}
.modal-footer{display:flex;justify-content:flex-end;gap:8px;margin-top:18px;padding-top:14px;border-top:1px solid var(--border)}
.priority-display{display:flex;gap:3px}
.pstar{font-size:13px;cursor:pointer;color:var(--border2)}
.pstar.on{color:#e84b4b}

/* Detail panel */
.detail-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.3);z-index:90;align-items:flex-start;justify-content:flex-end}
.detail-overlay.open{display:flex}
.detail-panel{width:420px;height:100vh;background:var(--bg);border-left:1px solid var(--border);overflow-y:auto;padding:24px;box-shadow:-4px 0 20px rgba(0,0,0,0.08)}
.detail-panel-title{font-size:17px;font-weight:600;margin-bottom:4px}
.detail-field{margin-bottom:14px}
.detail-field-label{font-size:10px;font-weight:700;color:var(--text3);text-transform:uppercase;letter-spacing:0.06em;margin-bottom:4px}
.detail-field-val{font-size:13px;color:var(--text)}
.notes-input{width:100%;padding:8px;font-size:13px;border:1px solid var(--border2);border-radius:var(--radius);background:var(--bg);color:var(--text);resize:vertical;min-height:80px;font-family:inherit}
.notes-input:focus{outline:none;border-color:var(--accent)}

/* Misc */
.empty{text-align:center;padding:40px;color:var(--text3);font-size:13px}
.divider{height:1px;background:var(--border);margin:8px 0}
</style>
</head>
<body>

<div class="app">

<!-- Sidebar -->
<div class="sidebar">
  <div class="sb-logo">
    <div class="sb-logo-icon">T</div>
    <div class="sb-logo-text">TIGPS</div>
  </div>
  <div class="sb-nav">
    <div class="sb-section">主要頁面</div>
    <div class="sb-item active" onclick="switchView('dashboard',this)">
      <svg class="sb-icon" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.4"><rect x="1.5" y="2" width="5.5" height="5.5" rx="1"/><rect x="9" y="2" width="5.5" height="5.5" rx="1"/><rect x="1.5" y="9.5" width="5.5" height="4.5" rx="1"/><rect x="9" y="9.5" width="5.5" height="4.5" rx="1"/></svg>
      總覽
    </div>
    <div class="sb-item" onclick="switchView('calendar',this)">
      <svg class="sb-icon" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.4"><rect x="1.5" y="2.5" width="13" height="12" rx="1.5"/><path d="M5 1.5v2M11 1.5v2M1.5 6.5h13"/></svg>
      行事曆
    </div>
    <div class="sb-item" onclick="switchView('tasklist',this)">
      <svg class="sb-icon" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.4"><path d="M2 4h12M2 8h9M2 12h6"/></svg>
      工作清單
    </div>
    <div class="sb-item" onclick="switchView('datatracker',this)">
      <svg class="sb-icon" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.4"><rect x="1.5" y="1.5" width="13" height="13" rx="1.5"/><path d="M1.5 5.5h13M1.5 9.5h13M5.5 5.5v9M10.5 5.5v9"/></svg>
      資料追蹤
    </div>
    <div class="sb-item" onclick="switchView('progress',this)">
      <svg class="sb-icon" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.4"><path d="M2 12L5 8.5L8 10L11 5.5L14 7.5"/></svg>
      進度追蹤
    </div>
  </div>
  <div class="sb-team">
    <div class="sb-team-title">研究團隊</div>
    <div class="team-grid" id="teamGrid"></div>
  </div>
</div>

<!-- Main -->
<div class="main">
  <div class="topbar">
    <div class="topbar-title" id="topbarTitle">總覽</div>
    <div class="topbar-actions">
      <button class="add-round" onclick="openAdd()" title="新增工作">＋</button>
    </div>
  </div>
  <div class="content">

    <!-- DASHBOARD VIEW -->
    <div class="view active" id="view-dashboard">
      <div class="cal-wrap">
        <!-- Left panel -->
        <div class="cal-left">
          <div class="cal-side-section">
            <div class="cal-side-title current">當月代辦</div>
            <div id="dashCurrentMonth"></div>
          </div>
          <div class="cal-side-section">
            <div class="cal-side-title next">下月代辦</div>
            <div id="dashNextMonth"></div>
          </div>
          <div class="divider"></div>
          <div class="cal-side-section" style="margin-top:10px">
            <div class="cal-side-title deadline">重要 Deadline</div>
            <div id="dashDeadlines"></div>
          </div>
        </div>
        <!-- Center calendar -->
        <div class="cal-center">
          <div class="cal-nav" style="flex-shrink:0">
            <button class="arrow-btn" onclick="changeCalMonth(-1)">‹</button>
            <div>
              <span class="cal-nav-title" id="calMonthTitle"></span>
              <span class="cal-nav-year" id="calYearLabel"></span>
            </div>
            <button class="arrow-btn" onclick="changeCalMonth(1)">›</button>
            <button class="cal-today-btn" onclick="goToday()">今天</button>
            <div style="margin-left:auto"><button class="add-round" onclick="openAdd()" title="新增工作">＋</button></div>
          </div>
          <div class="cal-grid">
            <div class="cal-dow-row">
              <div class="cal-dow">日</div><div class="cal-dow">一</div><div class="cal-dow">二</div>
              <div class="cal-dow">三</div><div class="cal-dow">四</div><div class="cal-dow">五</div><div class="cal-dow">六</div>
            </div>
            <div class="cal-body" id="calBody"></div>
          </div>
        </div>
        <!-- Right panel -->
        <div class="cal-right">
          <div class="cal-side-title meeting" style="display:block;margin-bottom:10px">會議安排</div>
          <div id="dashMeetings"></div>
        </div>
      </div>
    </div>

    <!-- CALENDAR VIEW (standalone) -->
    <div class="view" id="view-calendar">
      <div class="cal-nav" style="padding:10px 20px">
        <button class="arrow-btn" onclick="changeCalMonth(-1)">‹</button>
        <div>
          <span class="cal-nav-title" id="calMonthTitle2"></span>
          <span class="cal-nav-year" id="calYearLabel2"></span>
        </div>
        <button class="arrow-btn" onclick="changeCalMonth(1)">›</button>
        <button class="cal-today-btn" onclick="goToday()">今天</button>
        <div style="margin-left:auto"><button class="add-round" onclick="openAdd()">＋</button></div>
      </div>
      <div style="flex:1;overflow-y:auto;min-height:0;padding:0 12px 12px;display:flex;flex-direction:column">
        <div class="cal-dow-row">
          <div class="cal-dow">日</div><div class="cal-dow">一</div><div class="cal-dow">二</div>
          <div class="cal-dow">三</div><div class="cal-dow">四</div><div class="cal-dow">五</div><div class="cal-dow">六</div>
        </div>
        <div class="cal-body" id="calBody2"></div>
      </div>
    </div>

    <!-- TASK LIST VIEW -->
    <div class="view" id="view-tasklist">
      <div style="padding:10px 16px;border-bottom:1px solid var(--border);display:flex;align-items:center;gap:6px;flex-wrap:wrap;background:var(--bg)">
        <span style="font-size:12px;color:var(--text2)">分類：</span>
        <button class="filter-chip active" id="fl-all" onclick="setTaskFilter('all',this)">全部</button>
        <button class="filter-chip" id="fl-questionnaire" onclick="setTaskFilter('questionnaire',this)">問卷</button>
        <button class="filter-chip" id="fl-data" onclick="setTaskFilter('data',this)">資料</button>
        <button class="filter-chip" id="fl-admin" onclick="setTaskFilter('admin',this)">行政</button>
        <button class="filter-chip" id="fl-meeting" onclick="setTaskFilter('meeting',this)">會議</button>
        <button class="filter-chip" id="fl-platform" onclick="setTaskFilter('platform',this)">平台</button>
        <div style="margin-left:auto"><button class="add-round" onclick="openAdd()">＋</button></div>
      </div>
      <div class="table-wrap">
        <table class="data-table" id="taskTable">
          <thead>
            <tr>
              <th class="check-cell"></th>
              <th><button class="sort-btn" onclick="sortTasks('title')">討論主題 ↕</button></th>
              <th><button class="sort-btn" onclick="sortTasks('date')">日期 ↕</button></th>
              <th>人員</th>
              <th><button class="sort-btn" onclick="sortTasks('priority')">緊急程度 ↕</button></th>
              <th><button class="sort-btn" onclick="sortTasks('status')">狀態 ↕</button></th>
              <th>類型</th>
              <th>備註</th>
            </tr>
          </thead>
          <tbody id="taskTbody"></tbody>
        </table>
      </div>
    </div>

    <!-- DATA TRACKER VIEW -->
    <div class="view" id="view-datatracker">
      <div class="data-tracker">
        <div class="tracker-toolbar">
          <span style="font-size:12px;color:var(--text2)">問卷類型：</span>
          <button class="filter-chip active" id="dt-all" onclick="setDtFilter('all',this)">全部</button>
          <button class="filter-chip" id="dt-student" onclick="setDtFilter('student',this)">學生</button>
          <button class="filter-chip" id="dt-sibling" onclick="setDtFilter('sibling',this)">手足</button>
          <button class="filter-chip" id="dt-parent" onclick="setDtFilter('parent',this)">照顧者</button>
          <button class="filter-chip" id="dt-teacher" onclick="setDtFilter('teacher',this)">導師</button>
          <button class="filter-chip" id="dt-subject" onclick="setDtFilter('subject',this)">科任</button>
          <button class="filter-chip" id="dt-school" onclick="setDtFilter('school',this)">學校</button>
        </div>
        <table class="tracker-table" id="trackerTable">
          <thead>
            <tr>
              <th class="check-cell"></th>
              <th>問卷</th>
              <th>事項</th>
              <th>狀態</th>
              <th>緊急程度</th>
              <th>開始日期</th>
              <th>Deadline</th>
              <th>結束日期</th>
              <th>負責人</th>
              <th>備註</th>
            </tr>
          </thead>
          <tbody id="trackerTbody"></tbody>
        </table>
      </div>
    </div>

    <!-- PROGRESS VIEW -->
    <div class="view" id="view-progress">
      <div class="progress-view">
        <div class="stat-grid" id="statGrid"></div>
        <div class="prog-section">
          <div class="prog-section-title">各年度完成率</div>
          <div id="yearProg"></div>
        </div>
        <div class="prog-section">
          <div class="prog-section-title">各分類完成率</div>
          <div id="catProg"></div>
        </div>
        <div class="prog-section">
          <div class="prog-section-title">資料追蹤完成率（依問卷類型）</div>
          <div id="dtProg"></div>
        </div>
      </div>
    </div>

  </div>
</div>
</div>

<!-- Add Task Modal -->
<div class="modal-overlay" id="modalOverlay" onclick="if(event.target===this)closeModal()">
<div class="modal">
  <div class="modal-hdr">
    <span class="modal-title" id="modalTitleText">新增工作</span>
    <button class="close-btn" onclick="closeModal()">✕</button>
  </div>
  <div class="fg">
    <label class="flabel">討論主題 / 工作名稱</label>
    <input type="text" id="iTitle" placeholder="輸入標題...">
  </div>
  <div class="fg">
    <label class="flabel">說明 / 備註</label>
    <textarea id="iDesc" placeholder="說明（選填）"></textarea>
  </div>
  <div class="two-col">
    <div class="fg">
      <label class="flabel">日期</label>
      <input type="date" id="iDate">
    </div>
    <div class="fg">
      <label class="flabel">Deadline</label>
      <input type="date" id="iDeadline">
    </div>
  </div>
  <div class="two-col">
    <div class="fg">
      <label class="flabel">分類</label>
      <select id="iCat">
        <option value="questionnaire">問卷</option>
        <option value="data">資料</option>
        <option value="admin" selected>行政</option>
        <option value="meeting">會議</option>
        <option value="platform">平台</option>
      </select>
    </div>
    <div class="fg">
      <label class="flabel">狀態</label>
      <select id="iStatus">
        <option value="notstarted">Not started</option>
        <option value="inprogress">In progress</option>
        <option value="done">Done</option>
        <option value="blocked">Blocked</option>
      </select>
    </div>
  </div>
  <div class="fg">
    <label class="flabel">緊急程度</label>
    <div class="priority-display" id="priorityStars">
      <span class="pstar" data-v="1" onclick="setPriority(1)">!</span>
      <span class="pstar" data-v="2" onclick="setPriority(2)">!</span>
      <span class="pstar" data-v="3" onclick="setPriority(3)">!</span>
    </div>
  </div>
  <div class="fg">
    <label class="flabel">相關問卷（可複選）</label>
    <div class="chip-row" id="qTagChips">
      <button class="chip" data-q="student" onclick="toggleQChip(this)">學生</button>
      <button class="chip" data-q="sibling" onclick="toggleQChip(this)">手足</button>
      <button class="chip" data-q="parent" onclick="toggleQChip(this)">照顧者</button>
      <button class="chip" data-q="teacher" onclick="toggleQChip(this)">導師</button>
      <button class="chip" data-q="subject" onclick="toggleQChip(this)">科任</button>
      <button class="chip" data-q="school" onclick="toggleQChip(this)">學校</button>
    </div>
  </div>
  <div class="fg">
    <label class="flabel">新增者</label>
    <div class="chip-row" id="creatorChips"></div>
  </div>
  <div class="fg">
    <label class="flabel">參與成員</label>
    <div class="chip-row" id="partChips"></div>
  </div>
  <div class="modal-footer">
    <button class="btn" onclick="closeModal()">取消</button>
    <button class="btn btn-primary" onclick="saveTask()">新增</button>
  </div>
</div>
</div>

<!-- Detail Panel -->
<div class="detail-overlay" id="detailOverlay" onclick="if(event.target===this)closeDetail()">
<div class="detail-panel" id="detailPanel"></div>
</div>

<script>
const MEMBERS=[
  {id:'m1',name:'李研究員',initials:'李',color:'#0C447C',bg:'#B5D4F4'},
  {id:'m2',name:'陳助理',initials:'陳',color:'#085041',bg:'#9FE1CB'},
  {id:'m3',name:'王助理',initials:'王',color:'#712B13',bg:'#F5C4B3'},
  {id:'m4',name:'林助理',initials:'林',color:'#72243E',bg:'#F4C0D1'},
];
const CAT_INFO={
  questionnaire:{label:'問卷',chipCls:'tag-questionnaire',calCls:'cal-questionnaire',filterCls:'active-q'},
  data:{label:'資料',chipCls:'tag-data',calCls:'cal-data',filterCls:'active-d'},
  admin:{label:'行政',chipCls:'tag-admin',calCls:'cal-admin',filterCls:'active-a'},
  meeting:{label:'會議',chipCls:'tag-meeting',calCls:'cal-meeting',filterCls:'active-m'},
  platform:{label:'平台',chipCls:'tag-platform',calCls:'cal-platform',filterCls:'active-p'},
};
const STATUS_INFO={
  notstarted:{label:'Not started',cls:'st-notstarted'},
  inprogress:{label:'In progress',cls:'st-inprogress'},
  done:{label:'Done',cls:'st-done'},
  blocked:{label:'Blocked',cls:'st-blocked'},
};
const QTAG_INFO={
  student:{label:'學生',cls:'qt-student'},
  sibling:{label:'手足',cls:'qt-sibling'},
  parent:{label:'照顧者',cls:'qt-parent'},
  teacher:{label:'導師',cls:'qt-teacher'},
  subject:{label:'科任',cls:'qt-subject'},
  school:{label:'學校',cls:'qt-school'},
};
// CAL chip colours per category
const CAL_CHIP_STYLE={
  questionnaire:'background:#f0ecfd;color:#5b3da8',
  data:'background:#e8f0f9;color:#2a5db0',
  admin:'background:#fef3e2;color:#9a5c00',
  meeting:'background:#e2f4f1;color:#1a6b60',
  platform:'background:#fde8f0;color:#a0306a',
};

const DT_ITEMS=['問卷','第一次檢旗','第二次檢旗','編碼簿','資料檔','標籤說明','資料使用說明','使用手冊'];
const DT_QTYPES=['student','sibling','parent','teacher','subject','school'];

// --- State ---
let calYear=new Date().getFullYear();
let calMonth=new Date().getMonth();
let taskFilter='all';
let dtFilter='all';
let sortKey='date';
let sortAsc=true;
let selCreator='m1';
let selParts=[];
let selPriority=2;
let editingId=null;
let nextId=100;

// Generate tracker data
function makeDtId(qt,item){return `dt_${qt}_${item.replace(/[^a-zA-Z0-9]/g,'')}`}

let dtData=[];
DT_QTYPES.forEach(qt=>{
  DT_ITEMS.forEach(item=>{
    dtData.push({
      id:makeDtId(qt,item),
      qtype:qt,item,
      status:'notstarted',
      priority:2,
      startDate:'',
      deadline:'',
      endDate:'',
      assignee:'',
      notes:'',
      done:false,
    });
  });
});
// Seed some sample data
function setDt(qt,item,s,pr,sd,dl,ed,as,n){
  const d=dtData.find(x=>x.qtype===qt&&x.item===item);
  if(d){d.status=s;d.priority=pr;d.startDate=sd;d.deadline=dl;d.endDate=ed;d.assignee=as;d.notes=n;d.done=(s==='done')}
}
setDt('student','第一次檢旗','inprogress',3,'2024-07-22','2024-10-31','','m1','07/21 報表已出');
setDt('school','第一次檢旗','done',3,'','2024-10-31','','m1','');
setDt('sibling','第一次檢旗','notstarted',3,'','2024-10-31','','','');
setDt('teacher','第一次檢旗','done',3,'2024-07-16','2024-10-31','2024-07-22','m2','07/16 報表已出');
setDt('parent','第一次檢旗','notstarted',3,'','2024-10-31','','m1','');

let tasks=[
  {id:1,title:'W2資料釋出',desc:'',date:'2026-03-31',deadline:'2026-03-31',cat:'data',status:'notstarted',priority:3,qtags:[],creator:'m1',parts:['m1'],notes:''},
  {id:2,title:'W3資料釋出',desc:'',date:'2026-06-19',deadline:'2026-06-19',cat:'data',status:'notstarted',priority:3,qtags:[],creator:'m1',parts:['m1'],notes:''},
  {id:3,title:'第三屆研討會',desc:'',date:'2026-06-11',deadline:'2026-06-13',cat:'meeting',status:'notstarted',priority:3,qtags:[],creator:'m1',parts:['m1','m2'],notes:''},
  {id:4,title:'說明會邀請',desc:'',date:'2026-05-11',deadline:'',cat:'meeting',status:'notstarted',priority:3,qtags:[],creator:'m2',parts:['m2'],notes:''},
  {id:5,title:'W4受訪測試',desc:'',date:'2026-05-11',deadline:'',cat:'data',status:'notstarted',priority:3,qtags:['student'],creator:'m3',parts:['m3'],notes:''},
  {id:6,title:'預計W4調查開始',desc:'',date:'2026-05-18',deadline:'',cat:'questionnaire',status:'notstarted',priority:3,qtags:['student','sibling','parent','teacher','subject','school'],creator:'m1',parts:['m1','m2','m3','m4'],notes:''},
  {id:7,title:'W4問卷上架',desc:'',date:'2026-05-04',deadline:'',cat:'data',status:'notstarted',priority:3,qtags:[],creator:'m4',parts:['m4'],notes:''},
  {id:8,title:'新樣本加入追蹤數活動',desc:'',date:'2026-04-01',deadline:'',cat:'platform',status:'notstarted',priority:2,qtags:[],creator:'m2',parts:['m2'],notes:''},
  {id:9,title:'W4問卷IRB',desc:'',date:'2026-04-01',deadline:'',cat:'admin',status:'notstarted',priority:3,qtags:[],creator:'m1',parts:['m1'],notes:''},
  {id:10,title:'第三屆研討會稿截止',desc:'',date:'2026-03-27',deadline:'',cat:'meeting',status:'notstarted',priority:1,qtags:[],creator:'m1',parts:['m1'],notes:''},
  {id:11,title:'紅包活動',desc:'',date:'2026-02-06',deadline:'2026-03-08',cat:'platform',status:'notstarted',priority:1,qtags:[],creator:'m2',parts:['m2'],notes:''},
  {id:12,title:'W2學生資料釋出',desc:'',date:'2026-01-19',deadline:'',cat:'data',status:'notstarted',priority:3,qtags:['student'],creator:'m1',parts:['m1'],notes:''},
  {id:13,title:'清理樣本問卷',desc:'',date:'2026-04-30',deadline:'',cat:'admin',status:'inprogress',priority:2,qtags:[],creator:'m3',parts:['m3'],notes:''},
];

// --- Helpers ---
const gm=id=>MEMBERS.find(m=>m.id===id);
const today=new Date();
today.setHours(0,0,0,0);

function avatarHtml(m,size){
  const s=size||20;
  return `<div class="avatar-sm" style="width:${s}px;height:${s}px;font-size:${Math.round(s*0.45)}px;background:${m.bg};color:${m.color}" title="${m.name}">${m.initials}</div>`;
}
function priorityHtml(p){
  const marks=Array.from({length:3},(_,i)=>`<span style="color:${i<p?'#e84b4b':'#d1cfc9'};font-size:12px">!</span>`).join('');
  return `<span class="priority">${marks}</span>`;
}
function statusHtml(s){
  const si=STATUS_INFO[s]||STATUS_INFO.notstarted;
  return `<span class="status ${si.cls}">${si.label}</span>`;
}
function tagHtml(cat){
  const ci=CAT_INFO[cat]||CAT_INFO.admin;
  return `<span class="tag ${ci.chipCls}">${ci.label}</span>`;
}
function qtagHtml(q){
  const qi=QTAG_INFO[q];
  return qi?`<span class="qtag ${qi.cls}">${qi.label}</span>`:'';
}

// --- Team ---
function renderTeam(){
  document.getElementById('teamGrid').innerHTML=MEMBERS.map(m=>`
    <div class="member-cell">
      <div class="avatar" style="background:${m.bg};color:${m.color}">${m.initials}</div>
      <div class="member-name">${m.name}</div>
    </div>`).join('');
}

// --- Calendar ---
function renderCalendar(){
  const MONTHS=['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月'];
  ['','2'].forEach(suf=>{
    const tEl=document.getElementById('calMonthTitle'+suf);
    const yEl=document.getElementById('calYearLabel'+suf);
    const bEl=document.getElementById('calBody'+suf);
    if(!tEl)return;
    tEl.textContent=MONTHS[calMonth];
    yEl.textContent=calYear+'年';

    const first=new Date(calYear,calMonth,1);
    const lastDay=new Date(calYear,calMonth+1,0).getDate();
    const startDow=first.getDay();
    const monthStr=`${calYear}-${String(calMonth+1).padStart(2,'0')}`;
    const monthTasks=tasks.filter(t=>t.date&&t.date.startsWith(monthStr));

    let cells='';
    for(let i=0;i<startDow;i++) cells+=`<div class="cal-cell empty"></div>`;
    for(let d=1;d<=lastDay;d++){
      const ds=`${calYear}-${String(calMonth+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
      const dt=monthTasks.filter(t=>t.date===ds);
      const isT=new Date(calYear,calMonth,d).getTime()===today.getTime();
      const numCls=isT?'cal-date-num today':'cal-date-num';
      let chips=dt.slice(0,3).map(t=>{
        const cr=gm(t.creator);
        const st=CAL_CHIP_STYLE[t.cat]||'background:#f0f0f0;color:#555';
        return `<div class="cal-chip" style="${st}" onclick="openDetail(${t.id});event.stopPropagation()">
          ${cr?`<div style="width:12px;height:12px;border-radius:50%;background:${cr.bg};color:${cr.color};font-size:7px;display:flex;align-items:center;justify-content:center;flex-shrink:0">${cr.initials}</div>`:''}
          ${t.title}</div>`;
      }).join('');
      const more=dt.length>3?`<div class="cal-more">+${dt.length-3}</div>`:'';
      cells+=`<div class="cal-cell" onclick="openAddOnDate('${ds}')">${'<div class="'+numCls+'">'+d+'</div>'}${chips}${more}</div>`;
    }
    const total=startDow+lastDay;
    const rem=(7-total%7)%7;
    for(let i=0;i<rem;i++) cells+=`<div class="cal-cell empty"></div>`;
    bEl.innerHTML=cells;
  });

  renderDashSidebar();
}

function changeCalMonth(d){
  calMonth+=d;
  if(calMonth>11){calMonth=0;calYear++}
  if(calMonth<0){calMonth=11;calYear--}
  renderCalendar();
}
function goToday(){
  calYear=new Date().getFullYear();
  calMonth=new Date().getMonth();
  renderCalendar();
}

function renderDashSidebar(){
  const now=new Date();
  const curY=now.getFullYear(),curM=now.getMonth();
  const nextY=curM===11?curY+1:curY,nextM=curM===11?0:curM+1;
  const curStr=`${curY}-${String(curM+1).padStart(2,'0')}`;
  const nextStr=`${nextY}-${String(nextM+1).padStart(2,'0')}`;

  const cur=tasks.filter(t=>t.date&&t.date.startsWith(curStr)&&t.status!=='done');
  const nxt=tasks.filter(t=>t.date&&t.date.startsWith(nextStr)&&t.status!=='done');
  const dl=tasks.filter(t=>t.deadline&&t.priority===3&&t.status!=='done').sort((a,b)=>a.deadline.localeCompare(b.deadline)).slice(0,5);
  const mtg=tasks.filter(t=>t.cat==='meeting'&&t.date&&t.date>=curStr).sort((a,b)=>a.date.localeCompare(b.date)).slice(0,6);

  const renderSideItem=(t,dotColor)=>`<div class="cal-side-item" onclick="openDetail(${t.id})">
    <div class="cal-side-dot" style="background:${dotColor}"></div>
    <div>
      <div style="font-size:12px">${t.title}</div>
      <div style="font-size:10px;color:var(--text3)">${t.date||t.deadline||''}</div>
    </div>
  </div>`;

  document.getElementById('dashCurrentMonth').innerHTML=cur.length?cur.map(t=>renderSideItem(t,'#2a5db0')).join(''):`<div style="font-size:11px;color:var(--text3);padding:4px 8px">無</div>`;
  document.getElementById('dashNextMonth').innerHTML=nxt.length?nxt.map(t=>renderSideItem(t,'#9a5c00')).join(''):`<div style="font-size:11px;color:var(--text3);padding:4px 8px">無</div>`;
  document.getElementById('dashDeadlines').innerHTML=dl.length?dl.map(t=>renderSideItem(t,'#b03030')).join(''):`<div style="font-size:11px;color:var(--text3);padding:4px 8px">無</div>`;
  document.getElementById('dashMeetings').innerHTML=mtg.length?mtg.map(t=>`<div class="cal-side-item" onclick="openDetail(${t.id})">
    <div class="cal-side-dot" style="background:#1a6b60"></div>
    <div>
      <div style="font-size:12px">${t.title}</div>
      <div style="font-size:10px;color:var(--text3)">${t.date||''}</div>
    </div>
  </div>`).join(''):`<div style="font-size:11px;color:var(--text3);padding:4px 8px">無</div>`;
}

// --- Task List ---
let sortedTasks=[];
function renderTaskList(){
  let filtered=taskFilter==='all'?[...tasks]:tasks.filter(t=>t.cat===taskFilter);
  filtered.sort((a,b)=>{
    let va=a[sortKey]||'',vb=b[sortKey]||'';
    if(sortKey==='priority'){va=a.priority;vb=b.priority}
    if(sortKey==='status'){va=a.status;vb=b.status}
    return sortAsc?(va>vb?1:-1):(va<vb?1:-1);
  });
  sortedTasks=filtered;
  document.getElementById('taskTbody').innerHTML=filtered.map(t=>{
    const cr=gm(t.creator);
    const pts=(t.parts||[]).map(gm).filter(Boolean);
    const si=STATUS_INFO[t.status]||STATUS_INFO.notstarted;
    return `<tr class="${t.done?'done-row':''}">
      <td class="check-cell"><div class="check ${t.done?'done':''}" onclick="toggleTask(${t.id})">${t.done?'✓':''}</div></td>
      <td><div class="task-title-cell ${t.done?'done':''}" onclick="openDetail(${t.id})">${t.title}</div>${t.qtags&&t.qtags.length?'<div style="display:flex;gap:3px;flex-wrap:wrap;margin-top:3px">'+t.qtags.map(qtagHtml).join('')+'</div>':''}</td>
      <td style="white-space:nowrap"><div style="font-size:12px;color:var(--text2)">${t.date||''}</div>${t.deadline&&t.deadline!==t.date?`<div style="font-size:11px;color:#b03030">→ ${t.deadline}</div>`:''}</td>
      <td><div style="display:flex;align-items:center;gap:4px">
        ${cr?avatarHtml(cr):''}
        <div class="avatars-stack">${pts.map(m=>avatarHtml(m)).join('')}</div>
      </div></td>
      <td class="priority-cell">${priorityHtml(t.priority)}</td>
      <td><span class="status ${si.cls}">${si.label}</span></td>
      <td>${tagHtml(t.cat)}</td>
      <td style="font-size:11px;color:var(--text2);max-width:120px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${t.notes||''}</td>
    </tr>`;
  }).join('');
}
function setTaskFilter(f,el){
  taskFilter=f;
  document.querySelectorAll('[id^="fl-"]').forEach(b=>b.classList.remove('active'));
  if(el)el.classList.add('active');
  renderTaskList();
}
function sortTasks(k){
  if(sortKey===k)sortAsc=!sortAsc; else{sortKey=k;sortAsc=true}
  renderTaskList();
}
function toggleTask(id){
  const t=tasks.find(x=>x.id===id);
  if(!t)return;
  t.done=!t.done;
  t.status=t.done?'done':(t.status==='done'?'notstarted':t.status);
  renderTaskList();renderCalendar();renderProgress();
}

// --- Data Tracker ---
function renderTracker(){
  let filtered=dtFilter==='all'?[...dtData]:dtData.filter(x=>x.qtype===dtFilter);
  document.getElementById('trackerTbody').innerHTML=filtered.map(x=>{
    const qi=QTAG_INFO[x.qtype]||{label:x.qtype,cls:''};
    const as=x.assignee?gm(x.assignee):null;
    const si=STATUS_INFO[x.status]||STATUS_INFO.notstarted;
    return `<tr class="${x.done?'done-row':''}">
      <td class="check-cell"><div class="check ${x.done?'done':''}" onclick="toggleDt('${x.id}')">${x.done?'✓':''}</div></td>
      <td><span class="qtag ${qi.cls}">${qi.label}</span></td>
      <td style="font-size:13px;font-weight:500">${x.item}</td>
      <td><span class="status ${si.cls}">${si.label}</span></td>
      <td>${priorityHtml(x.priority)}</td>
      <td style="font-size:12px;color:var(--text2);white-space:nowrap">${x.startDate||''}</td>
      <td style="font-size:12px;color:${x.deadline?'#b03030':'var(--text2)'};white-space:nowrap">${x.deadline||''}</td>
      <td style="font-size:12px;color:var(--text2);white-space:nowrap">${x.endDate||''}</td>
      <td>${as?`<div style="display:flex;align-items:center;gap:5px">${avatarHtml(as)}<span style="font-size:11px;color:var(--text2)">${as.name}</span></div>`:''}</td>
      <td style="font-size:11px;color:var(--text2);max-width:120px">${x.notes||''}</td>
    </tr>`;
  }).join('');
}
function setDtFilter(f,el){
  dtFilter=f;
  document.querySelectorAll('[id^="dt-"]').forEach(b=>b.classList.remove('active'));
  if(el)el.classList.add('active');
  renderTracker();
}
function toggleDt(id){
  const x=dtData.find(d=>d.id===id);
  if(!x)return;
  x.done=!x.done;
  x.status=x.done?'done':(x.status==='done'?'notstarted':x.status);
  renderTracker();renderProgress();
}

// --- Progress ---
function renderProgress(){
  const total=tasks.length;
  const done=tasks.filter(t=>t.done).length;
  const high=tasks.filter(t=>t.priority===3&&!t.done).length;
  const inprog=tasks.filter(t=>t.status==='inprogress').length;
  document.getElementById('statGrid').innerHTML=`
    <div class="stat-card"><div class="stat-val">${total}</div><div class="stat-lbl">工作總數</div></div>
    <div class="stat-card"><div class="stat-val">${done}</div><div class="stat-lbl">已完成</div></div>
    <div class="stat-card"><div class="stat-val">${inprog}</div><div class="stat-lbl">進行中</div></div>
    <div class="stat-card"><div class="stat-val">${high}</div><div class="stat-lbl">高優先待辦</div></div>`;

  const years=Array.from(new Set(tasks.map(t=>t.date?t.date.split('-')[0]:''))).filter(Boolean).sort();
  document.getElementById('yearProg').innerHTML=years.map(y=>{
    const yt=tasks.filter(t=>t.date&&t.date.startsWith(y));
    const yd=yt.filter(t=>t.done).length;
    const pct=yt.length?Math.round(yd/yt.length*100):0;
    return `<div class="prog-row"><div class="prog-label-row"><span>${y}</span><span>${yd}/${yt.length} (${pct}%)</span></div><div class="prog-bar"><div class="prog-fill" style="width:${pct}%"></div></div></div>`;
  }).join('');

  const cats=['questionnaire','data','admin','meeting','platform'];
  const catNames={questionnaire:'問卷',data:'資料',admin:'行政',meeting:'會議',platform:'平台'};
  const catColors={questionnaire:'cat-q',data:'cat-d',admin:'cat-a',meeting:'cat-m',platform:'cat-q'};
  document.getElementById('catProg').innerHTML=cats.map(c=>{
    const ct=tasks.filter(t=>t.cat===c);
    const cd=ct.filter(t=>t.done).length;
    const pct=ct.length?Math.round(cd/ct.length*100):0;
    return `<div class="prog-row"><div class="prog-label-row"><span>${catNames[c]}</span><span>${cd}/${ct.length} (${pct}%)</span></div><div class="prog-bar"><div class="prog-fill ${catColors[c]}" style="width:${pct}%"></div></div></div>`;
  }).join('');

  const qtypes=['student','sibling','parent','teacher','subject','school'];
  const qtypeNames={student:'學生',sibling:'手足',parent:'照顧者',teacher:'導師',subject:'科任',school:'學校'};
  document.getElementById('dtProg').innerHTML=qtypes.map(q=>{
    const qt=dtData.filter(x=>x.qtype===q);
    const qd=qt.filter(x=>x.done).length;
    const pct=qt.length?Math.round(qd/qt.length*100):0;
    const qi=QTAG_INFO[q];
    return `<div class="prog-row"><div class="prog-label-row"><span><span class="qtag ${qi.cls}">${qtypeNames[q]}</span></span><span>${qd}/${qt.length} (${pct}%)</span></div><div class="prog-bar"><div class="prog-fill" style="width:${pct}%"></div></div></div>`;
  }).join('');
}

// --- Modal ---
function openAdd(){
  editingId=null;
  selCreator='m1';selParts=[];selPriority=2;
  document.getElementById('iTitle').value='';
  document.getElementById('iDesc').value='';
  document.getElementById('iDate').value='';
  document.getElementById('iDeadline').value='';
  document.getElementById('iCat').value='admin';
  document.getElementById('iStatus').value='notstarted';
  document.querySelectorAll('#qTagChips .chip').forEach(b=>b.className='chip');
  renderModalMembers();
  updatePriorityStars();
  document.getElementById('modalTitleText').textContent='新增工作';
  document.getElementById('modalOverlay').classList.add('open');
  setTimeout(()=>document.getElementById('iTitle').focus(),50);
}
function openAddOnDate(ds){
  openAdd();
  document.getElementById('iDate').value=ds;
}
function closeModal(){
  document.getElementById('modalOverlay').classList.remove('open');
}
function renderModalMembers(){
  document.getElementById('creatorChips').innerHTML=MEMBERS.map(m=>`
    <div class="mem-chip ${m.id===selCreator?'sel':''}" id="cr-${m.id}" onclick="pickCreator('${m.id}')">
      <div style="width:16px;height:16px;border-radius:50%;background:${m.bg};color:${m.color};font-size:8px;display:flex;align-items:center;justify-content:center">${m.initials}</div>${m.name}
    </div>`).join('');
  document.getElementById('partChips').innerHTML=MEMBERS.map(m=>`
    <div class="mem-chip ${selParts.includes(m.id)?'sel':''}" id="pt-${m.id}" onclick="togglePart('${m.id}')">
      <div style="width:16px;height:16px;border-radius:50%;background:${m.bg};color:${m.color};font-size:8px;display:flex;align-items:center;justify-content:center">${m.initials}</div>${m.name}
    </div>`).join('');
}
function pickCreator(id){
  selCreator=id;
  document.querySelectorAll('[id^="cr-"]').forEach(el=>el.classList.remove('sel'));
  const el=document.getElementById('cr-'+id);
  if(el)el.classList.add('sel');
}
function togglePart(id){
  const i=selParts.indexOf(id);
  if(i>=0)selParts.splice(i,1);else selParts.push(id);
  document.querySelectorAll('[id^="pt-"]').forEach(el=>el.classList.remove('sel'));
  selParts.forEach(pid=>{const el=document.getElementById('pt-'+pid);if(el)el.classList.add('sel')});
}
function setPriority(v){selPriority=v;updatePriorityStars()}
function updatePriorityStars(){
  document.querySelectorAll('.pstar').forEach(s=>{
    s.classList.toggle('on',parseInt(s.dataset.v)<=selPriority);
  });
}
function toggleQChip(btn){
  const q=btn.dataset.q;
  if(btn.classList.contains('on-'+q))btn.className='chip';
  else btn.className='chip on-'+q;
}
function saveTask(){
  const title=document.getElementById('iTitle').value.trim();
  if(!title){alert('請輸入工作名稱');return}
  const qtags=[...document.querySelectorAll('#qTagChips .chip')].filter(b=>b.className!=='chip').map(b=>b.dataset.q);
  const data={
    title,desc:document.getElementById('iDesc').value,
    date:document.getElementById('iDate').value,
    deadline:document.getElementById('iDeadline').value,
    cat:document.getElementById('iCat').value,
    status:document.getElementById('iStatus').value,
    priority:selPriority,qtags,
    creator:selCreator,parts:[...selParts],notes:'',
    done:document.getElementById('iStatus').value==='done',
  };
  if(editingId){
    const idx=tasks.findIndex(t=>t.id===editingId);
    if(idx>=0)tasks[idx]={...tasks[idx],...data};
  }else{
    tasks.push({id:nextId++,...data});
  }
  closeModal();
  renderCalendar();renderTaskList();renderProgress();
}

// --- Detail panel ---
function openDetail(id){
  const t=tasks.find(x=>x.id===id);
  if(!t)return;
  const cr=gm(t.creator);
  const pts=(t.parts||[]).map(gm).filter(Boolean);
  const si=STATUS_INFO[t.status]||STATUS_INFO.notstarted;
  document.getElementById('detailPanel').innerHTML=`
    <div style="display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:16px">
      <div style="font-size:17px;font-weight:600;flex:1;line-height:1.4">${t.title}</div>
      <button class="close-btn" onclick="closeDetail()" style="flex-shrink:0;margin-left:8px">✕</button>
    </div>
    ${t.desc?`<div style="font-size:13px;color:var(--text2);margin-bottom:16px">${t.desc}</div>`:''}
    <div class="detail-field">
      <div class="detail-field-label">狀態</div>
      <div class="detail-field-val">${statusHtml(t.status)}</div>
    </div>
    <div class="detail-field">
      <div class="detail-field-label">分類</div>
      <div class="detail-field-val">${tagHtml(t.cat)}</div>
    </div>
    <div class="detail-field">
      <div class="detail-field-label">緊急程度</div>
      <div class="detail-field-val">${priorityHtml(t.priority)}</div>
    </div>
    ${t.date?`<div class="detail-field"><div class="detail-field-label">日期</div><div class="detail-field-val">${t.date}</div></div>`:''}
    ${t.deadline?`<div class="detail-field"><div class="detail-field-label">Deadline</div><div class="detail-field-val" style="color:#b03030">${t.deadline}</div></div>`:''}
    ${t.qtags&&t.qtags.length?`<div class="detail-field"><div class="detail-field-label">相關問卷</div><div style="display:flex;gap:4px;flex-wrap:wrap">${t.qtags.map(qtagHtml).join('')}</div></div>`:''}
    <div class="detail-field">
      <div class="detail-field-label">新增者</div>
      <div style="display:flex;align-items:center;gap:6px">${cr?`${avatarHtml(cr,22)}<span style="font-size:13px">${cr.name}</span>`:'-'}</div>
    </div>
    <div class="detail-field">
      <div class="detail-field-label">參與成員</div>
      <div style="display:flex;gap:6px;flex-wrap:wrap">${pts.map(m=>`<div style="display:flex;align-items:center;gap:4px">${avatarHtml(m,22)}<span style="font-size:12px">${m.name}</span></div>`).join('')||'-'}</div>
    </div>
    <div class="detail-field">
      <div class="detail-field-label">備註</div>
      <textarea class="notes-input" id="detailNotes" placeholder="新增備註...">${t.notes||''}</textarea>
    </div>
    <div style="display:flex;gap:8px;margin-top:8px">
      <button class="btn btn-primary" onclick="saveDetailNotes(${t.id})">儲存備註</button>
      <button class="btn" onclick="editTask(${t.id})">編輯</button>
      <button class="btn" style="margin-left:auto;color:#b03030;border-color:#f0a0a0" onclick="deleteTask(${t.id})">刪除</button>
    </div>`;
  document.getElementById('detailOverlay').classList.add('open');
}
function closeDetail(){document.getElementById('detailOverlay').classList.remove('open')}
function saveDetailNotes(id){
  const t=tasks.find(x=>x.id===id);
  if(t){t.notes=document.getElementById('detailNotes').value;renderTaskList()}
  closeDetail();
}
function editTask(id){
  const t=tasks.find(x=>x.id===id);
  if(!t)return;
  closeDetail();
  editingId=id;
  selCreator=t.creator||'m1';
  selParts=[...(t.parts||[])];
  selPriority=t.priority||2;
  document.getElementById('iTitle').value=t.title;
  document.getElementById('iDesc').value=t.desc||'';
  document.getElementById('iDate').value=t.date||'';
  document.getElementById('iDeadline').value=t.deadline||'';
  document.getElementById('iCat').value=t.cat||'admin';
  document.getElementById('iStatus').value=t.status||'notstarted';
  document.querySelectorAll('#qTagChips .chip').forEach(b=>{
    const q=b.dataset.q;
    b.className=(t.qtags||[]).includes(q)?'chip on-'+q:'chip';
  });
  renderModalMembers();
  updatePriorityStars();
  document.getElementById('modalTitleText').textContent='編輯工作';
  document.getElementById('modalOverlay').classList.add('open');
}
function deleteTask(id){
  if(!confirm('確定要刪除這項工作嗎？'))return;
  tasks=tasks.filter(t=>t.id!==id);
  closeDetail();
  renderCalendar();renderTaskList();renderProgress();
}

// --- View switch ---
function switchView(v,el){
  document.querySelectorAll('.view').forEach(x=>x.classList.remove('active'));
  document.getElementById('view-'+v).classList.add('active');
  document.querySelectorAll('.sb-item').forEach(x=>x.classList.remove('active'));
  if(el)el.classList.add('active');
  const titles={dashboard:'總覽',calendar:'行事曆',tasklist:'工作清單',datatracker:'資料追蹤',progress:'進度追蹤'};
  document.getElementById('topbarTitle').textContent=titles[v]||v;
  if(v==='tasklist')renderTaskList();
  if(v==='datatracker')renderTracker();
  if(v==='progress')renderProgress();
  if(v==='calendar')renderCalendar();
}

// --- Init ---
renderTeam();
renderCalendar();
renderTaskList();
</script>
</body>
</html>
