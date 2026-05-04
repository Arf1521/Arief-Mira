<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Form Laporan Gejala / Kegagalan</title>
  <style>
    :root{--bg:#f4f6f8;--card:#fff;--text:#111827;--muted:#6b7280;--line:#e5e7eb;--pri:#2563eb}
    *{box-sizing:border-box}
    body{font-family:Arial,Helvetica,sans-serif;background:var(--bg);margin:0;color:var(--text)}
    header{background:var(--pri);color:#fff;padding:16px;font-size:20px;font-weight:800}
    .wrap{max-width:1100px;margin:16px auto;padding:0 14px}
    .card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:18px;margin-bottom:16px}
    .nav{display:flex;gap:10px;flex-wrap:wrap}
    .nav button{background:var(--pri);color:#fff;border:0;border-radius:12px;padding:10px 16px;font-weight:800;cursor:pointer}
    .nav button.active{background:#1e40af}
    label{font-weight:700;font-size:13px;margin-top:12px;display:block}
    input,select,textarea{width:100%;padding:12px;margin-top:6px;border:1px solid #d1d5db;border-radius:12px;background:#fff;font-size:14px}
    textarea{min-height:130px;resize:vertical}
    .btn{background:var(--pri);color:#fff;border:0;border-radius:12px;padding:12px 18px;font-weight:800;cursor:pointer}
    .btn:disabled{opacity:.55;cursor:not-allowed}
    .row{display:grid;grid-template-columns:1fr 1fr;gap:14px}
    @media(max-width:860px){.row{grid-template-columns:1fr}}
    .hide{display:none}
    table{width:100%;border-collapse:collapse}
    th,td{border:1px solid var(--line);padding:10px;font-size:13px;vertical-align:top}
    th{background:#f8fafc;text-align:left}
    .filters{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}
    @media(max-width:920px){.filters{grid-template-columns:1fr 1fr}}
    @media(max-width:620px){.filters{grid-template-columns:1fr}}
    .hint{color:var(--muted);font-size:13px;line-height:1.5;margin-top:8px}
    .pill{display:inline-block;padding:5px 12px;border-radius:999px;font-size:12px;font-weight:700;background:#eef2ff;color:#1e40af}
    .err{background:#fee2e2;color:#991b1b}
    .ok{background:#dcfce7;color:#166534}
    .thumbWrap{position:relative;display:inline-block}
    .thumb{width:58px;height:58px;object-fit:cover;border-radius:10px;border:1px solid var(--line)}
    .preview{position:absolute;left:66px;top:-4px;z-index:20;display:none;padding:8px;background:#fff;border:1px solid var(--line);border-radius:12px;box-shadow:0 15px 30px rgba(0,0,0,.18);max-width:420px}
    .preview img{max-width:400px;max-height:320px;border-radius:10px;display:block}
    .thumbWrap:hover .preview{display:block}
  </style>
</head>
<body>
<header>🛠️ Form Laporan Gejala / Kegagalan Area Kerja</header>
<div class="wrap">
  <div class="card nav">
    <button id="tabEntry" class="active" onclick="showTab('entry')">Form Lapor</button>
    <button id="tabHistory" onclick="showTab('history')">History</button>
    <button id="tabLeader" onclick="showTab('leader')">Leaderboard</button>
  </div>

  <div id="page-entry" class="card">
    <h3 style="margin:0 0 10px 0;">Isi Form Laporan</h3>
    <div class="row">
      <div>
        <label>Judul Laporan</label>
        <input id="judul" value="Kartu Lapor Temuan" />
      </div>
      <div>
        <label>Tanggal Temuan</label>
        <input type="date" id="tanggal" />
      </div>
    </div>

    <div class="row">
      <div>
        <label>Nama Pemeriksa</label>
        <select id="nama"><option value="">-- pilih --</option></select>
      </div>
      <div>
        <label>Area Kerja</label>
        <select id="area"><option value="">-- pilih --</option></select>
      </div>
    </div>

    <label>Keterangan Temuan Kondisi</label>
    <textarea id="ket" placeholder="Tuliskan keterangan temun kondisi, apa yang terjadi, dampak dan usulan rekomendasi."></textarea>

    <div class="row">
      <div>
        <label>Lampiran File</label>
        <input type="file" id="lampiran" />
      </div>
      <div>
        <label>Link Referensi (opsional)</label>
        <input type="url" id="link" placeholder="https://" />
      </div>
    </div>

    <div style="margin-top:16px;display:flex;gap:12px;flex-wrap:wrap;">
      <button class="btn" id="btnSimpan" onclick="simpan()">Kirim Laporan</button>
      <button class="btn" style="background:#374151" onclick="resetEntry()">Reset</button>
    </div>
    <div id="msgEntry" class="hint"></div>
  </div>

  <div id="page-history" class="card hide">
    <h3 style="margin:0 0 10px 0;">Riwayat Laporan</h3>
    <div class="filters">
      <div><label>Dari Tanggal</label><input type="date" id="fStart" oninput="loadHistory()" /></div>
      <div><label>Sampai Tanggal</label><input type="date" id="fEnd" oninput="loadHistory()" /></div>
      <div><label>Nama Pemeriksa</label><select id="fNama"><option value="">Semua</option></select></div>
      <div><label>Area Kerja</label><select id="fArea"><option value="">Semua</option></select></div>
      <div style="grid-column:1/-1"><label>Cari Keterangan</label><input id="fKey" placeholder="Kata kunci..." oninput="loadHistory()" /></div>
    </div>

    <div id="msgHistory" class="hint"></div>
    <div style="overflow:auto;margin-top:14px">
      <table>
        <thead>
          <tr>
            <th>Tanggal</th>
            <th>Nama</th>
            <th>Area</th>
            <th>Deskripsi</th>
            <th>Lampiran</th>
            <th>Link</th>
          </tr>
        </thead>
        <tbody id="hist"></tbody>
      </table>
    </div>
  </div>

  <div id="page-leader" class="card hide">
    <h3 style="margin:0 0 10px 0;">Leaderboard Pemeriksa</h3>
    <div class="row">
      <div><label>Periode Dari</label><input type="month" id="lbStart" onchange="renderLeaderboard()" /></div>
      <div><label>Periode Sampai</label><input type="month" id="lbEnd" onchange="renderLeaderboard()" /></div>
    </div>
    <div id="msgLeader" class="hint"></div>
    <div id="lb" style="margin-top:14px"></div>
  </div>
</div>

<script>
const NAMA_RAW = [
  "Japir","Ahmad Fauzi","Eko Sasmito Sakti","Haeruddin","Lukman Hakim","Sahrul",
  "Syaharudin","Yunus","Suji","Semion","Muhammad Bahroni","Ray Padli",
  "Tommy Herru Mardiyanto","Sulyanto Sarira","Rudiantoro","Iing Solikhin",
  "Novianto Adityas","Destriawan F.S.","Muahmmad. Rofiul","Supriyadi","Riswandi",
  "Nur Rakhmad","Jupri Ependi","Suwistiono","Didik Ratmawan","Muchamad Nurochis",
  "Sugiatno","Panji Ronggo Winarso","Ronik","Donal Victor Mokolinug",
  "Andri Hermansyah","Jumardi","Dendi Kristiano","Hendrik","Teguh Santoso",
  "Edi Harianto","Sandra","Indra Utomo","Imam Syafi'i","Deni Julianto","Taslim",
  "M. Hidayat","Galang Rambo A","Fadli Rasyid","Moh. Bulgis S","M. Syaifur R",
  "Dwi Janwar","Haekal","Aprian Hidayat","Irpansyah","Wahyu Sumantri",
  "Dani Halil","Muliadi","Usman Amin","Supriadi","M Adi Rahman",
  "Arnold Andriadi","Budiyanto","Donal Victor M","Amri Sattari","Baharudin",
  "Wawan Darmawan","Abd Rahim","Abdul Rahim","Apip","Suhadi Rahman",
  "Dedy Budiantoro","Suharman","Udin Setiawan","Jamal Syam S","Mursal",
  "Samuel Lesing","Juliansyah","Laode M. Iqbal","Arief Sutarno","Sri Priyaten",
  "Van Heru Sinaga","Roland"
];
const AREA_RAW = [
  "Hopper 1","Feeder1","Crusher 1","Stacking 1","Hopper 2","Feeder 2","Crusher 2",
  "Stacking 2","Hopper 3","Feeder 3","Crusher 3","Hopper 4","Feeder4","Crusher 4",
  "Prima Stacking","Hopper 5","Feeder 5","Breaker 5","Gundlach Crusher",
  "Transfer conv 1","Transfer conv 2","Clean Coal Stacking 5","Wash feed conv.",
  "Washing Plant","Tunnel 1","Fan Tunnel 1","Tunnel pump","Boom Stacking",
  "Rake Bar","Long Travel","Scrapper Chain","Stockpile 1 Feeder 4",
  "Stockpile 1 Feeder 3","Stockpile 2 Feeder 2","Stockpile 2 Feeder 1",
  "Stockpile 3 Emergency Feeder 1","Stockpile 3","Stockpile 3 Emergency Feeder 2",
  "PRC","Surge Bin 150","CV04","Surge Bin 250","CV07","CV08","Hopper 6",
  "Feeder 6","Breaker 6","CV01","CV02","Tripper","Dribble Chute","CV03",
  "Tunnel 2","Fan Tunnel 2","Stockpile 4","Stockpile 5","Stockpile 6",
  "Stockpile 7","Stockpile 8","Washdown Pump","Hopper 7","Feeder 7","CV111",
  "Sizer 7","CV112","Breaker 7","Hopper 8","Feeder 8","Breaker 8","CV113",
  "Sizer 8","CV114","CV115","CV101","Spilliage Conveyor","Surge Bin 400",
  "CV116","Sediment Pump","Fire Pump 1","Fire Pump 2","Dust Suppression Pump 1",
  "Dust Suppression Pump 2","Dust Suppression Pump 3","Chemical Dust Suppression Pump"
];

function uniqueSorted(arr){
  return Array.from(new Set(arr.map(x=>String(x||"").trim()).filter(Boolean)))
    .sort((a,b)=>a.localeCompare(b,'id',{sensitivity:'base'}));
}
const NAMA = uniqueSorted(NAMA_RAW);
const AREA = uniqueSorted(AREA_RAW);

function fillSelect(selectEl, values, includeAll=false){
  selectEl.innerHTML = includeAll ? '<option value="">Semua</option>' : '<option value="">-- pilih --</option>';
  values.forEach(v=>selectEl.add(new Option(v,v)));
}

function showTab(tab){
  const tabs = [
    ['entry','tabEntry','page-entry'],
    ['history','tabHistory','page-history'],
    ['leader','tabLeader','page-leader']
  ];
  tabs.forEach(([key,btnId,pageId])=>{
    document.getElementById(pageId).classList.toggle('hide', key!==tab);
    document.getElementById(btnId).classList.toggle('active', key===tab);
  });
  if(tab==='history') loadHistory();
  if(tab==='leader') renderLeaderboard();
}

const SP = {
  listTitle: 'Form Galeri',
  titleValue: 'Kartu Lapor Temuan',
  libraryFolderServerRelative: '/sites/TeamProject/Dokumen Berbagi/Kartu Lapor Temuan'
};
let spFieldMap = null;
let spListItemType = null;
let spCacheItems = [];

async function spFetch(url, opts={}){
  const res = await fetch(url, { ...opts, headers: { Accept:'application/json;odata=verbose', ...(opts.headers||{}) } });
  const json = await res.json();
  if(!res.ok){
    const msg = json?.error?.message?.value || `HTTP ${res.status}`;
    throw new Error(msg);
  }
  return json;
}

async function getRequestDigest(){
  const j = await spFetch(`${_spPageContextInfo.webAbsoluteUrl}/_api/contextinfo`, { method:'POST' });
  return j.d.GetContextWebInformation.FormDigestValue;
}

async function ensureFieldMap(){
  if(spFieldMap) return spFieldMap;
  const listUrl = `${_spPageContextInfo.webAbsoluteUrl}/_api/web/lists/GetByTitle('${encodeURIComponent(SP.listTitle)}')`;
  const fieldsJson = await spFetch(`${listUrl}/fields?$select=Title,InternalName`);
  const map = {};
  fieldsJson.d.results.forEach(f=> map[f.Title] = f.InternalName);
  spFieldMap = map;
  const listJson = await spFetch(`${listUrl}?$select=ListItemEntityTypeFullName`);
  spListItemType = listJson.d.ListItemEntityTypeFullName;
  return spFieldMap;
}

function isImageFileName(name){
  return /\.(png|jpg|jpeg|gif|webp|bmp)$/i.test(name||'');
}

async function uploadToLibrary(file, digest){
  const fileName = file.name.replace(/'/g,'');
  const uploadUrl = `${_spPageContextInfo.webAbsoluteUrl}/_api/web/GetFolderByServerRelativeUrl('${encodeURIComponent(SP.libraryFolderServerRelative)}')/Files/add(url='${encodeURIComponent(fileName)}',overwrite=true)`;
  const buf = await file.arrayBuffer();
  const j = await spFetch(uploadUrl, { method:'POST', headers:{ 'X-RequestDigest': digest, 'Content-Type':'application/octet-stream' }, body: buf });
  const absUrl = location.origin + j.d.ServerRelativeUrl;
  return { absUrl, serverRelUrl: j.d.ServerRelativeUrl, fileName, buffer: buf };
}

async function createListItem(fields, digest){
  const url = `${_spPageContextInfo.webAbsoluteUrl}/_api/web/lists/GetByTitle('${encodeURIComponent(SP.listTitle)}')/items`;
  const body = { __metadata:{ type: spListItemType }, ...fields };
  const j = await spFetch(url, { method:'POST', headers:{ 'X-RequestDigest': digest, 'Content-Type':'application/json;odata=verbose' }, body: JSON.stringify(body) });
  return j.d;
}

async function addListAttachment(itemId, fileName, fileBuffer, digest){
  const url = `${_spPageContextInfo.webAbsoluteUrl}/_api/web/lists/GetByTitle('${encodeURIComponent(SP.listTitle)}')/items(${itemId})/AttachmentFiles/add(FileName='${encodeURIComponent(fileName.replace(/'/g,''))}')`;
  await spFetch(url, { method:'POST', headers:{ 'X-RequestDigest': digest, 'Content-Type':'application/octet-stream' }, body: fileBuffer });
}

function setMsg(id, html, kind){
  const el = document.getElementById(id);
  el.innerHTML = html ? `<span class="pill ${kind||''}">${html}</span>` : '';
}

function resetEntry(){
  document.getElementById('tanggal').value = '';
  document.getElementById('nama').value = '';
  document.getElementById('area').value = '';
  document.getElementById('ket').value = '';
  document.getElementById('lampiran').value = '';
  document.getElementById('link').value = '';
  setMsg('msgEntry','');
}

async function simpan(){
  const btn = document.getElementById('btnSimpan');
  btn.disabled = true;
  setMsg('msgEntry','Memproses...','');
  try{
    if(typeof _spPageContextInfo === 'undefined') throw new Error('Harus dijalankan di SharePoint Site.');
    await ensureFieldMap();
    const digest = await getRequestDigest();
    const judul = document.getElementById('judul').value.trim() || SP.titleValue;
    const tgl = document.getElementById('tanggal').value;
    const nama = document.getElementById('nama').value;
    const area = document.getElementById('area').value;
    const ket = document.getElementById('ket').value.trim();
    const linkManual = document.getElementById('link').value.trim();
    const file = document.getElementById('lampiran').files[0] || null;
    if(!tgl || !nama || !area || !ket) throw new Error('Tanggal, Nama Pemeriksa, Area Kerja, dan Deskripsi wajib diisi.');
    const fNama = spFieldMap['Nama Pemeriksa'];
    const fArea = spFieldMap['Area'];
    const fTgl = spFieldMap['Tanggal Temuan'];
    const fKet = spFieldMap['Keterangan Kondisi Temuan'];
    const fLink = spFieldMap['Link'];
    if(!fNama||!fArea||!fTgl||!fKet||!fLink) throw new Error('Kolom SharePoint tidak ditemukan. Pastikan nama kolom sesuai.');
    let lampiranUrl = '';
    let uploadInfo = null;
    if(file){ uploadInfo = await uploadToLibrary(file, digest); lampiranUrl = uploadInfo.absUrl; }
    else if(linkManual){ lampiranUrl = linkManual; }
    const linkVal = lampiranUrl ? { __metadata:{ type: 'SP.FieldUrlValue' }, Url: lampiranUrl, Description: file ? file.name : 'Link' } : null;
    const fields = { Title: judul, [fNama]: nama, [fArea]: area, [fTgl]: `${tgl}T00:00:00`, [fKet]: ket };
    if(linkVal) fields[fLink] = linkVal;
    const item = await createListItem(fields, digest);
    if(uploadInfo && uploadInfo.buffer){ await addListAttachment(item.Id, uploadInfo.fileName, uploadInfo.buffer, digest); }
    setMsg('msgEntry','Laporan berhasil dikirim','ok');
    resetEntry();
    showTab('history');
  }catch(e){ setMsg('msgEntry',(e?.message)||String(e),'err'); }
  finally{ btn.disabled = false; }
}

function buildThumbCell(imgUrl){
  const safe = String(imgUrl||'');
  if(!safe) return '';
  if(!isImageFileName(safe.split('?')[0])) return `<a href="${safe}" target="_blank">file</a>`;
  return `<span class="thumbWrap"><a href="${safe}" target="_blank"><img class="thumb" src="${safe}" alt="lampiran"></a><span class="preview"><img src="${safe}" alt="preview"></span></span>`;
}

async function loadHistory(){
  try{
    if(typeof _spPageContextInfo === 'undefined'){ document.getElementById('msgHistory').innerHTML = '<span class="pill err">Harus dijalankan di SharePoint Site.</span>'; return; }
    await ensureFieldMap();
    const fNama = spFieldMap['Nama Pemeriksa'];
    const fArea = spFieldMap['Area'];
    const fTgl = spFieldMap['Tanggal Temuan'];
    const fKet = spFieldMap['Keterangan Kondisi Temuan'];
    const fLink = spFieldMap['Link'];
    const s = document.getElementById('fStart').value;
    const e = document.getElementById('fEnd').value;
    const n = document.getElementById('fNama').value;
    const a = document.getElementById('fArea').value;
    const k = (document.getElementById('fKey').value||'').trim().toLowerCase();
    const select = `Id,Title,${fNama},${fArea},${fTgl},${fKet},${fLink},Attachments,AttachmentFiles`;
    const url = `${_spPageContextInfo.webAbsoluteUrl}/_api/web/lists/GetByTitle('${encodeURIComponent(SP.listTitle)}')/items?$top=2000&$orderby=Id desc&$select=${select}&$expand=AttachmentFiles&$filter=Title eq '${encodeURIComponent(SP.titleValue)}'`;
    const j = await spFetch(url,{ method:'GET' });
    const rows = j.d.results.map(it=>{
      const linkObj = it[fLink];
      const linkUrl = linkObj?.Url || '';
      const attFiles = it.AttachmentFiles?.results || [];
      const firstAtt = attFiles.length ? location.origin + attFiles[0].ServerRelativeUrl : '';
      return {
        tanggal: (it[fTgl]||'').slice(0,10),
        nama: it[fNama] || '',
        area: it[fArea] || '',
        ket: it[fKet] || '',
        lampiranUrl: firstAtt || linkUrl || '',
        linkUrl: linkUrl || ''
      };
    });
    const filtered = rows.filter(r=>{
      if(s && r.tanggal < s) return false;
      if(e && r.tanggal > e) return false;
      if(n && r.nama !== n) return false;
      if(a && r.area !== a) return false;
      if(k && !String(r.ket||'').toLowerCase().includes(k)) return false;
      return true;
    });
    spCacheItems = filtered;
    const tbody = document.getElementById('hist');
    tbody.innerHTML = filtered.length ? filtered.map(r=>`<tr><td>${r.tanggal}</td><td>${escapeHtml(r.nama)}</td><td>${escapeHtml(r.area)}</td><td style="white-space:pre-wrap">${escapeHtml(r.ket)}</td><td>${buildThumbCell(r.lampiranUrl)}</td><td>${r.linkUrl ? `<a href="${r.linkUrl}" target="_blank">link</a>` : ''}</td></tr>`).join('') : '<tr><td colspan="6">Tidak ada data</td></tr>';
    document.getElementById('msgHistory').innerHTML = '<span class="pill ok">OK</span>';
  }catch(e){ document.getElementById('msgHistory').innerHTML = `<span class="pill err">${escapeHtml(e?.message||String(e))}</span>`; }
}

function ymToStart(ym){ return new Date(`${ym}-01T00:00:00`); }
function ymToEnd(ym){ const d = ymToStart(ym); return new Date(d.getFullYear(), d.getMonth()+1, 0, 23, 59, 59); }
function toISODate(d){ const y=d.getFullYear(), m=String(d.getMonth()+1).padStart(2,'0'), dd=String(d.getDate()).padStart(2,'0'); return `${y}-${m}-${dd}`; }

function renderLeaderboard(){
  const startM = document.getElementById('lbStart').value;
  const endM = document.getElementById('lbEnd').value;
  if(!startM || !endM){ document.getElementById('msgLeader').innerHTML = '<span class="pill err">Pilih periode bulan.</span>'; document.getElementById('lb').innerHTML = ''; return; }
  const s = toISODate(ymToStart(startM));
  const e = toISODate(ymToEnd(endM));
  const within = (spCacheItems||[]).filter(r=>r.tanggal && r.tanggal>=s && r.tanggal<=e);
  if(!within.length){ document.getElementById('msgLeader').innerHTML = '<span class="pill err">Tidak ada data pada periode ini (load History dulu).</span>'; document.getElementById('lb').innerHTML = ''; return; }
  const map = new Map();
  within.forEach(r=> map.set(r.nama, (map.get(r.nama)||0)+1));
  const ranked = Array.from(map.entries()).map(([nama,count])=>({nama,count})).sort((a,b)=>b.count-a.count);
  const top = ranked[0]?.count || 1;
  document.getElementById('msgLeader').innerHTML = '<span class="pill ok">OK</span>';
  document.getElementById('lb').innerHTML = ranked.map((x,i)=>{
    const pct = Math.round((x.count/top)*100);
    return `<div class="card" style="margin:0 0 12px 0;background:#f8fafc"><div style="display:flex;justify-content:space-between;gap:12px;align-items:center;flex-wrap:wrap"><div><b>${escapeHtml(x.nama)}</b><div class="hint">Rank #${i+1}</div></div><div style="font-size:24px;font-weight:900">${x.count}</div></div><div class="hint" style="margin-top:8px"><div style="height:10px;background:#e5e7eb;border-radius:6px;overflow:hidden"><div style="width:${pct}%;height:10px;background:#22c55e"></div></div></div></div>`;
  }).join('');
}

function escapeHtml(s=''){ return String(s).replace(/[&<>"']/g,m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }

(function init(){
  fillSelect(document.getElementById('nama'), NAMA, false);
  fillSelect(document.getElementById('area'), AREA, false);
  fillSelect(document.getElementById('fNama'), NAMA, true);
  fillSelect(document.getElementById('fArea'), AREA, true);
  document.getElementById('tanggal').value = '';
  document.getElementById('nama').value = '';
  document.getElementById('area').value = '';
  const now = new Date();
  const ym = `${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}`;
  document.getElementById('lbStart').value = ym;
  document.getElementById('lbEnd').value = ym;
})();
</script>
</body>
</html>


