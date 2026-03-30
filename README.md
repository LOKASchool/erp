<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Loka School — Learning Management System</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,400&family=Barlow+Condensed:wght@700;800;900&family=Playfair+Display:ital,wght@0,600;1,400&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
:root{
  --Y:#F2C20A; --Yd:#D4A800; --Yb:#FFFBE6; --Yl:#FEF3C7;
  --M:#7A1550; --Md:#5C0D3C; --Ml:#A8317A; --Mb:#F9EEF5;
  --ink:#1A0D14; --muted:#6B5060; --pale:#A890A0;
  --white:#FFFFFF; --off:#FAFAF8; --border:rgba(122,21,80,.1);
  --green:#16A34A; --greenb:#DCFCE7;
  --blue:#1D4ED8;  --blueb:#DBEAFE;
  --orange:#EA580C; --orangeb:#FFEDD5;
  --red:#DC2626; --redb:#FEE2E2;
  --fD:'Barlow Condensed',sans-serif;
  --fB:'Barlow',sans-serif;
  --fH:'Playfair Display',serif;
  --sidebar:240px;
  --header:56px;
}
html,body{height:100%;overflow:hidden;}
body{font-family:var(--fB);background:var(--off);color:var(--ink);font-size:14px;}

/* ─── SCROLLBAR ─── */
::-webkit-scrollbar{width:5px;height:5px;}
::-webkit-scrollbar-track{background:transparent;}
::-webkit-scrollbar-thumb{background:rgba(122,21,80,.2);border-radius:4px;}

/* ─── SCREENS ─── */
#login-screen{display:flex;align-items:center;justify-content:center;height:100vh;background:var(--M);position:relative;overflow:hidden;}
#app-screen{display:none;height:100vh;flex-direction:row;}
#app-screen.visible{display:flex;}

/* ─── LOGIN ─── */
.login-bg{position:absolute;inset:0;opacity:.06;}
.login-card{background:var(--white);border-radius:12px;padding:48px 44px;width:480px;max-width:94vw;position:relative;z-index:2;}
.login-logo{font-family:var(--fD);font-size:52px;font-weight:900;color:var(--Y);line-height:.9;margin-bottom:4px;text-shadow:2px 2px 0 var(--M);}
.login-tagline{font-family:var(--fB);font-size:12px;font-weight:500;color:var(--muted);letter-spacing:.06em;margin-bottom:28px;}
.login-heading{font-family:var(--fH);font-size:20px;color:var(--M);margin-bottom:6px;}
.login-sub{font-size:12px;color:var(--muted);margin-bottom:24px;}
.role-cards{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:24px;}
.role-card{border:2px solid var(--border);border-radius:8px;padding:16px 12px;text-align:center;cursor:pointer;transition:all .18s;background:var(--white);}
.role-card:hover{border-color:var(--Y);background:var(--Yb);}
.role-card.selected{border-color:var(--M);background:var(--Mb);}
.role-card .rc-icon{font-size:28px;margin-bottom:8px;display:block;}
.role-card .rc-label{font-size:13px;font-weight:700;color:var(--M);}
.role-card .rc-desc{font-size:10.5px;color:var(--muted);margin-top:2px;}
.user-select{margin-bottom:20px;}
.user-select label{font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);display:block;margin-bottom:6px;}
.user-select select{width:100%;padding:10px 12px;border:1.5px solid var(--border);border-radius:6px;font-family:var(--fB);font-size:13px;color:var(--ink);background:var(--white);outline:none;cursor:pointer;}
.user-select select:focus{border-color:var(--M);}
.login-btn{width:100%;padding:12px;background:var(--M);color:var(--Y);font-family:var(--fD);font-size:16px;font-weight:900;letter-spacing:.08em;text-transform:uppercase;border:none;border-radius:6px;cursor:pointer;transition:background .18s;}
.login-btn:hover{background:var(--Md);}
.login-btn:disabled{background:#ccc;color:#888;cursor:not-allowed;}

/* ─── SIDEBAR ─── */
#sidebar{width:var(--sidebar);background:var(--M);display:flex;flex-direction:column;height:100vh;flex-shrink:0;overflow:hidden;}
.sb-logo{padding:18px 20px 16px;border-bottom:1px solid rgba(255,255,255,.08);}
.sb-logo-text{font-family:var(--fD);font-size:36px;font-weight:900;color:var(--Y);line-height:.9;}
.sb-logo-sub{font-size:10px;font-weight:500;color:rgba(255,255,255,.4);letter-spacing:.08em;margin-top:3px;}
.sb-user{padding:14px 20px;border-bottom:1px solid rgba(255,255,255,.08);display:flex;align-items:center;gap:10px;}
.sb-avatar{width:34px;height:34px;border-radius:4px;background:var(--Y);display:flex;align-items:center;justify-content:center;font-family:var(--fD);font-size:13px;font-weight:900;color:var(--M);flex-shrink:0;}
.sb-user-info{min-width:0;}
.sb-user-name{font-size:12.5px;font-weight:700;color:var(--white);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.sb-user-role{font-size:10px;color:rgba(255,255,255,.45);font-weight:500;text-transform:uppercase;letter-spacing:.06em;}
.sb-nav{flex:1;overflow-y:auto;padding:12px 10px;}
.sb-section{font-size:9.5px;font-weight:700;letter-spacing:.16em;text-transform:uppercase;color:rgba(255,255,255,.3);padding:10px 10px 6px;margin-top:8px;}
.sb-item{display:flex;align-items:center;gap:10px;padding:9px 10px;border-radius:6px;cursor:pointer;transition:all .15s;margin-bottom:2px;text-decoration:none;}
.sb-item:hover{background:rgba(255,255,255,.08);}
.sb-item.active{background:var(--Y);}
.sb-item .si-icon{font-size:16px;width:20px;text-align:center;flex-shrink:0;}
.sb-item .si-label{font-size:13px;font-weight:600;color:rgba(255,255,255,.8);}
.sb-item.active .si-label{color:var(--M);}
.sb-item .si-badge{margin-left:auto;background:var(--orange);color:white;font-size:10px;font-weight:700;padding:2px 6px;border-radius:100px;}
.sb-item.active .si-badge{background:var(--M);color:var(--Y);}
.sb-bottom{padding:12px 10px;border-top:1px solid rgba(255,255,255,.08);}
.sb-logout{display:flex;align-items:center;gap:10px;padding:9px 10px;border-radius:6px;cursor:pointer;transition:all .15s;}
.sb-logout:hover{background:rgba(255,0,0,.12);}
.sb-logout span{font-size:13px;font-weight:600;color:rgba(255,255,255,.5);}

/* ─── MAIN AREA ─── */
#main{flex:1;display:flex;flex-direction:column;overflow:hidden;}
#topbar{height:var(--header);background:var(--white);border-bottom:1px solid var(--border);display:flex;align-items:center;padding:0 24px;gap:16px;flex-shrink:0;}
.tb-page-title{font-family:var(--fH);font-size:18px;font-weight:600;color:var(--M);flex:1;}
.tb-date{font-size:12px;color:var(--muted);font-weight:500;}
.tb-notif{width:34px;height:34px;border-radius:6px;background:var(--Mb);display:flex;align-items:center;justify-content:center;cursor:pointer;position:relative;font-size:16px;}
.tb-notif-dot{position:absolute;top:6px;right:6px;width:7px;height:7px;border-radius:50%;background:var(--orange);border:2px solid var(--white);}
#content{flex:1;overflow-y:auto;padding:24px;}

/* ─── CARDS ─── */
.card{background:var(--white);border-radius:10px;border:1px solid var(--border);padding:20px;}
.card-y{background:var(--Y);border-radius:10px;padding:20px;}
.card-m{background:var(--M);border-radius:10px;padding:20px;color:var(--white);}
.card-mb{background:var(--Mb);border-radius:10px;padding:20px;border:1px solid rgba(122,21,80,.12);}
.card-yb{background:var(--Yb);border-radius:10px;padding:20px;border:1px solid rgba(242,194,10,.3);}

/* ─── STAT CARDS ─── */
.stats-row{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-bottom:22px;}
.stat-card{background:var(--white);border-radius:10px;border:1px solid var(--border);padding:18px 20px;}
.stat-card.y{background:var(--Y);}
.stat-card.m{background:var(--M);}
.stat-num{font-family:var(--fD);font-size:38px;font-weight:900;line-height:1;margin-bottom:4px;}
.stat-card.y .stat-num{color:var(--M);}
.stat-card.m .stat-num{color:var(--Y);}
.stat-card:not(.y):not(.m) .stat-num{color:var(--M);}
.stat-label{font-size:11px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;}
.stat-card.y .stat-label{color:rgba(90,20,60,.7);}
.stat-card.m .stat-label{color:rgba(242,194,10,.7);}
.stat-card:not(.y):not(.m) .stat-label{color:var(--muted);}
.stat-sub{font-size:11px;margin-top:5px;}
.stat-card.y .stat-sub{color:rgba(90,20,60,.6);}
.stat-card.m .stat-sub{color:rgba(255,255,255,.5);}
.stat-card:not(.y):not(.m) .stat-sub{color:var(--pale);}

/* ─── GRID LAYOUTS ─── */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:18px;}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:14px;}
.g-main{display:grid;grid-template-columns:1fr 320px;gap:18px;}

/* ─── SECTION HEADERS ─── */
.sec-hdr{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;}
.sec-title{font-family:var(--fH);font-size:17px;font-weight:600;color:var(--M);}
.sec-action{font-size:12px;font-weight:700;color:var(--M);cursor:pointer;padding:5px 12px;border-radius:4px;border:1.5px solid var(--M);background:transparent;transition:all .15s;display:flex;align-items:center;gap:5px;}
.sec-action:hover{background:var(--M);color:var(--Y);}

