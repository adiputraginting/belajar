<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kuis Matematika - Matriks</title>
<style>
:root{--navy:#183153;--blue:#2563eb;--pale:#eff6ff;--ink:#172033;--muted:#64748b;--line:#dbe3ef;--green:#15803d;--red:#b91c1c}
*{box-sizing:border-box}body{margin:0;background:#f4f7fb;color:var(--ink);font:16px/1.55 Arial,Helvetica,sans-serif}
header{background:linear-gradient(120deg,#183153,#2563eb);color:#fff;padding:34px 18px;text-align:center}header h1{margin:0 0 8px;font-size:clamp(25px,4vw,36px)}header p{margin:0;opacity:.92}
main{width:min(900px,calc(100% - 28px));margin:24px auto 50px}.panel,.question{background:#fff;border:1px solid var(--line);border-radius:14px;padding:20px;margin-bottom:16px;box-shadow:0 4px 16px #1831530a}
.fields{display:grid;grid-template-columns:1fr 1fr;gap:14px}.field label{display:block;font-weight:700;margin-bottom:6px}.field input{width:100%;padding:11px;border:1px solid #cbd5e1;border-radius:8px;font-size:16px}
.note{color:var(--muted);font-size:14px}.section-title{color:var(--navy);margin:26px 0 12px;font-size:21px}.qhead{display:flex;gap:10px;align-items:flex-start}.num{background:var(--pale);color:var(--blue);font-weight:700;border-radius:8px;padding:3px 9px;white-space:nowrap}.qtext{font-weight:700;margin:0 0 12px}.choices{display:grid;gap:9px}.choice{display:flex;gap:10px;align-items:flex-start;padding:10px 12px;border:1px solid var(--line);border-radius:9px;cursor:pointer}.choice:hover{background:#f8fbff;border-color:#93b4f8}.choice input{margin-top:5px;accent-color:var(--blue)}
textarea{width:100%;min-height:105px;padding:11px;border:1px solid #cbd5e1;border-radius:9px;font:inherit;resize:vertical}.matrix{display:inline-grid;grid-template-columns:repeat(2,auto);gap:4px 14px;padding:7px 15px;margin:4px 2px;vertical-align:middle;border-left:2px solid;border-right:2px solid;border-radius:5px;font-family:Georgia,serif}
.actions{display:flex;gap:10px;flex-wrap:wrap;margin:22px 0}.btn{border:0;border-radius:9px;padding:12px 18px;font-weight:700;font-size:15px;cursor:pointer}.primary{background:var(--blue);color:white}.secondary{background:#e2e8f0;color:#1e293b}.btn:hover{filter:brightness(.96)}
#result{display:none}.score{font-size:28px;font-weight:800;color:var(--navy)}.feedback{padding:10px 12px;border-radius:8px;margin:8px 0;background:#f1f5f9}.correct{color:var(--green);font-weight:700}.wrong{color:var(--red);font-weight:700}.essay-key{margin-top:8px;color:var(--muted);font-size:14px}
@media(max-width:600px){.fields{grid-template-columns:1fr}.panel,.question{padding:16px}}
@media print{body{background:white}.actions,#result{display:none!important}.panel,.question{box-shadow:none;break-inside:avoid}header{background:#fff;color:#000;padding:10px}}
</style>
</head>
<body>
<header><h1>Kuis Matematika: Matriks</h1><p>15 Pilihan Ganda · 5 Essay</p></header>
<main>
<form id="quizForm">
<section class="panel">
<h2 style="margin-top:0">Identitas Peserta</h2>
<div class="fields">
<div class="field"><label for="nama">Nama lengkap</label><input id="nama" name="nama" placeholder="Masukkan nama" required></div>
<div class="field"><label for="kelas">Kelas</label><input id="kelas" name="kelas" placeholder="Contoh: XI A" required></div>
</div>
<p class="note">Petunjuk: Pilih satu jawaban yang paling tepat untuk setiap soal pilihan ganda. Jawablah soal essay dengan langkah penyelesaian yang jelas. Nilai pilihan ganda dihitung otomatis; essay diperiksa oleh guru.</p>
</section>
<h2 class="section-title">A. Pilihan Ganda</h2>
<div id="mcq"></div>
<h2 class="section-title">B. Essay</h2>
<div id="essay"></div>
<div class="actions"><button class="btn primary" type="submit">Kirim dan Periksa Jawaban</button><button class="btn secondary" type="button" onclick="window.print()">Cetak Kuis</button><button class="btn secondary" type="reset" id="resetBtn">Reset</button></div>
</form>
<section class="panel" id="result" aria-live="polite"><h2 style="margin-top:0">Hasil Kuis</h2><div id="resultContent"></div><div class="actions"><button class="btn secondary" onclick="window.print()">Cetak Hasil</button></div></section>
</main>
<script>
const questions = [
{q:'Ordo dari matriks A = [[2, 1, 0], [3, 4, 5]] adalah ....',o:['2 × 2','2 × 3','3 × 2','3 × 3'],a:1,why:'Matriks memiliki 2 baris dan 3 kolom, sehingga ordonya 2 × 3.'},
{q:'Jika A = [[4, 0], [−2, 7]], elemen a₂₁ adalah ....',o:['4','0','−2','7'],a:2,why:'Elemen a₂₁ berada pada baris ke-2 kolom ke-1, yaitu −2.'},
{q:'Matriks yang semua elemen di luar diagonal utamanya bernilai nol disebut matriks ....',o:['Baris','Kolom','Diagonal','Nol'],a:2,why:'Matriks diagonal memiliki elemen bukan nol hanya pada diagonal utama.'},
{q:'Hasil penjumlahan [[1, 2], [3, 4]] + [[2, 0], [1, 5]] adalah ....',o:['[[3, 2], [4, 9]]','[[2, 2], [3, 20]]','[[3, 0], [4, 5]]','[[1, 2], [1, 9]]'],a:0,why:'Jumlahkan elemen yang seletak: 1+2=3, 2+0=2, 3+1=4, dan 4+5=9.'},
{q:'Hasil pengurangan [[5, 3], [4, 8]] − [[2, 1], [3, 6]] adalah ....',o:['[[7, 4], [7, 14]]','[[3, 2], [1, 2]]','[[3, 4], [1, 14]]','[[2, 2], [1, 2]]'],a:1,why:'Kurangkan elemen seletak: 5−2=3, 3−1=2, 4−3=1, dan 8−6=2.'},
{q:'Jika A = [[1, 2], [3, 4]], maka 2A = ....',o:['[[1, 4], [6, 8]]','[[2, 2], [3, 8]]','[[2, 4], [6, 8]]','[[3, 4], [5, 6]]'],a:2,why:'Setiap elemen dikalikan 2 sehingga diperoleh [[2,4],[6,8]].'},
{q:'Transpose dari A = [[1, 2, 3], [4, 5, 6]] adalah ....',o:['[[1, 2], [3, 4], [5, 6]]','[[1, 4], [2, 5], [3, 6]]','[[4, 5, 6], [1, 2, 3]]','[[6, 5, 4], [3, 2, 1]]'],a:1,why:'Transpose menukar baris menjadi kolom.'},
{q:'Determinan matriks [[3, 2], [1, 4]] adalah ....',o:['8','10','12','14'],a:1,why:'Determinan = (3×4) − (2×1) = 12−2 = 10.'},
{q:'Determinan matriks [[5, 1], [2, 3]] adalah ....',o:['11','13','15','17'],a:1,why:'Determinan = (5×3) − (1×2) = 15−2 = 13.'},
{q:'Jika A = [[1, 2], [0, 1]] dan B = [[3, 1], [2, 4]], elemen baris 1 kolom 1 dari AB adalah ....',o:['3','5','7','9'],a:2,why:'Elemen (1,1) = (1×3) + (2×2) = 7.'},
{q:'Matriks identitas berordo 2 × 2 adalah ....',o:['[[0, 0], [0, 0]]','[[1, 1], [1, 1]]','[[1, 0], [0, 1]]','[[0, 1], [1, 0]]'],a:2,why:'Matriks identitas memiliki 1 pada diagonal utama dan 0 pada elemen lainnya.'},
{q:'Dua matriks dapat dijumlahkan jika ....',o:['Jumlah barisnya sama','Jumlah kolomnya sama','Ordonya sama','Determinan keduanya sama'],a:2,why:'Penjumlahan matriks mensyaratkan jumlah baris dan kolom yang sama (ordo sama).'},
{q:'Jika A = [[a, 2], [3, 4]] dan A = [[5, 2], [3, 4]], nilai a adalah ....',o:['2','3','4','5'],a:3,why:'Matriks yang sama memiliki elemen-elemen seletak yang sama, sehingga a=5.'},
{q:'Invers matriks [[2, 0], [0, 3]] adalah ....',o:['[[2, 0], [0, 3]]','[[1/2, 0], [0, 1/3]]','[[3, 0], [0, 2]]','[[0, 1/2], [1/3, 0]]'],a:1,why:'Invers matriks diagonal tersebut adalah [[1/2,0],[0,1/3]], karena hasil kalinya dengan matriks asal adalah identitas.'},
{q:'Jika matriks A berordo 2 × 3 dan B berordo 3 × 4, ordo hasil perkalian AB adalah ....',o:['2 × 4','3 × 3','4 × 2','2 × 3'],a:0,why:'Untuk AB, dimensi dalam 3 dan 3 cocok; hasilnya berordo 2 × 4.'}
];
const essays = [
'Jelaskan pengertian matriks, baris, kolom, elemen, dan ordo matriks. Berikan satu contoh matriks berordo 2 × 3.',
'Diketahui A = [[2, 1], [3, 4]] dan B = [[1, 5], [2, 0]]. Hitunglah A + B dan A − B.',
'Diketahui A = [[2, 3], [1, 4]]. Tentukan transpose A dan determinan A.',
'Diketahui A = [[1, 2], [3, 1]] dan B = [[2, 0], [1, 4]]. Hitunglah hasil perkalian AB.',
'Tentukan invers dari matriks A = [[2, 1], [1, 1]]. Tunjukkan langkah penyelesaiannya.'
];
const mcqBox=document.getElementById('mcq');
questions.forEach((q,i)=>{const d=document.createElement('section');d.className='question';d.innerHTML=`<div class="qhead"><span class="num">${i+1}</span><div style="flex:1"><p class="qtext">${q.q}</p><div class="choices">${q.o.map((v,j)=>`<label class="choice"><input type="radio" name="q${i}" value="${j}" required><span><b>${'ABCD'[j]}.</b> ${v}</span></label>`).join('')}</div></div></div>`;mcqBox.appendChild(d)});
const essayBox=document.getElementById('essay');
essays.forEach((q,i)=>{const d=document.createElement('section');d.className='question';d.innerHTML=`<div class="qhead"><span class="num">${i+16}</span><div style="flex:1"><p class="qtext">${q}</p><textarea name="e${i}" placeholder="Tuliskan jawaban dan langkah penyelesaian di sini..."></textarea></div></div>`;essayBox.appendChild(d)});
document.getElementById('quizForm').addEventListener('submit',e=>{e.preventDefault();let score=0,detail='';questions.forEach((q,i)=>{const chosen=document.querySelector(`input[name="q${i}"]:checked`);const ok=chosen&&Number(chosen.value)===q.a;if(ok)score++;detail+=`<div class="feedback"><b>Soal ${i+1}:</b> <span class="${ok?'correct':'wrong'}">${ok?'Benar':'Belum tepat'}</span><br>${q.why}</div>`});const nama=document.getElementById('nama').value;const kelas=document.getElementById('kelas').value;const nilai=score/questions.length*100;document.getElementById('resultContent').innerHTML=`<p><b>Nama:</b> ${escapeHTML(nama)} &nbsp; <b>Kelas:</b> ${escapeHTML(kelas)}</p><div class="score">${score} / 15 benar — Nilai PG: ${nilai.toFixed(2)}</div><p class="note">Nilai di atas hanya berasal dari pilihan ganda (bobot 100 untuk 15 soal). Lima jawaban essay perlu dinilai secara manual oleh guru.</p><h3>Pembahasan Pilihan Ganda</h3>${detail}<h3>Jawaban Essay</h3><p class="note">Jawaban essay tetap tersimpan pada formulir di atas selama halaman tidak dimuat ulang. Periksa kelengkapan konsep, proses perhitungan, dan kesimpulan.</p>`;const r=document.getElementById('result');r.style.display='block';r.scrollIntoView({behavior:'smooth'});});
function escapeHTML(s){return s.replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
document.getElementById('resetBtn').addEventListener('click',()=>{document.getElementById('result').style.display='none'});
</script>
</body>
</html>
