<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bundle Buddy V2</title>
<style>
*{box-sizing:border-box;font-family:-apple-system,BlinkMacSystemFont,sans-serif}
body{margin:0;background:#f6f5f2;padding:16px}
.header{background:white;padding:20px;border-radius:16px;margin-bottom:16px}
.search-box{background:white;padding:20px;border-radius:16px}
textarea{width:100%;border:2px solid #e8e8e8;border-radius:12px;padding:14px;font-size:16px;min-height:90px}
.btn{width:100%;background:#00b578;color:white;border:none;border-radius:12px;padding:16px;font-size:17px;font-weight:700;margin-top:14px}
.card{background:white;border-radius:16px;padding:16px;margin-bottom:12px;border:2px solid transparent}
.card.best{border-color:#00b578;background:#f0fdf6}
.seller{font-weight:800}
.badge{background:#00b578;color:white;padding:4px 10px;border-radius:20px;font-size:12px}
.items li{font-size:14px;padding:6px 0;display:flex;justify-content:space-between}
.save{margin-top:10px;background:#e8f8f0;padding:10px;border-radius:10px;font-weight:700;color:#00a86b}
</style>
</head>
<body>
<div class="header">
<h1>Bundle Buddy V2 💡</h1>
<p>Scrivi cosa vuoi. L'app trova il venditore che ha TUTTO!</p>
</div>
<div class="search-box">
<textarea id="wishlist">stivali, giocattolo bambino 5 anni</textarea>
<button class="btn" onclick="cerca()">🔍 TROVA CHI HA TUTTO</button>
</div>
<div id="results" style="margin-top:16px"></div>
<script>
const venditoriDB = [
 {nome:"Mamma_Marta_Russi", citta:"Russi (RA)", items:[
   {nome:"Stivali neri n.39", prezzo:22, match:["stivali"]},
   {nome:"Lego Duplo 5 anni", prezzo:15, match:["giocattolo","bambino","5 anni"]},
 ], spedizione:4.99, rating:4.9},
 {nome:"BabyWorld_Bazar", citta:"Bologna", items:[
   {nome:"Giocattolo legno 5 anni", prezzo:13, match:["giocattolo","5 anni"]},
   {nome:"Stivali pioggia bimbo", prezzo:10, match:["stivali"]},
 ], spedizione:3.99, rating:4.7},
];
function cerca(){
 let q=document.getElementById('wishlist').value.toLowerCase();
 let richieste=q.split(',').map(s=>s.trim()).filter(s=>s);
 let risultati=venditoriDB.map(v=>{
   let trovati=[]; richieste.forEach(req=>{
     let t=v.items.find(it=>it.match.some(m=>req.includes(m))||it.nome.toLowerCase().includes(req));
     if(t) trovati.push(t);
   }); return {venditore:v,trovati,matchCount:trovati.length};
 }).filter(r=>r.matchCount>0).sort((a,b)=>b.matchCount-a.matchCount);
 let html=''; risultati.forEach(r=>{
   let totale=r.trovati.reduce((s,it)=>s+it.prezzo,0);
   let risparmio=(r.matchCount-1)*r.venditore.spedizione;
   let isBest=r.matchCount==richieste.length;
   html+=`<div class="card ${isBest?'best':''}"><div><b>${r.venditore.nome}</b> ${r.venditore.citta} ${isBest?'🎉 HA TUTTO!':''}</div><ul class="items">${r.trovati.map(it=>`<li>${it.nome}<b>€${it.prezzo}</b></li>`).join('')}</ul><div class="save">${isBest?`Risparmi €${risparmio.toFixed(2)} di spedizione!`:''} Totale €${(totale+r.venditore.spedizione).toFixed(2)}</div></div>`;
 }); document.getElementById('results').innerHTML=html;
}
window.onload=cerca;
</script>
</body>
</html>
