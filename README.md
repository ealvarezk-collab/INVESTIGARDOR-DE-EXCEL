<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Jefe de Terreno · Excel</title>
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  :root{
    --bg:#0b1219;--panel:#141f2b;--panel2:#1b2836;--line:#26374a;
    --txt:#e9f0f7;--muted:#8ea3b8;
    --green:#217346;--green2:#2fbf71;
    --ok:#2fbf71;--bad:#ff5d5d;--gold:#ffc857;--naranja:#ff8a3d;
  }
  body{font-family:'Segoe UI',system-ui,-apple-system,sans-serif;
    background:radial-gradient(1200px 600px at 50% -10%,#1a2f42 0%,var(--bg) 60%);
    color:var(--txt);min-height:100vh;display:flex;justify-content:center;align-items:flex-start;padding:16px;}
  .pantalla{display:none;width:100%;max-width:920px}
  .pantalla.activa{display:block;animation:fade .35s ease}
  @keyframes fade{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:none}}
  .card{background:linear-gradient(180deg,var(--panel),var(--panel2));border:1px solid var(--line);border-radius:18px;padding:24px;box-shadow:0 22px 60px rgba(0,0,0,.5)}
  .logo{font-size:56px;line-height:1;margin-bottom:8px;text-align:center}
  h1{font-size:28px;letter-spacing:-.5px;margin-bottom:6px;text-align:center}
  h1 span{color:var(--green2)}
  .sub{color:var(--muted);font-size:15.5px;margin-bottom:22px;line-height:1.55;text-align:center}
  .reglas{background:#0f1a25;border:1px solid var(--line);border-radius:12px;padding:16px 18px;margin-bottom:20px;font-size:14px;color:var(--muted);line-height:1.8}
  .reglas b{color:var(--txt)}
  .reglas .grid{display:grid;grid-template-columns:1fr 1fr;gap:6px 18px;margin-top:8px}
  @media(max-width:560px){.reglas .grid{grid-template-columns:1fr}}
  .numsel{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:20px;justify-content:center}
  .numsel label{cursor:pointer;border:1.5px solid var(--line);background:#0f1a25;padding:10px 18px;border-radius:10px;font-size:14px;font-weight:600;transition:.18s}
  .numsel label:hover{border-color:var(--green2)}
  .numsel input{display:none}
  .numsel label:has(input:checked){border-color:var(--green2);background:rgba(47,191,113,.12)}
  .numsel label:has(input:checked) span{color:var(--green2)}
  .btn{font-family:inherit;font-size:16px;font-weight:700;cursor:pointer;background:linear-gradient(180deg,var(--green2),var(--green));color:#04170d;border:none;border-radius:12px;padding:15px 32px;transition:.2s;letter-spacing:.3px}
  .btn:hover{transform:translateY(-2px);box-shadow:0 10px 24px rgba(47,191,113,.3)}
  .btn.sec{background:#0f1a25;color:var(--txt);border:1.5px solid var(--line)}
  .btn.sec:hover{border-color:var(--green2);box-shadow:none}
  .hud{display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap;margin-bottom:12px}
  .hud-izq{font-size:13.5px;color:var(--muted);font-weight:600}
  .hud-der{display:flex;align-items:center;gap:8px;flex-wrap:wrap}
  .costo{display:flex;align-items:center;gap:8px;font-weight:800;background:#0f1a25;border:1px solid var(--line);padding:7px 14px;border-radius:99px;font-size:15px;transition:.3s}
  .costo b{color:var(--txt);transition:color .3s;font-family:'Consolas',monospace}
  .costo.ahorro{border-color:var(--ok);background:rgba(47,191,113,.1)}
  .costo.ahorro b{color:var(--green2)}
  .costo.sobre{border-color:var(--bad);background:rgba(255,93,93,.1)}
  .costo.sobre b{color:var(--bad)}
  .costo .delta{font-size:11.5px;font-weight:700;padding:2px 8px;border-radius:99px;margin-left:2px;opacity:0;transition:opacity .3s}
  .costo .delta.show{opacity:1}
  .costo .delta.ahorro{background:rgba(47,191,113,.2);color:var(--green2)}
  .costo .delta.sobre{background:rgba(255,93,93,.2);color:var(--bad)}
  .badge-racha{display:flex;align-items:center;gap:6px;font-weight:800;font-size:14px;background:#0f1a25;border:1px solid var(--line);padding:7px 12px;border-radius:99px;color:var(--naranja);transition:.3s}
  .badge-racha b{color:var(--gold);font-family:'Consolas',monospace}
  .badge-racha.caliente{border-color:var(--naranja);background:rgba(255,138,61,.15);animation:latido .6s ease}
  @keyframes latido{0%,100%{transform:scale(1)}50%{transform:scale(1.15)}}
  .btn-audio{width:38px;height:38px;border-radius:99px;cursor:pointer;background:#0f1a25;border:1.5px solid var(--line);color:var(--txt);font-size:17px;display:grid;place-items:center;transition:.2s;font-family:inherit}
  .btn-audio:hover{border-color:var(--green2);transform:scale(1.05)}
  .btn-audio.mute{opacity:.45;filter:grayscale(1)}
  .tablero-wrap{background:#0d1822;border:1px solid var(--line);border-radius:12px;padding:12px 14px;margin-bottom:16px}
  .tablero-label{display:flex;justify-content:space-between;align-items:center;font-size:10.5px;color:var(--muted);letter-spacing:1.5px;text-transform:uppercase;font-weight:700;margin-bottom:10px}
  .tablero-label .prog-mini{font-size:12px;font-weight:700;color:var(--txt);text-transform:none;letter-spacing:.3px}
  .tablero{display:grid;grid-template-columns:repeat(auto-fill,minmax(76px,1fr));gap:8px}
  .caso-card{background:#0f1a25;border:1.5px solid var(--line);border-radius:10px;padding:8px 6px;text-align:center;transition:.3s}
  .caso-card.activo{border-color:var(--gold);background:rgba(255,200,87,.08);box-shadow:0 0 0 2px rgba(255,200,87,.15)}
  .caso-card.perfecto{border-color:var(--ok);background:rgba(47,191,113,.1)}
  .caso-card.parcial{border-color:var(--naranja);background:rgba(255,138,61,.08)}
  .caso-num{font-size:10.5px;color:var(--muted);font-weight:700;margin-bottom:6px;letter-spacing:.5px;text-transform:uppercase}
  .caso-card.activo .caso-num{color:var(--gold)}
  .caso-card.perfecto .caso-num{color:var(--ok)}
  .caso-card.parcial .caso-num{color:var(--naranja)}
  .caso-fases{display:flex;justify-content:center;gap:4px}
  .fase-dot{width:11px;height:11px;border-radius:50%;background:#1d2a39;border:1.5px solid #2c3e54;transition:.3s}
  .fase-dot.ok{background:var(--green2);border-color:var(--green2);box-shadow:0 0 8px rgba(47,191,113,.6)}
  .fase-dot.bad{background:var(--bad);border-color:var(--bad)}
  .fase-dot.act{background:var(--gold);border-color:var(--gold);animation:pulso 1.1s infinite}
  @keyframes pulso{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.5;transform:scale(.85)}}
  .pasos{display:flex;gap:8px;margin-bottom:14px;flex-wrap:wrap}
  .paso{flex:1;min-width:100px;font-size:12px;font-weight:700;padding:8px 12px;border-radius:9px;background:#0f1a25;border:1px solid var(--line);color:var(--muted);text-align:center;transition:.25s;letter-spacing:.3px}
  .paso.on{background:rgba(255,200,87,.12);border-color:var(--gold);color:var(--gold)}
  .paso.done-ok{background:rgba(47,191,113,.12);border-color:var(--ok);color:var(--ok)}
  .paso.done-bad{background:rgba(255,93,93,.12);border-color:var(--bad);color:var(--bad)}
  .contexto{display:inline-block;font-size:11.5px;letter-spacing:1px;text-transform:uppercase;color:var(--naranja);background:rgba(255,138,61,.1);border:1px solid rgba(255,138,61,.35);padding:5px 12px;border-radius:99px;margin-bottom:12px;font-weight:700}
  .caso{font-size:16px;line-height:1.6;margin-bottom:18px;background:#0f1a25;border-left:4px solid var(--gold);border-radius:0 10px 10px 0;padding:14px 16px;color:var(--txt)}
  .caso b{color:var(--gold)}
  .fase-titulo{font-size:14px;color:var(--muted);font-weight:700;margin-bottom:12px;text-transform:uppercase;letter-spacing:.8px}
  .fase-titulo span{color:var(--txt);text-transform:none;letter-spacing:0;font-weight:600}
  .opciones{display:grid;grid-template-columns:1fr 1fr;gap:10px}
  .opciones.una{grid-template-columns:1fr}
  @media(max-width:640px){.opciones{grid-template-columns:1fr}}
  .opcion{display:flex;align-items:center;gap:12px;text-align:left;background:#0f1a25;border:1.5px solid var(--line);color:var(--txt);padding:14px 16px;border-radius:12px;cursor:pointer;font-size:15px;font-family:inherit;transition:.15s}
  .opcion:hover:not(:disabled){border-color:var(--green2);background:#132433;transform:translateY(-2px)}
  .opcion:disabled{cursor:default;opacity:.95}
  .opcion .ic{width:34px;height:34px;flex:0 0 34px;display:grid;place-items:center;border-radius:8px;background:#1d2c3c;font-size:17px;transition:.18s}
  .opcion .lbl{font-weight:700}
  .opcion .desc{font-size:12.5px;color:var(--muted);margin-top:2px;line-height:1.3}
  .opcion .fn{font-family:'Consolas','Courier New',monospace;font-weight:700;letter-spacing:.3px;font-size:14.5px}
  .opcion.correcta{border-color:var(--ok);background:rgba(47,191,113,.14)}
  .opcion.correcta .ic{background:var(--ok);color:#04170d}
  .opcion.incorrecta{border-color:var(--bad);background:rgba(255,93,93,.12)}
  .opcion.incorrecta .ic{background:var(--bad);color:#1a0505}
  .feedback{display:none;margin-top:18px;padding:16px 18px;border-radius:12px;background:#0f1a25;border-left:5px solid var(--line)}
  .feedback.visible{display:block;animation:fade .3s ease}
  .feedback.ok{border-left-color:var(--ok)}
  .feedback.bad{border-left-color:var(--bad)}
  .fb-titulo{font-weight:800;font-size:15px;margin-bottom:12px}
  .feedback.ok .fb-titulo{color:var(--ok)}
  .feedback.bad .fb-titulo{color:var(--bad)}
  .fb-compara{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:14px}
  @media(max-width:640px){.fb-compara{grid-template-columns:1fr}}
  .fb-lado{border-radius:10px;padding:12px 14px;border:1.5px solid var(--line);background:#0d1822}
  .fb-lado.mal{border-color:rgba(255,93,93,.4);background:rgba(255,93,93,.06)}
  .fb-lado.ok{border-color:rgba(47,191,113,.4);background:rgba(47,191,113,.06)}
  .fb-lado-tit{font-size:10.5px;text-transform:uppercase;letter-spacing:1.2px;font-weight:700;margin-bottom:6px}
  .fb-lado.mal .fb-lado-tit{color:var(--bad)}
  .fb-lado.ok .fb-lado-tit{color:var(--ok)}
  .fb-lado-nom{font-family:'Consolas',monospace;font-size:15px;font-weight:700;margin-bottom:6px;line-height:1.35;word-break:break-word}
  .fb-lado.mal .fb-lado-nom{color:#ff8a8a}
  .fb-lado.ok .fb-lado-nom{color:var(--green2)}
  .fb-lado-desc{font-size:12.5px;color:var(--muted);line-height:1.5}
  .fb-porque{background:rgba(255,200,87,.07);border:1px solid rgba(255,200,87,.25);border-radius:10px;padding:12px 14px;font-size:13.5px;line-height:1.6;color:var(--txt);margin-top:12px}
  .fb-porque .et{display:inline-block;font-size:10.5px;text-transform:uppercase;letter-spacing:1.2px;color:var(--gold);font-weight:800;margin-bottom:6px}
  .fb-porque b{color:var(--gold)}
  .fb-exp-ok{background:rgba(47,191,113,.06);border:1px solid rgba(47,191,113,.25);border-radius:10px;padding:12px 14px;font-size:13.5px;line-height:1.6;color:var(--muted);margin-top:10px}
  .fb-exp-ok b{color:var(--green2)}
  .acciones{display:flex;justify-content:flex-end;margin-top:16px}
  #btn-next{display:none}
  .tablero-final{background:#0d1822;border:1px solid var(--line);border-radius:12px;padding:16px;margin-bottom:20px;text-align:center}
  .tablero-final .titulo{font-size:11.5px;color:var(--muted);text-transform:uppercase;letter-spacing:1.5px;font-weight:700;margin-bottom:12px}
  .personaje{display:flex;flex-direction:column;align-items:center;margin:0 auto 16px;animation:aparecer .8s cubic-bezier(.34,1.56,.64,1)}
  .personaje svg{width:100%;max-width:440px;height:auto;filter:drop-shadow(0 14px 26px rgba(0,0,0,.55));border-radius:14px}
  .personaje-rol{font-size:12px;font-weight:800;letter-spacing:1.5px;text-transform:uppercase;color:var(--gold);margin-top:10px}
  .personaje-msg{font-size:16px;font-weight:800;margin-top:4px}
  @keyframes aparecer{from{opacity:0;transform:translateY(40px) scale(.6)}to{opacity:1;transform:none}}
  .medalla{font-size:58px;text-align:center;margin-bottom:6px}
  #fin-titulo{text-align:center;font-size:25px;margin-bottom:6px}
  #fin-msg{text-align:center;color:var(--muted);margin-bottom:22px;font-size:15.5px;line-height:1.5}
  .stats{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:22px}
  @media(max-width:640px){.stats{grid-template-columns:repeat(2,1fr)}}
  .stat{background:#0f1a25;border:1px solid var(--line);border-radius:12px;padding:14px 8px;text-align:center}
  .stat span{display:block;font-size:21px;font-weight:800;color:var(--green2);margin-bottom:3px}
  .stat small{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.7px;font-weight:700}
  .stat.money span{font-size:15px;font-family:'Consolas',monospace}
  .stat.money span.ok{color:var(--green2)}
  .stat.money span.bad{color:var(--bad)}
  .stat.money span.neutral{color:var(--gold)}
  .repaso h3{font-size:12.5px;margin-bottom:12px;color:var(--gold);text-transform:uppercase;letter-spacing:1.2px}
  .repaso-item{background:#0f1a25;border:1px solid var(--line);border-left:4px solid var(--bad);border-radius:10px;padding:12px 14px;margin-bottom:10px}
  .repaso-caso{font-size:13.5px;line-height:1.55;margin-bottom:7px;color:var(--txt)}
  .repaso-fn{font-family:'Consolas',monospace;color:var(--ok);font-weight:700;font-size:13.5px;margin-bottom:4px}
  .repaso-exp{color:var(--muted);font-size:12.5px;line-height:1.55}
  .perfecto{text-align:center;color:var(--ok);font-weight:700;font-size:15px;background:rgba(47,191,113,.1);border:1px solid rgba(47,191,113,.3);padding:16px;border-radius:12px;margin-bottom:16px}
  .centrado{text-align:center;margin-top:6px}

  /* ============================================================
     NUEVO · MODAL DE IDENTIFICACIÓN Y PANEL DOCENTE
     ============================================================ */
  .btn-docente{
    position:fixed;bottom:18px;right:18px;z-index:50;
    background:#0f1a25;border:1.5px solid var(--line);color:var(--txt);
    padding:10px 16px;border-radius:99px;font-family:inherit;font-size:13px;font-weight:700;
    cursor:pointer;display:flex;align-items:center;gap:8px;transition:.2s;
    box-shadow:0 8px 20px rgba(0,0,0,.4);
  }
  .btn-docente:hover{border-color:var(--gold);color:var(--gold);transform:translateY(-2px)}

  .modal-overlay{
    display:none;position:fixed;inset:0;z-index:100;
    background:rgba(5,10,16,.85);backdrop-filter:blur(6px);
    justify-content:center;align-items:center;padding:20px;
  }
  .modal-overlay.activo{display:flex;animation:fade .25s ease}
  .modal-card{
    background:linear-gradient(180deg,var(--panel),var(--panel2));
    border:1px solid var(--line);border-radius:18px;padding:28px;
    max-width:480px;width:100%;box-shadow:0 30px 80px rgba(0,0,0,.7);
    animation:slideUp .4s cubic-bezier(.34,1.56,.64,1);
  }
  @keyframes slideUp{from{opacity:0;transform:translateY(40px) scale(.95)}to{opacity:1;transform:none}}
  .modal-card h2{font-size:22px;margin-bottom:8px;text-align:center}
  .modal-card p{color:var(--muted);font-size:14px;line-height:1.55;text-align:center;margin-bottom:20px}
  .modal-card input{
    width:100%;font-family:inherit;font-size:16px;padding:14px 16px;
    background:#0f1a25;border:1.5px solid var(--line);border-radius:12px;color:var(--txt);
    margin-bottom:14px;transition:.2s;
  }
  .modal-card input:focus{outline:none;border-color:var(--green2);background:#132433}
  .modal-card .error{
    color:var(--bad);font-size:13px;text-align:center;margin-bottom:14px;display:none;
  }
  .modal-card .error.visible{display:block}

  /* Panel docente */
  .docente-cabecera{
    display:flex;justify-content:space-between;align-items:center;
    gap:12px;flex-wrap:wrap;margin-bottom:20px;
  }
  .docente-cabecera h2{font-size:22px;display:flex;align-items:center;gap:10px}
  .docente-acciones{display:flex;gap:8px;flex-wrap:wrap}
  .btn-small{
    font-family:inherit;font-size:13px;font-weight:700;cursor:pointer;
    background:#0f1a25;border:1.5px solid var(--line);color:var(--txt);
    padding:9px 16px;border-radius:9px;transition:.18s;
  }
  .btn-small:hover{border-color:var(--green2);color:var(--green2)}
  .btn-small.peligro:hover{border-color:var(--bad);color:var(--bad)}

  .metricas-resumen{
    display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:20px;
  }
  @media(max-width:640px){.metricas-resumen{grid-template-columns:repeat(2,1fr)}}
  .metrica-card{
    background:#0d1822;border:1px solid var(--line);border-radius:12px;
    padding:14px 10px;text-align:center;
  }
  .metrica-card span{display:block;font-size:22px;font-weight:800;color:var(--green2);margin-bottom:3px}
  .metrica-card small{font-size:10.5px;color:var(--muted);text-transform:uppercase;letter-spacing:.8px;font-weight:700}

  .tabla-alumnos{
    width:100%;border-collapse:collapse;font-size:13.5px;margin-bottom:20px;
    background:#0d1822;border-radius:12px;overflow:hidden;
  }
  .tabla-alumnos th{
    background:#0f1a25;color:var(--muted);text-align:left;padding:10px 12px;
    font-size:10.5px;font-weight:800;letter-spacing:1px;text-transform:uppercase;
    border-bottom:1px solid var(--line);
  }
  .tabla-alumnos td{padding:11px 12px;border-bottom:1px solid var(--line);color:var(--txt)}
  .tabla-alumnos tr:last-child td{border-bottom:none}
  .tabla-alumnos tr:hover td{background:rgba(47,191,113,.05)}
  .tabla-alumnos .num{font-family:'Consolas',monospace;font-weight:700}
  .tabla-alumnos .ok{color:var(--green2)}
  .tabla-alumnos .bad{color:var(--bad)}
  .tabla-alumnos .gold{color:var(--gold)}

  .seccion-docente{margin-bottom:22px}
  .seccion-docente h3{
    font-size:12.5px;color:var(--gold);text-transform:uppercase;
    letter-spacing:1.2px;margin-bottom:10px;
  }
  .barra-item{
    background:#0d1822;border:1px solid var(--line);border-radius:10px;
    padding:11px 14px;margin-bottom:8px;display:flex;justify-content:space-between;
    align-items:center;gap:12px;
  }
  .barra-item .nombre{
    flex:1;font-size:13.5px;color:var(--txt);overflow:hidden;
    text-overflow:ellipsis;white-space:nowrap;
  }
  .barra-item .valores{
    display:flex;gap:10px;align-items:center;font-family:'Consolas',monospace;
    font-size:13px;font-weight:700;flex-shrink:0;
  }
  .barra-item .pct{color:var(--gold)}
  .barra-item .fallas{color:var(--bad)}
  .barra-item .ok{color:var(--green2)}

  .vacio{
    text-align:center;color:var(--muted);font-size:14px;
    padding:40px 20px;background:#0d1822;border:1px dashed var(--line);
    border-radius:12px;line-height:1.6;
  }

  .filtro-usuario{
    display:flex;gap:8px;align-items:center;flex-wrap:wrap;margin-bottom:14px;
  }
  .filtro-usuario select{
    font-family:inherit;font-size:13.5px;padding:8px 12px;border-radius:9px;
    background:#0f1a25;border:1.5px solid var(--line);color:var(--txt);cursor:pointer;
  }
  .filtro-usuario select:focus{outline:none;border-color:var(--green2)}
</style>
</head>
<body>

<!-- ============ MODAL DE IDENTIFICACIÓN (NUEVO) ============ -->
<div class="modal-overlay" id="modal-identificacion">
  <div class="modal-card">
    <div style="text-align:center;font-size:40px;margin-bottom:8px">👤</div>
    <h2>Identifícate</h2>
    <p>Ingresa tu nombre o número de lista para registrar tu progreso y permitir al docente ver tus resultados.</p>
    <input type="text" id="input-usuario" placeholder="Ej: Juan Pérez o 24" maxlength="40" autocomplete="off">
    <div class="error" id="error-usuario">Debes escribir un nombre para continuar</div>
    <button class="btn" style="width:100%" onclick="confirmarUsuario()">Comenzar a jugar</button>
    <p style="font-size:12px;margin-top:14px;margin-bottom:0">
      Si ya jugaste antes, escribe el mismo nombre para que se sumen tus partidas.
    </p>
  </div>
</div>

<!-- ============ BOTÓN FLOTANTE DOCENTE (NUEVO) ============ -->
<button class="btn-docente" onclick="abrirPanelDocente()" title="Ver métricas de los estudiantes">
  📊 Panel Docente
</button>

<!-- ============ PANTALLA INICIO ============ -->
<section id="pantalla-inicio" class="pantalla activa">
  <div class="card">
    <div class="logo">🏗️</div>
    <h1>Jefe de <span>Terreno</span> · Excel</h1>
    <p class="sub">Recibes problemas reales desde la obra. Tu trabajo: diagnosticar, elegir la herramienta y reconocer la fórmula correcta.</p>
    <div class="reglas">
      <b>🎯 Cómo se juega</b>
      <div class="grid">
        <div>🏗️ <b>Costo base:</b> $50.000.000</div>
        <div>💰 <b>Acierto:</b> −$250.000</div>
        <div>💸 <b>Error:</b> +$800.000</div>
        <div>📋 <b>Cada caso:</b> 3 fases = 1 tarjeta</div>
        <div>🔥 <b>Racha:</b> aciertos seguidos</div>
        <div>🔊 <b>Audio:</b> se puede silenciar</div>
      </div>
      <div style="margin-top:12px">
        <b>Jugador actual:</b> <span id="jugador-actual" style="color:var(--green2);font-weight:800">—</span>
      </div>
    </div>
    <p style="font-size:12.5px;color:var(--muted);font-weight:700;text-transform:uppercase;letter-spacing:1px;margin-bottom:10px;text-align:center">
      Tamaño del proyecto
    </p>
    <div class="numsel">
      <label><input type="radio" name="num" value="5"><span>5 · Obra pequeña</span></label>
      <label><input type="radio" name="num" value="8" checked><span>8 · Obra media</span></label>
      <label><input type="radio" name="num" value="12"><span>12 · Edificio</span></label>
      <label><input type="radio" name="num" value="16"><span>16 · Megaproyecto</span></label>
    </div>
    <div class="centrado"><button class="btn" onclick="iniciar()">▶ Iniciar obra</button></div>
  </div>
</section>

<!-- ============ PANTALLA JUEGO ============ -->
<section id="pantalla-juego" class="pantalla">
  <div class="card">
    <div class="hud">
      <div class="hud-izq" id="prog-texto">Caso 1 de 8</div>
      <div class="hud-der">
        <div class="badge-racha" id="badge-racha">🔥 <b id="racha">0</b></div>
        <div class="costo" id="costo-wrap">
          🏗️ Costo: <b id="costo">$50.000.000</b>
          <span class="delta" id="costo-delta"></span>
        </div>
        <button class="btn-audio" id="btn-audio" onclick="toggleAudio()">🔊</button>
      </div>
    </div>
    <div class="tablero-wrap">
      <div class="tablero-label">
        <span>📋 Tablero de control</span>
        <span class="prog-mini" id="tablero-info">0 / 8 completados</span>
      </div>
      <div class="tablero" id="tablero"></div>
    </div>
    <div class="pasos">
      <div class="paso" data-paso="1">1 · Diagnóstico</div>
      <div class="paso" data-paso="2">2 · Herramienta</div>
      <div class="paso" data-paso="3">3 · Fórmula</div>
    </div>
    <span class="contexto" id="contexto">Contexto</span>
    <p class="caso" id="caso">Caso…</p>
    <div class="fase-titulo" id="fase-titulo">Fase 1 · Diagnóstico</div>
    <div class="opciones" id="opciones"></div>
    <div class="feedback" id="feedback">
      <div class="fb-titulo" id="fb-titulo"></div>
      <div id="fb-texto"></div>
    </div>
    <div class="acciones">
      <button class="btn" id="btn-next" onclick="avanzar()">Continuar →</button>
    </div>
  </div>
</section>

<!-- ============ PANTALLA FINAL ============ -->
<section id="pantalla-final" class="pantalla">
  <div class="card">
    <div class="tablero-final">
      <div class="titulo">📋 Tablero final de la obra</div>
      <div class="tablero" id="tablero-final"></div>
    </div>
    <div class="personaje" id="personaje-final"></div>
    <div class="medalla" id="fin-medalla">🏆</div>
    <h2 id="fin-titulo">¡Obra entregada!</h2>
    <p id="fin-msg"></p>
    <div class="stats">
      <div class="stat"><span id="fin-aciertos">0/0</span><small>Aciertos</small></div>
      <div class="stat"><span id="fin-pct">0%</span><small>Efectividad</small></div>
      <div class="stat money"><span id="fin-costo">$0</span><small>Costo final</small></div>
      <div class="stat money"><span id="fin-delta">$0</span><small>Ahorro / Sobrecosto</small></div>
    </div>
    <div class="repaso" id="repaso"></div>
    <div class="centrado" style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap;margin-top:6px">
      <button class="btn" onclick="repetirNivel()">🔁 Repetir este nivel</button>
      <button class="btn sec" onclick="volverAlInicio()">🏠 Elegir otro nivel</button>
    </div>
    <p id="auto-volver" style="text-align:center;color:var(--muted);font-size:12.5px;margin-top:16px">
      ⏱️ Volviendo al inicio en <b id="auto-count" style="color:var(--gold)">20</b> segundos…
    </p>
  </div>
</section>

<!-- ============ PANTALLA DOCENTE (NUEVO) ============ -->
<section id="pantalla-docente" class="pantalla">
  <div class="card">
    <div class="docente-cabecera">
      <h2>📊 Panel del Docente</h2>
      <div class="docente-acciones">
        <button class="btn-small" onclick="exportarCSV()">⬇️ Exportar CSV</button>
        <button class="btn-small peligro" onclick="resetearDatos()">🗑️ Borrar todo</button>
        <button class="btn-small" onclick="cerrarPanelDocente()">✕ Cerrar</button>
      </div>
    </div>

    <div id="docente-contenido"></div>
  </div>
</section>

<script>
/* =========================================================
   VERBOS DE DIAGNÓSTICO (FASE 1)
   ========================================================= */
const VERBOS = {
  CONTAR:{ic:'🔢',lbl:'CONTAR',desc:'Contar cuántas veces aparece un valor'},
  SUMAR:{ic:'➕',lbl:'SUMAR',desc:'Sumar solo los que cumplen una condición'},
  PROMEDIO:{ic:'⚖️',lbl:'PROMEDIAR',desc:'Promediar solo los que cumplen una condición'},
  BUSCAR:{ic:'🔍',lbl:'BUSCAR',desc:'Buscar un dato dentro de una tabla'},
  DECIDIR:{ic:'🔀',lbl:'DECIDIR',desc:'Asignar un resultado según se cumplan condiciones'},
  EXTRAER:{ic:'✂️',lbl:'EXTRAER TEXTO',desc:'Sacar una parte de un texto'},
  POSICION:{ic:'🎯',lbl:'UBICAR',desc:'Encontrar la posición de un carácter dentro de un texto'},
  UNIR:{ic:'🔗',lbl:'UNIR TEXTOS',desc:'Combinar varios textos en uno solo'},
  HOY:{ic:'📅',lbl:'FECHA DE HOY',desc:'Obtener la fecha actual del sistema'},
  DIF:{ic:'⏳',lbl:'DIFERENCIA FECHAS',desc:'Calcular cuánto tiempo hay entre dos fechas'},
  PARTE:{ic:'📆',lbl:'PARTE DE FECHA',desc:'Extraer solo el año, el mes o el día de una fecha'}
};

const VERBO_FUNCIONAL = {
  CONTAR:'Devuelve un NÚMERO ENTERO (cuántas veces aparece un valor). No suma los valores, solo cuenta ocurrencias.',
  SUMAR:'Devuelve un TOTAL ACUMULADO de los valores numéricos. Suma montos, no cuenta ocurrencias.',
  PROMEDIO:'Devuelve un PROMEDIO (total dividido entre cantidad). No acumula totales.',
  BUSCAR:'Devuelve el DATO de otra columna asociado a una clave. No suma, no cuenta, no clasifica.',
  DECIDIR:'Devuelve un TEXTO O CATEGORÍA según se cumplan condiciones. No suma ni busca.',
  EXTRAER:'Devuelve una PARTE de un texto existente (los primeros caracteres, los últimos, o los del medio).',
  POSICION:'Devuelve un NÚMERO que indica en qué posición está un carácter dentro de un texto.',
  UNIR:'Devuelve un TEXTO NUEVO combinando varios textos existentes.',
  HOY:'Devuelve la FECHA actual del sistema, sin hora.',
  DIF:'Devuelve un NÚMERO que expresa cuánto tiempo hay entre dos fechas.',
  PARTE:'Devuelve un COMPONENTE de una fecha (año, mes o día) como número suelto.'
};

const ICONOS = {
  'CONTAR.SI':'🔢','SUMAR.SI':'➕','PROMEDIO.SI':'⚖️',
  'SI':'🔀','SI.CONJUNTO':'🎚️','Y':'🔗','O':'🔗',
  'BUSCARV':'🔍','BUSCARX':'🔎',
  'CONCATENAR':'🔗','IZQUIERDA':'⬅️','DERECHA':'➡️','EXTRAE':'✂️','ENCONTRAR':'🎯',
  'HOY':'📅','SIFECHA':'⏳','DIAS.LAB':'🗓️','AÑO':'📆','DIA':'📅','MES':'📅'
};

const PROPOSITO_FUNC = {
  'SI':'evalúa UNA condición y devuelve un valor u otro según el resultado',
  'SI.CONJUNTO':'evalúa varios pares condición/resultado en orden y devuelve el del primero que se cumpla',
  'Y':'devuelve VERDADERO solo si TODAS las condiciones se cumplen a la vez',
  'O':'devuelve VERDADERO si AL MENOS UNA condición se cumple',
  'SUMAR.SI':'suma SOLO los valores que cumplen un criterio (suma condicional)',
  'CONTAR.SI':'cuenta CUÁNTAS celdas cumplen un criterio (no suma los valores)',
  'PROMEDIO.SI':'promedia SOLO los valores que cumplen un criterio',
  'BUSCARV':'busca una clave en la PRIMERA columna de un rango y devuelve otra columna a la derecha',
  'BUSCARX':'busca una clave en cualquier columna y devuelve otra, sin importar el orden',
  'CONCATENAR':'une varios textos (literales y/o referencias) en un solo texto nuevo',
  'IZQUIERDA':'extrae los primeros N caracteres de un texto',
  'DERECHA':'extrae los últimos N caracteres de un texto',
  'EXTRAE':'extrae N caracteres desde una posición intermedia del texto',
  'ENCONTRAR':'devuelve la POSICIÓN (número) de un texto dentro de otro',
  'HOY':'devuelve la fecha actual del sistema',
  'SIFECHA':'calcula la diferencia entre dos fechas en años, meses o días',
  'DIAS.LAB':'cuenta días HÁBILES entre dos fechas (excluye fines de semana y feriados)',
  'AÑO':'extrae solo el AÑO de una fecha',
  'DIA':'extrae solo el DÍA del mes (1-31) de una fecha',
  'MES':'extrae solo el MES (1-12) de una fecha'
};

/* =========================================================
   BANCO DE CASOS (54)
   ========================================================= */
const BANCO = [
{cat:'Condicional simple',contexto:'Recepción de materiales',caso:`El inspector anotó en <b>B2</b>: "APTO" o "NO APTO". Marca "Recibido" si es APTO, "Devuelto" si no.`,diag:{ok:'DECIDIR',ops:['DECIDIR','CONTAR','BUSCAR','EXTRAER']},func:{ok:'SI',ops:['SI','SI.CONJUNTO','Y','BUSCARV'],porque:{'SI.CONJUNTO':`=SI.CONJUNTO(...) recorre VARIOS pares condición/resultado. Aquí hay UNA sola condición.`,'Y':`=Y(...) solo devuelve VERDADERO o FALSO. No devuelve textos.`,'BUSCARV':`=BUSCARV(...) busca en tabla. Aquí no hay tabla.`}},form:{ok:'=SI(B2="APTO";"Recibido";"Devuelto")',ops:['=SI(B2="APTO";"Recibido";"Devuelto")','=SI.CONJUNTO(B2="APTO";"Recibido";"Devuelto")','=CONTAR.SI(B2;"APTO")'],porqueForm:{'=SI.CONJUNTO(B2="APTO";"Recibido";"Devuelto")':`SI.CONJUNTO necesita al menos 2 pares. Aquí solo hay 1.`,'=CONTAR.SI(B2;"APTO")':`Devuelve 1 o 0. El problema pide un texto.`}},expl:'SI evalúa UNA condición y devuelve un valor u otro.'},
{cat:'Condicional simple',contexto:'Control dimensional de muros',caso:`En <b>B2</b> está el espesor del muro. Debe medir exactamente 20 cm. Marca "Conforme" o "Revisar".`,diag:{ok:'DECIDIR',ops:['DECIDIR','CONTAR','SUMAR','EXTRAER']},func:{ok:'SI',ops:['SI','CONTAR.SI','SUMAR.SI','BUSCARV'],porque:{'CONTAR.SI':`Devuelve 1 o 0. El problema pide un texto.`,'SUMAR.SI':`Suma valores. Aquí no se suma.`,'BUSCARV':`Busca en tabla. No hay tabla.`}},form:{ok:'=SI(B2=20;"Conforme";"Revisar")',ops:['=SI(B2=20;"Conforme";"Revisar")','=CONTAR.SI(B2;20)','=SI(B2>20;"Conforme";"Revisar")'],porqueForm:{'=CONTAR.SI(B2;20)':`Cuenta, no decide.`,'=SI(B2>20;"Conforme";"Revisar")':`Evalúa "mayor que". El problema pide EXACTAMENTE 20.`}},expl:'SI compara el valor con un número exacto usando =.'},
{cat:'Condicional simple',contexto:'Estado de facturas',caso:`Si el monto en <b>B2</b> supera <b>$1.000.000</b> marca "Revisar gerencia"; si no, "Aprobación directa".`,diag:{ok:'DECIDIR',ops:['DECIDIR','CONTAR','PROMEDIO','EXTRAER']},func:{ok:'SI',ops:['SI','SI.CONJUNTO','O','BUSCARX'],porque:{'SI.CONJUNTO':`Solo hay 2 resultados. SI.CONJUNTO es para 3+.`,'O':`Devuelve V/F. No devuelve textos.`,'BUSCARX':`Busca en tabla. No hay tabla.`}},form:{ok:'=SI(B2>1000000;"Revisar gerencia";"Aprobación directa")',ops:['=SI(B2>1000000;"Revisar gerencia";"Aprobación directa")','=SI(B2>1000000;"Aprobación directa";"Revisar gerencia")','=SI.CONJUNTO(B2>1000000;"Revisar gerencia";"Aprobación directa")'],porqueForm:{'=SI(B2>1000000;"Aprobación directa";"Revisar gerencia")':`Textos invertidos.`,'=SI.CONJUNTO(B2>1000000;"Revisar gerencia";"Aprobación directa")':`Falta el cierre del par.`}},expl:'SI evalúa la condición y devuelve uno de dos textos.'},
/* ... el resto del banco va aquí (idéntico al que ya tienes) ... */
];

/* =========================================================
   PERSONAJES POR NIVEL (igual que antes)
   ========================================================= */
const PERSONAJES_NIVEL = {
  5:{rol:'Maestro de Construcción',emoji:'👷',casco:'#ffc857',cascoBorde:'#a67814',cascoBrillo:'#e0ab2c',chaleco:'#ff8a3d',chalecoBorde:'#b8591a',banda:'#ffe066',detalle:'bandas',fondo:'andamio'},
  8:{rol:'Supervisor de Obra',emoji:'🦺',casco:'#f5f5f5',cascoBorde:'#a0a0a0',cascoBrillo:'#d0d0d0',chaleco:'#ff8a3d',chalecoBorde:'#b8591a',banda:'#ffe066',detalle:'bandas',fondo:'grua'},
  12:{rol:'Administrador de Obra',emoji:'📊',casco:'#f5f5f5',cascoBorde:'#a0a0a0',cascoBrillo:'#d0d0d0',chaleco:'#3b82f6',chalecoBorde:'#1e40af',banda:'#ffffff',detalle:'bandas',fondo:'edificio'},
  16:{rol:'Gerente de Proyecto',emoji:'💼',casco:null,chaleco:'#1f2937',chalecoBorde:'#000000',banda:'#dc2626',detalle:'corbata',fondo:'skyline'}
};

function getFondoSVG(tipo){
  switch(tipo){
    case 'andamio': return `<rect x="0" y="0" width="400" height="340" fill="#8eb8d6"/><path d="M0 220 L60 195 L120 205 L180 190 L240 200 L300 185 L360 195 L400 200 L400 240 L0 240 Z" fill="#7a9db8" opacity=".6"/><rect x="0" y="240" width="400" height="100" fill="#8b7355"/><rect x="0" y="240" width="400" height="8" fill="#6b5438"/><g stroke="#7f8c8d" stroke-width="5" fill="none"><line x1="60" y1="240" x2="60" y2="90"/><line x1="130" y1="240" x2="130" y2="90"/><line x1="60" y1="130" x2="130" y2="130"/><line x1="60" y1="180" x2="130" y2="180"/></g><rect x="55" y="125" width="80" height="10" fill="#a0522d"/><rect x="270" y="195" width="110" height="45" fill="#ff8a3d" rx="4" stroke="#b8591a" stroke-width="2"/><rect x="270" y="195" width="110" height="14" fill="#ffc857"/><text x="325" y="228" font-family="Arial Black" font-size="13" font-weight="900" fill="#000" text-anchor="middle">OBRA</text>`;
    case 'grua': return `<rect x="0" y="0" width="400" height="340" fill="#9ec3dc"/><circle cx="330" cy="60" r="28" fill="#ffe680" opacity=".9"/><rect x="0" y="240" width="400" height="100" fill="#9c8b6e"/><rect x="85" y="90" width="14" height="150" fill="#f4b400" stroke="#a67814" stroke-width="1.5"/><rect x="45" y="82" width="200" height="14" fill="#f4b400" stroke="#a67814" stroke-width="1.5"/><rect x="230" y="86" width="12" height="30" fill="#333"/><line x1="236" y1="116" x2="236" y2="160" stroke="#333" stroke-width="2"/><rect x="225" y="160" width="22" height="14" fill="#555"/><polygon points="75,90 108,90 100,70 84,70" fill="#f4b400" stroke="#a67814" stroke-width="1.5"/>`;
    case 'edificio': return `<rect x="0" y="0" width="400" height="340" fill="#a8c8de"/><circle cx="80" cy="55" r="22" fill="#ffe680" opacity=".9"/><rect x="0" y="240" width="400" height="100" fill="#8b7355"/><rect x="230" y="70" width="130" height="170" fill="#c9c2b6" stroke="#8a8378" stroke-width="2"/><g fill="#5a6b7a"><rect x="245" y="85" width="18" height="18"/><rect x="275" y="85" width="18" height="18"/><rect x="305" y="85" width="18" height="18"/><rect x="245" y="120" width="18" height="18"/><rect x="275" y="120" width="18" height="18"/><rect x="245" y="155" width="18" height="18"/></g><g stroke="#7f8c8d" stroke-width="3" fill="none"><line x1="215" y1="240" x2="215" y2="90"/><line x1="230" y1="240" x2="230" y2="90"/></g><rect x="30" y="90" width="10" height="150" fill="#f4b400"/><rect x="10" y="84" width="140" height="10" fill="#f4b400"/>`;
    case 'skyline': return `<rect x="0" y="0" width="400" height="340" fill="#1a2942"/><circle cx="330" cy="55" r="18" fill="#ffe680" opacity=".9"/><rect x="0" y="240" width="400" height="100" fill="#0f1a2a"/><rect x="10" y="120" width="45" height="120" fill="#243b5a"/><rect x="65" y="90" width="55" height="150" fill="#1d3149"/><rect x="130" y="140" width="40" height="100" fill="#2a4463"/><rect x="330" y="105" width="60" height="135" fill="#1d3149"/><rect x="185" y="80" width="9" height="160" fill="#f4b400"/><rect x="140" y="74" width="200" height="10" fill="#f4b400"/><rect x="195" y="200" width="9" height="40" fill="#f4b400" opacity=".85"/>`;
    default: return '';
  }
}

function getPersonajeSVG(nivel, feliz){
  const c = PERSONAJES_NIVEL[nivel] || PERSONAJES_NIVEL[5];
  const tieneCasco = c.casco !== null;
  const cara = feliz
    ? `<ellipse cx="200" cy="180" rx="5" ry="6" fill="#1a1a1a"/><ellipse cx="230" cy="180" rx="5" ry="6" fill="#1a1a1a"/><circle cx="201.5" cy="178" r="1.6" fill="#fff"/><circle cx="231.5" cy="178" r="1.6" fill="#fff"/><path d="M195 205 Q215 225 235 205" stroke="#1a1a1a" stroke-width="4.5" fill="none" stroke-linecap="round"/><circle cx="185" cy="200" r="7" fill="#ff9b8a" opacity=".55"/><circle cx="245" cy="200" r="7" fill="#ff9b8a" opacity=".55"/>`
    : `<ellipse cx="200" cy="180" rx="5" ry="6" fill="#1a1a1a"/><ellipse cx="230" cy="180" rx="5" ry="6" fill="#1a1a1a"/><path d="M191 163 L207 168" stroke="#1a1a1a" stroke-width="3.5" stroke-linecap="round"/><path d="M239 163 L223 168" stroke="#1a1a1a" stroke-width="3.5" stroke-linecap="round"/><path d="M195 215 Q215 197 235 215" stroke="#1a1a1a" stroke-width="4.5" fill="none" stroke-linecap="round"/><path d="M193 188 Q188 198 193 203 Q198 198 193 188 Z" fill="#4db8ff"/>`;
  const cascoSVG = tieneCasco ? `<path d="M144 148 Q160 78 200 78 Q240 78 256 148 Z" fill="${c.casco}" stroke="${c.cascoBorde}" stroke-width="3"/><rect x="134" y="145" width="132" height="16" rx="4" fill="${c.casco}" stroke="${c.cascoBorde}" stroke-width="3"/>` : '';
  const pechoSVG = c.detalle === 'corbata' ? `<path d="M200 258 L208 273 L200 278 L192 273 Z" fill="#fff"/><path d="M200 278 L200 330 L205 324 L200 278 Z" fill="${c.banda}"/>` : `<rect x="150" y="290" width="100" height="10" fill="${c.banda}" rx="2"/><rect x="150" y="320" width="100" height="10" fill="${c.banda}" rx="2"/>`;
  return `<svg viewBox="0 0 400 340" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="xMidYMid meet">${getFondoSVG(c.fondo)}<path d="M150 273 Q175 253 200 253 Q225 253 250 273 L260 360 L140 360 Z" fill="${c.chaleco}" stroke="${c.chalecoBorde}" stroke-width="2.5"/><path d="M150 273 L128 328" stroke="${c.chaleco}" stroke-width="16" stroke-linecap="round"/><circle cx="128" cy="335" r="11" fill="#f7c9a1" stroke="#c9a070" stroke-width="1.5"/><path d="M250 273 L272 328" stroke="${c.chaleco}" stroke-width="16" stroke-linecap="round"/><circle cx="272" cy="335" r="11" fill="#f7c9a1" stroke="#c9a070" stroke-width="1.5"/><rect x="190" y="233" width="20" height="30" fill="#f7c9a1"/><circle cx="200" cy="185" r="46" fill="#f7c9a1" stroke="#c9a070" stroke-width="2"/><path d="M158 173 Q164 143 200 143 Q236 143 242 173 Q220 157 200 157 Q180 157 158 173 Z" fill="#2c1e14"/>${cascoSVG}${cara}${pechoSVG}</svg>`;
}

/* =========================================================
   MOTOR DE AUDIO (igual que antes)
   ========================================================= */
let audioCtx = null, sonidoActivo = true;
function initAudio(){ if(!audioCtx){ try{ audioCtx = new (window.AudioContext || window.webkitAudioContext)(); } catch(e){ return; } } if(audioCtx.state === 'suspended') audioCtx.resume(); }
function tocarSecuencia(notas){ if(!sonidoActivo) return; try{ initAudio(); } catch(e){ return; } if(!audioCtx) return; notas.forEach(n => { const osc = audioCtx.createOscillator(), gain = audioCtx.createGain(); osc.type = n.tipo || 'sine'; osc.frequency.value = n.f; const t0 = audioCtx.currentTime + n.t; gain.gain.setValueAtTime(0, t0); gain.gain.linearRampToValueAtTime(n.vol ?? 0.18, t0 + 0.01); gain.gain.exponentialRampToValueAtTime(0.001, t0 + n.d); osc.connect(gain); gain.connect(audioCtx.destination); osc.start(t0); osc.stop(t0 + n.d + 0.02); }); }
function sonido(tipo){ if(!sonidoActivo) return; switch(tipo){ case 'click': tocarSecuencia([{f:620,t:0,d:0.05,tipo:'sine',vol:0.08}]); break; case 'acierto': tocarSecuencia([{f:523.25,t:0,d:0.12,tipo:'sine',vol:0.18},{f:659.25,t:0.08,d:0.12,tipo:'sine',vol:0.16},{f:783.99,t:0.16,d:0.22,tipo:'sine',vol:0.14}]); break; case 'error': tocarSecuencia([{f:196,t:0,d:0.18,tipo:'square',vol:0.10},{f:146,t:0.12,d:0.24,tipo:'square',vol:0.12}]); break; case 'racha': tocarSecuencia([{f:880,t:0,d:0.08,tipo:'sine',vol:0.14},{f:1174,t:0.06,d:0.18,tipo:'sine',vol:0.14}]); break; case 'nivel': tocarSecuencia([{f:523.25,t:0,d:0.10,tipo:'triangle',vol:0.18},{f:659.25,t:0.08,d:0.10,tipo:'triangle',vol:0.18},{f:783.99,t:0.16,d:0.10,tipo:'triangle',vol:0.18},{f:1046.5,t:0.24,d:0.30,tipo:'triangle',vol:0.18}]); break; case 'obra': tocarSecuencia([{f:392,t:0,d:0.15,tipo:'sine',vol:0.16},{f:523.25,t:0.12,d:0.15,tipo:'sine',vol:0.16},{f:659.25,t:0.24,d:0.15,tipo:'sine',vol:0.16},{f:783.99,t:0.36,d:0.35,tipo:'sine',vol:0.18}]); break; case 'sobrecosto': tocarSecuencia([{f:220,t:0,d:0.30,tipo:'sawtooth',vol:0.09}]); break; } }
function toggleAudio(){ sonidoActivo = !sonidoActivo; const btn = document.getElementById('btn-audio'); if(btn){ btn.textContent = sonidoActivo ? '🔊' : '🔇'; btn.classList.toggle('mute', !sonidoActivo); } if(sonidoActivo) sonido('click'); }

/* =========================================================
   ⭐ NUEVO · PERSISTENCIA Y MÉTRICAS POR USUARIO ⭐
   ========================================================= */
const STORAGE_KEY = 'jefeObraExcel_v1';
let usuarioActual = null;
let partidaActual = null;
let tiempoFase = 0;

/* Estructura guardada:
   {
     version: 1,
     usuarios: {
       "juan": {
         nombre: "Juan",
         primeraVez: "ISO",
         ultimaVez: "ISO",
         partidas: [ { id, fecha, nivel, duracionSeg, aciertos, totalInteracciones,
                       efectividad, puntos, costoFinal, mejorRacha,
                       detalles: [ {caso, fase, correcta, elegida, ok, tiempoMs} ] } ]
       }
     }
   } */

function cargarDatos(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(!raw) return { version: 1, usuarios: {} };
    const d = JSON.parse(raw);
    if(!d.usuarios) d.usuarios = {};
    return d;
  } catch(e){
    return { version: 1, usuarios: {} };
  }
}

function guardarDatos(data){
  try{
    localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
    return true;
  } catch(e){
    console.error('No se pudo guardar:', e);
    return false;
  }
}

function normalizarNombre(s){
  return String(s).trim().toLowerCase()
    .normalize('NFD').replace(/[\u0300-\u036f]/g,''); // quita tildes
}

function registrarPartidaFin(partida){
  const data = cargarDatos();
  const clave = normalizarNombre(usuarioActual);
  if(!data.usuarios[clave]){
    data.usuarios[clave] = {
      nombre: usuarioActual,
      primeraVez: new Date().toISOString(),
      ultimaVez: new Date().toISOString(),
      partidas: []
    };
  }
  data.usuarios[clave].ultimaVez = new Date().toISOString();
  data.usuarios[clave].partidas.push(partida);
  guardarDatos(data);
}

/* Genera un ID único simple */
function uid(){
  return Date.now().toString(36) + Math.random().toString(36).slice(2,7);
}

/* ---------- FLUJO DE IDENTIFICACIÓN ---------- */
function pedirUsuario(){
  const modal = document.getElementById('modal-identificacion');
  const input = document.getElementById('input-usuario');
  const error = document.getElementById('error-usuario');
  modal.classList.add('activo');
  error.classList.remove('visible');
  input.value = '';
  setTimeout(() => input.focus(), 100);
  input.onkeydown = e => { if(e.key === 'Enter') confirmarUsuario(); };
}

function confirmarUsuario(){
  const input = document.getElementById('input-usuario');
  const error = document.getElementById('error-usuario');
  const nombre = input.value.trim();
  if(nombre.length < 2){
    error.classList.add('visible');
    input.focus();
    return;
  }
  usuarioActual = nombre;
  document.getElementById('jugador-actual').textContent = nombre;
  document.getElementById('modal-identificacion').classList.remove('activo');
  sonido('click');
  // Continúa con el inicio del juego
  continuarInicio();
}

/* Variable que guarda la acción pendiente tras identificarse */
let accionPendiente = null;

function continuarInicio(){
  if(accionPendiente){ const f = accionPendiente; accionPendiente = null; f(); }
}

/* ---------- HOOKS EN EL MOTOR DEL JUEGO ---------- */

/* Al iniciar una partida, guardamos tiempo y creamos partidaActual */
function iniciarPartidaTracking(nivel){
  partidaActual = {
    id: uid(),
    fecha: new Date().toISOString(),
    usuario: usuarioActual,
    nivel: nivel,
    inicioMs: Date.now(),
    duracionSeg: 0,
    aciertos: 0,
    totalInteracciones: 0,
    efectividad: 0,
    puntos: 0,
    costoFinal: 0,
    mejorRacha: 0,
    detalles: []
  };
  tiempoFase = Date.now();
}

/* Al responder, guardamos el detalle */
function registrarRespuestaTracking(fase, elegido, correcta, esCorrecta, casoTexto){
  if(!partidaActual) return;
  const ahora = Date.now();
  const tiempoMs = ahora - tiempoFase;
  tiempoFase = ahora;
  partidaActual.detalles.push({
    caso: casoTexto.slice(0, 80),
    fase: fase,
    correcta: correcta,
    elegida: elegido,
    ok: esCorrecta,
    tiempoMs: tiempoMs
  });
}

/* Al terminar, cerramos y guardamos */
function cerrarPartidaTracking(){
  if(!partidaActual) return;
  partidaActual.duracionSeg = Math.round((Date.now() - partidaActual.inicioMs) / 1000);
  partidaActual.aciertos = aciertos;
  partidaActual.totalInteracciones = totalInteracciones;
  partidaActual.efectividad = totalInteracciones ? Math.round(aciertos / totalInteracciones * 100) : 0;
  partidaActual.puntos = mejorRacha * 10 + aciertos * 5; // ejemplo simple
  partidaActual.costoFinal = costoObra;
  partidaActual.mejorRacha = mejorRacha;
  registrarPartidaFin(partidaActual);
  partidaActual = null;
}

/* =========================================================
   ⭐ NUEVO · PANEL DOCENTE ⭐
   ========================================================= */
function abrirPanelDocente(){
  const data = cargarDatos();
  const contenido = document.getElementById('docente-contenido');
  const usuarios = Object.entries(data.usuarios);

  if(usuarios.length === 0){
    contenido.innerHTML = `
      <div class="vacio">
        📭 <b>Aún no hay datos registrados</b><br>
        Cuando los estudiantes jueguen, sus partidas aparecerán aquí automáticamente.
      </div>`;
    mostrar('pantalla-docente');
    return;
  }

  // ===== Métricas globales =====
  let totalPartidas = 0, totalAciertos = 0, totalRespuestas = 0, sumaEfectividad = 0;
  usuarios.forEach(([, u]) => {
    u.partidas.forEach(p => {
      totalPartidas++;
      totalAciertos += p.aciertos;
      totalRespuestas += p.totalInteracciones;
      sumaEfectividad += p.efectividad;
    });
  });
  const efectividadGlobal = totalRespuestas ? Math.round(totalAciertos / totalRespuestas * 100) : 0;
  const efectividadProm = totalPartidas ? Math.round(sumaEfectividad / totalPartidas) : 0;

  // ===== Ranking de alumnos =====
  const ranking = usuarios.map(([clave, u]) => {
    const aciertos = u.partidas.reduce((s,p) => s + p.aciertos, 0);
    const total = u.partidas.reduce((s,p) => s + p.totalInteracciones, 0);
    const efectividad = total ? Math.round(aciertos/total * 100) : 0;
    const ultima = u.partidas.length ? new Date(u.ultimaVez) : null;
    return {
      nombre: u.nombre,
      partidas: u.partidas.length,
      aciertos: aciertos,
      total: total,
      efectividad: efectividad,
      ultima: ultima
    };
  }).sort((a,b) => b.efectividad - a.efectividad);

  // ===== Análisis por pregunta (fallas) =====
  const porPregunta = {};
  usuarios.forEach(([, u]) => {
    u.partidas.forEach(p => {
      p.detalles.forEach(d => {
        const key = d.caso.slice(0, 60);
        if(!porPregunta[key]) porPregunta[key] = { ok: 0, total: 0, fase2Fallas: 0 };
        porPregunta[key].total++;
        if(d.ok) porPregunta[key].ok++;
        else if(d.fase === 2) porPregunta[key].fase2Fallas++;
      });
    });
  });

  const preguntasDificiles = Object.entries(porPregunta)
    .map(([caso, v]) => ({
      caso,
      total: v.total,
      ok: v.ok,
      tasaFalla: v.total ? Math.round((v.total - v.ok) / v.total * 100) : 0
    }))
    .filter(p => p.total >= 2)
    .sort((a,b) => b.tasaFalla - a.tasaFalla)
    .slice(0, 8);

  // ===== Análisis por función (fase 2) =====
  const porFuncion = {};
  usuarios.forEach(([, u]) => {
    u.partidas.forEach(p => {
      p.detalles.forEach(d => {
        if(d.fase === 2){
          if(!porFuncion[d.correcta]) porFuncion[d.correcta] = { ok: 0, total: 0 };
          porFuncion[d.correcta].total++;
          if(d.ok) porFuncion[d.correcta].ok++;
        }
      });
    });
  });

  const funcionesDificiles = Object.entries(porFuncion)
    .map(([func, v]) => ({
      func,
      total: v.total,
      ok: v.ok,
      tasaFalla: v.total ? Math.round((v.total - v.ok) / v.total * 100) : 0
    }))
    .filter(f => f.total >= 2)
    .sort((a,b) => b.tasaFalla - a.tasaFalla)
    .slice(0, 8);

  // ===== Render =====
  contenido.innerHTML = `
    <div class="metricas-resumen">
      <div class="metrica-card"><span>${usuarios.length}</span><small>Estudiantes</small></div>
      <div class="metrica-card"><span>${totalPartidas}</span><small>Partidas</small></div>
      <div class="metrica-card"><span>${efectividadGlobal}%</span><small>Efectividad global</small></div>
      <div class="metrica-card"><span>${efectividadProm}%</span><small>Efectividad promedio</small></div>
    </div>

    <div class="seccion-docente">
      <h3>🏆 Ranking de estudiantes</h3>
      <table class="tabla-alumnos">
        <thead>
          <tr>
            <th>Estudiante</th>
            <th>Partidas</th>
            <th>Aciertos</th>
            <th>Total</th>
            <th>Efectividad</th>
          </tr>
        </thead>
        <tbody>
          ${ranking.map(r => `
            <tr>
              <td>${esc(r.nombre)}</td>
              <td class="num">${r.partidas}</td>
              <td class="num ok">${r.aciertos}</td>
              <td class="num">${r.total}</td>
              <td class="num ${r.efectividad >= 70 ? 'ok' : r.efectividad >= 50 ? 'gold' : 'bad'}">${r.efectividad}%</td>
            </tr>
          `).join('')}
        </tbody>
      </table>
    </div>

    ${preguntasDificiles.length ? `
      <div class="seccion-docente">
        <h3>⚠️ Preguntas con más fallas (candidatas a reforzar)</h3>
        ${preguntasDificiles.map(p => `
          <div class="barra-item">
            <div class="nombre">${esc(p.caso)}…</div>
            <div class="valores">
              <span class="pct">${p.tasaFalla}% fallas</span>
              <span>(${p.total - p.ok}/${p.total})</span>
            </div>
          </div>
        `).join('')}
      </div>
    ` : ''}

    ${funcionesDificiles.length ? `
      <div class="seccion-docente">
        <h3>🔧 Funciones donde más se equivocan (Fase 2)</h3>
        ${funcionesDificiles.map(f => `
          <div class="barra-item">
            <div class="nombre">${esc(f.func)}</div>
            <div class="valores">
              <span class="fallas">${f.tasaFalla}% fallas</span>
              <span>(${f.total - f.ok}/${f.total})</span>
            </div>
          </div>
        `).join('')}
      </div>
    ` : ''}

    <div class="seccion-docente">
      <h3>📋 Partidas recientes</h3>
      ${usuarios.flatMap(([, u]) =>
        u.partidas.slice(-3).reverse().map(p => `
          <div class="barra-item">
            <div class="nombre">
              <b>${esc(u.nombre)}</b> · ${new Date(p.fecha).toLocaleDateString('es-CL')} ·
              Nivel ${p.nivel} casos · ${p.duracionSeg}s
            </div>
            <div class="valores">
              <span class="${p.efectividad >= 70 ? 'ok' : p.efectividad >= 50 ? 'pct' : 'fallas'}">${p.efectividad}%</span>
            </div>
          </div>
        `)
      ).slice(0, 10).join('')}
    </div>
  `;

  mostrar('pantalla-docente');
}

function cerrarPanelDocente(){
  detenerAutoRetorno();
  mostrar('pantalla-inicio');
}

/* ---------- EXPORTAR CSV ---------- */
function exportarCSV(){
  const data = cargarDatos();
  const filas = [];

  // Cabecera del CSV
  filas.push([
    'usuario','partida_id','fecha','nivel','duracion_seg','aciertos','total_interacciones',
    'efectividad_pct','costo_final','mejor_racha',
    'caso','fase','correcta','elegida','ok','tiempo_ms'
  ].join(';'));

  Object.values(data.usuarios).forEach(u => {
    u.partidas.forEach(p => {
      if(p.detalles && p.detalles.length){
        p.detalles.forEach(d => {
          filas.push([
            u.nombre,
            p.id,
            p.fecha,
            p.nivel,
            p.duracionSeg,
            p.aciertos,
            p.totalInteracciones,
            p.efectividad,
            p.costoFinal,
            p.mejorRacha,
            `"${(d.caso || '').replace(/"/g,'""')}"`,
            d.fase,
            d.correcta,
            d.elegida,
            d.ok ? 1 : 0,
            d.tiempoMs
          ].join(';'));
        });
      } else {
        filas.push([
          u.nombre, p.id, p.fecha, p.nivel, p.duracionSeg,
          p.aciertos, p.totalInteracciones, p.efectividad,
          p.costoFinal, p.mejorRacha, '', '', '', '', '', ''
        ].join(';'));
      }
    });
  });

  const csv = '\uFEFF' + filas.join('\n'); // BOM para Excel
  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  const fecha = new Date().toISOString().slice(0,10);
  a.href = url;
  a.download = `jefe-obra-metricas_${fecha}.csv`;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

/* ---------- RESET ---------- */
function resetearDatos(){
  if(!confirm('⚠️ ¿Seguro que quieres borrar TODOS los datos de TODOS los estudiantes?\n\nEsta acción no se puede deshacer.')) return;
  if(!confirm('🚨 Confirmación final: se perderán todas las partidas registradas. ¿Continuar?')) return;
  localStorage.removeItem(STORAGE_KEY);
  alert('✅ Datos borrados correctamente.');
  abrirPanelDocente();
}

/* =========================================================
   MOTOR DEL JUEGO
   ========================================================= */
const $ = s => document.querySelector(s);
const COSTO_BASE = 50000000;
const AHORRO_ACIERTO = 250000;
const SOBRECOSTO_ERROR = 800000;

let preguntas = [], idxCaso = 0, faseActual = 1;
let aciertos = 0, totalInteracciones = 0;
let costoObra = COSTO_BASE;
let resultadosPorCaso = [];
let fallos = [];
let bloqueado = false;
let nivelActual = 8;
let autoTimer = null;
let racha = 0, mejorRacha = 0;

function mostrar(id){
  document.querySelectorAll('.pantalla').forEach(p => p.classList.remove('activa'));
  document.getElementById(id).classList.add('activa');
  window.scrollTo({top:0,behavior:'smooth'});
}
function barajar(arr){ const a = arr.slice(); for(let i = a.length-1; i>0; i--){ const j = Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]]; } return a; }
function formatear(n){ const abs = Math.abs(n); return (n<0?'-$':'$') + abs.toLocaleString('es-CL'); }
function animarNumero(el, desde, hasta, dur=600){ const t0 = performance.now(); function paso(t){ const p = Math.min((t-t0)/dur,1); const v = desde + (hasta-desde)*(1-Math.pow(1-p,3)); el.textContent = formatear(Math.round(v)); if(p<1) requestAnimationFrame(paso); } requestAnimationFrame(paso); }
function actualizarEstiloCosto(){ const wrap = $('#costo-wrap'); wrap.classList.remove('ahorro','sobre'); if(costoObra < COSTO_BASE) wrap.classList.add('ahorro'); else if(costoObra > COSTO_BASE) wrap.classList.add('sobre'); }
function esc(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }
function actualizarBadgeRacha(){ const badgeRacha = document.getElementById('badge-racha'); const elRacha = document.getElementById('racha'); if(elRacha) elRacha.textContent = racha; if(badgeRacha){ badgeRacha.classList.remove('caliente'); if(racha >= 3){ void badgeRacha.offsetWidth; badgeRacha.classList.add('caliente'); } } }

function feedbackError(fase, elegido, correcta, caso){
  const nombreElegido = fase === 1 ? VERBOS[elegido].lbl : elegido;
  const nombreCorrecta = fase === 1 ? VERBOS[correcta].lbl : correcta;
  const descElegida = fase === 1 ? VERBOS[elegido].desc : (PROPOSITO_FUNC[elegido] || 'una función que no aplica aquí');
  const descCorrecta = fase === 1 ? VERBOS[correcta].desc : (PROPOSITO_FUNC[correcta] || '');
  let porque;
  if(fase === 2 && caso.func.porque && caso.func.porque[elegido]){ porque = caso.func.porque[elegido]; }
  else if(fase === 3 && caso.form.porqueForm && caso.form.porqueForm[elegido]){ porque = caso.form.porqueForm[elegido]; }
  else if(fase === 1){
    const funcElegido = VERBO_FUNCIONAL[elegido] || descElegida;
    const funcCorrecto = VERBO_FUNCIONAL[correcta] || descCorrecta;
    porque = `<b>${nombreElegido}</b>: ${funcElegido}<br><br><b>${nombreCorrecta}</b>: ${funcCorrecto}<br><br>El problema pide específicamente <b>${nombreCorrecta.toLowerCase()}</b>.`;
  } else {
    porque = `La opción elegida tiene un problema funcional concreto: la función que usa no devuelve el tipo de resultado que el caso necesita.`;
  }
  return `<div class="fb-compara"><div class="fb-lado mal"><div class="fb-lado-tit">❌ Lo que elegiste</div><div class="fb-lado-nom">${esc(nombreElegido)}</div><div class="fb-lado-desc">${descElegida}</div></div><div class="fb-lado ok"><div class="fb-lado-tit">✅ Lo correcto</div><div class="fb-lado-nom">${esc(nombreCorrecta)}</div><div class="fb-lado-desc">${descCorrecta}</div></div></div><div class="fb-porque"><span class="et">🔍 ¿Por qué NO es ${esc(nombreElegido)}?</span><br>${porque}</div><div class="fb-exp-ok"><b>💡 Recuerda:</b> ${caso.expl}</div>`;
}

function feedbackOK(fase, correcta, caso){
  const nombre = fase === 1 ? VERBOS[correcta].lbl : correcta;
  const desc = fase === 1 ? VERBOS[correcta].desc : (PROPOSITO_FUNC[correcta] || '');
  return `<div class="fb-exp-ok"><b>${esc(nombre)}</b> — ${desc}<br><br><b>💡 Recuerda:</b> ${caso.expl}</div>`;
}

/* ---------- INICIAR (ahora pide usuario) ---------- */
function iniciar(){
  // Si no hay usuario, mostrar modal y guardar la acción pendiente
  if(!usuarioActual){
    accionPendiente = () => iniciar();
    pedirUsuario();
    return;
  }

  initAudio();
  const nivelDesdeForm = document.querySelector('input[name="num"]:checked');
  const n = nivelDesdeForm ? parseInt(nivelDesdeForm.value, 10) : nivelActual;
  nivelActual = n;
  const pool = barajar(BANCO).slice(0, Math.min(n, BANCO.length));

  preguntas = pool.map(p => ({
    cat: p.cat, contexto: p.contexto, caso: p.caso,
    diag: { ok: p.diag.ok, ops: barajar(p.diag.ops) },
    func: { ok: p.func.ok, ops: barajar(p.func.ops), porque: p.func.porque || {} },
    form: { ok: p.form.ok, ops: barajar(p.form.ops), porqueForm: p.form.porqueForm || {} },
    expl: p.expl
  }));

  idxCaso = 0; faseActual = 1; aciertos = 0; totalInteracciones = 0;
  costoObra = COSTO_BASE; racha = 0; mejorRacha = 0;
  resultadosPorCaso = preguntas.map(() => ({ diag: null, func: null, form: null }));
  fallos = [];

  /* ===== NUEVO: iniciar tracking de partida ===== */
  iniciarPartidaTracking(n);

  mostrar('pantalla-juego');
  construirTablero('tablero');
  actualizarEstiloCosto();
  actualizarBadgeRacha();
  render();
}

function construirTablero(contenedorId){
  const cont = document.getElementById(contenedorId);
  cont.innerHTML = '';
  for(let i = 0; i < preguntas.length; i++){
    const card = document.createElement('div');
    card.className = 'caso-card';
    card.dataset.caso = i;
    card.innerHTML = `<div class="caso-num">Caso ${i+1}</div><div class="caso-fases"><span class="fase-dot" data-fase="1"></span><span class="fase-dot" data-fase="2"></span><span class="fase-dot" data-fase="3"></span></div>`;
    cont.appendChild(card);
  }
  actualizarInfoTablero();
  actualizarEstadoTarjetas();
}

function pintarFaseCaso(i, fase, estado, contenedorId='tablero'){
  const cont = document.getElementById(contenedorId);
  const card = cont.children[i];
  if(!card) return;
  const dot = card.querySelector(`.fase-dot[data-fase="${fase}"]`);
  if(!dot) return;
  dot.classList.remove('act','ok','bad');
  dot.classList.add(estado === 'ok' ? 'ok' : estado === 'bad' ? 'bad' : 'act');
}

function actualizarEstadoTarjetas(contenedorId='tablero'){
  const cont = document.getElementById(contenedorId);
  resultadosPorCaso.forEach((r, i) => {
    const card = cont.children[i];
    if(!card) return;
    const vals = [r.diag, r.func, r.form];
    const completos = vals.filter(v => v !== null).length;
    card.classList.remove('activo','perfecto','parcial');
    if(completos === 3){
      if(vals.every(v => v === 1)) card.classList.add('perfecto');
      else card.classList.add('parcial');
    } else if(i === idxCaso){ card.classList.add('activo'); }
  });
}

function actualizarInfoTablero(){
  const perfectos = resultadosPorCaso.filter(r => r.diag === 1 && r.func === 1 && r.form === 1).length;
  const completados = resultadosPorCaso.filter(r => r.diag !== null && r.func !== null && r.form !== null).length;
  $('#tablero-info').textContent = `${completados} / ${preguntas.length} casos · ${perfectos} perfectos`;
}

function render(){
  bloqueado = false;
  const p = preguntas[idxCaso];
  $('#prog-texto').textContent = `Caso ${idxCaso+1} de ${preguntas.length}`;
  $('#costo').textContent = formatear(costoObra);
  actualizarEstiloCosto();
  $('#contexto').textContent = p.contexto;
  $('#caso').innerHTML = p.caso;
  pintarFaseCaso(idxCaso, faseActual, 'act');
  actualizarEstadoTarjetas();
  document.querySelectorAll('.paso').forEach(el => el.classList.remove('on'));
  document.querySelector(`.paso[data-paso="${faseActual}"]`).classList.add('on');
  const opciones = $('#opciones');
  opciones.innerHTML = '';
  opciones.className = 'opciones';

  if(faseActual === 1){
    $('#fase-titulo').innerHTML = '🔎 Fase 1 · <span>¿Qué necesitas hacer?</span>';
    opciones.classList.add('una');
    p.diag.ops.forEach(op => {
      const v = VERBOS[op];
      const b = document.createElement('button');
      b.className = 'opcion';
      b.innerHTML = `<span class="ic">${v.ic}</span><div><div class="lbl">${v.lbl}</div><div class="desc">${v.desc}</div></div>`;
      b.onclick = () => responder(1, op, b);
      opciones.appendChild(b);
    });
  } else if(faseActual === 2){
    $('#fase-titulo').innerHTML = '🔧 Fase 2 · <span>Elige la herramienta (función)</span>';
    p.func.ops.forEach(op => {
      const b = document.createElement('button');
      b.className = 'opcion';
      b.innerHTML = `<span class="ic">${ICONOS[op] || '🔧'}</span><div class="fn">${op}</div>`;
      b.onclick = () => responder(2, op, b);
      opciones.appendChild(b);
    });
  } else {
    $('#fase-titulo').innerHTML = '📝 Fase 3 · <span>Reconoce la fórmula correcta</span>';
    opciones.classList.add('una');
    p.form.ops.forEach(op => {
      const b = document.createElement('button');
      b.className = 'opcion';
      b.innerHTML = `<span class="ic">🧮</span><div class="fn">${op}</div>`;
      b.onclick = () => responder(3, op, b);
      opciones.appendChild(b);
    });
  }
  $('#feedback').className = 'feedback';
  $('#btn-next').style.display = 'none';
  $('#costo-delta').className = 'delta';
}

function responder(fase, valor, boton){
  if(bloqueado) return;
  bloqueado = true;
  const p = preguntas[idxCaso];
  const correcta = fase === 1 ? p.diag.ok : fase === 2 ? p.func.ok : p.form.ok;
  const esCorrecta = valor === correcta;
  totalInteracciones++;
  const desde = costoObra;

  if(esCorrecta){
    aciertos++; racha++;
    if(racha > mejorRacha) mejorRacha = racha;
    costoObra -= AHORRO_ACIERTO;
    sonido(racha >= 3 ? 'racha' : 'acierto');
  } else {
    racha = 0;
    costoObra += SOBRECOSTO_ERROR;
    sonido('error');
  }

  /* ===== NUEVO: registrar respuesta en la partida actual ===== */
  registrarRespuestaTracking(fase, valor, correcta, esCorrecta, p.caso);

  animarNumero($('#costo'), desde, costoObra);
  actualizarBadgeRacha();
  const delta = $('#costo-delta');
  delta.textContent = esCorrecta ? `−$${AHORRO_ACIERTO.toLocaleString('es-CL')}` : `+$${SOBRECOSTO_ERROR.toLocaleString('es-CL')}`;
  delta.className = 'delta show ' + (esCorrecta ? 'ahorro' : 'sobre');
  setTimeout(() => { delta.className = 'delta'; }, 1600);
  setTimeout(actualizarEstiloCosto, 200);

  document.querySelectorAll('.opcion').forEach(b => {
    b.disabled = true;
    const txt = b.textContent;
    if(fase === 1){ const v = VERBOS[correcta]; if(txt.includes(v.lbl)) b.classList.add('correcta'); }
    else { if(txt.includes(correcta)) b.classList.add('correcta'); }
  });
  if(!esCorrecta) boton.classList.add('incorrecta');

  const campo = fase === 1 ? 'diag' : fase === 2 ? 'func' : 'form';
  resultadosPorCaso[idxCaso][campo] = esCorrecta ? 1 : 0;
  pintarFaseCaso(idxCaso, fase, esCorrecta ? 'ok' : 'bad');
  actualizarEstadoTarjetas();
  actualizarInfoTablero();

  const pasoEl = document.querySelector(`.paso[data-paso="${fase}"]`);
  pasoEl.classList.remove('on');
  pasoEl.classList.add(esCorrecta ? 'done-ok' : 'done-bad');

  const fb = $('#feedback');
  const tit = $('#fb-titulo');
  const txt = $('#fb-texto');
  fb.classList.add('visible', esCorrecta ? 'ok' : 'bad');
  if(esCorrecta){
    tit.textContent = `✅ ¡Correcto! Ahorro aplicado: −$${AHORRO_ACIERTO.toLocaleString('es-CL')}`;
    txt.innerHTML = feedbackOK(fase, correcta, p);
  } else {
    tit.textContent = `❌ Error · Sobrecosto: +$${SOBRECOSTO_ERROR.toLocaleString('es-CL')}`;
    txt.innerHTML = feedbackError(fase, valor, correcta, p);
    const r = resultadosPorCaso[idxCaso];
    if(fase === 3 && (!r.diag || !r.func || !r.form)){
      fallos.push({ caso: p.caso, correcta: p.func.ok, expl: p.expl });
    }
  }

  if(fase === 3){
    const r = resultadosPorCaso[idxCaso];
    if(r.diag === 1 && r.func === 1 && r.form === 1){
      setTimeout(() => sonido('nivel'), 400);
    }
  }

  const btn = $('#btn-next');
  if(fase === 3){ btn.textContent = (idxCaso === preguntas.length-1) ? 'Ver entrega de obra →' : 'Siguiente caso →'; }
  else { btn.textContent = `Ir a fase ${fase+1} →`; }
  btn.style.display = 'inline-flex';
  btn.focus();
}

function avanzar(){
  if(faseActual < 3){ faseActual++; render(); }
  else {
    idxCaso++; faseActual = 1;
    if(idxCaso >= preguntas.length){ terminar(); }
    else {
      document.querySelectorAll('.paso').forEach(el => el.classList.remove('done-ok','done-bad','on'));
      render();
    }
  }
}

function terminar(){
  const pct = Math.round(aciertos / totalInteracciones * 100);
  const diferencia = costoObra - COSTO_BASE;

  /* ===== NUEVO: guardar partida ===== */
  cerrarPartidaTracking();

  const contFinal = $('#tablero-final');
  contFinal.innerHTML = '';
  resultadosPorCaso.forEach((r, i) => {
    const card = document.createElement('div');
    card.className = 'caso-card';
    const vals = [r.diag, r.func, r.form];
    if(vals.every(v => v === 1)) card.classList.add('perfecto');
    else if(vals.every(v => v !== null)) card.classList.add('parcial');
    card.innerHTML = `<div class="caso-num">Caso ${i+1}</div><div class="caso-fases"><span class="fase-dot ${r.diag===1?'ok':'bad'}"></span><span class="fase-dot ${r.func===1?'ok':'bad'}"></span><span class="fase-dot ${r.form===1?'ok':'bad'}"></span></div>`;
    contFinal.appendChild(card);
  });

  $('#fin-aciertos').textContent = `${aciertos}/${totalInteracciones}`;
  $('#fin-pct').textContent = pct + '%';
  $('#fin-costo').textContent = formatear(costoObra);

  const elDelta = $('#fin-delta');
  if(diferencia < 0){ elDelta.textContent = formatear(diferencia); elDelta.className = 'ok'; }
  else if(diferencia > 0){ elDelta.textContent = '+' + formatear(diferencia).slice(1); elDelta.className = 'bad'; }
  else { elDelta.textContent = '$0'; elDelta.className = 'neutral'; }

  let medalla='🚧', titulo='', msg='';
  if(diferencia <= -5000000 && pct >= 85){ medalla='🏆'; titulo='¡Obra con utilidad!'; msg='Cerraste muy por debajo del costo base y con alta efectividad.'; }
  else if(diferencia < 0){ medalla='🎯'; titulo='Obra entregada bajo costo'; msg='Buen manejo del presupuesto.'; }
  else if(diferencia === 0){ medalla='🤝'; titulo='Obra entregada al costo'; msg='Cerraste justo en el costo base.'; }
  else if(diferencia <= 5000000){ medalla='👍'; titulo='Obra con sobrecosto leve'; msg='Hubo retrabajos.'; }
  else if(diferencia <= 12000000){ medalla='⚠️'; titulo='Obra con sobrecosto importante'; msg='Los errores pesaron en el presupuesto.'; }
  else { medalla='🚨'; titulo='Obra en crisis'; msg='Varios errores generaron sobrecostos graves.'; }

  $('#fin-medalla').textContent = medalla;
  $('#fin-titulo').textContent = titulo;
  $('#fin-msg').innerHTML = `${msg} · Nivel: ${nivelActual} casos · Mejor racha: <b style="color:var(--naranja)">🔥 ${mejorRacha}</b>`;

  const personajeEl = document.getElementById('personaje-final');
  if(personajeEl){
    const esFeliz = diferencia < 0;
    const c = PERSONAJES_NIVEL[nivelActual] || PERSONAJES_NIVEL[5];
    personajeEl.innerHTML = `${getPersonajeSVG(nivelActual, esFeliz)}<div class="personaje-rol">${c.emoji} ${c.rol}</div><div class="personaje-msg" style="color:${esFeliz ? 'var(--green2)' : 'var(--bad)'}">${esFeliz ? `💰 ¡Ahorraste ${formatear(Math.abs(diferencia))}!` : diferencia > 0 ? `💸 Sobrecosto de ${formatear(diferencia)}` : `🤝 Cerraste al costo`}</div>`;
  }

  const rep = $('#repaso');
  if(fallos.length === 0){
    rep.innerHTML = '<p class="perfecto">🎉 ¡No fallaste ninguna fase! Obra impecable.</p>';
  } else {
    const vistos = new Set(), unicos = [];
    fallos.forEach(f => {
      const key = f.correcta + '|' + f.caso.slice(0,40);
      if(!vistos.has(key)){ vistos.add(key); unicos.push(f); }
    });
    rep.innerHTML = '<h3>Casos para repasar</h3>' + unicos.map(f => `<div class="repaso-item"><p class="repaso-caso">${f.caso}</p><p class="repaso-fn">✔ Función: ${f.correcta}</p><p class="repaso-exp">${f.expl}</p></div>`).join('');
  }

  if(diferencia < 0 && pct >= 85) sonido('obra');
  else if(diferencia < 0) sonido('nivel');
  else if(diferencia <= 5000000) sonido('acierto');
  else sonido('sobrecosto');

  mostrar('pantalla-final');
  iniciarAutoRetorno(20);
}

function iniciarAutoRetorno(segundos){
  if(autoTimer){ clearInterval(autoTimer); autoTimer = null; }
  const elCount = document.getElementById('auto-count');
  const elWrap  = document.getElementById('auto-volver');
  if(elWrap) elWrap.style.display = 'block';
  let restantes = segundos;
  if(elCount) elCount.textContent = restantes;
  autoTimer = setInterval(() => {
    restantes--;
    if(elCount) elCount.textContent = restantes;
    if(restantes <= 0){ clearInterval(autoTimer); autoTimer = null; volverAlInicio(); }
  }, 1000);
}
function detenerAutoRetorno(){ if(autoTimer){ clearInterval(autoTimer); autoTimer = null; } }
function volverAlInicio(){ detenerAutoRetorno(); sonido('click'); preguntas = []; resultadosPorCaso = []; idxCaso = 0; faseActual = 1; mostrar('pantalla-inicio'); }
function repetirNivel(){ detenerAutoRetorno(); sonido('click'); iniciar(); }

document.addEventListener('keydown', e => {
  if(!$('#pantalla-juego').classList.contains('activa')) return;
  if(['1','2','3','4'].includes(e.key)){
    const ops = document.querySelectorAll('.opcion');
    const i = parseInt(e.key,10) - 1;
    if(ops[i] && !ops[i].disabled) ops[i].click();
  }
  if(e.key === 'Enter' && $('#btn-next').style.display !== 'none'){ e.preventDefault(); avanzar(); }
});
</script>
</body>
</html>
