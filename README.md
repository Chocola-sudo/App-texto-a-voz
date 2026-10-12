# App-texto-a-voz
Aplicación de texto a voz con sonidos binaurales
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover,user-scalable=no">
<meta name="theme-color" content="#0a0d16">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Hypno Studio">
<title>Hypno Studio — Audio Hipnótico Completo</title>

<link rel="manifest" href='data:application/manifest+json,{"name":"Hypno Studio","short_name":"Hypno","start_url":".","display":"standalone","background_color":"%230a0d16","theme_color":"%230a0d16","orientation":"any","icons":[{"src":"data:image/svg+xml,%3Csvg xmlns=%27http://www.w3.org/2000/svg%27 viewBox=%270 0 512 512%27%3E%3Crect width=%27512%27 height=%27512%27 rx=%27110%27 fill=%27%230a0d16%27/%3E%3Ccircle cx=%27256%27 cy=%27256%27 r=%27170%27 fill=%27none%27 stroke=%27%238fa8ff%27 stroke-width=%2714%27/%3E%3Ccircle cx=%27256%27 cy=%27256%27 r=%27110%27 fill=%27none%27 stroke=%27%23c0a8ff%27 stroke-width=%2712%27/%3E%3Ccircle cx=%27256%27 cy=%27256%27 r=%2755%27 fill=%27%238fa8ff%27/%3E%3C/svg%3E","sizes":"512x512","type":"image/svg+xml","purpose":"any maskable"}]}'>

<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 512 512'%3E%3Crect width='512' height='512' rx='110' fill='%230a0d16'/%3E%3Ccircle cx='256' cy='256' r='170' fill='none' stroke='%238fa8ff' stroke-width='14'/%3E%3Ccircle cx='256' cy='256' r='55' fill='%238fa8ff'/%3E%3C/svg%3E">

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">

<script src="https://cdn.jsdelivr.net/npm/lamejs@1.2.1/lame.min.js" crossorigin="anonymous"></script>