/* ─── BADGES / TAGS ─── */
.badge{display:inline-flex;align-items:center;padding:3px 10px;border-radius:100px;font-size:11px;font-weight:700;letter-spacing:.02em;}
.badge-upcoming{background:var(--Yl);color:var(--Yd);}
.badge-ongoing{background:var(--M);color:var(--Y);}
.badge-completed{background:var(--greenb);color:var(--green);}
.badge-overdue{background:var(--redb);color:var(--red);}
.badge-homework{background:var(--blueb);color:var(--blue);}
.badge-assignment{background:var(--Mb);color:var(--M);}
.badge-festival{background:#FFF3E0;color:#E65100;}
.badge-event{background:var(--blueb);color:var(--blue);}
.badge-national{background:var(--orangeb);color:var(--orange);}

/* ─── LESSON CARDS ─── */
.lesson-card{background:var(--white);border-radius:10px;border:1px solid var(--border);padding:16px 18px;margin-bottom:10px;cursor:pointer;transition:all .15s;}
.lesson-card:hover{border-color:var(--Y);box-shadow:0 2px 8px rgba(122,21,80,.08);}
.lesson-card.ongoing{border-left:4px solid var(--M);}
.lesson-card.upcoming{border-left:4px solid var(--Y);}
.lesson-card.completed{border-left:4px solid var(--green);opacity:.7;}
.lc-top{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:8px;}
.lc-subject{font-size:10px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);}
.lc-title{font-family:var(--fH);font-size:16px;font-weight:600;color:var(--M);margin:3px 0;}
.lc-meta{display:flex;align-items:center;gap:12px;font-size:12px;color:var(--muted);}
.lc-meta span{display:flex;align-items:center;gap:4px;}
.lc-objectives{margin-top:10px;}
.lc-obj-item{font-size:12px;color:#374040;padding:3px 0;display:flex;align-items:center;gap:7px;}
.lc-obj-item::before{content:'';width:5px;height:5px;border-radius:50%;background:var(--Y);border:1.5px solid var(--M);flex-shrink:0;}

/* ─── ASSIGNMENT CARDS ─── */
.assign-card{background:var(--white);border-radius:10px;border:1px solid var(--border);padding:16px 18px;margin-bottom:10px;}
.assign-card.overdue{border-left:4px solid var(--red);}
.assign-card.active{border-left:4px solid var(--M);}
.ac-top{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:6px;}
.ac-title{font-size:14.5px;font-weight:700;color:var(--ink);}
.ac-sub{font-size:12px;color:var(--muted);margin-bottom:8px;}
.ac-due{font-size:12px;font-weight:600;color:var(--orange);}
.ac-due.ok{color:var(--green);}
.progress-bar{height:6px;background:var(--Yl);border-radius:3px;overflow:hidden;margin-top:8px;}
.progress-fill{height:100%;background:var(--M);border-radius:3px;transition:width .4s;}

/* ─── TIMETABLE ─── */
.tt-wrapper{overflow-x:auto;}
.tt-table{width:100%;border-collapse:collapse;min-width:700px;}
.tt-table th{background:var(--M);color:var(--Y);padding:9px 12px;font-size:11px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;text-align:left;}
.tt-table th:first-child{width:100px;border-radius:6px 0 0 0;}
.tt-table th:last-child{border-radius:0 6px 0 0;}
.tt-table td{padding:8px 12px;border-bottom:1px solid var(--border);font-size:12.5px;vertical-align:top;}
.tt-table tr:nth-child(even) td{background:rgba(242,194,10,.04);}
.tt-time{font-weight:700;color:var(--M);font-size:11px;white-space:nowrap;}
.tt-subject{font-weight:700;color:var(--ink);}
.tt-teacher{font-size:11px;color:var(--muted);}
.tt-break{background:var(--Yb)!important;color:var(--Yd);font-weight:600;font-size:12px;text-align:center;}
.tt-lunch{background:var(--greenb)!important;color:var(--green);font-weight:600;font-size:12px;text-align:center;}
.tt-today{background:var(--Mb)!important;}

/* ─── CALENDAR ─── */
.cal-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:4px;}
.cal-header{display:grid;grid-template-columns:repeat(7,1fr);gap:4px;margin-bottom:6px;}
.cal-day-hdr{font-size:10.5px;font-weight:700;color:var(--muted);text-align:center;padding:4px 0;letter-spacing:.06em;text-transform:uppercase;}
.cal-day{border-radius:6px;padding:6px;min-height:48px;border:1px solid transparent;cursor:pointer;transition:all .12s;position:relative;}
.cal-day:hover{border-color:var(--Y);background:var(--Yb);}
.cal-day.today{background:var(--M);border-color:var(--M);}
.cal-day.today .cal-day-num{color:var(--Y);}
.cal-day.has-event{border-color:rgba(242,194,10,.4);background:var(--Yb);}
.cal-day.other-month{opacity:.3;}
.cal-day-num{font-size:12px;font-weight:700;color:var(--ink);margin-bottom:3px;}
.cal-event-dot{width:6px;height:6px;border-radius:50%;display:inline-block;margin:1px;}
.cal-events-list{margin-top:14px;}
.cal-event-item{display:flex;align-items:center;gap:10px;padding:10px 14px;border-radius:8px;margin-bottom:8px;border:1px solid var(--border);background:var(--white);}
.cal-event-date{font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);min-width:36px;text-align:center;line-height:1;}
.cal-event-month{font-size:9px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:.06em;}
.cal-event-name{font-size:13.5px;font-weight:700;color:var(--ink);}

/* ─── ANNOUNCEMENT ─── */
.ann-card{background:var(--white);border-radius:10px;border:1px solid var(--border);padding:16px 18px;margin-bottom:10px;}
.ann-card.high{border-left:4px solid var(--red);}
.ann-card.medium{border-left:4px solid var(--Y);}
.ann-card.low{border-left:4px solid var(--green);}
.ann-title{font-size:14.5px;font-weight:700;color:var(--ink);margin-bottom:5px;}
.ann-body{font-size:13px;color:var(--muted);line-height:1.65;margin-bottom:8px;}
.ann-meta{font-size:11px;color:var(--pale);display:flex;align-items:center;gap:12px;}

/* ─── STUDENT PROGRESS ─── */
.progress-row{display:flex;align-items:center;gap:10px;padding:10px 0;border-bottom:1px solid var(--border);}
.progress-row:last-child{border-bottom:none;}
.prog-subject{font-size:12px;font-weight:700;color:var(--M);width:120px;flex-shrink:0;}
.prog-bar-wrap{flex:1;height:8px;background:var(--Yl);border-radius:4px;overflow:hidden;}
.prog-bar-fill{height:100%;border-radius:4px;background:var(--M);transition:width .5s;}
.prog-score{font-size:12px;font-weight:700;color:var(--M);width:38px;text-align:right;flex-shrink:0;}

/* ─── FORM ELEMENTS ─── */
.form-group{margin-bottom:16px;}
.form-label{font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);display:block;margin-bottom:6px;}
.form-input{width:100%;padding:9px 12px;border:1.5px solid var(--border);border-radius:6px;font-family:var(--fB);font-size:13px;color:var(--ink);background:var(--white);outline:none;transition:border .15s;}
.form-input:focus{border-color:var(--M);}
.form-select{width:100%;padding:9px 12px;border:1.5px solid var(--border);border-radius:6px;font-family:var(--fB);font-size:13px;color:var(--ink);background:var(--white);outline:none;cursor:pointer;}
.form-select:focus{border-color:var(--M);}
.form-textarea{width:100%;padding:9px 12px;border:1.5px solid var(--border);border-radius:6px;font-family:var(--fB);font-size:13px;color:var(--ink);background:var(--white);outline:none;resize:vertical;min-height:80px;}
.form-textarea:focus{border-color:var(--M);}
.btn{padding:9px 18px;border-radius:6px;font-family:var(--fB);font-size:13px;font-weight:700;cursor:pointer;transition:all .15s;border:none;display:inline-flex;align-items:center;gap:6px;}
.btn-primary{background:var(--M);color:var(--Y);}
.btn-primary:hover{background:var(--Md);}
.btn-secondary{background:var(--Y);color:var(--M);}
.btn-secondary:hover{background:var(--Yd);}
.btn-ghost{background:transparent;color:var(--M);border:1.5px solid var(--border);}
.btn-ghost:hover{border-color:var(--M);background:var(--Mb);}
.btn-danger{background:var(--redb);color:var(--red);}
.btn-sm{padding:5px 12px;font-size:12px;}

/* ─── MODAL ─── */
.modal-overlay{position:fixed;inset:0;background:rgba(26,13,20,.55);z-index:1000;display:flex;align-items:center;justify-content:center;padding:20px;}
.modal{background:var(--white);border-radius:12px;width:560px;max-width:100%;max-height:90vh;overflow-y:auto;}
.modal-header{padding:20px 24px 16px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;}
.modal-title{font-family:var(--fH);font-size:18px;font-weight:600;color:var(--M);}
.modal-close{width:32px;height:32px;border-radius:6px;background:var(--off);border:none;cursor:pointer;font-size:16px;display:flex;align-items:center;justify-content:center;color:var(--muted);}
.modal-close:hover{background:var(--redb);color:var(--red);}
.modal-body{padding:20px 24px;}
.modal-footer{padding:14px 24px;border-top:1px solid var(--border);display:flex;align-items:center;justify-content:flex-end;gap:10px;}

/* ─── TODAY SCHEDULE STRIP ─── */
.today-strip{background:var(--M);border-radius:10px;padding:16px 20px;margin-bottom:22px;color:white;overflow-x:auto;}
.ts-label{font-size:10px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:rgba(255,255,255,.5);margin-bottom:10px;}
.ts-items{display:flex;gap:10px;min-width:max-content;}
.ts-item{background:rgba(255,255,255,.1);border-radius:8px;padding:10px 14px;border:1px solid rgba(255,255,255,.1);min-width:120px;}
.ts-item.current{background:var(--Y);border-color:var(--Y);}
.ts-item.done{opacity:.45;}
.ts-time{font-size:10px;font-weight:700;color:rgba(255,255,255,.6);margin-bottom:3px;}
.ts-item.current .ts-time{color:rgba(90,20,60,.7);}
.ts-subj{font-size:13px;font-weight:700;color:var(--white);}
.ts-item.current .ts-subj{color:var(--M);}

/* ─── TEACHER LIST ─── */
.teacher-card{background:var(--white);border-radius:10px;border:1px solid var(--border);padding:16px 18px;display:flex;align-items:center;gap:14px;margin-bottom:10px;}
.t-avatar{width:44px;height:44px;border-radius:6px;background:var(--Y);display:flex;align-items:center;justify-content:center;font-family:var(--fD);font-size:16px;font-weight:900;color:var(--M);flex-shrink:0;}
.t-name{font-size:14px;font-weight:700;color:var(--ink);}
.t-subject{font-size:12px;color:var(--muted);}
.t-classes{display:flex;flex-wrap:wrap;gap:5px;margin-top:5px;}
.t-class-tag{background:var(--Mb);color:var(--M);font-size:10.5px;font-weight:700;padding:2px 8px;border-radius:100px;}

/* ─── EMPTY STATE ─── */
.empty{text-align:center;padding:40px 20px;}
.empty-icon{font-size:40px;margin-bottom:10px;opacity:.3;}
.empty-text{font-size:13px;color:var(--muted);}

/* ─── TABS ─── */
.tab-bar{display:flex;gap:4px;border-bottom:2px solid var(--border);margin-bottom:20px;}
.tab-btn{padding:8px 18px;font-size:13px;font-weight:700;color:var(--muted);cursor:pointer;border:none;background:transparent;border-bottom:2px solid transparent;margin-bottom:-2px;transition:all .15s;}
.tab-btn:hover{color:var(--M);}
.tab-btn.active{color:var(--M);border-bottom-color:var(--M);}

/* ─── UTILITIES ─── */
.flex{display:flex;} .flex-1{flex:1;} .items-center{align-items:center;} .justify-between{justify-content:space-between;}
.gap-8{gap:8px;} .gap-10{gap:10px;} .gap-14{gap:14px;}
.mb-8{margin-bottom:8px;} .mb-12{margin-bottom:12px;} .mb-16{margin-bottom:16px;} .mb-20{margin-bottom:20px;} .mb-22{margin-bottom:22px;}
.mt-auto{margin-top:auto;}
.text-muted{color:var(--muted);} .text-sm{font-size:12px;} .font-bold{font-weight:700;}
.w-full{width:100%;}
.hidden{display:none!important;}

/* ─── RESPONSIVE ─── */
@media(max-width:900px){
  :root{--sidebar:200px;}
  .stats-row{grid-template-columns:1fr 1fr;}
  .g-main{grid-template-columns:1fr;}
}
</style>
</head>
<body>

<!-- ════════ LOGIN SCREEN ════════ -->
<div id="login-screen">
  <!-- folk art bg -->
  <svg class="login-bg" viewBox="0 0 800 600" xmlns="http://www.w3.org/2000/svg">
    <circle cx="50" cy="50" r="120" fill="white"/><circle cx="750" cy="550" r="150" fill="white"/>
    <circle cx="700" cy="80" r="80" fill="white" opacity=".5"/>
    <circle cx="100" cy="520" r="90" fill="white" opacity=".4"/>
    <path d="M0 300 Q200 100 400 300 Q600 500 800 300" stroke="white" stroke-width="2" fill="none"/>
  </svg>
  <div class="login-card">
    <div class="login-logo">LOKA</div>
    <div class="login-tagline">Learning Management System · Imagine the Impossible</div>
    <div class="login-heading">Welcome Back</div>
    <div class="login-sub">Select your role to continue</div>
    <div class="role-cards">
      <div class="role-card" onclick="selectRole('student')" id="rc-student">
        <span class="rc-icon">🎒</span>
        <div class="rc-label">Student</div>
        <div class="rc-desc">View lessons, assignments &amp; schedule</div>
      </div>
      <div class="role-card" onclick="selectRole('teacher')" id="rc-teacher">
        <span class="rc-icon">📖</span>
        <div class="rc-label">Teacher</div>
        <div class="rc-desc">Plan lessons, assign homework</div>
      </div>
      <div class="role-card" onclick="selectRole('admin')" id="rc-admin">
        <span class="rc-icon">🏫</span>
        <div class="rc-label">Admin</div>
        <div class="rc-desc">Manage school &amp; view reports</div>
      </div>
    </div>
    <div class="user-select" id="user-select-wrap">
      <label>Select Your Name</label>
      <select id="user-select"><option value="">— Choose name —</option></select>
    </div>
    <button class="login-btn" id="login-btn" onclick="doLogin()" disabled>Enter Loka LMS →</button>
  </div>
</div>

