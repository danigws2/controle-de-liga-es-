# controle-de-liga-es-
Contador de ligações diárias para registrar chamadas atendidas e não atendidas, acompanhar resultados e manter um histórico organizado por dia.



Codigo atual 

<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#f5f1e9">
<title>Meninas da Fênix</title>
<style>
:root{--bg:#f5f1e9;--paper:#fffdf9;--ink:#25251f;--muted:#858176;--line:#e5dfd4;--green:#398454;--green-bg:#edf5ee;--red:#c94e48;--red-bg:#fbefed;--gold:#b8892f;--gold-bg:#faf3e2;color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media(prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#1c1b17;--paper:#26241f;--ink:#f1ede4;--muted:#a09a8c;--line:#3a372f;--green:#6cc58a;--green-bg:#1f2e24;--red:#ee8a84;--red-bg:#341f1d;--gold:#e0b45c;--gold-bg:#322a18;color-scheme:dark}}
:root[data-theme="dark"]{--bg:#1c1b17;--paper:#26241f;--ink:#f1ede4;--muted:#a09a8c;--line:#3a372f;--green:#6cc58a;--green-bg:#1f2e24;--red:#ee8a84;--red-bg:#341f1d;--gold:#e0b45c;--gold-bg:#322a18;color-scheme:dark}
*{box-sizing:border-box}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--bg);color:var(--ink);font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif}
button,input{font:inherit;color:inherit}button{cursor:pointer}
.app{width:min(100% - 30px,580px);margin:auto;padding:28px 0 45px}
header,.date-row,.section-head,.history-head,.row{display:flex;align-items:center;justify-content:space-between;gap:12px}
header{padding-bottom:22px;border-bottom:1px solid var(--line)}
.brand{font-size:12px;font-weight:750;letter-spacing:2.5px}
.status{font-size:10px;color:var(--muted)}
.intro{padding:26px 0 20px}
h1{font:400 clamp(34px,8vw,44px)/1.12 Georgia,serif;letter-spacing:-1.5px;margin:0 0 10px}
.subtitle{font-size:13px;color:var(--muted)}
.phrase{background:var(--gold-bg);color:var(--gold);border-radius:14px;padding:15px 17px;margin-bottom:15px;font:italic 16px/1.45 Georgia,serif}
.phrase small{display:block;font:600 10px -apple-system,sans-serif;letter-spacing:.5px;margin-top:8px;opacity:.85}
.panel{background:var(--paper);border:1px solid var(--line);border-radius:17px;padding:21px;margin-bottom:15px}
.date-label,.section-head,.label{font-size:10px;letter-spacing:1.2px;text-transform:uppercase;color:var(--muted)}
.date-value{font:19px Georgia,serif;margin-top:6px}
input[type=date],input[type=text],input[type=tel],input[type=time],input[type=number]{min-width:0;padding:10px;border:1px solid var(--line);border-radius:8px;background:var(--paper);font-size:13px}
input[type=date]{width:145px}
:focus-visible{outline:2px solid var(--ink);outline-offset:2px}
.total-wrap{text-align:center;padding:24px 0 20px}
.total{font:84px/1 Georgia,serif;letter-spacing:-4px;display:inline-block}
.total.pop{animation:pop .25s}
@keyframes pop{50%{transform:scale(1.12)}}
@media(prefers-reduced-motion:reduce){.total.pop{animation:none}}
.total-caption{font-size:12px;color:var(--muted);margin-top:9px}
.goal{margin:0 0 18px}
.goal-top{display:flex;justify-content:space-between;align-items:center;font-size:12px;color:var(--muted);margin-bottom:8px;gap:10px}
.goal-top input{width:68px;padding:6px 8px;text-align:center}
.bar{height:9px;background:var(--line);border-radius:9px;overflow:hidden}
.bar i{display:block;height:100%;width:0;background:var(--green);border-radius:9px;transition:width .35s}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.stat{border-radius:12px;padding:13px}
.stat.green{background:var(--green-bg);color:var(--green)}.stat.red{background:var(--red-bg);color:var(--red)}
.stat-label{font-size:11px;font-weight:600}
.stat-num{font:32px Georgia,serif;margin-top:7px}
.stat-note{font-size:10px;opacity:.8;margin-top:3px}
.tally-section{margin-top:19px;padding-top:17px;border-top:1px solid var(--line)}
.tallies{display:flex;flex-wrap:wrap;gap:13px 12px;padding:20px 2px 0;min-height:53px}
.group{position:relative;width:36px;height:37px;display:flex;align-items:flex-end;justify-content:space-between;padding:1px 3px 0}
.mark{display:block;width:2px;height:31px;border-radius:2px;background:var(--ink)}
.group.green .mark,.group.green .slash{background:var(--green)}.group.red .mark,.group.red .slash{background:var(--red)}
.slash{position:absolute;width:2px;height:39px;border-radius:2px;left:17px;top:-1px;transform:rotate(43deg)}
.empty{font-size:12px;color:var(--muted);padding:5px 0 12px}
.actions,.undo-row{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.actions{margin:15px 0 8px}.undo-row{margin-bottom:22px}
.btn{border-radius:11px;border:1px solid transparent;padding:14px 8px;font-size:13px;font-weight:650;min-height:51px;color:#fff}
.btn:active{transform:scale(.98)}.btn:disabled{opacity:.35;cursor:not-allowed}
.attend{background:#398454}.missed{background:#c94e48}
.undo{background:transparent;border:1px solid var(--line);color:var(--muted);font-size:11px;padding:10px 5px;border-radius:9px}
.undo:disabled{opacity:.35;cursor:not-allowed}
h2{font:400 24px Georgia,serif;margin:0}
.count{font-size:11px;color:var(--muted)}
.sub-h{margin:22px 0 10px}
.list{display:grid;gap:8px;margin-top:12px}
.item{display:flex;align-items:center;gap:10px;background:var(--paper);border:1px solid var(--line);border-radius:11px;padding:11px 13px;font-size:13px}
.item .grow{flex:1;min-width:0}
.item .t{font-size:11px;color:var(--muted);font-variant-numeric:tabular-nums}
.dot{width:9px;height:9px;border-radius:50%;flex:none}
.dot.a{background:var(--green)}.dot.m{background:var(--red)}
.x,.chk{background:none;border:1px solid var(--line);border-radius:8px;width:32px;height:32px;color:var(--muted);flex:none}
.chk.on{background:var(--green);border-color:var(--green);color:#fff}
.item.done .grow{text-decoration:line-through;opacity:.5}
.name{font-weight:650;overflow-wrap:anywhere}
.history-item{width:100%;text-align:left;justify-content:space-between}
.history-item.selected{border-color:var(--ink)}
.hn{display:flex;gap:10px;font:20px Georgia,serif}
.g{color:var(--green)}.r{color:var(--red)}
.form{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:14px}
.form .full{grid-column:1/-1}
.form .btn{background:var(--ink);color:var(--bg);min-height:44px}
.note{font-size:11px;color:var(--muted);margin-top:10px;line-height:1.5}
footer{border-top:1px solid var(--line);margin-top:25px;padding-top:16px;text-align:center;font-size:10px;line-height:1.7;color:var(--muted)}
@media(max-width:370px){.app{width:calc(100% - 22px)}.panel{padding:15px}.total{font-size:72px}}
[hidden]{display:none!important}
.linkbtn{background:none;border:0;color:var(--muted);text-decoration:underline;font-size:10px;padding:4px}
.login{position:fixed;inset:0;background:var(--bg);display:flex;align-items:center;justify-content:center;padding:22px;z-index:20;overflow:auto}
.login-card{width:min(100%,380px);background:var(--paper);border:1px solid var(--line);border-radius:17px;padding:24px;display:grid;gap:12px}
.login-card h2{margin:4px 0 0}
.login-card input[type=text]{width:100%}
.pin{width:100%;min-width:0;padding:11px;border:1px solid var(--line);border-radius:8px;background:var(--paper);text-align:center;letter-spacing:6px;font-size:18px}
.pin::placeholder{letter-spacing:0;font-size:13px}
</style>
</head>
<body>
<main class="app" id="app" hidden>
  <header>
    <div class="brand">MENINAS DA FÊNIX 🔥</div>
    <div class="status"><span id="who"></span> <button class="linkbtn" id="logout" type="button">Sair</button></div>
  </header>

  <section class="intro">
    <h1>Uma ligação<br>de cada vez.</h1>
    <div class="subtitle">Atendidas ou não, registre todas.</div>
  </section>

  <div class="phrase" id="phrase"></div>

  <section class="panel">
    <div class="row">
      <div>
        <div class="date-label">Data selecionada</div>
        <div class="date-value" id="dateLabel">Hoje</div>
      </div>
      <input type="date" id="datePicker" aria-label="Selecionar data">
    </div>

    <div class="total-wrap">
      <div class="total" id="total">0</div>
      <div class="total-caption">ligações no total</div>
    </div>

    <div class="goal">
      <div class="goal-top">
        <span id="goalText">Meta do dia</span>
        <label>Meta <input type="number" id="goalInput" min="1" max="999" inputmode="numeric"></label>
      </div>
      <div class="bar"><i id="goalBar"></i></div>
    </div>

    <div class="stats">
      <div class="stat green"><div class="stat-label">✓ Atendidas</div><div class="stat-num" id="attendedTotal">0</div><div class="stat-note" id="attendedPercent"></div></div>
      <div class="stat red"><div class="stat-label">× Não atendidas</div><div class="stat-num" id="missedTotal">0</div><div class="stat-note" id="missedPercent"></div></div>
    </div>

    <div class="tally-section">
      <div class="section-head"><span>Palitinhos verdes</span><span id="greenGroups"></span></div>
      <div class="tallies" id="greenTallies"></div>
    </div>
    <div class="tally-section">
      <div class="section-head"><span>Palitinhos vermelhos</span><span id="redGroups"></span></div>
      <div class="tallies" id="redTallies"></div>
    </div>
  </section>

  <div class="actions">
    <button class="btn attend" id="addAttended">✓ Atendida +1</button>
    <button class="btn missed" id="addMissed">× Não atendida +1</button>
  </div>
  <div class="undo-row">
    <button class="undo" id="undoAttended">↶ Desfazer atendida</button>
    <button class="undo" id="undoMissed">↶ Desfazer não atendida</button>
  </div>

  <section class="panel">
    <div class="history-head"><h2>Retornos</h2><span class="count" id="retCount"></span></div>
    <div class="form">
      <input type="text" id="retName" class="full" placeholder="Nome da pessoa" maxlength="60" aria-label="Nome">
      <input type="tel" id="retPhone" placeholder="Telefone" maxlength="20" aria-label="Telefone">
      <input type="time" id="retTime" aria-label="Horário combinado">
      <input type="text" id="retNote" class="full" placeholder="Anotação (ex.: ligar depois das 15h)" maxlength="100" aria-label="Anotação">
      <button class="btn full" id="retAdd">Adicionar retorno</button>
    </div>
    <div class="list" id="retList"></div>
    <div class="note">🔒 Esta lista fica só neste aparelho e não é compartilhada com ninguém.</div>
  </section>

  <section>
    <div class="history-head"><h2>Histórico</h2><span class="count" id="historyCount"></span></div>
    <div class="list" id="historyList"></div>
  </section>

  <footer>Seus registros são salvos neste navegador.<br>Use o mesmo navegador para manter seu histórico.</footer>
</main>

<div class="login" id="login" hidden><div class="login-card" id="loginCard"></div></div>

<script>
(() => {
  let K = "", KR = "", KG = ""; // chaves de cada perfil, definidas ao entrar
  const $ = id => document.getElementById(id);

  /* ---------- Armazenamento (trocar aqui quando o banco de dados estiver pronto) ---------- */
  const Store = {
    get(key, fallback) {
      try { const v = JSON.parse(localStorage.getItem(key)); return v ?? fallback; } catch { return fallback; }
    },
    set(key, value) {
      try { localStorage.setItem(key, JSON.stringify(value)); }
      catch { alert("Não foi possível salvar. Verifique o espaço disponível no navegador."); }
    }
  };

  /* ---------- Frase do dia: a mesma para todas, muda a cada dia ---------- */
  const PHRASES = [
    "Renasça das cinzas todo dia, fênix. Hoje tem recomeço! 🔥",
    "Tudo posso naquele que me fortalece. (Filipenses 4.13)",
    "Respira, sorri e disca. Você é incrível! ✨",
    "A resposta branda desvia o furor. (Provérbios 15.1)",
    "Cada ligação é um passinho, e passinho vira caminhada 🌸",
    "Os que esperam no Senhor renovarão as suas forças e subirão com asas como águias. (Isaías 40.31)",
    "Menina que sorri ao telefone muda o dia de alguém 💛",
    "Este é o dia que fez o Senhor; regozijemo-nos e alegremo-nos nele. (Salmos 118.24)",
    "Hoje a meta é bater a meta... e ainda tomar um cafezinho ☕",
    "O coração alegre serve de bom remédio. (Provérbios 17.22)",
    "Água, pausa e carinho com você mesma 💧",
    "Tudo quanto fizerdes, fazei-o de todo o coração. (Colossenses 3.23)",
    "Juntas somos fênix: cada uma brilha do seu jeito 🔥💛",
    "Se Deus é por nós, quem será contra nós? (Romanos 8.31)",
    "Você está fazendo melhor do que imagina 🌷",
    "Não nos cansemos de fazer o bem, porque a seu tempo ceifaremos. (Gálatas 6.9)",
    "Um não de hoje pode virar um sim amanhã 🌈",
    "Esforça-te, e tem bom ânimo; o Senhor, teu Deus, é contigo. (Josué 1.9)",
    "Ninguém atende todas. O que conta é continuar, linda! 💪",
    "A força e a honra são os seus vestidos. (Provérbios 31.25)"
  ];
  const today0 = new Date(); today0.setHours(0, 0, 0, 0);
  const dayNum = Math.floor(today0.getTime() / 864e5);
  const phrase = PHRASES[(dayNum * 7 + 3) % PHRASES.length];
  $("phrase").textContent = "“" + phrase + "”";
  const small = document.createElement("small");
  small.textContent = "Frase do dia, igual para o time todo 💛";
  $("phrase").appendChild(small);

  /* ---------- Dados ---------- */
  const two = n => String(n).padStart(2, "0");
  const localDate = (d = new Date()) => `${d.getFullYear()}-${two(d.getMonth() + 1)}-${two(d.getDate())}`;
  const nowTime = () => { const d = new Date(); return two(d.getHours()) + ":" + two(d.getMinutes()); };

  function readData() {
    const x = Store.get(K, {});
    if (!x || typeof x !== "object" || Array.isArray(x)) return {};
    const clean = {};
    for (const [date, v] of Object.entries(x)) {
      if (!/^\d{4}-\d{2}-\d{2}$/.test(date) || !v || typeof v !== "object") continue;
      clean[date] = {
        attended: Math.max(0, Math.floor(Number(v.attended) || 0)),
        missed: Math.max(0, Math.floor(Number(v.missed) || 0)),
        log: Array.isArray(v.log) ? v.log.filter(l => l && (l.k === "a" || l.k === "m")).map(l => ({ id: String(l.id), t: String(l.t || ""), k: l.k })) : []
      };
    }
    return clean;
  }

  let data = {}, returns = [], goal = 80;
  let selectedDate = localDate();
  function loadAll() {
    data = readData();
    const r = Store.get(KR, []); returns = Array.isArray(r) ? r : [];
    goal = Math.max(1, Math.floor(Number(Store.get(KG, 80)) || 80));
  }

  const count = (date, type) => Math.max(0, Number(data[date]?.[type]) || 0);
  const day = date => data[date] || (data[date] = { attended: 0, missed: 0, log: [] });
  const saveData = () => Store.set(K, data);
  const saveRet = () => Store.set(KR, returns);

  function formatDate(date) {
    if (date === localDate()) return "Hoje";
    const y = new Date(); y.setDate(y.getDate() - 1);
    if (date === localDate(y)) return "Ontem";
    const [yy, m, d] = date.split("-").map(Number);
    return new Date(yy, m - 1, d).toLocaleDateString("pt-BR", { day: "numeric", month: "long", year: "numeric" });
  }

  /* ---------- Palitinhos ---------- */
  function groupOfFive(color, n) {
    const el = document.createElement("span");
    el.className = "group " + color;
    for (let i = 0; i < (n === 5 ? 4 : n); i++) {
      const m = document.createElement("span"); m.className = "mark"; el.appendChild(m);
    }
    if (n === 5) { const s = document.createElement("span"); s.className = "slash"; el.appendChild(s); }
    return el;
  }
  function renderTallies(id, color, n) {
    const c = $(id); c.replaceChildren();
    if (!n) { const e = document.createElement("div"); e.className = "empty"; e.textContent = "Nenhuma ligação registrada nesta categoria."; c.appendChild(e); return; }
    const shown = Math.min(n, 500);
    for (let i = 0; i < Math.floor(shown / 5); i++) c.appendChild(groupOfFive(color, 5));
    if (shown % 5) c.appendChild(groupOfFive(color, shown % 5));
    if (n > 500) { const e = document.createElement("div"); e.className = "empty"; e.textContent = `Exibindo 500 palitinhos de ${n}.`; c.appendChild(e); }
  }

  /* ---------- Retornos ---------- */
  function renderReturns() {
    const list = $("retList"); list.replaceChildren();
    const pending = returns.filter(r => !r.done).length;
    $("retCount").textContent = pending + (pending === 1 ? " pendente" : " pendentes");
    if (!returns.length) { const e = document.createElement("div"); e.className = "empty"; e.textContent = "Nenhum retorno anotado ainda."; list.appendChild(e); return; }
    [...returns].sort((a, b) => (a.done - b.done) || (a.time || "99").localeCompare(b.time || "99")).forEach(r => {
      const it = document.createElement("div"); it.className = "item" + (r.done ? " done" : "");
      const chk = document.createElement("button"); chk.type = "button"; chk.className = "chk" + (r.done ? " on" : ""); chk.textContent = "✓";
      chk.setAttribute("aria-label", r.done ? "Marcar como pendente" : "Marcar como retornado");
      chk.addEventListener("click", () => { r.done = !r.done; saveRet(); renderReturns(); });
      const g = document.createElement("div"); g.className = "grow";
      const n = document.createElement("div"); n.className = "name"; n.textContent = r.name;
      const s = document.createElement("div"); s.className = "t";
      s.textContent = [r.time, r.phone, r.note].filter(Boolean).join(" · ") || "Sem detalhes";
      g.append(n, s);
      const x = document.createElement("button"); x.type = "button"; x.className = "x"; x.textContent = "×"; x.setAttribute("aria-label", "Apagar retorno de " + r.name);
      x.addEventListener("click", () => { returns = returns.filter(i => i.id !== r.id); saveRet(); renderReturns(); });
      it.append(chk, g, x); list.appendChild(it);
    });
  }

  /* ---------- Histórico ---------- */
  function renderHistory() {
    const list = $("historyList"); list.replaceChildren();
    const dates = Object.keys(data).filter(d => count(d, "attended") + count(d, "missed") > 0).sort((a, b) => b.localeCompare(a));
    $("historyCount").textContent = dates.length + (dates.length === 1 ? " dia" : " dias");
    if (!dates.length) { const e = document.createElement("div"); e.className = "empty"; e.textContent = "Suas ligações aparecerão aqui."; list.appendChild(e); return; }
    dates.forEach(date => {
      const b = document.createElement("button"); b.type = "button"; b.className = "item history-item" + (date === selectedDate ? " selected" : "");
      const l = document.createElement("div");
      const a = document.createElement("div"); a.className = "name"; a.textContent = formatDate(date);
      const s = document.createElement("div"); s.className = "t"; s.textContent = date.split("-").reverse().join("/") + " · " + (count(date, "attended") + count(date, "missed")) + " ligações";
      l.append(a, s);
      const n = document.createElement("div"); n.className = "hn";
      const g = document.createElement("span"); g.className = "g"; g.textContent = "● " + count(date, "attended");
      const r = document.createElement("span"); r.className = "r"; r.textContent = "● " + count(date, "missed");
      n.append(g, r); b.append(l, n);
      b.addEventListener("click", () => { selectedDate = date; render(); window.scrollTo({ top: 0, behavior: "smooth" }); });
      list.appendChild(b);
    });
  }

  /* ---------- Tela principal ---------- */
  let lastTotal = null;
  function render() {
    const a = count(selectedDate, "attended"), m = count(selectedDate, "missed"), total = a + m;
    $("datePicker").value = selectedDate;
    $("dateLabel").textContent = formatDate(selectedDate);
    $("total").textContent = total;
    if (lastTotal !== null && total > lastTotal) { const t = $("total"); t.classList.remove("pop"); void t.offsetWidth; t.classList.add("pop"); }
    lastTotal = total;
    $("attendedTotal").textContent = a; $("missedTotal").textContent = m;
    $("attendedPercent").textContent = (total ? Math.round(a / total * 100) : 0) + "% do total";
    $("missedPercent").textContent = (total ? Math.round(m / total * 100) : 0) + "% do total";
    $("greenGroups").textContent = Math.floor(a / 5) + " grupos de 5";
    $("redGroups").textContent = Math.floor(m / 5) + " grupos de 5";
    $("undoAttended").disabled = a === 0; $("undoMissed").disabled = m === 0;

    const pct = Math.min(100, Math.round(total / goal * 100));
    $("goalBar").style.width = pct + "%";
    $("goalInput").value = goal;
    $("goalText").textContent = total >= goal ? "Meta batida! 🎉 " + total + " de " + goal : total + " de " + goal + " · faltam " + (goal - total);

    renderTallies("greenTallies", "green", a);
    renderTallies("redTallies", "red", m);
    renderReturns(); renderHistory();
  }

  /* ---------- Ações ---------- */
  const vibrate = ms => { try { navigator.vibrate && navigator.vibrate(ms); } catch {} };
  let lastClick = 0;

  function add(type) {
    const now = Date.now();
    if (now - lastClick < 350) return; // proteção contra clique duplo
    lastClick = now;
    const d = day(selectedDate);
    d[type === "a" ? "attended" : "missed"]++;
    d.log.push({ id: now + "-" + Math.random().toString(36).slice(2, 6), t: nowTime(), k: type });
    vibrate(25); saveData(); render();
  }

  function cleanup() {
    const d = data[selectedDate];
    if (d && !d.attended && !d.missed && !d.log.length) delete data[selectedDate];
  }

  function undo(type) {
    const d = data[selectedDate]; const key = type === "a" ? "attended" : "missed";
    if (!d || !d[key]) return;
    d[key]--;
    for (let i = d.log.length - 1; i >= 0; i--) if (d.log[i].k === type) { d.log.splice(i, 1); break; }
    vibrate(15); cleanup(); saveData(); render();
  }

  $("addAttended").addEventListener("click", () => add("a"));
  $("addMissed").addEventListener("click", () => add("m"));
  $("undoAttended").addEventListener("click", () => undo("a"));
  $("undoMissed").addEventListener("click", () => undo("m"));
  $("datePicker").addEventListener("change", e => { if (e.target.value) { selectedDate = e.target.value; render(); } });
  $("goalInput").addEventListener("change", e => {
    const v = Math.floor(Number(e.target.value));
    goal = v >= 1 ? Math.min(999, v) : goal;
    Store.set(KG, goal); render();
  });
  $("retAdd").addEventListener("click", () => {
    const name = $("retName").value.trim();
    if (!name) { $("retName").focus(); return; }
    returns.push({ id: Date.now() + "-" + Math.random().toString(36).slice(2, 6), name, phone: $("retPhone").value.trim(), time: $("retTime").value, note: $("retNote").value.trim(), done: false });
    ["retName", "retPhone", "retTime", "retNote"].forEach(i => $(i).value = "");
    saveRet(); renderReturns();
  });

  /* ---------- Perfis e PIN (guardados só neste aparelho) ---------- */
  const KP = 'fenix-perfis-v1', KS = 'fenix-sessao-v1', MAXP = 5;
  const LEGACY = ['contagem-ligacoes-colorida-v2', 'contagem-retornos-v1', 'contagem-meta-v1'];
  let profiles = Store.get(KP, []); if (!Array.isArray(profiles)) profiles = [];
  const el = (tag, cls, text) => { const e = document.createElement(tag); if (cls) e.className = cls; if (text != null) e.textContent = text; return e; };
  const newSalt = () => Math.random().toString(36).slice(2) + Date.now().toString(36);
  const keysOf = id => [`fenix-${id}-dias`, `fenix-${id}-retornos`, `fenix-${id}-meta`];

  async function hashPin(salt, pin) {
    const s = salt + ':' + pin;
    try {
      const b = await crypto.subtle.digest('SHA-256', new TextEncoder().encode(s));
      return Array.from(new Uint8Array(b)).map(x => x.toString(16).padStart(2, '0')).join('');
    } catch { let h = 5381; for (const c of s) h = ((h << 5) + h + c.charCodeAt(0)) >>> 0; return 'f' + h; }
  }

  function pinInput(label) {
    const i = el('input', 'pin'); i.type = 'password'; i.inputMode = 'numeric'; i.maxLength = 4; i.autocomplete = 'off';
    i.placeholder = '••••'; i.setAttribute('aria-label', label);
    i.addEventListener('input', () => { i.value = i.value.replace(/\D/g, ''); });
    return i;
  }

  function enter(p) {
    [K, KR, KG] = keysOf(p.id);
    try { sessionStorage.setItem(KS, p.id); } catch {}
    loadAll(); selectedDate = localDate(); lastTotal = null;
    $('who').textContent = 'Oi, ' + p.name + ' 💛';
    $('login').hidden = true; $('app').hidden = false;
    render(); window.scrollTo(0, 0);
  }

  function showPin(p) {
    const card = $('loginCard'); card.replaceChildren();
    const inp = pinInput('PIN'), msg = el('div', 'note'), go = el('button', 'btn attend', 'Entrar');
    const back = el('button', 'undo', '← Voltar'), forgot = el('button', 'undo', 'Esqueci meu PIN');
    msg.setAttribute('role', 'alert'); go.type = back.type = forgot.type = 'button';
    const submit = async () => {
      if (inp.value.length !== 4) { msg.textContent = 'O PIN tem 4 números.'; return; }
      if (await hashPin(p.salt, inp.value) === p.hash) enter(p);
      else { msg.textContent = 'PIN incorreto. Tente de novo.'; inp.value = ''; inp.focus(); }
    };
    go.addEventListener('click', submit);
    inp.addEventListener('keydown', e => { if (e.key === 'Enter') submit(); });
    back.addEventListener('click', showLogin);
    let sure = false;
    forgot.addEventListener('click', () => {
      if (!sure) { sure = true; forgot.textContent = 'Apagar o perfil de ' + p.name + ' e todos os dados dele? Toque de novo para confirmar'; return; }
      keysOf(p.id).forEach(k => { try { localStorage.removeItem(k); } catch {} });
      profiles = profiles.filter(x => x.id !== p.id); Store.set(KP, profiles); showLogin();
    });
    card.append(el('div', 'brand', 'MENINAS DA FÊNIX 🔥'), el('h2', null, 'Oi, ' + p.name + ' 💛'), el('div', 'note', 'Digite seu PIN de 4 números.'), inp, msg, go, back, forgot);
    inp.focus();
  }

  function showCreate() {
    const first = !profiles.length;
    const card = $('loginCard'); card.replaceChildren();
    const name = el('input'); name.type = 'text'; name.maxLength = 20; name.placeholder = 'Seu nome'; name.setAttribute('aria-label', 'Seu nome');
    const p1 = pinInput('Crie um PIN de 4 números'), p2 = pinInput('Repita o PIN');
    p1.placeholder = 'Crie um PIN de 4 números'; p2.placeholder = 'Repita o PIN';
    const msg = el('div', 'note'), go = el('button', 'btn attend', 'Criar meu perfil'), back = el('button', 'undo', '← Voltar');
    msg.setAttribute('role', 'alert'); go.type = back.type = 'button';
    go.addEventListener('click', async () => {
      const n = name.value.trim();
      if (!n) { msg.textContent = 'Escreva seu nome.'; name.focus(); return; }
      if (profiles.some(x => x.name.toLowerCase() === n.toLowerCase())) { msg.textContent = 'Já existe alguém com esse nome.'; return; }
      if (p1.value.length !== 4) { msg.textContent = 'O PIN precisa ter 4 números.'; return; }
      if (p1.value !== p2.value) { msg.textContent = 'Os dois PINs não são iguais.'; return; }
      const salt = newSalt(), p = { id: Date.now().toString(36), name: n, salt, hash: await hashPin(salt, p1.value) };
      if (first) keysOf(p.id).forEach((k, i) => { try { const v = localStorage.getItem(LEGACY[i]); if (v) localStorage.setItem(k, v); } catch {} });
      profiles.push(p); Store.set(KP, profiles); enter(p);
    });
    back.addEventListener('click', showLogin);
    card.append(el('div', 'brand', 'MENINAS DA FÊNIX 🔥'), el('h2', null, first ? 'Vamos começar!' : 'Nova menina'), name, p1, p2, msg, go);
    if (!first) card.append(back);
    if (first) card.append(el('div', 'note', 'Se você já usava o site, seus registros anteriores ficam neste primeiro perfil.'));
    card.append(el('div', 'note', 'O PIN só protege a tela neste aparelho. Não é uma segurança forte.'));
    name.focus();
  }

  function showLogin() {
    $('login').hidden = false; $('app').hidden = true;
    if (!profiles.length) return showCreate();
    const card = $('loginCard'); card.replaceChildren();
    const list = el('div', 'list');
    profiles.forEach(p => {
      const b = el('button', 'item history-item'); b.type = 'button';
      b.append(el('span', 'name', p.name), el('span', 't', 'Entrar →'));
      b.addEventListener('click', () => showPin(p));
      list.appendChild(b);
    });
    card.append(el('div', 'brand', 'MENINAS DA FÊNIX 🔥'), el('h2', null, 'Quem é você?'), list);
    if (profiles.length < MAXP) { const n = el('button', 'undo', '+ Nova menina'); n.type = 'button'; n.addEventListener('click', showCreate); card.append(n); }
  }

  $('logout').addEventListener('click', () => { try { sessionStorage.removeItem(KS); } catch {} showLogin(); });

  let sid = null; try { sid = sessionStorage.getItem(KS); } catch {}
  const auto = profiles.find(p => p.id === sid);
  if (auto) enter(auto); else showLogin();
})();
</script>
</body>
</html>
