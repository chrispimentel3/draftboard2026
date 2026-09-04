<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Mega Bowl 2026 — Draft Board</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@500;600;800&family=Archivo+Narrow:wght@400;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --felt:#1E2C28;
    --felt-dark:#16211E;
    --felt-line:#2E423C;
    --card:#E8E3D5;
    --card-dim:#C9C4B7;
    --ink:#1A211F;
    --ink-soft:#5E6763;
    --bone:#EDE9DE;
    --mine:#E4B34A;

    --qb:#C2452D;
    --rb:#3B6EA5;
    --wr:#D08A1E;
    --te:#7A4E9E;
    --k:#6B7B8C;
    --def:#4C6B3C;
  }

  *{box-sizing:border-box;}

  html,body{margin:0;padding:0;}

  body{
    background:var(--felt);
    color:var(--bone);
    font-family:'Archivo',-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;
    -webkit-font-smoothing:antialiased;
  }

  /* ---------- masthead ---------- */
  header.top{
    padding:34px 28px 20px;
    border-bottom:1px solid var(--felt-line);
  }

  .title{
    font-size:clamp(30px,5vw,52px);
    font-weight:800;
    letter-spacing:-0.025em;
    line-height:0.94;
    margin:0;
  }
  .title span{color:var(--mine);}

  .subtitle{
    margin:10px 0 0;
    font-family:'Archivo Narrow',sans-serif;
    font-size:15px;
    color:#9AA5A0;
    max-width:62ch;
    line-height:1.5;
  }

  /* ---------- controls ---------- */
  .controls{
    display:flex;
    flex-wrap:wrap;
    gap:22px;
    align-items:center;
    padding:16px 28px;
    border-bottom:1px solid var(--felt-line);
    background:var(--felt-dark);
    position:relative;
    z-index:6;
  }

  .chipset{display:flex;gap:6px;flex-wrap:wrap;align-items:center;}

  .chipset-label{
    font-family:'Archivo Narrow',sans-serif;
    font-size:13px;
    color:#8B9691;
    margin-right:4px;
  }

  .chip{
    font-family:'Archivo Narrow',sans-serif;
    font-weight:700;
    font-size:13px;
    letter-spacing:0.02em;
    padding:5px 11px;
    border-radius:2px;
    border:1px solid transparent;
    background:#25332F;
    color:#B9C2BE;
    cursor:pointer;
    transition:background .12s,color .12s,border-color .12s;
  }
  .chip:hover{background:#2E3E39;}
  .chip:focus-visible{outline:2px solid var(--mine);outline-offset:2px;}
  .chip[aria-pressed="true"]{color:#12100C;}
  .chip[data-pos="QB"][aria-pressed="true"]{background:var(--qb);color:#fff;}
  .chip[data-pos="RB"][aria-pressed="true"]{background:var(--rb);color:#fff;}
  .chip[data-pos="WR"][aria-pressed="true"]{background:var(--wr);}
  .chip[data-pos="TE"][aria-pressed="true"]{background:var(--te);color:#fff;}
  .chip[data-pos="K"][aria-pressed="true"]{background:var(--k);color:#fff;}
  .chip[data-pos="DEF"][aria-pressed="true"]{background:var(--def);color:#fff;}
  .chip.reset[aria-pressed="true"]{background:var(--bone);color:var(--ink);}

  .search{
    font-family:'Archivo Narrow',sans-serif;
    font-size:14px;
    padding:6px 10px;
    width:190px;
    background:#25332F;
    border:1px solid var(--felt-line);
    border-radius:2px;
    color:var(--bone);
  }
  .search::placeholder{color:#79837F;}
  .search:focus{outline:2px solid var(--mine);outline-offset:1px;}

  .hint{
    font-family:'Archivo Narrow',sans-serif;
    font-size:13px;
    color:#79837F;
    margin-left:auto;
  }

  /* ---------- board ---------- */
  .scroller{
    overflow:auto;
    max-height:calc(100vh - 60px);
    padding-bottom:40px;
  }

  .board{
    display:grid;
    grid-template-columns:52px repeat(12,158px);
    min-width:max-content;
  }

  /* header cells */
  .th{
    position:sticky;
    top:0;
    z-index:4;
    background:var(--felt-dark);
    border-right:1px solid var(--felt-line);
    border-bottom:2px solid var(--felt-line);
    padding:10px 10px 8px;
    cursor:pointer;
    user-select:none;
  }
  .th.corner{
    left:0;
    z-index:5;
    cursor:default;
    border-right:1px solid var(--felt-line);
  }
  .th .seat{
    font-family:'Archivo Narrow',sans-serif;
    font-size:11px;
    color:#79837F;
    letter-spacing:0.06em;
  }
  .th .team{
    font-family:'Archivo Narrow',sans-serif;
    font-weight:700;
    font-size:14.5px;
    line-height:1.15;
    margin-top:2px;
    color:var(--bone);
    white-space:nowrap;
    overflow:hidden;
    text-overflow:ellipsis;
  }
  .th .tally{
    display:flex;
    gap:3px;
    margin-top:7px;
    flex-wrap:wrap;
  }
  .tally b{
    font-family:'Archivo Narrow',sans-serif;
    font-size:10.5px;
    font-weight:700;
    padding:1px 4px;
    border-radius:2px;
    color:#12100C;
  }
  .th.mine{background:#2B2A1C;border-bottom-color:var(--mine);}
  .th.mine .team{color:var(--mine);}
  .th.spot{background:#33443E;}

  /* round rail */
  .rd{
    position:sticky;
    left:0;
    z-index:3;
    background:var(--felt-dark);
    border-right:1px solid var(--felt-line);
    border-bottom:1px solid var(--felt-line);
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    gap:2px;
    padding:4px 0;
  }
  .rd .n{
    font-weight:800;
    font-size:17px;
    color:#8B9691;
    line-height:1;
  }
  .rd .dir{
    font-size:11px;
    color:#4E5B57;
    line-height:1;
  }

  /* pick cards */
  .cell{
    border-right:1px solid var(--felt-line);
    border-bottom:1px solid var(--felt-line);
    padding:6px;
  }
  .cell.mine{background:rgba(228,179,74,0.07);}
  .cell.spot{background:rgba(255,255,255,0.05);}

  .pick{
    position:relative;
    height:100%;
    background:var(--card);
    border-left:5px solid var(--card-dim);
    border-radius:2px;
    padding:6px 8px 7px 8px;
    color:var(--ink);
    transition:opacity .12s, filter .12s;
  }
  .pick .no{
    font-family:'Archivo Narrow',sans-serif;
    font-size:10.5px;
    font-weight:700;
    color:#8C8878;
    letter-spacing:0.04em;
  }
  .pick .name{
    font-family:'Archivo Narrow',sans-serif;
    font-weight:700;
    font-size:15px;
    line-height:1.12;
    margin:1px 0 4px;
    letter-spacing:-0.005em;
  }
  .pick .meta{
    display:flex;
    align-items:center;
    gap:5px;
  }
  .pos{
    font-family:'Archivo Narrow',sans-serif;
    font-size:10.5px;
    font-weight:700;
    padding:1px 5px;
    border-radius:2px;
    color:#fff;
  }
  .nfl{
    font-family:'Archivo Narrow',sans-serif;
    font-size:11.5px;
    color:#7C7869;
    font-weight:600;
    letter-spacing:0.05em;
  }

  .pos.QB,.tally b.QB{background:var(--qb);color:#fff;}
  .pos.RB,.tally b.RB{background:var(--rb);color:#fff;}
  .pos.WR,.tally b.WR{background:var(--wr);color:#241a06;}
  .pos.TE,.tally b.TE{background:var(--te);color:#fff;}
  .pos.K,.tally b.K{background:var(--k);color:#fff;}
  .pos.DEF,.tally b.DEF{background:var(--def);color:#fff;}

  .pick.QB{border-left-color:var(--qb);}
  .pick.RB{border-left-color:var(--rb);}
  .pick.WR{border-left-color:var(--wr);}
  .pick.TE{border-left-color:var(--te);}
  .pick.K{border-left-color:var(--k);}
  .pick.DEF{border-left-color:var(--def);}

  .pick.muted{opacity:0.2;filter:saturate(0.2);}

  .pick.mine{
    box-shadow:0 0 0 2px var(--mine);
  }

  /* footer legend */
  footer{
    padding:22px 28px 60px;
    border-top:1px solid var(--felt-line);
    font-family:'Archivo Narrow',sans-serif;
    font-size:13.5px;
    color:#8B9691;
    line-height:1.6;
  }
  footer strong{color:var(--bone);font-weight:700;}

  @media (prefers-reduced-motion:reduce){
    *{transition:none !important;}
  }
</style>
</head>
<body>

<header class="top">
  <h1 class="title">Mega Bowl 2026 <span>Draft Board</span></h1>
  <p class="subtitle">12 teams, 15 rounds, snake order. Half-PPR. Seat 8 is yours — highlighted in gold. Click any team name to isolate that roster, or filter by position to see how runs moved through the room.</p>
</header>

<div class="controls">
  <div class="chipset">
    <span class="chipset-label">Position</span>
    <button class="chip reset" data-pos="ALL" aria-pressed="true">All</button>
    <button class="chip" data-pos="QB" aria-pressed="false">QB</button>
    <button class="chip" data-pos="RB" aria-pressed="false">RB</button>
    <button class="chip" data-pos="WR" aria-pressed="false">WR</button>
    <button class="chip" data-pos="TE" aria-pressed="false">TE</button>
    <button class="chip" data-pos="K" aria-pressed="false">K</button>
    <button class="chip" data-pos="DEF" aria-pressed="false">DEF</button>
  </div>
  <input class="search" id="search" type="search" placeholder="Find a player…" aria-label="Find a player">
  <span class="hint" id="hint">180 picks</span>
</div>

<div class="scroller">
  <div class="board" id="board"></div>
</div>

<footer>
  <strong>Reading the board.</strong> Rounds run left-to-right on odd rounds and right-to-left on even rounds — the arrow in the round rail shows the direction. The number in each card's corner is the overall pick. Colored tape on the left edge of each card marks the position, and the tally under each team name is that roster's final positional shape.
</footer>

<script>
const TEAMS = ["deez nuts","TyRick Hill","Undisputed","Any Given Ya…","L'Omar","I Chase Brow…","Bed Bath & B…","TaylorMade","JAH PLS","Play Action …","BillsMafia","Crabcakes an…"];
const MY_SEAT = 8;

// Each round listed in DRAFT ORDER (pick 1..12 of that round).
const ROUNDS = [
["Jahmyr Gibbs|RB|DET","Ja'Marr Chase|WR|CIN","Bijan Robinson|RB|ATL","Jonathan Taylor|RB|IND","Puka Nacua|WR|LAR","Saquon Barkley|RB|PHI","Amon-Ra St. Brown|WR|DET","Christian McCaffrey|RB|SF","James Cook III|RB|BUF","Jaxon Smith-Njigba|WR|SEA","Kenneth Walker III|RB|KC","CeeDee Lamb|WR|DAL"],
["Chase Brown|RB|CIN","Justin Jefferson|WR|MIN","Derrick Henry|RB|BAL","De'Von Achane|RB|MIA","Malik Nabers|WR|NYG","Omarion Hampton|RB|LAC","A.J. Brown|WR|NE","Lamar Jackson|QB|BAL","Nico Collins|WR|HOU","Kyren Williams|RB|LAR","Javonte Williams|RB|DAL","Drake London|WR|ATL"],
["George Pickens|WR|DAL","Travis Etienne Jr.|RB|NO","Brock Bowers|TE|LV","Ashton Jeanty|RB|LV","Zay Flowers|WR|BAL","Josh Allen|QB|BUF","Rashee Rice|WR|KC","Breece Hall|RB|NYJ","DeVonta Smith|WR|PHI","Jeremiyah Love|RB|ARI","Chris Olave|WR|NO","Trey McBride|TE|ARI"],
["Tee Higgins|WR|CIN","D'Andre Swift|RB|CHI","Ladd McConkey|WR|LAC","Jaylen Waddle|WR|DEN","Emeka Egbuka|WR|TB","Garrett Wilson|WR|NYJ","Cam Skattebo|RB|NYG","Bucky Irving|RB|TB","Tetairoa McMillan|WR|CAR","David Montgomery|RB|HOU","Colston Loveland|TE|CHI","Bhayshul Tuten|RB|JAX"],
["Luther Burden III|WR|CHI","Joe Burrow|QB|CIN","Terry McLaurin|WR|WAS","Sam LaPorta|TE|DET","Quinshon Judkins|RB|CLE","DJ Moore|WR|BUF","Tyler Warren|TE|IND","Jadarian Price|RB|SEA","Rome Odunze|WR|CHI","Jalen Hurts|QB|PHI","Jameson Williams|WR|DET","Mike Evans|WR|SF"],
["Rhamondre Stevenson|RB|NE","Davante Adams|WR|LAR","Tucker Kraft|TE|GB","Parker Washington|WR|JAX","Christian Watson|WR|GB","Justin Herbert|QB|LAC","Marvin Harrison Jr.|WR|ARI","Carnell Tate|WR|TEN","Drake Maye|QB|NE","Caleb Williams|QB|CHI","TreVeyon Henderson|RB|NE","Josh Downs|WR|IND"],
["MarShawn Lloyd|RB|GB","DK Metcalf|WR|PIT","Quentin Johnston|WR|LAC","Stefon Diggs|WR|WAS","Jaylen Warren|RB|PIT","Tony Pollard|RB|TEN","Jonathon Brooks|RB|CAR","Jayden Daniels|QB|WAS","Dak Prescott|QB|DAL","Rico Dowdle|RB|PIT","Harold Fannin Jr.|TE|CLE","Trevor Lawrence|QB|JAX"],
["Blake Corum|RB|LAR","Brock Purdy|QB|SF","Chuba Hubbard|RB|CAR","RJ Harvey|RB|DEN","J.K. Dobbins|RB|DEN","Chris Godwin Jr.|WR|TB","Jordan Mason|RB|MIN","Brian Thomas Jr.|WR|JAX","KC Concepcion|WR|CLE","Courtland Sutton|WR|DEN","Jordan Addison|WR|MIN","Dalton Kincaid|TE|BUF"],
["Michael Pittman Jr.|WR|PIT","Makai Lemon|WR|PHI","Jacory Croskey-Merritt|RB|WAS","Kenny Gainwell|RB|TB","Kyle Pitts Sr.|TE|ATL","Alec Pierce|WR|IND","Aaron Jones Sr.|RB|MIN","George Kittle|TE|SF","Josh Jacobs|RB|GB","Isaiah Likely|TE|NYG","Michael Wilson|WR|ARI","Jayden Reed|WR|GB"],
["Kyle Monangai|RB|CHI","Bo Nix|QB|DEN","Wan'Dale Robinson|WR|TEN","Matthew Golden|WR|GB","Travis Kelce|TE|KC","Brandon Aubrey|K|DAL","Rams|DEF|LAR","De'Zhaun Stribling|WR|SF","Mike Washington Jr.|RB|LV","Woody Marks|RB|HOU","Cameron Dicker|K|LAC","Isiah Pacheco|RB|DET"],
["Jalen Coker|WR|CAR","Matthew Stafford|QB|LAR","Xavier Worthy|WR|KC","Romeo Doubs|WR|NE","Texans|DEF|HOU","Tyjae Spears|RB|TEN","Rachaad White|RB|WAS","Kyler Murray|QB|MIN","Mark Andrews|TE|BAL","Ka'imi Fairbairn|K|HOU","Chris Rodriguez Jr.|RB|JAX","Patrick Mahomes|QB|KC"],
["Broncos|DEF|DEN","Dallas Goedert|TE|PHI","Deebo Samuel Sr.|WR|SF","Khalil Shakir|WR|BUF","Keaton Mitchell|RB|LAC","Tyler Allgeier|RB|ARI","Jake Ferguson|TE|DAL","Brian Robinson|RB|ATL","Seahawks|DEF|SEA","Jaxson Dart|QB|NYG","Alvin Kamara|RB|NO","Jared Goff|QB|DET"],
["Jordyn Tyson|WR|NO","Najee Harris|RB|NYG","Eagles|DEF|PHI","Jason Myers|K|SEA","Jakobi Meyers|WR|JAX","Tank Bigsby|RB|PHI","Jonah Coleman|RB|DEN","Ravens|DEF|BAL","Oronde Gadsden|TE|LAC","Kayshon Boutte|WR|HOU","Jordan Love|QB|GB","Cam Little|K|JAX"],
["Zach Charbonnet|RB|SEA","Vikings|DEF|MIN","Cooper Kupp|WR|SEA","Patriots|DEF|NE","Tyler Loop|K|BAL","Juwan Johnson|TE|NO","Rashid Shaheed|WR|SEA","Harrison Mevis|K|LAR","Tyler Shough|QB|NO","Will Reichard|K|MIN","Kaelon Black|RB|SF","Jake Bates|K|DET"],
["Justice Hill|RB|BAL","Steelers|DEF|PIT","James Conner|RB|ARI","Hunter Henry|TE|NE","Ja'Kobi Lane|WR|BAL","Ray Davis|RB|BUF","Jalen McMillan|WR|TB","Denzel Boston|WR|CLE","Andy Borregales|K|NE","49ers|DEF|SF","Cairo Santos|K|CHI","Tre Tucker|WR|LV"]
];

// Build seat-indexed grid: grid[round][seat] = {name,pos,team,overall}
const grid = [];
const rosters = Array.from({length:12},()=>[]);

ROUNDS.forEach((round, r) => {
  const row = new Array(12);
  round.forEach((raw, i) => {
    const [name,pos,team] = raw.split("|");
    const seat = (r % 2 === 0) ? (i + 1) : (12 - i);
    const overall = r * 12 + i + 1;
    const pick = {name,pos,team,overall,round:r+1,seat};
    row[seat-1] = pick;
    rosters[seat-1].push(pick);
  });
  grid.push(row);
});

const ORDER = ["QB","RB","WR","TE","K","DEF"];
const board = document.getElementById("board");
const frag = document.createDocumentFragment();

// header row
const corner = document.createElement("div");
corner.className = "th corner";
corner.innerHTML = '<div class="seat">RD</div>';
frag.appendChild(corner);

TEAMS.forEach((t, i) => {
  const seat = i + 1;
  const counts = {};
  rosters[i].forEach(p => counts[p.pos] = (counts[p.pos]||0)+1);
  const tally = ORDER.filter(p => counts[p])
    .map(p => `<b class="${p}">${p} ${counts[p]}</b>`).join("");

  const th = document.createElement("div");
  th.className = "th" + (seat === MY_SEAT ? " mine" : "");
  th.dataset.seat = seat;
  th.setAttribute("role","button");
  th.setAttribute("tabindex","0");
  th.innerHTML = `<div class="seat">SEAT ${seat}</div>
    <div class="team">${t}</div>
    <div class="tally">${tally}</div>`;
  frag.appendChild(th);
});

// body rows
grid.forEach((row, r) => {
  const rail = document.createElement("div");
  rail.className = "rd";
  rail.innerHTML = `<div class="n">${r+1}</div><div class="dir">${r % 2 === 0 ? "→" : "←"}</div>`;
  frag.appendChild(rail);

  row.forEach(p => {
    const cell = document.createElement("div");
    cell.className = "cell" + (p.seat === MY_SEAT ? " mine" : "");
    cell.dataset.seat = p.seat;
    cell.innerHTML = `<div class="pick ${p.pos}${p.seat === MY_SEAT ? " mine" : ""}"
        data-pos="${p.pos}" data-name="${p.name.toLowerCase()}">
      <div class="no">${p.overall}</div>
      <div class="name">${p.name}</div>
      <div class="meta"><span class="pos ${p.pos}">${p.pos}</span><span class="nfl">${p.team}</span></div>
    </div>`;
    frag.appendChild(cell);
  });
});

board.appendChild(frag);

// ---- filtering ----
let posFilter = "ALL";
let query = "";
let spotlight = null;

const picks = [...board.querySelectorAll(".pick")];
const hint = document.getElementById("hint");

function apply(){
  let shown = 0;
  picks.forEach(el => {
    const okPos = posFilter === "ALL" || el.dataset.pos === posFilter;
    const okQ = !query || el.dataset.name.includes(query);
    const on = okPos && okQ;
    el.classList.toggle("muted", !on);
    if(on) shown++;
  });

  board.querySelectorAll(".cell").forEach(c =>
    c.classList.toggle("spot", spotlight !== null && +c.dataset.seat === spotlight));
  board.querySelectorAll(".th[data-seat]").forEach(t =>
    t.classList.toggle("spot", spotlight !== null && +t.dataset.seat === spotlight));

  const bits = [];
  bits.push(`${shown} of 180 picks`);
  if(spotlight) bits.push(TEAMS[spotlight-1]);
  hint.textContent = bits.join(" · ");
}

document.querySelectorAll(".chip").forEach(chip => {
  chip.addEventListener("click", () => {
    posFilter = chip.dataset.pos;
    document.querySelectorAll(".chip").forEach(c =>
      c.setAttribute("aria-pressed", String(c === chip)));
    apply();
  });
});

document.getElementById("search").addEventListener("input", e => {
  query = e.target.value.trim().toLowerCase();
  apply();
});

function toggleSeat(seat){
  spotlight = (spotlight === seat) ? null : seat;
  apply();
}
board.querySelectorAll(".th[data-seat]").forEach(th => {
  th.addEventListener("click", () => toggleSeat(+th.dataset.seat));
  th.addEventListener("keydown", e => {
    if(e.key === "Enter" || e.key === " "){ e.preventDefault(); toggleSeat(+th.dataset.seat); }
  });
});

apply();
</script>
</body>
</html>