<!-- ════════ APP SCREEN ════════ -->
<div id="app-screen">
  <!-- SIDEBAR -->
  <div id="sidebar">
    <div class="sb-logo">
      <div class="sb-logo-text">LOKA</div>
      <div class="sb-logo-sub">Learning Management</div>
    </div>
    <div class="sb-user">
      <div class="sb-avatar" id="sb-avatar">LK</div>
      <div class="sb-user-info">
        <div class="sb-user-name" id="sb-name">User</div>
        <div class="sb-user-role" id="sb-role">Role</div>
      </div>
    </div>
    <nav class="sb-nav" id="sb-nav"></nav>
    <div class="sb-bottom">
      <div class="sb-logout" onclick="doLogout()">
        <span>⬅</span><span>Sign Out</span>
      </div>
    </div>
  </div>

  <!-- MAIN -->
  <div id="main">
    <div id="topbar">
      <div class="tb-page-title" id="tb-title">Dashboard</div>
      <div class="tb-date" id="tb-date"></div>
      <div class="tb-notif" onclick="showAnnouncements()">🔔<div class="tb-notif-dot"></div></div>
    </div>
    <div id="content"></div>
  </div>
</div>

<!-- ════════ MODAL ════════ -->
<div class="modal-overlay hidden" id="modal-overlay" onclick="closeModal(event)">
  <div class="modal" id="modal">
    <div class="modal-header">
      <div class="modal-title" id="modal-title">Modal</div>
      <button class="modal-close" onclick="closeModalDirect()">✕</button>
    </div>
    <div class="modal-body" id="modal-body"></div>
    <div class="modal-footer" id="modal-footer"></div>
  </div>
</div>

<script>
// ════════════════════════════════════════════════
//  DATA
// ════════════════════════════════════════════════
const DATA = {
  teachers:[
    {id:'t1',name:'Mr. Rajesh Kumar',subject:'Mathematics',avatar:'RK',classes:['Class 3A','Class 4B','Class 5A']},
    {id:'t2',name:'Ms. Anita Singh',subject:'English',avatar:'AS',classes:['Class 3A','Class 5A','Class 6A']},
    {id:'t3',name:'Ms. Sunita Devi',subject:'EVS / Science',avatar:'SD',classes:['Class 3A','Class 4A','Class 4B']},
    {id:'t4',name:'Mr. Ramesh Sharma',subject:'Hindi',avatar:'RS',classes:['Class 3A','Class 3B','Class 4A']},
    {id:'t5',name:'Ms. Nidhi Kumari',subject:'Folk Art & Craft',avatar:'NK',classes:['Class 3A','Class 4A','Class 5A','Class 6A']},
    {id:'t6',name:'Mr. Bikash Das',subject:'Music & Performing Arts',avatar:'BD',classes:['Class 3A','Class 4A','Class 5A']},
    {id:'t7',name:'Mr. Suresh Yadav',subject:'Physical Education',avatar:'SY',classes:['Class 3A','Class 3B','Class 4A','Class 4B']},
  ],
  students:[
    {id:'s1',name:'Aryan Mishra',class:'Class 3A',avatar:'AM',roll:1,progress:{Mathematics:82,English:90,EVS:75,Hindi:88,Art:95}},
    {id:'s2',name:'Priya Gupta',class:'Class 3A',avatar:'PG',roll:2,progress:{Mathematics:91,English:85,EVS:88,Hindi:92,Art:78}},
    {id:'s3',name:'Rohan Kumar',class:'Class 3A',avatar:'RK',roll:3,progress:{Mathematics:68,English:72,EVS:80,Hindi:65,Art:90}},
    {id:'s4',name:'Sneha Sharma',class:'Class 3A',avatar:'SS',roll:4,progress:{Mathematics:95,English:88,EVS:92,Hindi:85,Art:72}},
    {id:'s5',name:'Vikram Singh',class:'Class 3A',avatar:'VS',roll:5,progress:{Mathematics:75,English:68,EVS:70,Hindi:78,Art:85}},
    {id:'s6',name:'Kavya Pathak',class:'Class 3A',avatar:'KP',roll:6,progress:{Mathematics:88,English:94,EVS:85,Hindi:90,Art:96}},
    {id:'s7',name:'Aditya Jha',class:'Class 3A',avatar:'AJ',roll:7,progress:{Mathematics:78,English:65,EVS:82,Hindi:70,Art:88}},
  ],
  timetable:{
    'Class 3A':{
      Monday:   [{t:'8:00',e:'8:45',sub:'Mathematics',tchr:'Mr. Kumar'},{t:'8:45',e:'9:30',sub:'English',tchr:'Ms. Singh'},{t:'9:30',e:'10:15',sub:'EVS',tchr:'Ms. Devi'},{t:'10:15',e:'10:30',sub:'Break',special:true},{t:'10:30',e:'11:15',sub:'Hindi',tchr:'Mr. Sharma'},{t:'11:15',e:'12:00',sub:'Folk Art',tchr:'Ms. Kumari'},{t:'12:00',e:'12:45',sub:'Lunch',special:true,'lunch':true},{t:'12:45',e:'1:30',sub:'PE',tchr:'Mr. Yadav'},{t:'1:30',e:'2:30',sub:'Reading / Library',tchr:''}],
      Tuesday:  [{t:'8:00',e:'8:45',sub:'English',tchr:'Ms. Singh'},{t:'8:45',e:'9:30',sub:'Mathematics',tchr:'Mr. Kumar'},{t:'9:30',e:'10:15',sub:'Hindi',tchr:'Mr. Sharma'},{t:'10:15',e:'10:30',sub:'Break',special:true},{t:'10:30',e:'11:15',sub:'EVS',tchr:'Ms. Devi'},{t:'11:15',e:'12:00',sub:'Music',tchr:'Mr. Das'},{t:'12:00',e:'12:45',sub:'Lunch',special:true,'lunch':true},{t:'12:45',e:'1:30',sub:'Folk Art',tchr:'Ms. Kumari'},{t:'1:30',e:'2:30',sub:'Project Work',tchr:''}],
      Wednesday:[{t:'8:00',e:'8:45',sub:'Mathematics',tchr:'Mr. Kumar'},{t:'8:45',e:'9:30',sub:'EVS',tchr:'Ms. Devi'},{t:'9:30',e:'10:15',sub:'English',tchr:'Ms. Singh'},{t:'10:15',e:'10:30',sub:'Break',special:true},{t:'10:30',e:'11:15',sub:'Hindi',tchr:'Mr. Sharma'},{t:'11:15',e:'12:00',sub:'PE',tchr:'Mr. Yadav'},{t:'12:00',e:'12:45',sub:'Lunch',special:true,'lunch':true},{t:'12:45',e:'1:30',sub:'Music',tchr:'Mr. Das'},{t:'1:30',e:'2:30',sub:'SEL / Circle Time',tchr:''}],
      Thursday: [{t:'8:00',e:'8:45',sub:'Hindi',tchr:'Mr. Sharma'},{t:'8:45',e:'9:30',sub:'English',tchr:'Ms. Singh'},{t:'9:30',e:'10:15',sub:'Mathematics',tchr:'Mr. Kumar'},{t:'10:15',e:'10:30',sub:'Break',special:true},{t:'10:30',e:'11:15',sub:'Folk Art',tchr:'Ms. Kumari'},{t:'11:15',e:'12:00',sub:'EVS',tchr:'Ms. Devi'},{t:'12:00',e:'12:45',sub:'Lunch',special:true,'lunch':true},{t:'12:45',e:'1:30',sub:'Library',tchr:''},{t:'1:30',e:'2:30',sub:'Reflection Circle',tchr:''}],
      Friday:   [{t:'8:00',e:'8:45',sub:'EVS',tchr:'Ms. Devi'},{t:'8:45',e:'9:30',sub:'Mathematics',tchr:'Mr. Kumar'},{t:'9:30',e:'10:15',sub:'Hindi',tchr:'Mr. Sharma'},{t:'10:15',e:'10:30',sub:'Break',special:true},{t:'10:30',e:'11:15',sub:'English',tchr:'Ms. Singh'},{t:'11:15',e:'12:00',sub:'Music',tchr:'Mr. Das'},{t:'12:00',e:'12:45',sub:'Lunch',special:true,'lunch':true},{t:'12:45',e:'1:30',sub:'PE',tchr:'Mr. Yadav'},{t:'1:30',e:'2:30',sub:'Assembly',tchr:''}],
    }
  },
  lessons:[
    {id:1,subject:'Mathematics',cls:'Class 3A',teacher:'Mr. Rajesh Kumar',teacherId:'t1',topic:'Introduction to Fractions',date:'2024-11-06',status:'upcoming',description:'Students will understand fractions using real objects — cutting fruit, folding paper. Hands-on and joyful discovery.',materials:['Fraction strips','Coloured paper','Real fruit'],objectives:['Understand numerator and denominator','Identify halves, quarters, thirds','Represent fractions visually']},
    {id:2,subject:'English',cls:'Class 3A',teacher:'Ms. Anita Singh',teacherId:'t2',topic:'The Folk Tale of Magadha — Retelling & Drama',date:'2024-11-05',status:'ongoing',description:'Students read and retell a classic folk tale from Magadha. Groups dramatise key scenes from the story.',materials:['Folk tale storybook','Drama props','Writing journals'],objectives:['Read aloud with expression','Retell story in own words','Collaborate in groups']},
    {id:3,subject:'EVS',cls:'Class 3A',teacher:'Ms. Sunita Devi',teacherId:'t3',topic:'Our Soil — What Lives Beneath',date:'2024-11-04',status:'completed',description:'Students explored the school garden, collected soil samples, and observed living organisms under magnification.',materials:['Magnifying glasses','Soil jars','Observation books'],objectives:['Name 3 soil organisms','Record observations scientifically','Appreciate biodiversity']},
    {id:4,subject:'Mathematics',cls:'Class 3A',teacher:'Mr. Rajesh Kumar',teacherId:'t1',topic:'Multiplication Tables — Games & Songs',date:'2024-11-07',status:'upcoming',description:'Using songs, games, and movement to make tables joyful and memorable for every learner.',materials:['Number cards','Songs playlist','Worksheets'],objectives:['Learn 3, 4, 5 times tables','Use tables in simple word problems','Develop number fluency']},
    {id:5,subject:'Folk Art',cls:'Class 3A',teacher:'Ms. Nidhi Kumari',teacherId:'t5',topic:'Madhubani Motifs — Fish and Lotus',date:'2024-11-08',status:'upcoming',description:'Introduction to traditional Madhubani art. Students learn the fish and lotus motifs using ink and watercolour on handmade paper.',materials:['Black ink pens','Watercolours','Handmade paper'],objectives:['Learn Madhubani line technique','Create fish and lotus motifs','Appreciate Bihar folk art tradition']},
    {id:6,subject:'Hindi',cls:'Class 3A',teacher:'Mr. Ramesh Sharma',teacherId:'t4',topic:'कविता — Nature Poems of Magadha',date:'2024-11-05',status:'completed',description:'Students explored beautiful Hindi poetry about the Gangetic plains, trees, and birds of Magadha region.',materials:['Poetry anthology','Nature photographs','Writing journals'],objectives:['Read poetry with rhythm','Understand poetic images','Write a short poem']},
  ],
  assignments:[
    {id:1,subject:'Mathematics',cls:'Class 3A',teacherId:'t1',title:'Fractions in Daily Life',description:'Find 5 examples of fractions in your home. Draw and write about each one in your journal.',dueDate:'2024-11-08',assignedDate:'2024-11-05',status:'active',submissions:12,total:28,type:'homework'},
    {id:2,subject:'English',cls:'Class 3A',teacherId:'t2',title:'My Favourite Folk Tale',description:'Write a 1-page story about your favourite folk tale or legend. Illustrate with a drawing.',dueDate:'2024-11-10',assignedDate:'2024-11-04',status:'active',submissions:8,total:28,type:'assignment'},
    {id:3,subject:'EVS',cls:'Class 3A',teacherId:'t3',title:'Soil Observation Report',description:'Complete the observation notebook from class. Answer the 5 questions on page 12.',dueDate:'2024-11-07',assignedDate:'2024-11-04',status:'active',submissions:20,total:28,type:'homework'},
    {id:4,subject:'Hindi',cls:'Class 3A',teacherId:'t4',title:'प्रकृति कविता — Nature Poem',description:'Write an 8-line Hindi poem about something in nature that you love. Practice reading aloud.',dueDate:'2024-11-09',assignedDate:'2024-11-05',status:'active',submissions:5,total:28,type:'assignment'},
    {id:5,subject:'Mathematics',cls:'Class 3A',teacherId:'t1',title:'Times Tables Practice Sheet',description:'Complete pages 34-35 in your maths workbook. Practice 3, 4, and 5 times tables.',dueDate:'2024-11-06',assignedDate:'2024-11-03',status:'overdue',submissions:25,total:28,type:'homework'},
  ],
  holidays:[
    {id:1,date:'2024-11-01',name:'Diwali Holiday',type:'festival'},
    {id:2,date:'2024-11-02',name:'Diwali Holiday',type:'festival'},
    {id:3,date:'2024-11-15',name:'Sports Day',type:'event'},
    {id:4,date:'2024-11-19',name:'Guru Nanak Jayanti',type:'festival'},
    {id:5,date:'2024-12-25',name:'Christmas',type:'festival'},
    {id:6,date:'2025-01-14',name:'Makar Sankranti',type:'festival'},
    {id:7,date:'2025-01-26',name:'Republic Day',type:'national'},
    {id:8,date:'2025-03-14',name:'Holi',type:'festival'},
    {id:9,date:'2025-04-14',name:'Ambedkar Jayanti',type:'national'},
    {id:10,date:'2025-04-18',name:'Good Friday',type:'festival'},
    {id:11,date:'2024-11-20',name:'Annual Day / Loka Utsav',type:'event'},
    {id:12,date:'2024-12-20',name:'Winter Break Begins',type:'event'},
    {id:13,date:'2025-01-06',name:'School Reopens',type:'event'},
  ],
  announcements:[
    {id:1,title:'School Reopens After Diwali Break',body:'School will reopen on Monday 5th November 2024. All students must submit their holiday projects on the first day back. Please ensure your child is in full uniform.',date:'2024-10-28',author:'Principal',priority:'high'},
    {id:2,title:'Annual Sports Day — 15th November',body:'Inter-house sports day will be held on 15th November. All students to wear their house colours. Parents are warmly invited to attend and cheer.',date:'2024-10-25',author:'Sports Department',priority:'medium'},
    {id:3,title:'Folk Art Exhibition — Submissions Open',body:'Submit your Madhubani and Tikuli artwork for the annual Loka Folk Art Exhibition by 20th November. All stages welcome.',date:'2024-10-22',author:'Art Department',priority:'low'},
    {id:4,title:'Parent-Teacher Conference — 12th November',body:'The Term 1 Parent-Teacher-Child Conference will be held on 12th November. Booking slots are now open via the school office. Please bring your child\'s portfolio.',date:'2024-10-20',author:'Administration',priority:'high'},
  ]
};

