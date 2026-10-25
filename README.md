
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MEAL // DROP</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
<style>
:root{--bg:#0A0A0A;--paper:#FFFBF2;--red:#FF2E1F;--line:rgba(255,251,242,0.14)}
*{margin:0;padding:0;box-sizing:border-box}
body{background:var(--bg);color:var(--paper);font-family:'JetBrains Mono',monospace}
header{position:sticky;top:0;z-index:100;background:rgba(10,10,10,0.94);backdrop-filter:blur(12px);border-bottom:1px solid var(--line);display:flex;justify-content:space-between;align-items:center;padding:14px 4vw}
.left{display:flex;align-items:center;gap:14px}
.menu-btn{width:40px;height:40px;border-radius:50%;background:var(--paper);color:var(--bg);border:0;font-size:18px;cursor:pointer;display:grid;place-items:center}
.logo{font-family:'Syne',sans-serif;font-weight:800;font-size:26px} .logo em{color:var(--red);font-style:normal}
.cart-btn{background:var(--paper);color:var(--bg);border:0;padding:10px 18px;border-radius:100px;font-weight:800;font-size:12px;display:flex;gap:8px;align-items:center;cursor:pointer}
.cart-btn span{background:var(--red);color:#fff;width:20px;height:20px;border-radius:50%;display:grid;place-items:center}
.grid{padding:2vw 4vw 6vw;display:grid;grid-template-columns:repeat(12,1fr);gap:1.2vw}
@media(max-width:900px){.grid{grid-template-columns:1fr 1fr;gap:3vw;padding:4vw}}
.prod{grid-column:span 3;background:#111;border:1px solid var(--line);display:flex;flex-direction:column}
.prod.big{grid-column:span 6} @media(max-width:900px){.prod,.prod.big{grid-column:span 1}.prod.big{grid-column:span 2}}
.prod-img{aspect-ratio:4/3;position:relative;overflow:hidden;background:#0F0F0F}
.prod-img img{width:100%;height:100%;object-fit:cover}
.prod-tag{position:absolute;top:10px;left:10px;background:var(--paper);color:var(--bg);font-size:9px;font-weight:800;padding:4px 8px}
.prod-tag.red{background:var(--red);color:white}
.cat-label{position:absolute;top:10px;right:10px;background:rgba(0,0,0,0.7);color:white;font-size:9px;padding:4px 8px;border-radius:100px}
.prod-body{padding:14px;flex:1;display:flex;flex-direction:column;gap:6px}
.prod-body h3{font-family:'Syne',sans-serif;font-size:17px;text-transform:uppercase}
.prod-body small{font-size:11px;opacity:0.5;min-height:28px}
.prod-foot{display:flex;justify-content:space-between;align-items:center;margin-top:auto;padding-top:12px;border-top:1px dashed var(--line)}
.add{width:34px;height:34px;border-radius:50%;background:var(--paper);color:var(--bg);border:0;font-weight:800;font-size:18px;cursor:pointer;display:grid;place-items:center}
.overlay{position:fixed;inset:0;background:rgba(0,0,0,0.6);z-index:200;opacity:0;pointer-events:none;transition:0.3s}
.overlay.on{opacity:1;pointer-events:auto}
.drawer{position:fixed;top:0;right:0;bottom:0;width:min(420px,92vw);background:var(--paper);color:var(--bg);z-index:201;transform:translateX(100%);transition:0.45s;display:flex;flex-direction:column}
.drawer.on{transform:translateX(0)}
.left-drawer{position:fixed;top:0;left:0;bottom:0;width:min(360px,88vw);background:var(--paper);color:var(--bg);z-index:202;transform:translateX(-100%);transition:0.45s;display:flex;flex-direction:column;overflow:auto}
.left-drawer.on{transform:translateX(0)}
.left-head,.drawer-head{padding:18px 20px;display:flex;justify-content:space-between;border-bottom:1px solid rgba(0,0,0,0.1)}
.close{width:34px;height:34px;border-radius:50%;border:1px solid rgba(0,0,0,0.15);background:transparent;cursor:pointer}
.left-body{padding:22px;display:flex;flex-direction:column;gap:28px}
.left-section h4{font-family:'Syne',sans-serif;font-size:12px;letter-spacing:1.5px;opacity:0.4;margin-bottom:14px;text-transform:uppercase}
.cat-list{display:flex;flex-direction:column;gap:6px}
.cat-item{display:flex;justify-content:space-between;align-items:center;padding:12px 14px;border:1px solid rgba(0,0,0,0.08);border-radius:10px;cursor:pointer;background:white}
.cat-item.on{background:var(--bg);color:var(--paper)}
.contact{font-size:13px;line-height:1.6}
.contact b{display:block;font-family:'Syne',sans-serif;margin-top:10px}
.contact a{color:var(--bg);text-decoration:none;border-bottom:1px solid rgba(0,0,0,0.2)}
.social{display:flex;gap:8px;margin-top:14px}
.social a{width:36px;height:36px;border-radius:50%;background:var(--bg);color:var(--paper);display:grid;place-items:center;text-decoration:none}
.toast{position:fixed;bottom:20px;left:50%;transform:translateX(-50%) translateY(120px);background:var(--paper);color:var(--bg);padding:10px 16px;border-radius:100px;font-size:12px;font-weight:800;z-index:300;transition:0.4s}
.toast.on{transform:translateX(-50%) translateY(0)}
</style>
</head>
<body>
<header>
  <div class="left"><button class="menu-btn" onclick="openLeft()">☰</button><div class="logo">MEAL <em>//</em> DROP</div></div>
  <button class="cart-btn" onclick="openCart()">CART <span id="count">0</span></button>
</header>
<div class="grid" id="grid" style="padding-top:4vw"></div>
<div class="overlay" id="overlay" onclick="closeAll()"></div>
<div class="left-drawer" id="leftDrawer">
  <div class="left-head"><h2 style="font-family:Syne,sans-serif">MENÚ</h2><button class="close" onclick="closeAll()">✕</button></div>
  <div class="left-body">
    <div class="left-section"><h4>Categorías</h4><div class="cat-list" id="leftCatList"></div></div>
    <div class="left-section"><h4>Contacto</h4><div class="contact">
      <b>📍 Dirección</b>Calle 21 N° 482, Avellaneda<br>Santa Fe, Argentina
      <b>📞 WhatsApp</b><a href="https://wa.me/543482000000">+54 3482 00-0000</a>
      <b>🕒 Horarios</b>Lun a Dom: 19:00 - 00:30
      <b>✉️ Email</b><a href="mailto:hola@mealdrop.ar">hola@mealdrop.ar</a>
      <b>🚚 Envío</b>Gratis +$15.000 / $800 fijo
      <div class="social"><a href="#">IG</a><a href="#">TT</a><a href="https://wa.me/543482000000">WA</a></div>
    </div></div>
  </div>
</div>
<div class="drawer" id="drawer">
  <div class="drawer-head"><h2 style="font-family:Syne">TU PEDIDO</h2><button class="close" onclick="closeAll()">✕</button></div>
  <div class="drawer-items" id="items" style="flex:1;overflow:auto;padding:10px 20px"></div>
  <div class="drawer-foot" style="padding:18px 20px;border-top:1px solid rgba(0,0,0,0.1)">
    <div style="display:flex;justify-content:space-between"><span>Subtotal</span><span id="sub">$0</span></div>
    <div style="display:flex;justify-content:space-between"><span>Envío</span><span id="ship">Gratis</span></div>
    <div style="display:flex;justify-content:space-between;font-family:Syne;font-size:20px;font-weight:800;margin-top:10px"><span>Total</span><span id="tot">$0</span></div>
    <button style="width:100%;background:var(--bg);color:var(--paper);border:0;padding:16px;border-radius:12px;font-family:Syne;font-weight:800;margin-top:10px;cursor:pointer" onclick="checkout()">PEDIR POR WHATSAPP →</button>
  </div>
</div>
<div class="toast" id="toast"></div>
<script>
const CATS=['TODOS','PARRILLA','EMPANADAS','BURGERS','CEVICHE','PASTAS','BEBIDAS'];
const PRODUCTS=[
 {id:'asado',cat:'PARRILLA',name:'Asado Provincial 12H',desc:'Tira de asado, chimichurri',price:8200,tag:'BEST SELLER',img:'https://images.unsplash.com/photo-1558030006-450066393d65?q=80&w=600'},
 {id:'vacio',cat:'PARRILLA',name:'Vacío Ahumado',desc:'Cocción lenta 10h, papas rotas',price:8900,tag:'LEÑA',img:'https://images.unsplash.com/photo-1546964052-d93334927b6a?q=80&w=600'},
 {id:'empanadas',cat:'EMPANADAS',name:'Empanadas Leña x6',desc:'Carne cortada a cuchillo',price:4900,tag:'WOODFIRED',img:'https://images.unsplash.com/photo-1603894584373-5ac82b2ae398?q=80&w=600'},
 {id:'burger',cat:'BURGERS',name:'Smash Burger Doble',desc:'Blend 80/20, cheddar, papas',price:6500,tag:'NEW',img:'https://images.unsplash.com/photo-1568909344668-6f14a07b56a0?q=80&w=600'},
 {id:'ceviche',cat:'CEVICHE',name:'Ceviche del Litoral',desc:'Surubí, lima, ají',price:6800,tag:'SEASONAL',img:'https://images.unsplash.com/photo-1530554764233-e79e16c91d08?q=80&w=600'},
 {id:'pasta',cat:'PASTAS',name:'Pasta Fresca del Día',desc:'Masa madre, pomodoro',price:5800,tag:'HANDMADE',img:'https://images.unsplash.com/photo-1621996346565-e3dbc646d9a9?q=80&w=600'},
 {id:'vino',cat:'BEBIDAS',name:'Vino Natural Tinto',desc:'Criolla chica, 750ml',price:12500,tag:'NATURAL',img:'https://images.unsplash.com/photo-1510812431401-41d2bd2722f3?q=80&w=600'},
 {id:'birra',cat:'BEBIDAS',name:'Cerveza Artesanal',desc:'IPA Litoral, lata',price:2800,tag:'COLD',img:'https://images.unsplash.com/photo-1608270586620-248524c67de9?q=80&w=600'}
];
let activeCat='TODOS'; let cart=JSON.parse(localStorage.getItem('md_cart')||'[]');
function save(){localStorage.setItem('md_cart',JSON.stringify(cart));document.getElementById('count').textContent=cart.reduce((s,i)=>s+i.qty,0);}
function renderCats(){
 const left=document.getElementById('leftCatList');
 left.innerHTML=CATS.map(c=>{
   let count=c==='TODOS'?PRODUCTS.length:PRODUCTS.filter(p=>p.cat===c).length;
   return `<div class="cat-item ${c===activeCat?'on':''}" onclick="setCat('${c}');closeAll()"><b>${c}</b><span>${count}</span></div>`;
 }).join('');
}
function setCat(c){activeCat=c;renderCats();renderProducts();}
function renderProducts(){
 let list=activeCat==='TODOS'?PRODUCTS:PRODUCTS.filter(p=>p.cat===activeCat);
 document.getElementById('grid').innerHTML=list.map(p=>`<div class="prod"><div class="prod-img"><img src="${p.img}"><div class="prod-tag ${p.tag==='BEST SELLER'?'red':''}">${p.tag}</div><div class="cat-label">${p.cat}</div></div><div class="prod-body"><h3>${p.name}</h3><small>${p.desc}</small><div class="prod-foot"><div style="font-weight:800">$${p.price.toLocaleString('es-AR')}</div><button class="add" onclick="add('${p.id}')">+</button></div></div></div>`).join('');
}
function add(id){let f=cart.find(x=>x.id===id);if(f)f.qty++;else cart.push({id,qty:1});save();renderDrawer();let t=document.getElementById('toast');t.textContent='Agregado';t.classList.add('on');setTimeout(()=>t.classList.remove('on'),2000);}
function changeQty(id,d){let f=cart.find(x=>x.id===id);if(!f)return;f.qty+=d;if(f.qty<=0)cart=cart.filter(x=>x.id!==id);save();renderDrawer();}
function renderDrawer(){
 let c=document.getElementById('items');
 if(!cart.length)c.innerHTML=`<div style="padding:60px 20px;text-align:center;opacity:0.4">Carrito vacío</div>`;
 else c.innerHTML=cart.map(it=>{let p=PRODUCTS.find(x=>x.id===it.id);return `<div style="display:flex;gap:12px;padding:14px 0;border-bottom:1px solid rgba(0,0,0,0.08);align-items:center"><img src="${p.img}" style="width:60px;height:60px;object-fit:cover;border-radius:6px"><div style="flex:1"><b style="font-family:Syne;font-size:13px">${p.name}</b><div style="display:flex;gap:8px;margin-top:6px"><button onclick="changeQty('${it.id}',-1)" style="width:24px;height:24px;border-radius:50%;border:1px solid rgba(0,0,0,0.15)">−</button><span>${it.qty}</span><button onclick="changeQty('${it.id}',1)" style="width:24px;height:24px;border-radius:50%;border:1px solid rgba(0,0,0,0.15)">+</button></div></div><div style="font-weight:800">$${(p.price*it.qty).toLocaleString()}</div></div>`}).join('');
 let total=cart.reduce((s,it)=>s+it.qty*PRODUCTS.find(p=>p.id===it.id).price,0);
 document.getElementById('sub').textContent=`$${total.toLocaleString()}`;document.getElementById('ship').textContent=total>=15000||total===0?'Gratis':'$800';document.getElementById('tot').textContent=`$${(total+(total>=15000||total===0?0:800)).toLocaleString()}`;
}
function openLeft(){document.getElementById('leftDrawer').classList.add('on');document.getElementById('overlay').classList.add('on');}
function openCart(){document.getElementById('drawer').classList.add('on');document.getElementById('overlay').classList.add('on');renderDrawer();}
function closeAll(){document.getElementById('leftDrawer').classList.remove('on');document.getElementById('drawer').classList.remove('on');document.getElementById('overlay').classList.remove('on');}
function checkout(){let total=cart.reduce((s,it)=>s+it.qty*PRODUCTS.find(p=>p.id===it.id).price,0);let lines=cart.map(it=>{let p=PRODUCTS.find(x=>x.id===it.id);return `• [${p.cat}] ${p.name} x${it.qty}`}).join('%0A');window.open(`https://wa.me/543482000000?text=${lines}%0ATotal:$${total}`,'_blank');}
renderCats();renderProducts();save();renderDrawer();
</script>
</body>
</html>