<style>
:root{--bg-1:#0a0d16;--bg-2:#04060c;--acc:#8fa8ff;--txt:#d8e0ff;--ok:#6fffb0;--warn:#ffd9a0;--err:#ffc9c9;--pink:#ff8fa3}
*{margin:0;padding:0;box-sizing:border-box}
html,body{min-height:100%;background:radial-gradient(circle at top,#12172a 0,var(--bg-2) 80%);font-family:'Plus Jakarta Sans',system-ui,sans-serif;color:var(--txt);-webkit-tap-highlight-color:transparent;overscroll-behavior:none}
.app{max-width:1100px;margin:0 auto;padding:16px 14px 80px;padding-top:max(16px,env(safe-area-inset-top))}
header{margin-bottom:16px;display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap}
h1{font-size:20px;font-weight:700;letter-spacing:-.01em;background:linear-gradient(90deg,#cdd6ff,#8fa8ff,#c0a8ff);-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent}
.sub{font-size:11px;opacity:.55;margin-top:2px}
.card{background:rgba(255,255,255,.03);border:1px solid rgba(160,180,255,.12);border-radius:14px;padding:14px;margin-bottom:12px;backdrop-filter:blur(8px);-webkit-backdrop-filter:blur(8px)}
.card h2{font-size:11px;text-transform:uppercase;letter-spacing:.12em;color:#aebfff;margin-bottom:10px;font-weight:600;display:flex;justify-content:space-between;align-items:center;gap:8px}
.fila{display:flex;align-items:center;gap:10px;margin-bottom:10px;flex-wrap:wrap}
.fila:last-child{margin-bottom:0}
.fila label{flex:1;min-width:120px;opacity:.85;font-weight:500;font-size:13px}
.fila .ctrl{flex:2;min-width:170px;display:flex;align-items:center;gap:8px}
input[type=range]{flex:1;accent-color:var(--acc);height:4px;cursor:pointer}
input[type=number],input[type=text],select{background:rgba(255,255,255,.06);border:1px solid rgba(160,180,255,.2);border-radius:8px;color:#e6ecff;padding:8px 10px;font-size:13px;font-family:inherit;width:100%}
input:focus,select:focus{outline:none;border-color:var(--acc)}
select option{background:#0f1423}
textarea{width:100%;min-height:140px;max-height:40vh;background:rgba(255,255,255,.06);border:1px solid rgba(160,180,255,.2);border-radius:10px;color:#e6ecff;padding:12px;font-size:14px;font-family:inherit;line-height:1.5;resize:vertical}
textarea:focus{outline:none;border-color:var(--acc)}
.val{min-width:52px;text-align:right;font-size:12px;opacity:.75;font-variant-numeric:tabular-nums}
button{border:1px solid rgba(160,180,255,.3);background:rgba(140,160,255,.15);color:#dfe6ff;cursor:pointer;font-size:13px;font-weight:600;font-family:inherit;padding:10px 14px;border-radius:10px;transition:all .15s;display:inline-flex;align-items:center;justify-content:center;gap:6px;white-space:nowrap}
button:hover:not(:disabled){background:rgba(140,160,255,.28)}
button:active:not(:disabled){transform:scale(.97)}
button:disabled{opacity:.4;cursor:not-allowed}
button.primary{background:linear-gradient(135deg,#6d8bff,#8fa8ff);border-color:transparent;color:#0a0d16;font-weight:700}
button.ok{border-color:rgba(140,255,180,.4);color:#c9ffdf;background:rgba(100,255,150,.15)}
button.danger{border-color:rgba(255,140,140,.4);color:#ffc9c9;background:rgba(255,100,100,.1)}
button.ghost{background:transparent}
button.mini{padding:5px 9px;font-size:11px;border-radius:6px;min-width:32px}
.btn-row{display:flex;gap:8px;flex-wrap:wrap}
.btn-row>button{flex:1;min-width:110px}
.presets{display:grid;grid-template-columns:repeat(auto-fit,minmax(130px,1fr));gap:8px}
.preset{padding:10px;border-radius:10px;border:1px solid rgba(160,180,255,.2);background:rgba(255,255,255,.02);cursor:pointer;text-align:left;transition:all .15s;flex-direction:column;align-items:flex-start;display:flex}
.preset:hover{border-color:var(--acc)}
.preset.active{border-color:var(--acc);background:rgba(140,160,255,.18)}
.preset .n{font-size:12px;font-weight:600;color:#e6ecff;display:block;margin-bottom:2px}
.preset .d{font-size:10px;opacity:.6;line-height:1.3;white-space:normal}
.stats{display:flex;justify-content:space-between;font-size:11px;opacity:.6;margin-top:6px;font-variant-numeric:tabular-nums;flex-wrap:wrap;gap:8px}
#estado{font-size:12px;text-align:center;padding:10px;background:rgba(255,255,255,.04);border-radius:8px;margin-top:8px;min-height:36px;display:flex;align-items:center;justify-content:center;line-height:1.4;transition:all .2s}
#estado.hablando{background:rgba(140,255,180,.1);color:var(--ok)}
#estado.grabando{background:rgba(255,100,100,.1);color:var(--err)}
#estado.ok{background:rgba(140,255,180,.15);color:var(--ok)}
#estado.warn{background:rgba(255,190,90,.1);color:var(--warn)}
#estado.err{background:rgba(255,100,100,.15);color:var(--err)}
.prog{height:6px;background:rgba(255,255,255,.08);border-radius:3px;overflow:hidden;margin-top:8px;display:none}
.prog>i{display:block;height:100%;width:0;background:linear-gradient(90deg,var(--acc),#c0a8ff);transition:width .2s}
.aviso{font-size:12px;padding:10px 12px;border-radius:8px;line-height:1.5;margin-bottom:10px}
.aviso.info{background:rgba(140,160,255,.08);border:1px solid rgba(140,160,255,.25);color:#c8d2ff}
.aviso.warn{background:rgba(255,190,90,.08);border:1px solid rgba(255,190,90,.25);color:var(--warn)}
.aviso.err{background:rgba(255,100,100,.08);border:1px solid rgba(255,100,100,.25);color:var(--err)}
.badge{display:inline-block;font-size:10px;padding:2px 6px;border-radius:4px;background:rgba(140,160,255,.2);color:#c0d0ff;margin-left:6px;vertical-align:middle;font-weight:600;letter-spacing:.04em}
.badge.ok{background:rgba(100,255,150,.2);color:#c9ffdf}
.badge.warn{background:rgba(255,190,90,.25);color:#ffe0a8}
details{margin-top:8px;font-size:12px}
details summary{cursor:pointer;opacity:.7;padding:4px 0}
details summary:hover{opacity:1}
details[open] summary{margin-bottom:8px}
details p,details li{margin-top:6px;opacity:.8;line-height:1.55}
details ol,details ul{padding-left:20px}
.diag{font-family:ui-monospace,Menlo,monospace;font-size:11px;background:rgba(0,0,0,.35);border:1px solid rgba(160,180,255,.15);border-radius:8px;padding:10px;margin-top:8px;line-height:1.6;max-height:180px;overflow-y:auto;white-space:pre-wrap;word-break:break-word}
.diag .ok{color:var(--ok)}
.diag .err{color:var(--err)}
.diag .warn{color:var(--warn)}
.chk{display:flex;align-items:center;gap:8px;flex:1;font-size:13px;cursor:pointer}
.chk input{width:18px;height:18px;accent-color:var(--acc)}
.permiso{margin-top:8px;padding:10px;border-radius:8px;text-align:center;font-size:12px;background:rgba(140,160,255,.06);border:1px solid rgba(140,160,255,.2)}
.permiso.ok{background:rgba(100,255,150,.1);border-color:rgba(100,255,150,.3);color:var(--ok)}
.permiso.err{background:rgba(255,100,100,.1);border-color:rgba(255,100,100,.3);color:var(--err)}

/* Tonos grid */
.tonos-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(130px,1fr));gap:8px;margin-bottom:12px}
.tono-btn{padding:10px;border-radius:10px;border:1px solid rgba(160,180,255,.2);background:rgba(255,255,255,.02);cursor:pointer;text-align:center;transition:all .15s;display:flex;flex-direction:column;align-items:center}
.tono-btn:hover{border-color:var(--acc)}
.tono-btn.active{border-color:var(--acc);background:rgba(140,160,255,.18);box-shadow:0 0 0 1px var(--acc)}
.tono-btn .n{font-size:12px;font-weight:600;color:#e6ecff;display:block;margin-bottom:2px}
.tono-btn .d{font-size:10px;opacity:.55;line-height:1.2}

/* Mezclador vertical */
.mixer{display:flex;gap:12px;overflow-x:auto;padding:10px 4px;justify-content:center;flex-wrap:wrap}
.chan{display:flex;flex-direction:column;align-items:center;gap:6px;width:64px;flex-shrink:0}
.chan .name{font-size:10px;opacity:.7;text-align:center;height:22px;line-height:1.1;display:flex;align-items:center;justify-content:center}
.chan input[type=range].vert{
  writing-mode:vertical-lr;
  direction:rtl;
  width:28px;
  height:140px;
  accent-color:var(--acc);
}
.chan .db{font-size:10px;font-variant-numeric:tabular-nums;opacity:.8;text-align:center}
.chan .mute{font-size:9px;padding:3px 6px;border-radius:4px;border:1px solid rgba(255,140,140,.3);background:rgba(255,100,100,.08);color:#ffc9c9;cursor:pointer}
.chan .mute.on{background:rgba(255,100,100,.35);color:#fff}
.chan .solo{font-size:9px;padding:3px 6px;border-radius:4px;border:1px solid rgba(140,255,180,.3);background:rgba(100,255,150,.08);color:#c9ffdf;cursor:pointer}
.chan .solo.on{background:rgba(100,255,150,.35);color:#fff}

/* Cantidad +- */
.qty{display:flex;align-items:center;gap:6px}
.qty input[type=number]{max-width:70px;text-align:center}
.qty button{padding:6px 10px;min-width:34px}

/* Guía */
.guia-tabs{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:12px;border-bottom:1px solid rgba(160,180,255,.15);padding-bottom:8px}
.guia-tab{padding:6px 12px;border-radius:8px;background:rgba(255,255,255,.04);border:1px solid transparent;cursor:pointer;font-size:12px;font-weight:500;color:var(--txt);opacity:.7}
.guia-tab.active{background:rgba(140,160,255,.2);border-color:var(--acc);opacity:1}
.guia-content{font-size:13px;line-height:1.65}
.guia-content h3{font-size:14px;font-weight:600;color:#c8d2ff;margin-top:14px;margin-bottom:6px}
.guia-content h3:first-child{margin-top:0}
.guia-content p{margin-bottom:8px;opacity:.85}
.guia-content ul,.guia-content ol{padding-left:22px;margin-bottom:10px}
.guia-content li{margin-bottom:4px;opacity:.85}
.guia-content code{background:rgba(0,0,0,.35);padding:2px 6px;border-radius:4px;font-family:ui-monospace,Menlo,monospace;font-size:12px;color:#c0d0ff}
.guia-content table{width:100%;border-collapse:collapse;margin:8px 0;font-size:12px}
.guia-content th,.guia-content td{padding:6px 8px;text-align:left;border-bottom:1px solid rgba(160,180,255,.1)}
.guia-content th{color:#aebfff;font-weight:600;font-size:11px;text-transform:uppercase;letter-spacing:.05em}
.guia-content .tag{display:inline-block;font-size:10px;padding:2px 6px;border-radius:4px;background:rgba(140,160,255,.2);color:#c0d0ff;margin-right:4px}
.guia-content .warn{color:var(--warn)}
.guia-content .ok{color:var(--ok)}

@media(max-width:600px){
  .app{padding:12px;padding-bottom:80px}
  h1{font-size:17px}
  .fila label{min-width:100%;font-size:12px}
  .fila .ctrl{min-width:100%}
  textarea{min-height:120px}
  button{padding:10px 10px;font-size:12px}
  .presets{grid-template-columns:1fr 1fr}
  .tonos-grid{grid-template-columns:1fr 1fr 1fr}
  .mixer{justify-content:flex-start}
  .chan{width:56px}
  .chan input[type=range].vert{height:120px}
}
</style>
</head>
<body>
<div class="app">

  <header>
    <div>
      <h1>Hypno Studio <span class="badge ok">PWA</span></h1>
      <p class="sub">Voz · 11 fuentes de audio · Mezclador · Efectos · Export MP3</p>
    </div>
    <button class="ghost" id="btnGuia" style="flex-shrink:0">📖 Guía</button>
  </header>

  <div id="avisos"></div>

  <!-- PERMISO -->
  <div class="card">
    <h2>Permiso de audio</h2>
    <p style="font-size:12px;opacity:.7;margin-bottom:8px;line-height:1.5">Para exportar el audio necesitamos acceso al micrófono. Pulsa el botón y acepta cuando el navegador lo pida.</p>
    <button class="primary" id="btnPedirPermiso" style="width:100%">🎤 Solicitar permiso de micrófono</button>
    <div class="permiso" id="estadoPermiso">Sin solicitar</div>
  </div>

  <!-- TEXTO -->
  <div class="card">
    <h2>Texto a narrar</h2>
    <textarea id="texto" placeholder="Escribe o pega tu texto aquí..."></textarea>
    <div class="stats">
      <span><span id="charCount">0</span> / 100,000</span>
      <span id="chunkCount">0 bloques</span>
      <span id="duracionEst">~0s</span>
    </div>
    <div class="btn-row" style="margin-top:10px">
      <button class="ghost" id="btnLimpiar">🗑 Limpiar</button>
      <button class="ghost" id="btnEjemplo">📝 Ejemplo</button>
      <button class="ghost" id="btnCargarTxt">📂 .txt</button>
      <input type="file" id="fileTxt" accept=".txt,text/plain" hidden>
    </div>
  </div>

  <!-- VOZ -->
  <div class="card">
    <h2>Idioma y voz <span class="badge" id="contadorVoces">—</span></h2>
    <div class="fila">
      <label for="idioma">Idioma</label>
      <div class="ctrl">
        <select id="idioma">
          <option value="es" selected>Español</option>
          <option value="en">English</option>
        </select>
      </div>
    </div>
    <div class="fila">
      <label for="voz">Voz</label>
      <div class="ctrl"><select id="voz"></select></div>
    </div>
    <div class="fila">
      <button class="ghost" id="btnRecargarVoces" style="flex:1">🔄 Recargar voces</button>
    </div>
  </div>

  <!-- ESTILO LECTURA -->
  <div class="card">
    <h2>Estilo de lectura</h2>
    <div class="presets" id="presets">
      <button class="preset active" data-preset="neutro"><span class="n">Neutro</span><span class="d">Lectura clara</span></button>
      <button class="preset" data-preset="calido"><span class="n">Cálido</span><span class="d">Cercano y pausado</span></button>
      <button class="preset" data-preset="hipnotico"><span class="n">Hipnótico</span><span class="d">Velocidad descendente</span></button>
      <button class="preset" data-preset="susurro"><span class="n">Susurro</span><span class="d">Volumen bajo</span></button>
    </div>
  </div>

  <!-- VOZ AJUSTES -->
  <div class="card">
    <h2>Ajustes de voz</h2>
    <div class="fila">
      <label>Velocidad <span class="badge" id="rateBadge">1×</span></label>
      <div class="ctrl">
        <input type="range" id="rate" min="0.1" max="100" step="0.1" value="0.9">
        <span class="val" id="rateVal">0.9</span>
      </div>
    </div>
    <div class="fila" style="margin-top:-4px">
      <label style="flex:1;font-size:11px;opacity:.6">Atajos:</label>
      <div class="ctrl" style="justify-content:flex-end;gap:4px">
        <button class="mini" data-rate="0.5">0.5×</button>
        <button class="mini" data-rate="1">1×</button>
        <button class="mini" data-rate="2">2×</button>
        <button class="mini" data-rate="5">5×</button>
        <button class="mini" data-rate="10">10×</button>
        <button class="mini" data-rate="25">25×</button>
        <button class="mini" data-rate="100">100×</button>
      </div>
    </div>
    <div class="fila">
      <label>Tono (pitch)</label>
      <div class="ctrl">
        <input type="range" id="pitch" min="0.3" max="2" step="0.05" value="1.0">
        <span class="val" id="pitchVal">1.00</span>
      </div>
    </div>
    <div class="fila">
      <label>Volumen voz</label>
      <div class="ctrl">
        <input type="range" id="vol" min="0" max="1" step="0.02" value="0.9">
        <span class="val" id="volVal">0.90</span>
      </div>
    </div>
    <div class="fila">
      <label>Atenuación <span class="badge" id="atenuacionBadge">0 dB</span></label>
      <div class="ctrl">
        <input type="range" id="atenuacion" min="-60" max="0" step="1" value="0">
        <span class="val" id="atenuacionVal">0 dB</span>
      </div>
    </div>
    <div class="fila">
      <label>Pausa entre bloques</label>
      <div class="ctrl">
        <input type="range" id="pausa" min="0" max="600" step="20" value="180">
        <span class="val" id="pausaVal">180 ms</span>
      </div>
    </div>
    <div class="fila">
      <label>Cantidad (repeticiones)</label>
      <div class="ctrl">
        <div class="qty">
          <button class="mini" id="qtyMinus">−</button>
          <input type="number" id="rep" min="1" max="999" value="1">
          <button class="mini" id="qtyPlus">+</button>
        </div>
      </div>
    </div>
    <div class="fila" style="margin-top:-4px">
      <label style="font-size:11px;opacity:.6">Atajos:</label>
      <div class="ctrl" style="justify-content:flex-end;gap:4px">
        <button class="mini" data-rep="2">x2</button>
        <button class="mini" data-rep="5">x5</button>
        <button class="mini" data-rep="10">x10</button>
        <button class="mini" data-rep="25">x25</button>
        <button class="mini" data-rep="50">x50</button>
        <button class="mini" data-rep="100">x100</button>
      </div>
    </div>
    <div class="fila">
      <label class="chk"><input type="checkbox" id="prosodia" checked> Prosodia inteligente</label>
    </div>
  </div>

  <!-- FUENTES DE AUDIO -->
  <div class="card">
    <h2>Fuente de audio <span class="badge" id="tonoActivo">Ninguno</span></h2>
    <div class="tonos-grid" id="tonosGrid"></div>

    <div class="fila">
      <label>Frecuencia portadora (Hz)</label>
      <div class="ctrl">
        <input type="range" id="carrier" min="20" max="1000" step="1" value="200">
        <span class="val" id="carrierVal">200</span>
      </div>
    </div>
    <div class="fila">
      <label>Beat / pulso (Hz)</label>
      <div class="ctrl">
        <input type="range" id="beat" min="0.5" max="40" step="0.5" value="8">
        <span class="val" id="beatVal">8</span>
      </div>
    </div>
    <div class="fila">
      <label>Preset de frecuencia</label>
      <div class="ctrl">
        <select id="presetFreq">
          <option value="custom">Personalizado</option>
          <option value="delta">Delta · 2 Hz · Sueño profundo</option>
          <option value="theta">Theta · 6 Hz · Meditación</option>
          <option value="alpha">Alpha · 10 Hz · Relajación</option>
          <option value="smr">SMR · 13.5 Hz · Enfoque</option>
          <option value="beta">Beta · 20 Hz · Concentración</option>
          <option value="gamma">Gamma · 40 Hz · Procesamiento</option>
          <option value="solf174">Solfeggio 174 Hz · Alivio dolor</option>
          <option value="solf285">Solfeggio 285 Hz · Regeneración</option>
          <option value="solf396">Solfeggio 396 Hz · Liberar miedo</option>
          <option value="solf417">Solfeggio 417 Hz · Cambio</option>
          <option value="solf528">Solfeggio 528 Hz · Amor/ADN</option>
          <option value="solf639">Solfeggio 639 Hz · Relaciones</option>
          <option value="solf741">Solfeggio 741 Hz · Expresión</option>
          <option value="solf852">Solfeggio 852 Hz · Intuición</option>
          <option value="solf963">Solfeggio 963 Hz · Conciencia</option>
        </select>
      </div>
    </div>

    <details>
      <summary>Guía rápida de frecuencias cerebrales</summary>
      <table style="width:100%;font-size:11px;border-collapse:collapse;margin-top:6px">
        <tr><th style="text-align:left;padding:4px;color:#aebfff">Onda</th><th style="text-align:left;padding:4px;color:#aebfff">Rango</th><th style="text-align:left;padding:4px;color:#aebfff">Estado</th></tr>
        <tr><td style="padding:4px">Delta</td><td style="padding:4px">0.5 – 4 Hz</td><td style="padding:4px">Sueño profundo</td></tr>
        <tr><td style="padding:4px">Theta</td><td style="padding:4px">4 – 8 Hz</td><td style="padding:4px">Meditación, hipnosis</td></tr>
        <tr><td style="padding:4px">Alpha</td><td style="padding:4px">8 – 12 Hz</td><td style="padding:4px">Relajación alerta</td></tr>
        <tr><td style="padding:4px">Beta</td><td style="padding:4px">12 – 30 Hz</td><td style="padding:4px">Concentración</td></tr>
        <tr><td style="padding:4px">Gamma</td><td style="padding:4px">30 – 100 Hz</td><td style="padding:4px">Procesamiento alto</td></tr>
      </table>
    </details>
  </div>

  <!-- MEZCLADOR -->
  <div class="card">
    <h2>Mezclador <span class="badge" id="mezcladorEstado">— canales</span></h2>
    <div class="mixer" id="mixer"></div>
    <div style="font-size:11px;opacity:.6;margin-top:8px;text-align:center">Arrastra los sliders verticales. M = mute, S = solo.</div>
  </div>

  <!-- EFECTOS -->
  <div class="card">
    <h2>Efectos</h2>
    <div class="fila">
      <label class="chk"><input type="checkbox" id="ecoOn"> Eco (delay)</label>
    </div>
    <div class="fila">
      <label>Delay</label>
      <div class="ctrl">
        <input type="range" id="ecoDelay" min="50" max="1000" step="10" value="300">
        <span class="val" id="ecoDelayVal">300 ms</span>
      </div>
    </div>
    <div class="fila">
      <label>Feedback</label>
      <div class="ctrl">
        <input type="range" id="ecoFeedback" min="0" max="0.85" step="0.05" value="0.4">
        <span class="val" id="ecoFeedbackVal">0.40</span>
      </div>
    </div>
    <div class="fila">
      <label>Mezcla eco</label>
      <div class="ctrl">
        <input type="range" id="ecoMix" min="0" max="1" step="0.05" value="0.35">
        <span class="val" id="ecoMixVal">0.35</span>
      </div>
    </div>
    <div class="fila" style="margin-top:12px">
      <label class="chk"><input type="checkbox" id="reverbOn"> Reverb (sala)</label>
    </div>
    <div class="fila">
      <label>Tamaño de sala</label>
      <div class="ctrl">
        <input type="range" id="reverbSize" min="0.1" max="5" step="0.1" value="2">
        <span class="val" id="reverbSizeVal">2.0 s</span>
      </div>
    </div>
    <div class="fila">
      <label>Mezcla reverb</label>
      <div class="ctrl">
        <input type="range" id="reverbMix" min="0" max="1" step="0.05" value="0.25">
        <span class="val" id="reverbMixVal">0.25</span>
      </div>
    </div>
    <div class="fila" style="margin-top:12px">
      <label class="chk"><input type="checkbox" id="coroOn"> Coro (amplitud)</label>
    </div>
    <div class="fila">
      <label>Intensidad</label>
      <div class="ctrl">
        <input type="range" id="coroIntensity" min="0" max="1" step="0.05" value="0.4">
        <span class="val" id="coroIntensityVal">0.40</span>
      </div>
    </div>
  </div>

  <!-- REPRODUCCIÓN -->
  <div class="card">
    <h2>Reproducción</h2>
    <div class="btn-row">
      <button class="primary" id="btnPlay">▶ Reproducir</button>
      <button class="danger" id="btnStop">⏹ Detener</button>
    </div>
    <div id="estado">Listo para reproducir</div>
    <div class="prog" id="prog"><i id="progBar"></i></div>
  </div>

  <!-- EXPORTAR -->
  <div class="card">
    <h2>Exportar audio</h2>
    <p style="font-size:12px;opacity:.7;line-height:1.5;margin-bottom:10px">
      Graba voz + tonos + efectos y descarga el audio. En Android/iOS usa micrófono automáticamente. En desktop intenta captura del sistema y cae a micrófono si falla.
    </p>
    <div class="btn-row">
      <button class="ok" id="btnGrabarMP3">🎙 Grabar y exportar</button>
      <button class="ghost" id="btnDescargarUltimo" disabled>⬇ Último</button>
    </div>
    <details>
      <summary>Ver diagnóstico</summary>
      <div class="diag" id="diag">Iniciando…</div>
    </details>
  </div>

</div>

<!-- MODAL GUÍA -->
<div id="modalGuia" style="display:none;position:fixed;inset:0;z-index:100;background:rgba(0,0,0,.85);backdrop-filter:blur(12px);padding:20px;padding-top:max(20px,env(safe-area-inset-top));overflow-y:auto">
  <div style="max-width:800px;margin:0 auto;background:rgba(15,20,35,.98);border:1px solid rgba(160,180,255,.2);border-radius:16px;padding:20px">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:16px">
      <h2 style="font-size:16px;color:#c8d2ff;font-weight:700">📖 Guía completa</h2>
      <button class="ghost" id="btnCerrarGuia">✕ Cerrar</button>
    </div>
    <div class="guia-tabs" id="guiaTabs">
      <button class="guia-tab active" data-tab="inicio">Inicio</button>
      <button class="guia-tab" data-tab="voz">Voz</button>
      <button class="guia-tab" data-tab="tonos">Tonos</button>
      <button class="guia-tab" data-tab="efectos">Efectos</button>
      <button class="guia-tab" data-tab="mezcla">Mezcla</button>
      <button class="guia-tab" data-tab="export">Exportar</button>
      <button class="guia-tab" data-tab="faq">FAQ</button>
    </div>
    <div class="guia-content" id="guiaContent"></div>
  </div>
</div>

<script>
'use strict';

/* =========================================================================
   UTILIDADES
   ========================================================================= */
const $=id=>document.getElementById(id);
const clamp=(v,a,b)=>v<a?a:v>b?b:v;
const sleep=ms=>new Promise(r=>setTimeout(r,ms));
const dbToGain=db=>Math.pow(10,db/20);
const gainToDb=g=>g<=0?-Infinity:20*Math.log10(g);
const esBrave=navigator.brave?'brave':/Brave/.test(navigator.userAgent)?'brave':'otros';
const esLinux=/Linux/.test(navigator.platform);
const esAndroid=/Android/i.test(navigator.userAgent);
const esIOS=/iPad|iPhone|iPod/.test(navigator.userAgent);
const esLocal=location.protocol==='file:'||location.protocol==='content:';

const DIAG=[];
function diag(msg,tipo){
  DIAG.push({t:Date.now(),msg,tipo});
  const el=$('diag');
  if(el){
    el.innerHTML=DIAG.map(d=>{
      const c=d.tipo==='ok'?'ok':d.tipo==='err'?'err':d.tipo==='warn'?'warn':'';
      return `<span class="${c}">[${new Date(d.t).toLocaleTimeString()}] ${msg}</span>`;
    }).join('\n');
    el.scrollTop=el.scrollHeight;
  }
  console.log(msg);
}

/* =========================================================================
   ESTADO
   ========================================================================= */
const S={
  idioma:'es',vozIdx:0,estilo:'neutro',
  rate:.9,pitch:1.0,vol:.9,atenuacion:0,pausa:180,rep:1,prosodia:true,
  tono:'ninguno',carrier:200,beat:8,
  mixer:{voz:.9,tono:.7,ruido:.5,eco:.35,reverb:.25,coro:.4},
  mute:{voz:false,tono:false,ruido:false,eco:false,reverb:false,coro:false},
  solo:{voz:false,tono:false,ruido:false,eco:false,reverb:false,coro:false},
  eco:{on:false,delay:300,feedback:.4,mix:.35},
  reverb:{on:false,size:2,mix:.25},
  coro:{on:false,intensity:.4}
};

const MAX_CHARS=100000;
const CHUNK_MAX=180;
const CHUNK_IDEAL=140;

/* =========================================================================
   NORMALIZACIÓN Y DIVISIÓN
   ========================================================================= */
function normalizarTexto(txt){
  let t=String(txt||'')
    .replace(/\r\n/g,'\n').replace(/\s+/g,' ')
    .replace(/\s+([,.;:!?])/g,'$1');
  const abrev={'Sr.':'Señor','Sra.':'Señora','Srta.':'Señorita','Dr.':'Doctor','Dra.':'Doctora',
    'Ud.':'Usted','Uds.':'Ustedes','etc.':'etcétera','p.ej.':'por ejemplo',
    'Mr.':'Mister','Mrs.':'Misses','Ms.':'Miss'};
  for(const [k,v] of Object.entries(abrev)) t=t.replace(new RegExp(k.replace(/\./g,'\\.'),'g'),v);
  return t.trim();
}

function dividirEnBloques(txt){
  if(!txt) return [];
  const frases=txt.split(/(?<=[.!?;])\s+/).filter(s=>s.trim().length);
  const bloques=[];
  for(const frase of frases){
    const f=frase.trim();
    if(f.length<=CHUNK_MAX){bloques.push(f);continue}
    const subpartes=f.split(/(?<=,)\s+/);
    let buffer='';
    for(const sp of subpartes){
      const candidato=buffer?buffer+' '+sp:sp;
      if(candidato.length<=CHUNK_IDEAL){buffer=candidato}
      else{
        if(buffer) bloques.push(buffer);
        if(sp.length>CHUNK_MAX){
          const palabras=sp.split(' ');
          let linea='';
          for(const p of palabras){
            const cand=linea?linea+' '+p:p;
            if(cand.length<=CHUNK_IDEAL){linea=cand}
            else{if(linea) bloques.push(linea); linea=p}
          }
          buffer=linea;
        }else{buffer=sp}
      }
    }
    if(buffer) bloques.push(buffer);
  }
  return bloques.filter(b=>b.trim().length);
}

function enriquecerPuntuacion(b,i,t){if(!/[.!?…,;:]$/.test(b.trim())) b+=i===t-1?'.':',';return b}

function calcularProsodia(bloque,i,total,estilo,base){
  const esPregunta=/\?$/.test(bloque);
  const esExclam=/[!]$/.test(bloque);
  const esFinal=i===total-1;
  const esInicio=i===0;
  let rate=base.rate,pitch=base.pitch,pausa=base.pausa;
  if(estilo==='hipnotico'){
    const p=total>1?i/(total-1):0;
    rate=base.rate-p*0.25;
    pitch=base.pitch-p*0.08;
    pausa=base.pausa+p*400;
  }else if(estilo==='calido'){rate=base.rate*0.95;pitch=base.pitch*1.02;pausa=base.pausa+80}
  else if(estilo==='susurro'){rate=base.rate*0.85;pitch=base.pitch*1.05;pausa=base.pausa+150}
  if(esPregunta){pitch*=1.08;rate*=1.02}
  else if(esExclam){pitch*=1.05;rate*=1.05}
  if(esFinal){pitch*=0.97;rate*=0.98}
  if(esInicio){rate*=0.98}
  return {rate:clamp(rate,0.1,100),pitch:clamp(pitch,0.3,2),pausa:clamp(pausa,0,1200)};
}

/* =========================================================================
   VOCES
   ========================================================================= */
const PRIORIDAD={
  es:['es-MX','es-419','es-US','es-ES','es-AR','es-CO','es-CL','es'],
  en:['en-US','en-GB','en-AU','en-CA','en-IN','en']
};
let voces=[];
function cargarVoces(){
  return new Promise(resolve=>{
    if(!('speechSynthesis' in window)) return resolve([]);
    const intentar=()=>{
      const v=speechSynthesis.getVoices();
      if(v.length){voces=v;diag(`Voces: ${v.length}`,'ok');resolve(v);return true}
      return false;
    };
    if(intentar()) return;
    let tries=0;
    const tick=()=>{
      if(intentar()) return;
      if(tries++>25){diag('Timeout voces','warn');return resolve([])}
      setTimeout(tick,200);
    };
    speechSynthesis.onvoiceschanged=()=>intentar();
    tick();
  });
}
function elegirVoz(idioma){
  if(!voces.length) return null;
  const prefs=PRIORIDAD[idioma]||[idioma];
  for(const cod of prefs){const v=voces.find(x=>x.lang.replace('_','-')===cod);if(v) return v}
  const pref=idioma.slice(0,2);
  return voces.find(x=>x.lang.toLowerCase().startsWith(pref))||voces[0];
}
function rellenarVocesUI(){
  const sel=$('voz');
  const idioma=$('idioma').value;
  const prefs=PRIORIDAD[idioma]||[idioma];
  const ordenadas=[...voces].sort((a,b)=>{
    const ai=prefs.findIndex(p=>a.lang.toLowerCase().startsWith(p.toLowerCase().split('-')[0]));
    const bi=prefs.findIndex(p=>b.lang.toLowerCase().startsWith(p.toLowerCase().split('-')[0]));
    return (ai<0?99:ai)-(bi<0?99:bi);
  });
  if(!ordenadas.length){sel.innerHTML='<option>— sin voces —</option>';return}
  sel.innerHTML=ordenadas.map(v=>{
    const idx=voces.indexOf(v);
    const marca=v.lang.startsWith('es-MX')?' ⭐MX':v.lang==='es-419'?' 🌎LatAm':v.lang==='es-US'?' 🇺🇸US':'';
    return `<option value="${idx}">${v.name} (${v.lang})${marca}</option>`;
  }).join('');
  const auto=elegirVoz(idioma);
  if(auto){const idx=voces.indexOf(auto);if(idx>=0){sel.value=idx;S.vozIdx=idx}}
  $('contadorVoces').textContent=`${voces.length} voces`;
}

/* =========================================================================
   AUDIO — GRAFO PRINCIPAL
   ========================================================================= */
let ac=null;
const buses={};
let tonoNodes=null;
let ecoNodes=null;
let reverbNodes=null;
let coroNodes=null;

function initAudio(){
  if(ac) return;
  ac=new (window.AudioContext||window.webkitAudioContext)({latencyHint:'interactive'});

  // Buses por canal
  const master=ac.createGain();master.gain.value=1;
  const comp=ac.createDynamicsCompressor();
  comp.threshold.value=-6;comp.knee.value=6;comp.ratio.value=12;
  comp.attack.value=.003;comp.release.value=.25;

  // Buses individuales
  buses.voz=ac.createGain();
  buses.tono=ac.createGain();
  buses.ruido=ac.createGain();
  buses.eco=ac.createGain();
  buses.reverb=ac.createGain();
  buses.coro=ac.createGain();

  buses.voz.connect(master);
  buses.tono.connect(master);
  buses.ruido.connect(master);
  buses.eco.connect(master);
  buses.reverb.connect(master);
  buses.coro.connect(master);

  // Aplicar valores de mezcla
  for(const k of Object.keys(buses)){
    buses[k].gain.value=S.mixer[k]??.5;
  }

  master.connect(comp);
  comp.connect(ac.destination);

  // Destino para grabación
  const recDest=ac.createMediaStreamDestination();
  comp.connect(recDest);
  buses.recDest=recDest;

  // Efectos: crear cadena eco -> reverb -> coro -> master
  crearEco();
  crearReverb();
  crearCoro();

  diag(`AudioContext @ ${ac.sampleRate} Hz`,'ok');
}

function actualizarMezcla(){
  if(!ac) return;
  for(const k of Object.keys(buses)){
    if(k==='recDest') continue;
    const muted=S.mute[k];
    const soloActivo=Object.values(S.solo).some(v=>v);
    const soloEste=S.solo[k];
    const audible=soloActivo?soloEste:!muted;
    const target=audible?(S.mixer[k]??.5):0;
    buses[k].gain.setTargetAtTime(target,ac.currentTime,.05);
  }
}

/* =========================================================================
   EFECTOS
   ========================================================================= */
function crearEco(){
  if(!ac) return;
  if(ecoNodes){ecoNodes.delay.disconnect();ecoNodes.fb.disconnect();ecoNodes.wet.disconnect()}
  const delay=ac.createDelay(2);
  delay.delayTime.value=S.eco.delay/1000;
  const fb=ac.createGain();
  fb.gain.value=S.eco.feedback;
  const wet=ac.createGain();
  wet.gain.value=S.eco.on?S.eco.mix:0;

  // La entrada del eco viene del bus master (post compresor): simplificamos
  // Inyectamos eco desde el bus de voz (típico uso)
  delay.connect(fb);
  fb.connect(delay);
  delay.connect(wet);
  wet.connect(buses.eco);

  ecoNodes={delay,fb,wet};
  diag('Eco creado','ok');
}

function actualizarEco(){
  if(!ecoNodes||!ac) return;
  ecoNodes.delay.delayTime.setTargetAtTime(S.eco.delay/1000,ac.currentTime,.05);
  ecoNodes.fb.gain.setTargetAtTime(S.eco.feedback,ac.currentTime,.05);
  ecoNodes.wet.gain.setTargetAtTime(S.eco.on?S.eco.mix:0,ac.currentTime,.05);
}

function crearReverb(){
  if(!ac) return;
  if(reverbNodes){reverbNodes.convolver.disconnect();reverbNodes.wet.disconnect()}
  const convolver=ac.createConvolver();
  convolver.buffer=crearImpulso(S.reverb.size);
  const wet=ac.createGain();
  wet.gain.value=S.reverb.on?S.reverb.mix:0;
  convolver.connect(wet);
  wet.connect(buses.reverb);
  reverbNodes={convolver,wet};
  diag('Reverb creado','ok');
}

function crearImpulso(segundos){
  const sr=ac.sampleRate;
  const len=Math.floor(sr*segundos);
  const buf=ac.createBuffer(2,len,sr);
  for(let c=0;c<2;c++){
    const d=buf.getChannelData(c);
    for(let i=0;i<len;i++){
      const t=i/len;
      d[i]=(Math.random()*2-1)*Math.pow(1-t,2.5);
    }
  }
  return buf;
}

function actualizarReverb(){
  if(!reverbNodes||!ac) return;
  if(Math.abs(S.reverb.size-reverbNodes.sizeCache)>0.05){
    reverbNodes.convolver.buffer=crearImpulso(S.reverb.size);
    reverbNodes.sizeCache=S.reverb.size;
  }
  reverbNodes.wet.gain.setTargetAtTime(S.reverb.on?S.reverb.mix:0,ac.currentTime,.05);
}

function crearCoro(){
  if(!ac) return;
  if(coroNodes){coroNodes.delays.forEach(d=>d.disconnect());coroNodes.wet.disconnect()}
  const wet=ac.createGain();
  wet.gain.value=S.coro.on?S.coro.intensity:0;
  const delays=[];
  for(let i=0;i<3;i++){
    const d=ac.createDelay(0.1);
    d.delayTime.value=0.02+i*0.008;
    const g=ac.createGain();
    g.gain.value=1;
    d.connect(g);g.connect(wet);
    delays.push(d);
  }
  wet.connect(buses.coro);
  coroNodes={delays,wet};
  diag('Coro creado','ok');
}

function actualizarCoro(){
  if(!coroNodes||!ac) return;
  coroNodes.wet.gain.setTargetAtTime(S.coro.on?S.coro.intensity:0,ac.currentTime,.05);
  coroNodes.delays.forEach((d,i)=>{
    d.delayTime.setTargetAtTime(0.02+i*0.008*S.coro.intensity,ac.currentTime,.05);
  });
}

/* Router: conecta una fuente al bus + efectos */
function conectarFuente(node,busName,conEfectos=false){
  node.connect(buses[busName]);
  if(conEfectos){
    if(ecoNodes) node.connect(ecoNodes.delay);
    if(reverbNodes) node.connect(reverbNodes.convolver);
    if(coroNodes&&coroNodes.delays[0]) coroNodes.delays.forEach(d=>node.connect(d));
  }
}

/* =========================================================================
   FUENTES DE AUDIO — 11 tipos
   ========================================================================= */
function stopTono(){
  if(!tonoNodes) return;
  try{for(const n of tonoNodes.all){
    if(n.stop) try{n.stop()}catch(e){}
    if(n.disconnect) try{n.disconnect()}catch(e){}
  }}catch(e){}
  tonoNodes=null;
}

function startTono(){
  initAudio();
  if(!ac) return;
  if(ac.state==='suspended') ac.resume().catch(()=>{});
  stopTono();

  const tipo=S.tono;
  if(tipo==='ninguno') return;

  const carrier=S.carrier;
  const beat=S.beat;
  const all=[];
  const conEfectos=true;

  if(tipo==='binaural'){
    const merger=ac.createChannelMerger(2);
    const l=ac.createOscillator();l.type='sine';l.frequency.value=carrier-beat/2;
    const r=ac.createOscillator();r.type='sine';r.frequency.value=carrier+beat/2;
    l.connect(merger,0,0);r.connect(merger,0,1);
    merger.connect(buses.tono);
    l.start();r.start();
    all.push(l,r,merger);
    diag(`Binaural ${carrier-beat/2}/${carrier+beat/2} Hz`,'ok');
  }
  else if(tipo==='isocronico'){
    const c=ac.createOscillator();c.type='sine';c.frequency.value=carrier;
    const gate=ac.createGain();gate.gain.value=0;
    const mod=ac.createOscillator();mod.type='square';mod.frequency.value=beat;
    const mg=ac.createGain();mg.gain.value=1;
    mod.connect(mg);mg.connect(gate.gain);
    c.connect(gate);gate.connect(buses.tono);
    c.start();mod.start();
    all.push(c,mod,gate,mg);
    diag(`Isocrónico ${carrier} Hz @ ${beat} Hz`,'ok');
  }
  else if(tipo==='cuadrada'){
    const c=ac.createOscillator();c.type='square';c.frequency.value=carrier;
    const g=ac.createGain();g.gain.value=0.35;
    c.connect(g);g.connect(buses.tono);
    c.start();all.push(c,g);
    diag(`Cuadrada ${carrier} Hz`,'ok');
  }
  else if(tipo==='triangular'){
    const c=ac.createOscillator();c.type='triangle';c.frequency.value=carrier;
    const g=ac.createGain();g.gain.value=0.5;
    c.connect(g);g.connect(buses.tono);
    c.start();all.push(c,g);
    diag(`Triangular ${carrier} Hz`,'ok');
  }
  else if(tipo==='sierra'){
    const c=ac.createOscillator();c.type='sawtooth';c.frequency.value=carrier;
    const g=ac.createGain();g.gain.value=0.3;
    const lp=ac.createBiquadFilter();lp.type='lowpass';lp.frequency.value=carrier*4;
    c.connect(lp);lp.connect(g);g.connect(buses.tono);
    c.start();all.push(c,lp,g);
    diag(`Sierra ${carrier} Hz`,'ok');
  }
  else if(tipo==='hipnotico'){
    const c=ac.createOscillator();c.type='sine';c.frequency.value=carrier;
    const vib=ac.createOscillator();vib.type='sine';vib.frequency.value=0.15;
    const vg=ac.createGain();vg.gain.value=carrier*0.02;
    vib.connect(vg);vg.connect(c.frequency);
    const trem=ac.createOscillator();trem.type='sine';trem.frequency.value=beat;
    const tg=ac.createGain();tg.gain.value=0.4;
    const base=ac.createGain();base.gain.value=0.6;
    trem.connect(tg);tg.connect(base.gain);
    const out=ac.createGain();out.gain.value=0.5;
    c.connect(base);base.connect(out);out.connect(buses.tono);
    c.start();vib.start();trem.start();
    all.push(c,vib,vg,trem,tg,base,out);
    diag(`Hipnótico ${carrier} Hz`,'ok');
  }
  else if(tipo==='blanco'){
    const bufSize=2*ac.sampleRate;
    const buf=ac.createBuffer(2,bufSize,ac.sampleRate);
    for(let ch=0;ch<2;ch++){
      const d=buf.getChannelData(ch);
      for(let i=0;i<bufSize;i++) d[i]=Math.random()*2-1;
    }
    const src=ac.createBufferSource();src.buffer=buf;src.loop=true;
    const g=ac.createGain();g.gain.value=0.3;
    src.connect(g);g.connect(buses.ruido);
    src.start();all.push(src,g);
    diag('Ruido blanco','ok');
  }
  else if(tipo==='rosa'){
    const bufSize=4*ac.sampleRate;
    const buf=ac.createBuffer(2,bufSize,ac.sampleRate);
    for(let ch=0;ch<2;ch++){
      const d=buf.getChannelData(ch);
      let b0=0,b1=0,b2=0,b3=0,b4=0,b5=0,b6=0;
      for(let i=0;i<bufSize;i++){
        const w=Math.random()*2-1;
        b0=0.99886*b0+w*0.0555179;
        b1=0.99332*b1+w*0.0750759;
        b2=0.96900*b2+w*0.1538520;
        b3=0.86650*b3+w*0.3104856;
        b4=0.55000*b4+w*0.5329522;
        b5=-0.7616*b5-w*0.0168980;
        d[i]=(b0+b1+b2+b3+b4+b5+b6+w*0.5362)*0.11;
        b6=w*0.115926;
      }
    }
    const src=ac.createBufferSource();src.buffer=buf;src.loop=true;
    const g=ac.createGain();g.gain.value=0.4;
    src.connect(g);g.connect(buses.ruido);
    src.start();all.push(src,g);
    diag('Ruido rosa','ok');
  }
  else if(tipo==='marron'){
    const bufSize=4*ac.sampleRate;
    const buf=ac.createBuffer(2,bufSize,ac.sampleRate);
    for(let ch=0;ch<2;ch++){
      const d=buf.getChannelData(ch);
      let last=0;
      for(let i=0;i<bufSize;i++){
        const w=Math.random()*2-1;
        last=(last+0.02*w)/1.02;
        d[i]=last*3.5;
      }
    }
    const src=ac.createBufferSource();src.buffer=buf;src.loop=true;
    const g=ac.createGain();g.gain.value=0.5;
    src.connect(g);g.connect(buses.ruido);
    src.start();all.push(src,g);
    diag('Ruido marrón','ok');
  }
  else if(tipo==='solfeggio'){
    const freqs=[174,285,396,417,528,639,741,852,963];
    const idx=freqs.indexOf(carrier)>=0?freqs.indexOf(carrier):4;
    const baseFreq=freqs[idx]||528;
    freqs.forEach((f,i)=>{
      const c=ac.createOscillator();c.type='sine';c.frequency.value=f;
      const g=ac.createGain();g.gain.value=0.08/(i*0.3+1);
      c.connect(g);g.connect(buses.tono);
      c.start();all.push(c,g);
    });
    diag(`Solfeggio ${baseFreq} Hz`,'ok');
  }
  else if(tipo==='shepard'){
    const octavas=6;
    const baseFreq=Math.max(40,carrier/4);
    for(let i=0;i<octavas;i++){
      const f=baseFreq*Math.pow(2,i);
      if(f>12000) continue;
      const c=ac.createOscillator();c.type='sine';c.frequency.value=f;
      const g=ac.createGain();g.gain.value=0.15/octavas;
      c.connect(g);g.connect(buses.tono);
      c.start();all.push(c,g);
    }
    diag(`Shepard ${baseFreq} Hz base`,'ok');
  }

  tonoNodes={tipo,all};
}

/* =========================================================================
   TTS
   ========================================================================= */
let cancelarTTS=false;
function setEstado(txt,tipo){const e=$('estado');e.textContent=txt;e.className=tipo||''}

function detenerTTS(){
  cancelarTTS=true;
  try{speechSynthesis.cancel()}catch(e){}
  setEstado('Detenido');
  $('prog').style.display='none';
}

function hablarUna(u){
  return new Promise(resolve=>{
    let done=false;
    const safe=()=>{if(!done){done=true;resolve()}};
    const ms=Math.max(3000,u.text.length/30*1000/Math.max(.1,u.rate)+2000);
    const timer=setTimeout(()=>{diag('Timeout utterance','warn');safe()},ms);
    u.onend=()=>{clearTimeout(timer);safe()};
    u.onerror=e=>{clearTimeout(timer);if(e.error!=='canceled'&&e.error!=='interrupted') diag('Err TTS: '+e.error,'err');safe()};
    try{speechSynthesis.speak(u)}catch(e){clearTimeout(timer);safe()}
  });
}

async function reproducir(){
  detenerTTS();
  cancelarTTS=false;
  const texto=normalizarTexto($('texto').value);
  if(!texto){setEstado('Escribe algo primero','warn');return}

  const voz=voces[S.vozIdx]||elegirVoz(S.idioma);
  const idioma=S.idioma;
  const rep=clamp(+$('rep').value||1,1,999);
  const bloquesBase=dividirEnBloques(texto);
  if(!bloquesBase.length) return;
  const bloques=bloquesBase.map((b,i)=>enriquecerPuntuacion(b,i,bloquesBase.length));
  const total=bloques.length;

  // Ganancia de voz con atenuación en dB
  const vozGain=dbToGain(S.atenuacion);

  setEstado(`Reproduciendo… 0 / ${total*rep}`,'hablando');
  $('prog').style.display='block';
  $('progBar').style.width='0%';

  let contador=0;
  const totalUtter=total*rep;

  for(let r=0;r<rep;r++){
    for(let i=0;i<total;i++){
      if(cancelarTTS) break;
      const bloque=bloques[i];
      const p=S.prosodia
        ? calcularProsodia(bloque,i,total,S.estilo,{rate:S.rate,pitch:S.pitch,pausa:S.pausa})
        : {rate:S.rate,pitch:S.pitch,pausa:S.pausa};

      if(contador>0&&p.pausa>0){
        await sleep(p.pausa);
        if(cancelarTTS) break;
      }

      const u=new SpeechSynthesisUtterance(bloque);
      if(voz) u.voice=voz;
      u.lang=voz?voz.lang:(PRIORIDAD[idioma]?.[0]||idioma);
      u.rate=clamp(p.rate,0.1,100);
      u.pitch=clamp(p.pitch,0.3,2);
      u.volume=clamp(S.vol*vozGain,0,1);

      await hablarUna(u);
      if(cancelarTTS) break;
      contador++;
      $('progBar').style.width=Math.round(contador/totalUtter*100)+'%';
      setEstado(`Reproduciendo… ${contador} / ${totalUtter}`,'hablando');
    }
    if(cancelarTTS) break;
  }

  if(!cancelarTTS){
    setEstado('Lectura completada ✓','ok');
    $('progBar').style.width='100%';
    setTimeout(()=>{$('prog').style.display='none'},1500);
  }
}

/* =========================================================================
   EXPORTACIÓN
   ========================================================================= */
let ultimoBlob=null;
let grabando=false;
let permisoMic=false;

async function pedirPermisoMic(){
  try{
    const s=await navigator.mediaDevices.getUserMedia({audio:true});
    s.getTracks().forEach(t=>t.stop());
    permisoMic=true;
    $('estadoPermiso').textContent='✓ Permiso concedido';
    $('estadoPermiso').className='permiso ok';
    diag('Micrófono OK','ok');
  }catch(err){
    permisoMic=false;
    $('estadoPermiso').textContent='✗ '+err.message;
    $('estadoPermiso').className='permiso err';
    diag('Micrófono denegado: '+err.message,'err');
  }
}

function elegirMimeAudio(){
  const c=['audio/webm;codecs=opus','audio/webm','audio/mp4','audio/ogg;codecs=opus'];
  for(const m of c){if(window.MediaRecorder&&MediaRecorder.isTypeSupported(m)) return m}
  return null;
}

async function grabarYExportar(){
  if(grabando){setEstado('Ya grabando','warn');return}
  const texto=normalizarTexto($('texto').value);
  if(!texto){setEstado('Escribe algo','warn');return}
  if(!permisoMic){setEstado('Pulsa "Solicitar permiso de micrófono" primero','warn');return}

  let usarMic=esAndroid||esIOS;
  let stream=null;

  if(!usarMic&&navigator.mediaDevices?.getDisplayMedia){
    try{
      const s=await navigator.mediaDevices.getDisplayMedia({video:true,audio:{channelCount:2,sampleRate:44100,echoCancellation:false,noiseSuppression:false,autoGainControl:false}});
      const at=s.getAudioTracks();
      if(at.length){s.getVideoTracks().forEach(t=>t.stop());stream=new MediaStream(at);diag('Captura sistema OK','ok')}
      else{s.getTracks().forEach(t=>t.stop());usarMic=true;diag('Sin audio en captura → micrófono','warn')}
    }catch(err){diag('Captura falló: '+err.message+' → micrófono','warn');usarMic=true}
  }

  if(usarMic){
    try{
      stream=await navigator.mediaDevices.getUserMedia({audio:{echoCancellation:false,noiseSuppression:false,autoGainControl:false,channelCount:2,sampleRate:44100}});
      diag('Micrófono OK','ok');
    }catch(err){setEstado('Micrófono: '+err.message,'err');return}
  }

  const mime=elegirMimeAudio();
  if(!mime){stream.getTracks().forEach(t=>t.stop());setEstado('MediaRecorder no soportado','err');return}

  const chunks=[];
  let rec;
  try{rec=new MediaRecorder(stream,{mimeType:mime,audioBitsPerSecond:192000})}
  catch(e){rec=new MediaRecorder(stream)}

  rec.ondataavailable=e=>{if(e.data.size) chunks.push(e.data)};
  grabando=true;$('btnGrabarMP3').disabled=true;

  if(S.tono!=='ninguno') startTono();

  setEstado(`🔴 Grabando (${usarMic?'micrófono':'sistema'}) — reproduciendo…`,'grabando');
  rec.start(500);

  try{await reproducir();await sleep(800)}catch(e){diag('Err reproducción: '+e.message,'err')}

  await new Promise(resolve=>{rec.onstop=resolve;if(rec.state!=='inactive') try{rec.stop()}catch(e){resolve()}});

  grabando=false;
  stream.getTracks().forEach(t=>t.stop());
  stopTono();
  $('btnGrabarMP3').disabled=false;

  if(!chunks.length){setEstado('Sin audio capturado','err');return}

  const blobOrig=new Blob(chunks,{type:mime.split(';')[0]});
  diag(`Grabado: ${(blobOrig.size/1024).toFixed(1)} KB`,'ok');
  setEstado('Convirtiendo a MP3…','warn');

  let final=blobOrig,ext='webm';
  try{
    const mp3=await convertirAMP3(blobOrig);
    if(mp3&&mp3.size>0){final=mp3;ext='mp3';diag('MP3 OK','ok')}
  }catch(err){diag('MP3 falló: '+err.message,'warn')}

  ultimoBlob=final;
  $('btnDescargarUltimo').disabled=false;
  descargarBlob(final,`hypno-${Date.now()}.${ext}`);
  setEstado(`✓ Exportado (${(final.size/1024).toFixed(0)} KB, .${ext})`,'ok');
}

async function convertirAMP3(blob){
  if(typeof lamejs==='undefined') throw new Error('lamejs no cargado');
  const ab=await blob.arrayBuffer();
  const c=new (window.AudioContext||window.webkitAudioContext)();
  let buf;
  try{buf=await c.decodeAudioData(ab.slice(0))}catch(e){await c.close();throw new Error('decodeAudioData falló')}
  await c.close();

  const numCh=Math.min(2,buf.numberOfChannels);
  const sr=buf.sampleRate;
  const len=buf.length;
  const L=new Int16Array(len),R=numCh>1?new Int16Array(len):null;
  const sL=buf.getChannelData(0),sR=numCh>1?buf.getChannelData(1):null;
  for(let i=0;i<len;i++){
    const l=clamp(sL[i],-1,1);L[i]=l<0?l*0x8000:l*0x7FFF;
    if(R&&sR){const r=clamp(sR[i],-1,1);R[i]=r<0?r*0x8000:r*0x7FFF}
  }
  const enc=new lamejs.Mp3Encoder(numCh,sr,192);
  const out=[];
  const bs=1152;
  for(let i=0;i<len;i+=bs){
    const l=L.subarray(i,i+bs);
    const r=R?R.subarray(i,i+bs):undefined;
    const b=numCh>1?enc.encodeBuffer(l,r):enc.encodeBuffer(l);
    if(b.length>0) out.push(new Int8Array(b));
  }
  const f=enc.flush();
  if(f.length>0) out.push(new Int8Array(f));
  return new Blob(out,{type:'audio/mp3'});
}

function descargarBlob(blob,nombre){
  const u=URL.createObjectURL(blob);
  const a=document.createElement('a');
  a.href=u;a.download=nombre;
  document.body.appendChild(a);a.click();a.remove();
  setTimeout(()=>URL.revokeObjectURL(u),60000);
}

/* =========================================================================
   MEZCLADOR UI
   ========================================================================= */
function renderMixer(){
  const canales=[
    {k:'voz',n:'Voz'},
    {k:'tono',n:'Tono'},
    {k:'ruido',n:'Ruido'},
    {k:'eco',n:'Eco'},
    {k:'reverb',n:'Reverb'},
    {k:'coro',n:'Coro'}
  ];
  const html=canales.map(c=>{
    const v=S.mixer[c.k];
    const db=v>0?(20*Math.log10(v)).toFixed(1):'-∞';
    return `<div class="chan">
      <div class="name">${c.n}</div>
      <input type="range" class="vert" data-ch="${c.k}" min="0" max="1" step="0.02" value="${v}">
      <div class="db" id="db_${c.k}">${db} dB</div>
      <button class="mute${S.mute[c.k]?' on':''}" data-mute="${c.k}">M</button>
      <button class="solo${S.solo[c.k]?' on':''}" data-solo="${c.k}">S</button>
    </div>`;
  }).join('');
  $('mixer').innerHTML=html;
  $('mezcladorEstado').textContent=`${canales.length} canales`;

  $('mixer').querySelectorAll('input[data-ch]').forEach(el=>{
    el.oninput=()=>{
      const k=el.dataset.ch;
      S.mixer[k]=+el.value;
      const db=S.mixer[k]>0?(20*Math.log10(S.mixer[k])).toFixed(1):'-∞';
      const dEl=el.parentElement.querySelector('.db');
      if(dEl) dEl.textContent=`${db} dB`;
      actualizarMezcla();
    };
  });
  $('mixer').querySelectorAll('button[data-mute]').forEach(el=>{
    el.onclick=()=>{
      const k=el.dataset.mute;
      S.mute[k]=!S.mute[k];
      el.classList.toggle('on',S.mute[k]);
      actualizarMezcla();
    };
  });
  $('mixer').querySelectorAll('button[data-solo]').forEach(el=>{
    el.onclick=()=>{
      const k=el.dataset.solo;
      S.solo[k]=!S.solo[k];
      el.classList.toggle('on',S.solo[k]);
      actualizarMezcla();
    };
  });
}

/* =========================================================================
   TONOS UI
   ========================================================================= */
const TONOS=[
  {k:'ninguno',n:'Ninguno',d:'Solo voz'},
  {k:'binaural',n:'Binaural',d:'Auriculares'},
  {k:'isocronico',n:'Isocrónico',d:'Pulso on/off'},
  {k:'cuadrada',n:'Cuadrada',d:'Square wave'},
  {k:'triangular',n:'Triangular',d:'Suave'},
  {k:'sierra',n:'Sierra',d:'Sawtooth'},
  {k:'hipnotico',n:'Hipnótico',d:'Vibrato+AM'},
  {k:'blanco',n:'Ruido blanco',d:'Uniforme'},
  {k:'rosa',n:'Ruido rosa',d:'Suave'},
  {k:'marron',n:'Ruido marrón',d:'Grave'},
  {k:'solfeggio',n:'Solfeggio',d:'9 frecuencias'},
  {k:'shepard',n:'Shepard',d:'Tono infinito'}
];

function renderTonos(){
  $('tonosGrid').innerHTML=TONOS.map(t=>
    `<button class="tono-btn${S.tono===t.k?' active':''}" data-tono="${t.k}">
      <span class="n">${t.n}</span><span class="d">${t.d}</span>
    </button>`
  ).join('');
  $('tonosGrid').querySelectorAll('.tono-btn').forEach(el=>{
    el.onclick=()=>{
      S.tono=el.dataset.tono;
      $('tonosGrid').querySelectorAll('.tono-btn').forEach(x=>x.classList.toggle('active',x===el));
      const t=TONOS.find(x=>x.k===S.tono);
      $('tonoActivo').textContent=t?t.n:'Ninguno';
      if(S.tono==='ninguno'){stopTono()}
      else{initAudio();if(ac&&ac.state==='suspended') ac.resume().catch(()=>{});startTono()}
    };
  });
}

/* =========================================================================
   GUÍA
   ========================================================================= */
const GUIA={
  inicio:`<h3>Bienvenido a Hypno Studio</h3>
    <p>Una app todo-en-uno para generar audio hipnótico, meditaciones guiadas o locuciones con ambiente sonoro.</p>
    <h3>Flujo básico</h3>
    <ol>
      <li>Concede permiso de micrófono (necesario para exportar).</li>
      <li>Pega o escribe tu texto.</li>
      <li>Elige voz y estilo de lectura.</li>
      <li>Selecciona una fuente de audio de fondo (binaural, isocrónico, ruido...).</li>
      <li>Ajusta el mezclador y los efectos.</li>
      <li>Pulsa <b>▶ Reproducir</b> para escuchar, o <b>🎙 Grabar y exportar</b> para guardar MP3.</li>
    </ol>
    <h3>Requisitos recomendados</h3>
    <ul>
      <li>Navegador: Chrome, Edge o Brave actualizado.</li>
      <li>Auriculares para el modo binaural.</li>
      <li>HTTPS para que funcionen permisos y PWA.</li>
    </ul>`,

  voz:`<h3>Voz</h3>
    <p>La app usa el motor TTS del sistema operativo. La calidad depende de las voces instaladas.</p>
    <h3>Idiomas soportados</h3>
    <p>Solo español e inglés. Para español, prioriza <code>es-MX</code> (México), <code>es-419</code> (Latinoamérica) o <code>es-US</code>.</p>
    <h3>Cómo instalar es-MX</h3>
    <ul>
      <li><b>Android:</b> Ajustes → Sistema → Idiomas y entrada → Salida de texto a voz → Motor de Google → Instalar datos de voz → Español (México).</li>
      <li><b>Windows:</b> Configuración → Hora e idioma → Voz → Agregar voces → Español (México).</li>
      <li><b>macOS/iOS:</b> Ajustes → Accesibilidad → Contenido leído → Voces → Español → Mónica (México).</li>
    </ul>
    <h3>Velocidad</h3>
    <p>El slider va de <b>0.1× a 100×</b>. La mayoría de motores aceptan hasta 10× con calidad; por encima, el motor satura y el audio se distorsiona. Usa los atajos <code>0.5× 1× 2× 5× 10× 25× 100×</code> para saltar.</p>
    <h3>Atenuación (dB)</h3>
    <p>Reduce el volumen de la voz de 0 a -60 dB. Útil para mezclar voz suave con tonos fuertes. -30 dB = 3% del volumen original.</p>
    <h3>Prosodia inteligente</h3>
    <p>Divide el texto en bloques fonológicos, ajusta pitch/rate por tipo de frase y añade micro-pausas. Desactívala si prefieres lectura plana.</p>
    <h3>Cantidad</h3>
    <p>Repite todo el texto N veces. Con los botones +/− o los atajos x2, x5, x10, x25, x50, x100.</p>`,

  tonos:`<h3>Las 12 fuentes de audio</h3>
    <table>
      <tr><th>Fuente</th><th>Qué hace</th><th>Uso</th></tr>
      <tr><td>Ninguno</td><td>Solo voz</td><td>Locución limpia</td></tr>
      <tr><td>Binaural</td><td>2 senos, frec. ligeramente distintas por canal</td><td>Meditación (auriculares)</td></tr>
      <tr><td>Isocrónico</td><td>Tono encendido/apagado a N Hz</td><td>Enfoque, sin auriculares</td></tr>
      <tr><td>Cuadrada</td><td>Onda square pura</td><td>Trance, estados alterados</td></tr>
      <tr><td>Triangular</td><td>Onda triangle</td><td>Transiciones suaves</td></tr>
      <tr><td>Sierra</td><td>Onda sawtooth filtrada</td><td>Efectos psicoacústicos</td></tr>
      <tr><td>Hipnótico</td><td>Seno + vibrato lento + AM</td><td>Inducción hipnótica</td></tr>
      <tr><td>Ruido blanco</td><td>Aleatorio uniforme</td><td>Enmascarar ambiente</td></tr>
      <tr><td>Ruido rosa</td><td>Espectro 1/f</td><td>Relajación, tinnitus</td></tr>
      <tr><td>Ruido marrón</td><td>Integración del blanco</td><td>Sueño, concentración</td></tr>
      <tr><td>Solfeggio</td><td>9 senos a frecuencias sagradas</td><td>Sanación, meditación</td></tr>
      <tr><td>Shepard</td><td>Capas en octavas</td><td>Ilusión de tono infinito</td></tr>
    </table>
    <h3>Frecuencias cerebrales</h3>
    <table>
      <tr><th>Onda</th><th>Rango</th><th>Estado</th></tr>
      <tr><td>Delta</td><td>0.5 – 4 Hz</td><td>Sueño profundo</td></tr>
      <tr><td>Theta</td><td>4 – 8 Hz</td><td>Meditación, hipnosis</td></tr>
      <tr><td>Alpha</td><td>8 – 12 Hz</td><td>Relajación alerta</td></tr>
      <tr><td>Beta</td><td>12 – 30 Hz</td><td>Concentración</td></tr>
      <tr><td>Gamma</td><td>30 – 100 Hz</td><td>Procesamiento alto</td></tr>
    </table>
    <h3>Portadora vs Beat</h3>
    <p><b>Portadora:</b> el tono base que escuchas (40-1000 Hz). <b>Beat:</b> la diferencia entre canales (binaural) o la frecuencia de encendido/apagado (isocrónico).</p>
    <p>Ejemplo: portadora 200 Hz + beat 8 Hz = binaural de 196/204 Hz → sensación de 8 Hz (alpha).</p>`,

  efectos:`<h3>Eco</h3>
    <p>Repite el sonido tras un retardo. Parámetros:</p>
    <ul>
      <li><b>Delay:</b> tiempo entre repeticiones (50-1000 ms).</li>
      <li><b>Feedback:</b> cuánto del eco vuelve a entrar (0-0.85). Valores altos = ecos infinitos.</li>
      <li><b>Mezcla:</b> cuánto eco se oye vs. señal original.</li>
    </ul>
    <p>Uso hipnótico: delay 300 ms, feedback 0.4, mezcla 0.35.</p>
    <h3>Reverb</h3>
    <p>Simula espacios físicos con convolución. <b>Tamaño</b> = duración del impulso (0.1-5 s). Tamaños grandes = catedral.</p>
    <h3>Coro</h3>
    <p>Añade 3 delays ligeramente distintos para dar sensación de amplitud. Intensidad controla la separación.</p>`,

  mezcla:`<h3>Mezclador</h3>
    <p>6 canales verticales: voz, tono, ruido, eco, reverb, coro. Cada uno con:</p>
    <ul>
      <li><b>Slider vertical:</b> volumen 0-1 (con lectura en dB).</li>
      <li><b>M (Mute):</b> silencia el canal.</li>
      <li><b>S (Solo):</b> silencia todos los demás.</li>
    </ul>
    <h3>Consejos de mezcla</h3>
    <ul>
      <li><b>Hipnosis profunda:</b> voz 0.9, tono 0.4, reverb 0.3, eco 0.2.</li>
      <li><b>Meditación:</b> voz 0.7, binaural 0.5, ruido rosa 0.2.</li>
      <li><b>Enfoque:</b> voz 0.8, isocrónico 0.4, sin efectos.</li>
      <li><b>Trance:</b> voz 0.5, cuadrada 0.3, eco 0.6, reverb 0.4.</li>
    </ul>
    <p><b>-30 dB</b> en el slider de voz equivale a 0.0316 de ganancia lineal. Puedes usarlo para mezclar locuciones muy suaves con tonos fuertes.</p>`,

  export:`<h3>Exportar a MP3</h3>
    <p>La app intenta primero capturar el audio del sistema (desktop) y si falla, cambia automáticamente al micrófono (Android/iOS).</p>
    <h3>Por qué a veces usa micrófono</h3>
    <p>En Android y iOS, los navegadores no permiten capturar el audio del sistema directamente. El micrófono captura lo que sale por los altavoces o auriculares.</p>
    <h3>Mejores prácticas</h3>
    <ul>
      <li>Usa auriculares con cable para mejor calidad.</li>
      <li>Sube el volumen del dispositivo al 80-90 % (no al 100 %).</li>
      <li>Graba en una habitación silenciosa.</li>
      <li>Si el motor TTS satura, baja la velocidad.</li>
    </ul>
    <h3>Formato de salida</h3>
    <p>La app convierte el audio capturado (WebM/Opus) a <b>MP3 192 kbps</b> con lamejs. Si la conversión falla, descarga el archivo original.</p>
    <h3>Aviso legal</h3>
    <p>Las voces TTS son del sistema operativo. Las frecuencias binaurales/isocrónicas no tienen efectos médicos garantizados.</p>`,

  faq:`<h3>Preguntas frecuentes</h3>
    <h3>¿Por qué no encuentro español de México?</h3>
    <p>Porque no está instalado en tu sistema. Sigue la guía de la pestaña "Voz" para instalarlo.</p>
    <h3>¿Por qué la voz suena robótica?</h3>
    <p>Porque el motor TTS del sistema es básico. Para voces naturales necesitarías APIs externas (ElevenLabs, Azure Neural).</p>
    <h3>¿La velocidad 100× funciona?</h3>
    <p>Hasta ~10× con calidad. Por encima, el motor satura. La app avanza igualmente porque añade timeout.</p>
    <h3>¿Por qué pide micrófono?</h3>
    <p>Porque en móvil es la única forma de capturar el audio. En desktop se intenta primero capturar el sistema.</p>
    <h3>¿Puedo usar mis propios tonos?</h3>
    <p>Actualmente no. Todos los tonos se generan con Web Audio API.</p>
    <h3>¿Se guarda mi configuración?</h3>
    <p>Sí, en localStorage. Si quieres borrarla, ve a Configuración del navegador y elimina los datos del sitio.</p>
    <h3>¿Los tonos binaurales realmente funcionan?</h3>
    <p>Hay estudios mixtos. Algunos usuarios reportan mejoras en relajación/enfoque. No hay garantía médica.</p>`
};

function mostrarGuia(tab){
  $('modalGuia').style.display='block';
  document.body.style.overflow='hidden';
  $('guiaTabs').querySelectorAll('.guia-tab').forEach(t=>t.classList.toggle('active',t.dataset.tab===tab));
  $('guiaContent').innerHTML=GUIA[tab]||GUIA.inicio;
}
function cerrarGuia(){
  $('modalGuia').style.display='none';
  document.body.style.overflow='';
}

/* =========================================================================
   EVENTOS
   ========================================================================= */
window.addEventListener('DOMContentLoaded',async()=>{
  diag('App iniciada','ok');
  diag('Protocolo: '+location.protocol,'');
  diag(`Android:${esAndroid} iOS:${esIOS} Brave:${esBrave}`,'');

  await cargarVoces();
  rellenarVocesUI();
  renderTonos();
  renderMixer();
  renderAvisos();
  actualizarStats();

  $('btnPedirPermiso').onclick=pedirPermisoMic;

  $('texto').addEventListener('input',()=>{
    if($('texto').value.length>MAX_CHARS) $('texto').value=$('texto').value.slice(0,MAX_CHARS);
    actualizarStats();
  });
  $('btnLimpiar').onclick=()=>{$('texto').value='';actualizarStats();setEstado('Listo')};
  $('btnEjemplo').onclick=()=>{
    $('texto').value=`Respira profundo... y suelta el peso.
Gira hacia adentro, muy despacio.
Todo se aquieta.
El aire entra... el aire sale.
Estás aquí, ahora.
No hay prisa.
Solo este momento.
Respira.`;
    actualizarStats();
  };
  $('btnCargarTxt').onclick=()=>$('fileTxt').click();
  $('fileTxt').onchange=e=>{
    const f=e.target.files?.[0];if(!f) return;
    const r=new FileReader();
    r.onload=()=>{$('texto').value=String(r.result).slice(0,MAX_CHARS);actualizarStats()};
    r.readAsText(f);e.target.value='';
  };

  $('idioma').onchange=()=>{S.idioma=$('idioma').value;rellenarVocesUI();renderAvisos()};
  $('voz').onchange=()=>{S.vozIdx=+$('voz').value};
  $('btnRecargarVoces').onclick=async()=>{diag('Recargando…','warn');await cargarVoces();rellenarVocesUI();renderAvisos();setEstado(`Voces: ${voces.length}`,'ok')};

  // Presets lectura
  document.querySelectorAll('.preset').forEach(el=>{
    el.onclick=()=>aplicarPreset(el.dataset.preset);
  });

  // Sliders voz
  const bind=(id,key,fmt,extra)=>{
    const el=$(id),val=$(id+'Val');
    el.oninput=()=>{
      S[key]=+el.value;
      val.textContent=fmt?fmt(+el.value):el.value;
      if(key==='rate') actualizarBadgeRate();
      if(extra) extra(+el.value);
      actualizarStats();
    };
  };
  bind('rate','rate',v=>v.toFixed(1));
  bind('pitch','pitch',v=>v.toFixed(2));
  bind('vol','vol',v=>v.toFixed(2));
  bind('pausa','pausa',v=>`${v} ms`);
  bind('atenuacion','atenuacion',v=>`${v} dB`,v=>{
    $('atenuacionBadge').textContent=`${v} dB`;
  });
  bind('ecoDelay','eco.delay',v=>`${v} ms`,v=>{S.eco.delay=v;actualizarEco()});
  bind('ecoFeedback','eco.feedback',v=>v.toFixed(2),v=>{S.eco.feedback=v;actualizarEco()});
  bind('ecoMix','eco.mix',v=>v.toFixed(2),v=>{S.eco.mix=v;actualizarEco()});
  bind('reverbSize','reverb.size',v=>`${v.toFixed(1)} s`,v=>{S.reverb.size=v;actualizarReverb()});
  bind('reverbMix','reverb.mix',v=>v.toFixed(2),v=>{S.reverb.mix=v;actualizarReverb()});
  bind('coroIntensity','coro.intensity',v=>v.toFixed(2),v=>{S.coro.intensity=v;actualizarCoro()});

  $('carrier').oninput=()=>{S.carrier=+$('carrier').value;$('carrierVal').textContent=S.carrier;if(S.tono!=='ninguno') startTono()};
  $('beat').oninput=()=>{S.beat=+$('beat').value;$('beatVal').textContent=S.beat;if(S.tono!=='ninguno') startTono()};

  $('presetFreq').onchange=()=>{
    const v=$('presetFreq').value;
    const map={
      delta:{carrier:200,beat:2},
      theta:{carrier:200,beat:6},
      alpha:{carrier:200,beat:10},
      smr:{carrier:200,beat:13.5},
      beta:{carrier:200,beat:20},
      gamma:{carrier:200,beat:40},
      solf174:{carrier:174,beat:0.5},
      solf285:{carrier:285,beat:0.5},
      solf396:{carrier:396,beat:0.5},
      solf417:{carrier:417,beat:0.5},
      solf528:{carrier:528,beat:0.5},
      solf639:{carrier:639,beat:0.5},
      solf741:{carrier:741,beat:0.5},
      solf852:{carrier:852,beat:0.5},
      solf963:{carrier:963,beat:0.5}
    };
    if(map[v]){
      S.carrier=map[v].carrier;
      S.beat=map[v].beat;
      $('carrier').value=S.carrier;$('carrierVal').textContent=S.carrier;
      $('beat').value=S.beat;$('beatVal').textContent=S.beat;
      if(S.tono!=='ninguno') startTono();
    }
  };

  // Cantidad
  $('qtyMinus').onclick=()=>{const v=clamp(+$('rep').value-1,1,999);$('rep').value=v;S.rep=v;actualizarStats()};
  $('qtyPlus').onclick=()=>{const v=clamp(+$('rep').value+1,1,999);$('rep').value=v;S.rep=v;actualizarStats()};
  $('rep').oninput=()=>{S.rep=clamp(+$('rep').value,1,999);actualizarStats()};

  // Atajos rate
  document.querySelectorAll('button[data-rate]').forEach(el=>{
    el.onclick=()=>{
      const v=+el.dataset.rate;
      $('rate').value=v;S.rate=v;$('rateVal').textContent=v.toFixed(1);actualizarBadgeRate();actualizarStats();
    };
  });
  document.querySelectorAll('button[data-rep]').forEach(el=>{
    el.onclick=()=>{
      const v=+el.dataset.rep;
      $('rep').value=v;S.rep=v;actualizarStats();
    };
  });

  $('prosodia').onchange=()=>{S.prosodia=$('prosodia').checked};

  // Efectos toggle
  $('ecoOn').onchange=()=>{S.eco.on=$('ecoOn').checked;actualizarEco()};
  $('reverbOn').onchange=()=>{S.reverb.on=$('reverbOn').checked;actualizarReverb()};
  $('coroOn').onchange=()=>{S.coro.on=$('coroOn').checked;actualizarCoro()};

  // Reproducción
  $('btnPlay').onclick=reproducir;
  $('btnStop').onclick=()=>{detenerTTS();stopTono()};

  // Exportar
  $('btnGrabarMP3').onclick=grabarYExportar;
  $('btnDescargarUltimo').onclick=()=>{if(ultimoBlob) descargarBlob(ultimoBlob,`hypno-${Date.now()}.mp3`)};

  // Guía
  $('btnGuia').onclick=()=>mostrarGuia('inicio');
  $('btnCerrarGuia').onclick=cerrarGuia;
  $('guiaTabs').querySelectorAll('.guia-tab').forEach(t=>{
    t.onclick=()=>mostrarGuia(t.dataset.tab);
  });
  $('modalGuia').onclick=e=>{if(e.target===$('modalGuia')) cerrarGuia()};

  // Esc
  window.addEventListener('keydown',e=>{
    if(e.key==='Escape'){
      if($('modalGuia').style.display==='block') cerrarGuia();
      else{detenerTTS();stopTono()}
    }
  });
  window.addEventListener('beforeunload',()=>{try{speechSynthesis.cancel()}catch(e){}stopTono()});

  // SW
  if('serviceWorker' in navigator&&location.protocol==='https:'){
    const sw=`self.addEventListener('install',e=>self.skipWaiting());self.addEventListener('activate',e=>self.clients.claim());self.addEventListener('fetch',e=>e.respondWith(caches.open('hypno-v1').then(c=>fetch(e.request).then(r=>{c.put(e.request,r.clone());return r}).catch(()=>c.match(e.request)))));`;
    const u=URL.createObjectURL(new Blob([sw],{type:'application/javascript'}));
    navigator.serviceWorker.register(u).then(()=>diag('SW OK','ok')).catch(e=>diag('SW: '+e.message,'warn'));
  }
});

/* =========================================================================
   AUXILIARES
   ========================================================================= */
function aplicarPreset(nombre){
  const p={
    neutro:{rate:0.9,pitch:1.0,vol:.9,pausa:180},
    calido:{rate:0.85,pitch:1.02,vol:.85,pausa:260},
    hipnotico:{rate:0.9,pitch:1.0,vol:.75,pausa:280},
    susurro:{rate:0.75,pitch:1.05,vol:.35,pausa:340}
  }[nombre];
  S.estilo=nombre;S.rate=p.rate;S.pitch=p.pitch;S.vol=p.vol;S.pausa=p.pausa;
  $('rate').value=p.rate;$('rateVal').textContent=p.rate.toFixed(1);
  $('pitch').value=p.pitch;$('pitchVal').textContent=p.pitch.toFixed(2);
  $('vol').value=p.vol;$('volVal').textContent=p.vol.toFixed(2);
  $('pausa').value=p.pausa;$('pausaVal').textContent=`${p.pausa} ms`;
  actualizarBadgeRate();
  document.querySelectorAll('.preset').forEach(el=>el.classList.toggle('active',el.dataset.preset===nombre));
}

function actualizarBadgeRate(){
  const r=S.rate;
  const b=$('rateBadge');
  let t;
  if(r<1) t=`${r.toFixed(1)}× lento`;
  else if(r===1) t='1× normal';
  else if(r<=2) t=`${r.toFixed(1)}× rápido`;
  else if(r<=10) t=`${r.toFixed(1)}× muy rápido`;
  else t=`${r.toFixed(0)}× extremo`;
  b.textContent=t;
  b.style.background=r>10?'rgba(255,100,100,.25)':r>3?'rgba(255,190,90,.25)':'rgba(140,160,255,.2)';
  b.style.color=r>10?'#ffc9c9':r>3?'#ffe0a8':'#c0d0ff';
}

function actualizarStats(){
  const txt=$('texto').value;
  const n=txt.length;
  $('charCount').textContent=n.toLocaleString();
  $('charCount').style.color=n>MAX_CHARS?'var(--err)':'';
  const b=dividirEnBloques(normalizarTexto(txt));
  $('chunkCount').textContent=`${b.length} bloques`;
  const chars=b.reduce((a,c)=>a+c.length,0);
  const seg=Math.round(chars/14/Math.max(.1,S.rate)*(S.rep||1));
  $('duracionEst').textContent=`~${seg}s`;
}

function renderAvisos(){
  const cont=$('avisos');const a=[];
  if(esLocal) a.push({t:'err',m:`Estás abriendo el archivo desde <code>${location.protocol}</code>. Los permisos de micrófono y la captura están bloqueados. Súbelo a <b>https://</b> (GitHub Pages, Netlify, Cloudflare Pages) y ábrelo desde ahí.`});
  if(!('speechSynthesis' in window)) a.push({t:'err',m:'Sin soporte de síntesis de voz'});
  if(!navigator.mediaDevices?.getUserMedia) a.push({t:'err',m:'Sin soporte de micrófono (requiere HTTPS)'});
  if(esAndroid) a.push({t:'info',m:'En Android se graba por <b>micrófono</b> automáticamente. Sube el volumen y usa auriculares.'});
  if(esIOS) a.push({t:'warn',m:'En iOS la captura es limitada. Se usa micrófono.'});
  const mx=voces.find(v=>v.lang&&v.lang.startsWith('es-MX'));
  if(!mx&&S.idioma==='es') a.push({t:'info',m:'No hay voz <b>es-MX (México)</b>. Mira la guía → Voz para instalarla.'});
  cont.innerHTML=a.map(x=>`<div class="aviso ${x.t}">${x.m}</div>`).join('');
}
</script>
</body>
</html>