// ════════════════════════════════════════════════
//  STATE
// ════════════════════════════════════════════════
let STATE = { role:null, user:null, tab:'dashboard', calMonth: new Date().getMonth(), calYear: new Date().getFullYear() };

// ════════════════════════════════════════════════
//  LOGIN
// ════════════════════════════════════════════════
const ROLE_USERS = {
  student: DATA.students.map(s=>({id:s.id,name:s.name,class:s.class,avatar:s.avatar})),
  teacher: DATA.teachers.map(t=>({id:t.id,name:t.name,subject:t.subject,avatar:t.avatar})),
  admin:   [{id:'a1',name:'Ms. Vandana Prasad',role:'Principal',avatar:'VP'},{id:'a2',name:'Mr. Deepak Ojha',role:'Vice Principal',avatar:'DO'}]
};

function selectRole(r){
  document.querySelectorAll('.role-card').forEach(c=>c.classList.remove('selected'));
  document.getElementById('rc-'+r).classList.add('selected');
  STATE.role=r;
  const sel=document.getElementById('user-select');
  sel.innerHTML='<option value="">— Choose name —</option>';
  ROLE_USERS[r].forEach(u=>{
    const o=document.createElement('option'); o.value=u.id; o.textContent=u.name; sel.appendChild(o);
  });
  document.getElementById('login-btn').disabled=true;
  sel.onchange=()=>{ document.getElementById('login-btn').disabled=!sel.value; };
}

function doLogin(){
  const sel=document.getElementById('user-select');
  if(!sel.value||!STATE.role) return;
  const user=ROLE_USERS[STATE.role].find(u=>u.id===sel.value);
  STATE.user=user;
  document.getElementById('login-screen').style.display='none';
  document.getElementById('app-screen').classList.add('visible');
  // Set up user info
  document.getElementById('sb-avatar').textContent=user.avatar;
  document.getElementById('sb-name').textContent=user.name;
  document.getElementById('sb-role').textContent=STATE.role.charAt(0).toUpperCase()+STATE.role.slice(1);
  // Topbar date
  document.getElementById('tb-date').textContent=new Date().toLocaleDateString('en-IN',{weekday:'long',year:'numeric',month:'long',day:'numeric'});
  buildNav();
  navigate('dashboard');
}

function doLogout(){
  STATE={role:null,user:null,tab:'dashboard',calMonth:new Date().getMonth(),calYear:new Date().getFullYear()};
  document.getElementById('app-screen').classList.remove('visible');
  document.getElementById('login-screen').style.display='flex';
  document.querySelectorAll('.role-card').forEach(c=>c.classList.remove('selected'));
  document.getElementById('user-select').innerHTML='<option value="">— Choose name —</option>';
  document.getElementById('login-btn').disabled=true;
}

// ════════════════════════════════════════════════
//  NAVIGATION
// ════════════════════════════════════════════════
const NAV_ITEMS = {
  student:[
    {id:'dashboard',icon:'🏠',label:'Dashboard'},
    {id:'schedule',icon:'🕐',label:'My Schedule'},
    {id:'lessons',icon:'📖',label:'Lessons'},
    {id:'assignments',icon:'📝',label:'Assignments',badge:()=>DATA.assignments.filter(a=>a.status==='active'||a.status==='overdue').length},
    {id:'calendar',icon:'📅',label:'Calendar'},
  ],
  teacher:[
    {id:'dashboard',icon:'🏠',label:'Dashboard'},
    {id:'schedule',icon:'🕐',label:'My Schedule'},
    {id:'lessons',icon:'📖',label:'Lesson Planner'},
    {id:'assignments',icon:'📝',label:'Assignments',badge:()=>DATA.assignments.filter(a=>a.status==='active').length},
    {id:'students',icon:'👥',label:'My Students'},
    {id:'calendar',icon:'📅',label:'Calendar'},
  ],
  admin:[
    {id:'dashboard',icon:'🏠',label:'Dashboard'},
    {id:'classes',icon:'🏫',label:'All Classes'},
    {id:'teachers',icon:'👨‍🏫',label:'Teachers'},
    {id:'students',icon:'👥',label:'Students'},
    {id:'calendar',icon:'📅',label:'School Calendar'},
    {id:'announcements',icon:'📢',label:'Announcements'},
  ]
};

function buildNav(){
  const nav=document.getElementById('sb-nav');
  nav.innerHTML='';
  const items=NAV_ITEMS[STATE.role]||[];
  items.forEach(item=>{
    const badge=item.badge?item.badge():0;
    const el=document.createElement('div');
    el.className='sb-item'+(STATE.tab===item.id?' active':'');
    el.id='nav-'+item.id;
    el.onclick=()=>navigate(item.id);
    el.innerHTML=`<span class="si-icon">${item.icon}</span><span class="si-label">${item.label}</span>${badge?`<span class="si-badge">${badge}</span>`:''}`;
    nav.appendChild(el);
  });
}

function navigate(tab){
  STATE.tab=tab;
  // Update nav
  document.querySelectorAll('.sb-item').forEach(el=>el.classList.remove('active'));
  const navEl=document.getElementById('nav-'+tab);
  if(navEl) navEl.classList.add('active');
  // Page title
  const item=(NAV_ITEMS[STATE.role]||[]).find(i=>i.id===tab);
  document.getElementById('tb-title').textContent=item?item.label:'Dashboard';
  // Render
  const content=document.getElementById('content');
  content.innerHTML='';
  content.scrollTop=0;
  VIEWS[STATE.role]?.[tab]?.();
}

// ════════════════════════════════════════════════
//  HELPERS
// ════════════════════════════════════════════════
function badge(type,text){return `<span class="badge badge-${type}">${text}</span>`;}
function today(){return new Date().toISOString().split('T')[0];}
function fmt(d){return new Date(d+'T00:00:00').toLocaleDateString('en-IN',{day:'numeric',month:'short',year:'numeric'});}
function fmtShort(d){return new Date(d+'T00:00:00').toLocaleDateString('en-IN',{day:'numeric',month:'short'});}
function daysLeft(d){const diff=new Date(d+'T00:00:00')-new Date(today()+'T00:00:00');return Math.ceil(diff/86400000);}
function isOverdue(d){return daysLeft(d)<0;}
function currentDay(){return ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'][new Date().getDay()];}
function subjectColor(sub){
  const map={Mathematics:'#7A1550',English:'#1D4ED8',EVS:'#16A34A',Hindi:'#D97706',Art:'#9333EA','Folk Art':'#9333EA',Music:'#0891B2',PE:'#EA580C',Science:'#16A34A'};
  for(const k in map){if(sub.includes(k))return map[k];}
  return '#6B7280';
}

function html(el){document.getElementById('content').innerHTML=el;}

// ════════════════════════════════════════════════
//  STUDENT VIEWS
// ════════════════════════════════════════════════
const studentViews = {
  dashboard(){
    const day=currentDay();
    const todaySlots=(DATA.timetable['Class 3A'][day]||[]).filter(s=>!s.special&&!s.lunch);
    const pending=DATA.assignments.filter(a=>a.status==='active'||a.status==='overdue');
    const ongoing=DATA.lessons.filter(l=>l.status==='ongoing');
    const upcoming=DATA.lessons.filter(l=>l.status==='upcoming').slice(0,3);

    document.getElementById('content').innerHTML=`
    <!-- Today strip -->
    <div class="today-strip">
      <div class="ts-label">📅 Today's Classes — ${day}</div>
      <div class="ts-items">
        ${todaySlots.map((s,i)=>`<div class="ts-item ${i===0?'current':''}"><div class="ts-time">${s.t}–${s.e}</div><div class="ts-subj">${s.sub}</div><div style="font-size:10px;color:${i===0?'rgba(90,20,60,.6)':'rgba(255,255,255,.4)'};">${s.tchr}</div></div>`).join('')}
      </div>
    </div>

    <!-- Stats -->
    <div class="stats-row" style="grid-template-columns:repeat(4,1fr);">
      <div class="stat-card y"><div class="stat-num">${pending.length}</div><div class="stat-label">Pending Tasks</div><div class="stat-sub">Due this week</div></div>
      <div class="stat-card"><div class="stat-num">${ongoing.length}</div><div class="stat-label">Ongoing Lessons</div><div class="stat-sub">In progress now</div></div>
      <div class="stat-card m"><div class="stat-num">${upcoming.length}</div><div class="stat-label">Upcoming Lessons</div><div class="stat-sub">This week</div></div>
      <div class="stat-card"><div class="stat-num">5</div><div class="stat-label">Subjects</div><div class="stat-sub">This term</div></div>
    </div>

    <div class="g-main">
      <div>
        <!-- Ongoing Lessons -->
        <div class="sec-hdr mb-12"><div class="sec-title">🔵 Ongoing Lessons</div><button class="sec-action" onclick="navigate('lessons')">View All</button></div>
        ${ongoing.map(l=>`
        <div class="lesson-card ongoing">
          <div class="lc-top"><div><div class="lc-subject">${l.subject}</div><div class="lc-title">${l.topic}</div><div class="lc-meta"><span>📅 ${fmt(l.date)}</span><span>👩‍🏫 ${l.teacher}</span></div></div>${badge('ongoing','● In Progress')}</div>
          <div class="lc-objectives">${l.objectives.map(o=>`<div class="lc-obj-item">${o}</div>`).join('')}</div>
        </div>`).join('')||'<div class="empty"><div class="empty-icon">📖</div><div class="empty-text">No ongoing lessons</div></div>'}

        <!-- Upcoming Lessons -->
        <div class="sec-hdr mb-12 mt-4" style="margin-top:20px;"><div class="sec-title">⏳ Upcoming Lessons</div></div>
        ${upcoming.map(l=>`
        <div class="lesson-card upcoming">
          <div class="lc-top"><div><div class="lc-subject">${l.subject}</div><div class="lc-title">${l.topic}</div><div class="lc-meta"><span>📅 ${fmt(l.date)}</span><span>📚 ${l.materials.length} materials</span></div></div>${badge('upcoming','Upcoming')}</div>
        </div>`).join('')}
      </div>

      <div>
        <!-- Pending Assignments -->
        <div class="sec-hdr mb-12"><div class="sec-title">📝 My Assignments</div><button class="sec-action" onclick="navigate('assignments')">All</button></div>
        ${pending.slice(0,4).map(a=>{
          const dl=daysLeft(a.dueDate);
          const ov=dl<0;
          return`<div class="assign-card ${ov?'overdue':'active'}">
            <div class="ac-top"><div class="ac-title">${a.title}</div>${badge(ov?'overdue':'homework',ov?'Overdue':a.type)}</div>
            <div class="ac-sub">${a.subject}</div>
            <div class="ac-due ${ov?'':'ok'}">${ov?`${Math.abs(dl)} days overdue`:`Due in ${dl} day${dl!==1?'s':''} · ${fmtShort(a.dueDate)}`}</div>
          </div>`;
        }).join('')}

        <!-- My Progress -->
        <div class="sec-hdr mb-12" style="margin-top:20px;"><div class="sec-title">📊 My Progress</div></div>
        <div class="card">
          ${Object.entries(STATE.user&&DATA.students.find(s=>s.id===STATE.user.id)?.progress||{}).map(([sub,score])=>`
          <div class="progress-row">
            <div class="prog-subject">${sub}</div>
            <div class="prog-bar-wrap"><div class="prog-bar-fill" style="width:${score}%;background:${subjectColor(sub)};"></div></div>
            <div class="prog-score">${score}%</div>
          </div>`).join('')}
        </div>

        <!-- Next Holiday -->
        <div class="sec-hdr mb-12" style="margin-top:20px;"><div class="sec-title">🗓️ Upcoming Events</div></div>
        <div class="card" style="padding:14px 16px;">
          ${DATA.holidays.filter(h=>h.date>=today()).slice(0,3).map(h=>`
          <div class="cal-event-item" style="padding:8px 10px;margin-bottom:6px;">
            <div style="text-align:center;min-width:32px;"><div style="font-family:var(--fD);font-size:20px;font-weight:900;color:var(--M);line-height:1;">${new Date(h.date+'T00:00:00').getDate()}</div><div style="font-size:9px;font-weight:700;color:var(--muted);text-transform:uppercase;">${new Date(h.date+'T00:00:00').toLocaleString('en',{month:'short'})}</div></div>
            <div><div style="font-size:13px;font-weight:700;color:var(--ink);">${h.name}</div>${badge(h.type,h.type)}</div>
          </div>`).join('')}
        </div>
      </div>
    </div>
    <!-- Announcements -->
    <div class="sec-hdr mb-12" style="margin-top:22px;"><div class="sec-title">📢 Announcements</div></div>
    <div class="g2">
      ${DATA.announcements.slice(0,2).map(a=>`
      <div class="ann-card ${a.priority}">
        <div class="ann-title">${a.title}</div>
        <div class="ann-body">${a.body}</div>
        <div class="ann-meta"><span>👤 ${a.author}</span><span>📅 ${fmt(a.date)}</span></div>
      </div>`).join('')}
    </div>`;
  },

  schedule(){
    const days=['Monday','Tuesday','Wednesday','Thursday','Friday'];
    const tt=DATA.timetable['Class 3A'];
    document.getElementById('content').innerHTML=`
    <div class="card mb-20">
      <div class="sec-hdr"><div class="sec-title">📅 Weekly Timetable — Class 3A</div><span class="badge badge-ongoing">Academic Year 2024–25</span></div>
      <div class="tt-wrapper">
        <table class="tt-table">
          <thead><tr><th>Time</th>${days.map(d=>`<th>${d}</th>`).join('')}</tr></thead>
          <tbody>
          ${(()=>{
            const times=[...new Set(days.flatMap(d=>(tt[d]||[]).map(s=>s.t+'–'+s.e)))];
            const slots=(tt['Monday']||[]);
            return slots.map((slot,i)=>{
              const isBreak=slot.special&&!slot.lunch;
              const isLunch=slot.lunch;
              const rowClass=isBreak?'tt-break':isLunch?'tt-lunch':'';
              if(isBreak||isLunch){
                return`<tr><td class="tt-time">${slot.t}–${slot.e}</td><td colspan="5" class="${rowClass}">${isLunch?'🍱 Lunch Break':'☕ Short Break'}</td></tr>`;
              }
              return`<tr>${['Monday','Tuesday','Wednesday','Thursday','Friday'].map(d=>{
                const s=(tt[d]||[])[i];
                if(!s||s.special) return '<td>—</td>';
                const isCurDay=d===currentDay();
                return`<td class="${isCurDay?'tt-today':''}"><div class="tt-subject" style="color:${subjectColor(s.sub)};">${s.sub}</div><div class="tt-teacher">${s.tchr||''}</div><div style="font-size:10px;color:var(--pale);margin-top:2px;">${s.t}–${s.e}</div></td>`;
              }).join('')}<td class="tt-time">${slot.t}–${slot.e}</td></tr>`.replace('<td class="tt-time">','').replace('</tr>',`<td class="tt-time">${slot.t}–${slot.e}</td></tr>`);
            }).join('');
          })()}
          </tbody>
        </table>
      </div>
    </div>
    <div class="g3">
      ${['Mathematics','English','EVS','Hindi','Folk Art','Music'].map(sub=>`
      <div class="card" style="border-left:3px solid ${subjectColor(sub)};">
        <div style="font-size:12px;font-weight:700;color:${subjectColor(sub)};margin-bottom:4px;text-transform:uppercase;letter-spacing:.06em;">${sub}</div>
        <div style="font-size:13px;color:var(--muted);">${DATA.teachers.find(t=>t.subject.includes(sub.split(' ')[0]))?.name||'—'}</div>
      </div>`).join('')}
    </div>`;
  },

  lessons(){
    const all=DATA.lessons;
    document.getElementById('content').innerHTML=`
    <div style="display:flex;gap:10px;margin-bottom:18px;flex-wrap:wrap;">
      ${[['all','All Lessons'],['ongoing','Ongoing'],['upcoming','Upcoming'],['completed','Completed']].map(([f,l])=>`
      <button class="btn btn-ghost btn-sm" id="lf-${f}" onclick="filterLessons('${f}')">${l}</button>`).join('')}
    </div>
    <div id="lessons-list">
      ${renderLessonsList(all)}
    </div>`;
    document.getElementById('lf-all').classList.add('btn-primary');
    document.getElementById('lf-all').classList.remove('btn-ghost');
  },

  assignments(){
    document.getElementById('content').innerHTML=`
    <div class="tab-bar">
      ${[['pending','Pending'],['overdue','Overdue'],['all','All']].map(([f,l])=>`<button class="tab-btn ${f==='pending'?'active':''}" onclick="filterAssignments('${f}',this)">${l}</button>`).join('')}
    </div>
    <div id="assign-list">
      ${renderAssignList(DATA.assignments.filter(a=>a.status==='active'))}
    </div>`;
  },

  calendar: renderCalendarView,
};

function renderLessonsList(lessons){
  if(!lessons.length) return '<div class="empty"><div class="empty-icon">📖</div><div class="empty-text">No lessons found</div></div>';
  return lessons.map(l=>`
  <div class="lesson-card ${l.status}" onclick="showLessonDetail(${l.id})">
    <div class="lc-top">
      <div>
        <div class="lc-subject">${l.subject} · ${l.cls}</div>
        <div class="lc-title">${l.topic}</div>
        <div class="lc-meta"><span>📅 ${fmt(l.date)}</span><span>👩‍🏫 ${l.teacher}</span><span>📚 ${l.materials.length} materials</span></div>
      </div>
      ${badge(l.status,l.status.charAt(0).toUpperCase()+l.status.slice(1))}
    </div>
    <div style="font-size:12.5px;color:var(--muted);margin-top:8px;line-height:1.55;">${l.description.substring(0,120)}...</div>
  </div>`).join('');
}

function renderAssignList(assignments){
  if(!assignments.length) return '<div class="empty"><div class="empty-icon">✅</div><div class="empty-text">No assignments here</div></div>';
  return assignments.map(a=>{
    const dl=daysLeft(a.dueDate);
    const ov=a.status==='overdue'||dl<0;
    return`<div class="assign-card ${ov?'overdue':'active'}">
      <div class="ac-top">
        <div><div class="ac-title">${a.title}</div><div class="ac-sub">${a.subject}</div></div>
        ${badge(ov?'overdue':a.type,ov?'Overdue':a.type.charAt(0).toUpperCase()+a.type.slice(1))}
      </div>
      <div style="font-size:12.5px;color:var(--muted);margin:8px 0;line-height:1.55;">${a.description}</div>
      <div class="flex items-center justify-between">
        <div class="ac-due ${ov?'':'ok'}">${ov?`Overdue by ${Math.abs(dl)} day${Math.abs(dl)!==1?'s':''}`:`Due ${fmtShort(a.dueDate)} · ${dl} day${dl!==1?'s':''} left`}</div>
        <button class="btn btn-secondary btn-sm" onclick="markSubmitted(${a.id})">✓ Mark as Submitted</button>
      </div>
    </div>`;
  }).join('');
}

function filterLessons(f){
  document.querySelectorAll('[id^=lf-]').forEach(b=>{b.classList.add('btn-ghost');b.classList.remove('btn-primary');});
  document.getElementById('lf-'+f).classList.remove('btn-ghost');
  document.getElementById('lf-'+f).classList.add('btn-primary');
  const list=f==='all'?DATA.lessons:DATA.lessons.filter(l=>l.status===f);
  document.getElementById('lessons-list').innerHTML=renderLessonsList(list);
}

function filterAssignments(f,btn){
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  const list=f==='all'?DATA.assignments:f==='overdue'?DATA.assignments.filter(a=>a.status==='overdue'||daysLeft(a.dueDate)<0):DATA.assignments.filter(a=>a.status==='active'&&daysLeft(a.dueDate)>=0);
  document.getElementById('assign-list').innerHTML=renderAssignList(list);
}

function markSubmitted(id){
  const a=DATA.assignments.find(x=>x.id===id);
  if(a){a.submissions=Math.min(a.submissions+1,a.total);showToast('Assignment marked as submitted! ✓');}
  navigate('assignments');
}

function showLessonDetail(id){
  const l=DATA.lessons.find(x=>x.id===id);
  if(!l) return;
  openModal('Lesson Details',`
  <div style="margin-bottom:14px;">${badge(l.status,l.status.charAt(0).toUpperCase()+l.status.slice(1))}</div>
  <div style="font-size:12px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);margin-bottom:4px;">${l.subject}</div>
  <div style="font-family:var(--fH);font-size:22px;color:var(--M);font-weight:600;margin-bottom:12px;">${l.topic}</div>
  <div style="font-size:13px;color:var(--muted);line-height:1.7;margin-bottom:16px;">${l.description}</div>
  <div style="margin-bottom:14px;"><div style="font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);margin-bottom:8px;">Learning Objectives</div>${l.objectives.map(o=>`<div class="lc-obj-item">${o}</div>`).join('')}</div>
  <div><div style="font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);margin-bottom:8px;">Materials Needed</div><div style="display:flex;flex-wrap:wrap;gap:6px;">${l.materials.map(m=>`<span style="background:var(--Yb);color:var(--Yd);font-size:12px;font-weight:600;padding:4px 10px;border-radius:4px;">${m}</span>`).join('')}</div></div>
  <div style="margin-top:14px;display:flex;gap:10px;font-size:12.5px;color:var(--muted);"><span>📅 ${fmt(l.date)}</span><span>👩‍🏫 ${l.teacher}</span><span>🏫 ${l.cls}</span></div>
  `,'',true);
}

// ════════════════════════════════════════════════
//  TEACHER VIEWS
// ════════════════════════════════════════════════
const teacherViews = {
  dashboard(){
    const myLessons=DATA.lessons.filter(l=>l.teacherId===STATE.user.id);
    const myAssign=DATA.assignments.filter(a=>a.teacherId===STATE.user.id);
    const pendingReview=myAssign.filter(a=>a.submissions<a.total&&a.status==='active');
    document.getElementById('content').innerHTML=`
    <div class="stats-row">
      <div class="stat-card y"><div class="stat-num">${myLessons.filter(l=>l.status==='upcoming').length}</div><div class="stat-label">Upcoming Lessons</div><div class="stat-sub">Planned this week</div></div>
      <div class="stat-card"><div class="stat-num">${myLessons.filter(l=>l.status==='ongoing').length}</div><div class="stat-label">Ongoing</div><div class="stat-sub">In progress</div></div>
      <div class="stat-card m"><div class="stat-num">${myAssign.length}</div><div class="stat-label">Assignments</div><div class="stat-sub">Created by you</div></div>
      <div class="stat-card"><div class="stat-num">${DATA.students.length}</div><div class="stat-label">Students</div><div class="stat-sub">In your classes</div></div>
    </div>
    <div class="g-main">
      <div>
        <div class="sec-hdr mb-12"><div class="sec-title">📖 My Lesson Plans</div><button class="sec-action" onclick="navigate('lessons')">+ New Lesson</button></div>
        ${myLessons.length?myLessons.map(l=>`
        <div class="lesson-card ${l.status}">
          <div class="lc-top"><div><div class="lc-subject">${l.subject} · ${l.cls}</div><div class="lc-title">${l.topic}</div><div class="lc-meta"><span>📅 ${fmt(l.date)}</span></div></div><div style="display:flex;gap:6px;flex-direction:column;align-items:flex-end;">${badge(l.status,l.status.charAt(0).toUpperCase()+l.status.slice(1))}<button class="btn btn-ghost btn-sm" onclick="editLesson(${l.id})">✏ Edit</button></div></div>
        </div>`).join(''):'<div class="empty"><div class="empty-icon">📖</div><div class="empty-text">No lessons yet</div></div>'}

        <div class="sec-hdr mb-12" style="margin-top:22px;"><div class="sec-title">📝 My Assignments</div><button class="sec-action" onclick="openCreateAssignment()">+ New</button></div>
        ${myAssign.map(a=>`
        <div class="assign-card ${a.status}">
          <div class="ac-top"><div class="ac-title">${a.title}</div>${badge(a.type,a.type)}</div>
          <div class="ac-sub">${a.subject} · Due ${fmtShort(a.dueDate)}</div>
          <div class="flex items-center justify-between" style="margin-top:8px;">
            <div style="font-size:12px;color:var(--muted);">Submitted: <strong style="color:var(--M);">${a.submissions}/${a.total}</strong></div>
            <div class="progress-bar" style="width:140px;"><div class="progress-fill" style="width:${(a.submissions/a.total*100).toFixed(0)}%;"></div></div>
          </div>
        </div>`).join('')||'<div class="empty"><div class="empty-icon">📝</div><div class="empty-text">No assignments</div></div>'}
      </div>
      <div>
        <div class="sec-hdr mb-12"><div class="sec-title">👥 Class Snapshot</div></div>
        <div class="card mb-20">
          ${DATA.students.map(s=>`
          <div style="display:flex;align-items:center;gap:10px;padding:9px 0;border-bottom:1px solid var(--border);">
            <div style="width:30px;height:30px;border-radius:4px;background:var(--Y);display:flex;align-items:center;justify-content:center;font-family:var(--fD);font-size:11px;font-weight:900;color:var(--M);flex-shrink:0;">${s.avatar}</div>
            <div style="flex:1;min-width:0;"><div style="font-size:13px;font-weight:700;color:var(--ink);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;">${s.name}</div><div style="font-size:11px;color:var(--muted);">${s.class}</div></div>
            <div style="font-size:12px;font-weight:700;color:var(--M);">${Math.round(Object.values(s.progress).reduce((a,b)=>a+b,0)/Object.values(s.progress).length)}%</div>
          </div>`).join('')}
        </div>
        <div class="sec-hdr mb-12"><div class="sec-title">🗓️ Upcoming</div></div>
        <div class="card" style="padding:14px 16px;">
          ${DATA.holidays.filter(h=>h.date>=today()).slice(0,4).map(h=>`
          <div style="display:flex;align-items:center;gap:10px;padding:8px 0;border-bottom:1px solid var(--border);">
            <div style="text-align:center;min-width:30px;"><div style="font-family:var(--fD);font-size:18px;font-weight:900;color:var(--M);line-height:1;">${new Date(h.date+'T00:00:00').getDate()}</div><div style="font-size:9px;font-weight:700;color:var(--muted);text-transform:uppercase;">${new Date(h.date+'T00:00:00').toLocaleString('en',{month:'short'})}</div></div>
            <div><div style="font-size:13px;font-weight:700;color:var(--ink);">${h.name}</div>${badge(h.type,h.type)}</div>
          </div>`).join('')}
        </div>
      </div>
    </div>`;
  },

  schedule: studentViews.schedule,
  calendar: renderCalendarView,

  lessons(){
    const mine=DATA.lessons.filter(l=>l.teacherId===STATE.user.id);
    document.getElementById('content').innerHTML=`
    <div class="flex justify-between items-center mb-20">
      <div></div>
      <button class="btn btn-primary" onclick="openCreateLesson()">+ Create Lesson Plan</button>
    </div>
    <div style="display:flex;gap:8px;margin-bottom:16px;flex-wrap:wrap;">
      ${[['all','All'],['upcoming','Upcoming'],['ongoing','Ongoing'],['completed','Completed']].map(([f,l])=>`<button class="btn btn-ghost btn-sm" id="tlf-${f}" onclick="filterTeacherLessons('${f}')">${l}</button>`).join('')}
    </div>
    <div id="tlesson-list">${renderLessonsList(mine)}</div>`;
    document.getElementById('tlf-all').classList.add('btn-primary');
    document.getElementById('tlf-all').classList.remove('btn-ghost');
  },

  assignments(){
    const mine=DATA.assignments.filter(a=>a.teacherId===STATE.user.id);
    document.getElementById('content').innerHTML=`
    <div class="flex justify-between items-center mb-20">
      <div class="sec-title">My Assignments</div>
      <button class="btn btn-primary" onclick="openCreateAssignment()">+ Create Assignment</button>
    </div>
    ${mine.map(a=>`
    <div class="assign-card ${a.status}" style="margin-bottom:12px;">
      <div class="ac-top">
        <div><div class="ac-title">${a.title}</div><div class="ac-sub">${a.subject} · ${a.cls} · Assigned ${fmt(a.assignedDate)}</div></div>
        <div style="display:flex;gap:6px;align-items:center;">
          ${badge(a.type,a.type)}
          <button class="btn btn-ghost btn-sm" onclick="deleteAssignment(${a.id})">🗑</button>
        </div>
      </div>
      <div style="font-size:12.5px;color:var(--muted);margin:8px 0;line-height:1.55;">${a.description}</div>
      <div class="flex items-center justify-between">
        <div>
          <div style="font-size:12px;font-weight:700;color:${isOverdue(a.dueDate)?'var(--red)':'var(--green)'};">Due: ${fmt(a.dueDate)} ${isOverdue(a.dueDate)?'(Overdue)':''}</div>
          <div style="font-size:12px;color:var(--muted);margin-top:3px;">Submissions: <strong style="color:var(--M);">${a.submissions} / ${a.total}</strong></div>
        </div>
        <div style="text-align:right;">
          <div class="progress-bar" style="width:160px;"><div class="progress-fill" style="width:${(a.submissions/a.total*100).toFixed(0)}%;"></div></div>
          <div style="font-size:11px;color:var(--muted);margin-top:4px;">${((a.submissions/a.total)*100).toFixed(0)}% submitted</div>
        </div>
      </div>
    </div>`).join('')||'<div class="empty"><div class="empty-icon">📝</div><div class="empty-text">No assignments yet. Create your first one!</div></div>'}`;
  },

  students(){
    document.getElementById('content').innerHTML=`
    <div class="sec-hdr mb-20"><div class="sec-title">Class 3A · ${DATA.students.length} Students</div></div>
    <div class="g2">
    ${DATA.students.map(s=>{
      const avg=Math.round(Object.values(s.progress).reduce((a,b)=>a+b,0)/Object.values(s.progress).length);
      return`<div class="card" style="cursor:pointer;" onclick="showStudentDetail('${s.id}')">
        <div class="flex items-center gap-10 mb-12">
          <div style="width:44px;height:44px;border-radius:6px;background:var(--Y);display:flex;align-items:center;justify-content:center;font-family:var(--fD);font-size:16px;font-weight:900;color:var(--M);">${s.avatar}</div>
          <div><div style="font-size:14px;font-weight:700;color:var(--ink);">${s.name}</div><div style="font-size:12px;color:var(--muted);">Roll ${s.roll} · ${s.class}</div></div>
          <div style="margin-left:auto;text-align:center;"><div style="font-family:var(--fD);font-size:26px;font-weight:900;color:var(--M);line-height:1;">${avg}%</div><div style="font-size:10px;color:var(--muted);font-weight:700;">Avg</div></div>
        </div>
        ${Object.entries(s.progress).map(([sub,score])=>`
        <div class="progress-row" style="padding:5px 0;">
          <div class="prog-subject" style="width:100px;font-size:11.5px;">${sub}</div>
          <div class="prog-bar-wrap"><div class="prog-bar-fill" style="width:${score}%;background:${subjectColor(sub)};"></div></div>
          <div class="prog-score" style="font-size:11.5px;">${score}%</div>
        </div>`).join('')}
      </div>`;
    }).join('')}
    </div>`;
  }
};

// ════════════════════════════════════════════════
//  ADMIN VIEWS
// ════════════════════════════════════════════════
const adminViews = {
  dashboard(){
    const totalLessons=DATA.lessons.length;
    const totalAssign=DATA.assignments.length;
    const totalSubmissions=DATA.assignments.reduce((a,x)=>a+x.submissions,0);
    const totalTotal=DATA.assignments.reduce((a,x)=>a+x.total,0);
    document.getElementById('content').innerHTML=`
    <div class="stats-row">
      <div class="stat-card y"><div class="stat-num">${DATA.students.length}</div><div class="stat-label">Total Students</div><div class="stat-sub">Across all classes</div></div>
      <div class="stat-card m"><div class="stat-num">${DATA.teachers.length}</div><div class="stat-label">Teachers</div><div class="stat-sub">Active this term</div></div>
      <div class="stat-card"><div class="stat-num">${totalLessons}</div><div class="stat-label">Lesson Plans</div><div class="stat-sub">${DATA.lessons.filter(l=>l.status==='ongoing').length} ongoing</div></div>
      <div class="stat-card"><div class="stat-num">${((totalSubmissions/totalTotal)*100).toFixed(0)}%</div><div class="stat-label">Submission Rate</div><div class="stat-sub">${totalSubmissions} of ${totalTotal}</div></div>
    </div>
    <div class="g-main">
      <div>
        <div class="sec-hdr mb-12"><div class="sec-title">📚 Recent Lesson Activity</div></div>
        ${DATA.lessons.slice(0,5).map(l=>`
        <div class="lesson-card ${l.status}">
          <div class="lc-top"><div><div class="lc-subject">${l.subject} · ${l.cls}</div><div class="lc-title">${l.topic}</div><div class="lc-meta"><span>👩‍🏫 ${l.teacher}</span><span>📅 ${fmt(l.date)}</span></div></div>${badge(l.status,l.status.charAt(0).toUpperCase()+l.status.slice(1))}</div>
        </div>`).join('')}

        <div class="sec-hdr mb-12" style="margin-top:22px;"><div class="sec-title">📝 Assignment Completion</div></div>
        ${DATA.assignments.map(a=>`
        <div class="card mb-12" style="padding:14px 18px;">
          <div class="flex items-center justify-between mb-8">
            <div><div style="font-size:13.5px;font-weight:700;color:var(--ink);">${a.title}</div><div style="font-size:12px;color:var(--muted);">${a.subject} · ${a.cls}</div></div>
            ${badge(a.status==='overdue'?'overdue':a.type,a.status==='overdue'?'Overdue':a.type)}
          </div>
          <div class="flex items-center gap-10">
            <div class="progress-bar" style="flex:1;"><div class="progress-fill" style="width:${(a.submissions/a.total*100).toFixed(0)}%;"></div></div>
            <div style="font-size:12px;font-weight:700;color:var(--M);min-width:60px;text-align:right;">${a.submissions}/${a.total} done</div>
          </div>
        </div>`).join('')}
      </div>
      <div>
        <div class="sec-hdr mb-12"><div class="sec-title">👨‍🏫 Teachers at a Glance</div></div>
        ${DATA.teachers.map(t=>`
        <div class="teacher-card">
          <div class="t-avatar">${t.avatar}</div>
          <div style="flex:1;min-width:0;">
            <div class="t-name">${t.name}</div>
            <div class="t-subject">${t.subject}</div>
            <div class="t-classes">${t.classes.map(c=>`<span class="t-class-tag">${c}</span>`).join('')}</div>
          </div>
          <div style="text-align:center;min-width:40px;">
            <div style="font-family:var(--fD);font-size:20px;font-weight:900;color:var(--M);">${DATA.lessons.filter(l=>l.teacherId===t.id).length}</div>
            <div style="font-size:9px;font-weight:700;color:var(--muted);text-transform:uppercase;">Lessons</div>
          </div>
        </div>`).join('')}

        <div class="sec-hdr mb-12" style="margin-top:20px;"><div class="sec-title">📢 Latest Announcements</div></div>
        ${DATA.announcements.slice(0,2).map(a=>`
        <div class="ann-card ${a.priority}">
          <div class="ann-title">${a.title}</div>
          <div class="ann-meta"><span>👤 ${a.author}</span><span>📅 ${fmt(a.date)}</span></div>
        </div>`).join('')}
      </div>
    </div>`;
  },

  classes(){
    document.getElementById('content').innerHTML=`
    <div class="sec-hdr mb-20"><div class="sec-title">All Classes Overview</div></div>
    <div class="g2">
    ${['Class 3A','Class 3B','Class 4A','Class 4B','Class 5A','Class 6A'].map(cls=>{
      const lessons=DATA.lessons.filter(l=>l.cls===cls);
      const assigns=DATA.assignments.filter(a=>a.cls===cls);
      return`<div class="card">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;">
          <div style="font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);">${cls}</div>
          ${badge('ongoing','Active')}
        </div>
        <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:14px;">
          <div style="text-align:center;background:var(--Yb);border-radius:6px;padding:10px;"><div style="font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);">${lessons.length}</div><div style="font-size:10px;font-weight:700;color:var(--muted);text-transform:uppercase;">Lessons</div></div>
          <div style="text-align:center;background:var(--Mb);border-radius:6px;padding:10px;"><div style="font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);">${assigns.length}</div><div style="font-size:10px;font-weight:700;color:var(--muted);text-transform:uppercase;">Assignments</div></div>
          <div style="text-align:center;background:var(--Yb);border-radius:6px;padding:10px;"><div style="font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);">${DATA.teachers.filter(t=>t.classes.includes(cls)).length}</div><div style="font-size:10px;font-weight:700;color:var(--muted);text-transform:uppercase;">Teachers</div></div>
        </div>
        <div style="font-size:11px;font-weight:700;color:var(--muted);margin-bottom:6px;text-transform:uppercase;letter-spacing:.06em;">Subjects</div>
        <div style="display:flex;flex-wrap:wrap;gap:5px;">
          ${DATA.teachers.filter(t=>t.classes.includes(cls)).map(t=>`<span class="t-class-tag">${t.subject}</span>`).join('')}
        </div>
      </div>`;
    }).join('')}
    </div>`;
  },

  teachers(){
    document.getElementById('content').innerHTML=`
    <div class="sec-hdr mb-20"><div class="sec-title">Teaching Staff · ${DATA.teachers.length} Members</div></div>
    ${DATA.teachers.map(t=>`
    <div class="teacher-card" style="margin-bottom:10px;">
      <div class="t-avatar" style="width:50px;height:50px;font-size:18px;">${t.avatar}</div>
      <div style="flex:1;">
        <div style="display:flex;align-items:center;justify-content:space-between;">
          <div><div class="t-name" style="font-size:15px;">${t.name}</div><div class="t-subject">${t.subject}</div></div>
          <div style="display:flex;gap:16px;text-align:center;">
            <div><div style="font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);">${DATA.lessons.filter(l=>l.teacherId===t.id).length}</div><div style="font-size:10px;font-weight:700;color:var(--muted);text-transform:uppercase;">Lessons</div></div>
            <div><div style="font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);">${DATA.assignments.filter(a=>a.teacherId===t.id).length}</div><div style="font-size:10px;font-weight:700;color:var(--muted);text-transform:uppercase;">Assignments</div></div>
            <div><div style="font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);">${t.classes.length}</div><div style="font-size:10px;font-weight:700;color:var(--muted);text-transform:uppercase;">Classes</div></div>
          </div>
        </div>
        <div class="t-classes" style="margin-top:8px;">${t.classes.map(c=>`<span class="t-class-tag">${c}</span>`).join('')}</div>
      </div>
    </div>`).join('')}`;
  },

  students: teacherViews.students,
  calendar: renderCalendarView,

  announcements(){
    document.getElementById('content').innerHTML=`
    <div class="flex justify-between items-center mb-20">
      <div class="sec-title">School Announcements</div>
      <button class="btn btn-primary" onclick="openCreateAnnouncement()">+ New Announcement</button>
    </div>
    <div id="ann-list">
    ${DATA.announcements.map(a=>`
    <div class="ann-card ${a.priority}" style="margin-bottom:12px;">
      <div class="flex items-center justify-between mb-8">
        <div class="ann-title">${a.title}</div>
        <div style="display:flex;gap:6px;">
          ${badge(a.priority==='high'?'overdue':a.priority==='medium'?'upcoming':'completed',a.priority.charAt(0).toUpperCase()+a.priority.slice(1))}
          <button class="btn btn-ghost btn-sm" onclick="deleteAnnouncement(${a.id})">🗑</button>
        </div>
      </div>
      <div class="ann-body">${a.body}</div>
      <div class="ann-meta"><span>👤 ${a.author}</span><span>📅 ${fmt(a.date)}</span></div>
    </div>`).join('')}
    </div>`;
  }
};

// ════════════════════════════════════════════════
//  CALENDAR VIEW (shared)
// ════════════════════════════════════════════════
function renderCalendarView(){
  const year=STATE.calYear, month=STATE.calMonth;
  const firstDay=new Date(year,month,1).getDay();
  const daysInMonth=new Date(year,month+1,0).getDate();
  const monthNames=['January','February','March','April','May','June','July','August','September','October','November','December'];
  const dayNames=['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
  const todayDate=new Date().getDate(), todayMonth=new Date().getMonth(), todayYear=new Date().getFullYear();

  const monthHolidays=DATA.holidays.filter(h=>{const d=new Date(h.date+'T00:00:00');return d.getMonth()===month&&d.getFullYear()===year;});
  const holidayDays=new Set(monthHolidays.map(h=>new Date(h.date+'T00:00:00').getDate()));

  let calCells='';
  for(let i=0;i<firstDay;i++) calCells+=`<div class="cal-day other-month"></div>`;
  for(let d=1;d<=daysInMonth;d++){
    const isToday=d===todayDate&&month===todayMonth&&year===todayYear;
    const hasEvent=holidayDays.has(d);
    const dateStr=`${year}-${String(month+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
    const dayHols=DATA.holidays.filter(h=>h.date===dateStr);
    calCells+=`<div class="cal-day ${isToday?'today':''} ${hasEvent&&!isToday?'has-event':''}">
      <div class="cal-day-num">${d}</div>
      ${dayHols.map(h=>`<div class="cal-event-dot" style="background:${h.type==='festival'?'#E65100':h.type==='national'?'var(--orange)':'var(--M)'};"></div>`).join('')}
    </div>`;
  }

  const upcomingEvents=DATA.holidays.filter(h=>h.date>=today()).slice(0,6);

  document.getElementById('content').innerHTML=`
  <div class="g-main">
    <div>
      <div class="card mb-20">
        <div class="sec-hdr mb-16">
          <button class="btn btn-ghost btn-sm" onclick="calNav(-1)">◀ Prev</button>
          <div class="sec-title" style="margin:0;">${monthNames[month]} ${year}</div>
          <button class="btn btn-ghost btn-sm" onclick="calNav(1)">Next ▶</button>
        </div>
        <div class="cal-header">${dayNames.map(d=>`<div class="cal-day-hdr">${d}</div>`).join('')}</div>
        <div class="cal-grid">${calCells}</div>
        <div style="display:flex;gap:14px;margin-top:14px;flex-wrap:wrap;">
          <div style="display:flex;align-items:center;gap:5px;font-size:11.5px;color:var(--muted);"><div style="width:10px;height:10px;border-radius:50%;background:#E65100;"></div>Festival</div>
          <div style="display:flex;align-items:center;gap:5px;font-size:11.5px;color:var(--muted);"><div style="width:10px;height:10px;border-radius:50%;background:var(--M);"></div>School Event</div>
          <div style="display:flex;align-items:center;gap:5px;font-size:11.5px;color:var(--muted);"><div style="width:10px;height:10px;border-radius:50%;background:var(--orange);"></div>National</div>
        </div>
      </div>

      <div class="sec-hdr mb-12"><div class="sec-title">📅 Month Events</div></div>
      ${monthHolidays.length?monthHolidays.map(h=>`
      <div class="cal-event-item">
        <div class="cal-event-date">${new Date(h.date+'T00:00:00').getDate()}<div class="cal-event-month">${new Date(h.date+'T00:00:00').toLocaleString('en',{month:'short'})}</div></div>
        <div style="flex:1;"><div class="cal-event-name">${h.name}</div>${badge(h.type,h.type.charAt(0).toUpperCase()+h.type.slice(1))}</div>
        ${STATE.role==='admin'?`<button class="btn btn-danger btn-sm" onclick="deleteHoliday(${h.id})">🗑</button>`:''}
      </div>`).join(''):`<div class="empty"><div class="empty-text">No events this month</div></div>`}
    </div>

    <div>
      <div class="sec-hdr mb-12"><div class="sec-title">📌 Upcoming Events</div></div>
      <div class="card mb-20" style="padding:14px 16px;">
        ${upcomingEvents.map(h=>`
        <div style="display:flex;align-items:center;gap:10px;padding:9px 0;border-bottom:1px solid var(--border);">
          <div style="text-align:center;min-width:36px;"><div style="font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);line-height:1;">${new Date(h.date+'T00:00:00').getDate()}</div><div style="font-size:9px;font-weight:700;color:var(--muted);text-transform:uppercase;">${new Date(h.date+'T00:00:00').toLocaleString('en',{month:'short'})}</div></div>
          <div><div style="font-size:13px;font-weight:700;color:var(--ink);">${h.name}</div>${badge(h.type,h.type)}</div>
        </div>`).join('')}
      </div>

      ${STATE.role==='admin'?`
      <div class="sec-hdr mb-12"><div class="sec-title">+ Add Event</div></div>
      <div class="card">
        <div class="form-group"><label class="form-label">Event Name</label><input class="form-input" id="new-event-name" placeholder="e.g. Annual Day"></div>
        <div class="form-group"><label class="form-label">Date</label><input class="form-input" type="date" id="new-event-date"></div>
        <div class="form-group"><label class="form-label">Type</label><select class="form-select" id="new-event-type"><option value="event">School Event</option><option value="festival">Festival</option><option value="national">National Holiday</option></select></div>
        <button class="btn btn-primary w-full" onclick="addEvent()">Add to Calendar</button>
      </div>`:''}
    </div>
  </div>`;
}

function calNav(dir){
  STATE.calMonth+=dir;
  if(STATE.calMonth>11){STATE.calMonth=0;STATE.calYear++;}
  if(STATE.calMonth<0){STATE.calMonth=11;STATE.calYear--;}
  renderCalendarView();
}

function addEvent(){
  const name=document.getElementById('new-event-name')?.value?.trim();
  const date=document.getElementById('new-event-date')?.value;
  const type=document.getElementById('new-event-type')?.value;
  if(!name||!date) return showToast('Please fill all fields','error');
  const id=Math.max(...DATA.holidays.map(h=>h.id))+1;
  DATA.holidays.push({id,date,name,type});
  showToast('Event added to calendar! ✓');
  renderCalendarView();
}

function deleteHoliday(id){
  DATA.holidays=DATA.holidays.filter(h=>h.id!==id);
  showToast('Event removed');
  renderCalendarView();
}

// ════════════════════════════════════════════════
//  CREATE FORMS
// ════════════════════════════════════════════════
function openCreateLesson(){
  openModal('Create Lesson Plan',`
  <div class="form-group"><label class="form-label">Subject</label><select class="form-select" id="nl-sub"><option>Mathematics</option><option>English</option><option>EVS</option><option>Hindi</option><option>Folk Art</option><option>Music</option><option>PE</option></select></div>
  <div class="form-group"><label class="form-label">Class</label><select class="form-select" id="nl-cls">${STATE.user.classes?STATE.user.classes.map(c=>`<option>${c}</option>`).join(''):'<option>Class 3A</option>'}</select></div>
  <div class="form-group"><label class="form-label">Lesson Topic</label><input class="form-input" id="nl-topic" placeholder="e.g. Introduction to Fractions"></div>
  <div class="form-group"><label class="form-label">Date</label><input class="form-input" type="date" id="nl-date" value="${today()}"></div>
  <div class="form-group"><label class="form-label">Description</label><textarea class="form-textarea" id="nl-desc" placeholder="Describe the lesson approach..."></textarea></div>
  <div class="form-group"><label class="form-label">Learning Objectives (one per line)</label><textarea class="form-textarea" id="nl-obj" placeholder="Understand fractions&#10;Identify halves and quarters&#10;Represent fractions visually"></textarea></div>
  <div class="form-group"><label class="form-label">Materials (comma separated)</label><input class="form-input" id="nl-mat" placeholder="e.g. Textbook, Worksheets, Colour pencils"></div>
  `,`<button class="btn btn-ghost" onclick="closeModalDirect()">Cancel</button><button class="btn btn-primary" onclick="saveLesson()">Save Lesson Plan</button>`);
}

function saveLesson(){
  const topic=document.getElementById('nl-topic')?.value?.trim();
  const date=document.getElementById('nl-date')?.value;
  if(!topic||!date) return showToast('Please fill all required fields','error');
  const id=Math.max(...DATA.lessons.map(l=>l.id))+1;
  const obj=document.getElementById('nl-obj')?.value?.split('\n').filter(Boolean)||[];
  const mat=document.getElementById('nl-mat')?.value?.split(',').map(s=>s.trim()).filter(Boolean)||[];
  DATA.lessons.push({
    id,
    subject:document.getElementById('nl-sub').value,
    cls:document.getElementById('nl-cls').value,
    teacher:STATE.user.name,
    teacherId:STATE.user.id,
    topic,date,
    status:'upcoming',
    description:document.getElementById('nl-desc')?.value||'',
    objectives:obj,
    materials:mat
  });
  closeModalDirect();
  showToast('Lesson plan created! ✓');
  navigate('lessons');
}

function editLesson(id){
  const l=DATA.lessons.find(x=>x.id===id);
  if(!l) return;
  openModal('Edit Lesson Plan',`
  <div class="form-group"><label class="form-label">Topic</label><input class="form-input" id="el-topic" value="${l.topic}"></div>
  <div class="form-group"><label class="form-label">Date</label><input class="form-input" type="date" id="el-date" value="${l.date}"></div>
  <div class="form-group"><label class="form-label">Status</label><select class="form-select" id="el-status"><option ${l.status==='upcoming'?'selected':''}>upcoming</option><option ${l.status==='ongoing'?'selected':''}>ongoing</option><option ${l.status==='completed'?'selected':''}>completed</option></select></div>
  <div class="form-group"><label class="form-label">Description</label><textarea class="form-textarea" id="el-desc">${l.description}</textarea></div>
  `,`<button class="btn btn-ghost" onclick="closeModalDirect()">Cancel</button><button class="btn btn-primary" onclick="updateLesson(${id})">Update</button>`);
}

function updateLesson(id){
  const l=DATA.lessons.find(x=>x.id===id);
  if(!l) return;
  l.topic=document.getElementById('el-topic').value;
  l.date=document.getElementById('el-date').value;
  l.status=document.getElementById('el-status').value;
  l.description=document.getElementById('el-desc').value;
  closeModalDirect();
  showToast('Lesson updated! ✓');
  navigate('lessons');
}

function filterTeacherLessons(f){
  document.querySelectorAll('[id^=tlf-]').forEach(b=>{b.classList.add('btn-ghost');b.classList.remove('btn-primary');});
  document.getElementById('tlf-'+f).classList.remove('btn-ghost');
  document.getElementById('tlf-'+f).classList.add('btn-primary');
  const mine=DATA.lessons.filter(l=>l.teacherId===STATE.user.id);
  const list=f==='all'?mine:mine.filter(l=>l.status===f);
  document.getElementById('tlesson-list').innerHTML=renderLessonsList(list);
}

function openCreateAssignment(){
  openModal('Create Assignment / Homework',`
  <div class="form-group"><label class="form-label">Title</label><input class="form-input" id="na-title" placeholder="e.g. Fractions in Daily Life"></div>
  <div class="form-group"><label class="form-label">Subject</label><select class="form-select" id="na-sub"><option>Mathematics</option><option>English</option><option>EVS</option><option>Hindi</option><option>Folk Art</option></select></div>
  <div class="form-group"><label class="form-label">Class</label><select class="form-select" id="na-cls">${(STATE.user?.classes||['Class 3A']).map(c=>`<option>${c}</option>`).join('')}</select></div>
  <div class="form-group"><label class="form-label">Type</label><select class="form-select" id="na-type"><option value="homework">Homework</option><option value="assignment">Assignment</option></select></div>
  <div class="form-group"><label class="form-label">Description / Instructions</label><textarea class="form-textarea" id="na-desc" placeholder="Describe what students need to do..."></textarea></div>
  <div class="form-group"><label class="form-label">Due Date</label><input class="form-input" type="date" id="na-due"></div>
  `,`<button class="btn btn-ghost" onclick="closeModalDirect()">Cancel</button><button class="btn btn-primary" onclick="saveAssignment()">Assign</button>`);
}

function saveAssignment(){
  const title=document.getElementById('na-title')?.value?.trim();
  const due=document.getElementById('na-due')?.value;
  if(!title||!due) return showToast('Please fill all fields','error');
  const id=Math.max(...DATA.assignments.map(a=>a.id))+1;
  DATA.assignments.push({
    id, title,
    subject:document.getElementById('na-sub').value,
    cls:document.getElementById('na-cls').value,
    teacherId:STATE.user.id,
    description:document.getElementById('na-desc')?.value||'',
    type:document.getElementById('na-type').value,
    dueDate:due,
    assignedDate:today(),
    status:'active',
    submissions:0,
    total:28
  });
  closeModalDirect();
  showToast('Assignment created and assigned! ✓');
  navigate('assignments');
}

function deleteAssignment(id){
  if(!confirm('Delete this assignment?')) return;
  DATA.assignments=DATA.assignments.filter(a=>a.id!==id);
  showToast('Assignment deleted');
  navigate('assignments');
}

function openCreateAnnouncement(){
  openModal('New Announcement',`
  <div class="form-group"><label class="form-label">Title</label><input class="form-input" id="nc-title" placeholder="Announcement title..."></div>
  <div class="form-group"><label class="form-label">Message</label><textarea class="form-textarea" id="nc-body" placeholder="Write the announcement..." style="min-height:100px;"></textarea></div>
  <div class="form-group"><label class="form-label">Priority</label><select class="form-select" id="nc-pri"><option value="low">Low</option><option value="medium">Medium</option><option value="high">High / Urgent</option></select></div>
  <div class="form-group"><label class="form-label">Author / Department</label><input class="form-input" id="nc-auth" value="${STATE.user?.name||'Administration'}"></div>
  `,`<button class="btn btn-ghost" onclick="closeModalDirect()">Cancel</button><button class="btn btn-primary" onclick="saveAnnouncement()">Publish</button>`);
}

function saveAnnouncement(){
  const title=document.getElementById('nc-title')?.value?.trim();
  const body=document.getElementById('nc-body')?.value?.trim();
  if(!title||!body) return showToast('Please fill all fields','error');
  const id=Math.max(...DATA.announcements.map(a=>a.id))+1;
  DATA.announcements.unshift({id,title,body,priority:document.getElementById('nc-pri').value,author:document.getElementById('nc-auth').value,date:today()});
  closeModalDirect();
  showToast('Announcement published! ✓');
  navigate('announcements');
}

function deleteAnnouncement(id){
  DATA.announcements=DATA.announcements.filter(a=>a.id!==id);
  showToast('Announcement deleted');
  navigate('announcements');
}

function showStudentDetail(id){
  const s=DATA.students.find(x=>x.id===id);
  if(!s) return;
  const avg=Math.round(Object.values(s.progress).reduce((a,b)=>a+b,0)/Object.values(s.progress).length);
  openModal(s.name,`
  <div class="flex items-center gap-14 mb-16">
    <div style="width:56px;height:56px;border-radius:8px;background:var(--Y);display:flex;align-items:center;justify-content:center;font-family:var(--fD);font-size:22px;font-weight:900;color:var(--M);">${s.avatar}</div>
    <div><div style="font-size:13px;font-weight:700;color:var(--muted);">${s.class} · Roll ${s.roll}</div><div style="font-family:var(--fD);font-size:28px;font-weight:900;color:var(--M);line-height:1.1;">${avg}%<span style="font-size:14px;font-weight:600;color:var(--muted);margin-left:5px;">Average</span></div></div>
  </div>
  <div style="font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);margin-bottom:12px;">Subject Progress</div>
  ${Object.entries(s.progress).map(([sub,score])=>`
  <div class="progress-row">
    <div class="prog-subject">${sub}</div>
    <div class="prog-bar-wrap"><div class="prog-bar-fill" style="width:${score}%;background:${subjectColor(sub)};"></div></div>
    <div class="prog-score">${score}%</div>
  </div>`).join('')}
  <div style="margin-top:16px;">
    <div style="font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);margin-bottom:8px;">Pending Assignments</div>
    ${DATA.assignments.filter(a=>a.status==='active').slice(0,3).map(a=>`<div style="font-size:12.5px;color:var(--ink);padding:5px 0;border-bottom:1px solid var(--border);">📝 ${a.title} <span style="color:var(--muted);"> · Due ${fmtShort(a.dueDate)}</span></div>`).join('')}
  </div>
  `,'',true);
}

function showAnnouncements(){
  openModal('📢 Announcements',
    DATA.announcements.map(a=>`<div class="ann-card ${a.priority}" style="margin-bottom:10px;"><div class="ann-title">${a.title}</div><div class="ann-body">${a.body}</div><div class="ann-meta"><span>${a.author}</span><span>${fmt(a.date)}</span></div></div>`).join(''),
    '',true
  );
}

// ════════════════════════════════════════════════
//  MODAL HELPERS
// ════════════════════════════════════════════════
function openModal(title,body,footer='',noFooter=false){
  document.getElementById('modal-title').textContent=title;
  document.getElementById('modal-body').innerHTML=body;
  document.getElementById('modal-footer').innerHTML=footer;
  document.getElementById('modal-footer').style.display=noFooter?'none':'flex';
  document.getElementById('modal-overlay').classList.remove('hidden');
}
function closeModal(e){ if(e.target===document.getElementById('modal-overlay')) closeModalDirect(); }
function closeModalDirect(){ document.getElementById('modal-overlay').classList.add('hidden'); }

// ════════════════════════════════════════════════
//  TOAST
// ════════════════════════════════════════════════
function showToast(msg,type='success'){
  let t=document.getElementById('toast');
  if(!t){t=document.createElement('div');t.id='toast';t.style.cssText='position:fixed;bottom:24px;right:24px;padding:12px 20px;border-radius:8px;font-size:13px;font-weight:700;z-index:9999;transition:opacity .3s;';document.body.appendChild(t);}
  t.style.background=type==='error'?'#DC2626':'#16A34A';
  t.style.color='white';
  t.textContent=msg;
  t.style.opacity='1';
  clearTimeout(t._timer);
  t._timer=setTimeout(()=>{t.style.opacity='0';},2800);
}

// ════════════════════════════════════════════════
//  VIEWS ROUTER
// ════════════════════════════════════════════════
const VIEWS = { student:studentViews, teacher:teacherViews, admin:adminViews };

// Init date
document.addEventListener('DOMContentLoaded',()=>{
  document.getElementById('tb-date').textContent=new Date().toLocaleDateString('en-IN',{weekday:'long',year:'numeric',month:'long',day:'numeric'});
});
</script>
</body>
</html>
