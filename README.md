<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>The Softest Flex • Kélani</title>
  <link rel="manifest" href="manifest.json"/>
  <meta name="theme-color" content="#E8B6C5"/>
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <link rel="apple-touch-icon" href="icon-192.png"/>
  <style>
    :root{--card:#fff;--ink:#241a1c;--accent:#d7a9c7}
    *{box-sizing:border-box} body{margin:0;background:linear-gradient(180deg,#fdeff6 0,#fff 40%);font-family:ui-sans-serif,system-ui}
    header{padding:20px 16px} h1{margin:0;font-weight:700;letter-spacing:.3px}
    .wrap{max-width:780px;margin:0 auto;padding:0 16px 40px}
    .panel{background:var(--card);border:1px solid #f3e6ef;border-radius:16px;box-shadow:0 10px 30px rgba(215,169,199,.25);padding:16px;margin:14px 0}
    .row{display:grid;grid-template-columns:1fr 1fr;gap:10px} @media (max-width:720px){.row{grid-template-columns:1fr}}
    label{font-size:14px;color:#5b4551;margin:6px 0;display:block}
    select,textarea,input{width:100%;padding:12px;border:1px solid #ead6e4;border-radius:12px;background:#fff}
    button{background:#1f1a1d;color:#fff;border:0;border-radius:12px;padding:12px 16px;font-weight:600}
    button.secondary{background:#fff;color:#1f1a1d;border:1px solid #ead6e4}
    .actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}
    .out{white-space:pre-wrap;background:#fff;border:1px dashed #ead6e4;border-radius:12px;padding:12px;margin-top:10px}
    .imgbox{display:flex;justify-content:center;align-items:center;background:#faf7fb;border:1px solid #f0e3ee;border-radius:16px;min-height:320px;margin-top:12px;overflow:hidden}
    .imgbox img{max-width:100%;height:auto;display:block}
    .tag{display:inline-block;font-size:12px;padding:4px 8px;border:1px solid #ead6e4;border-radius:999px;background:#fff;margin-right:6px;margin-top:6px}
  </style>
</head>
<body>
<header class="wrap">
  <h1>🎮 Kélani — Plug & Play</h1>
  <div>
    <span class="tag">Outfits</span><span class="tag">Hair</span><span class="tag">Scenes</span><span class="tag">Makeup</span><span class="tag">Poses</span>
  </div>
</header>

<main class="wrap">
  <section class="panel">
    <div class="row">
      <div><label>Outfit</label><select id="outfit"></select></div>
      <div><label>Hair</label><select id="hair"></select></div>
      <div><label>Scene</label><select id="scene"></select></div>
      <div><label>Pose</label><select id="pose"></select></div>
      <div><label>Makeup</label><select id="makeup"></select></div>
      <div><label>Vibe</label>
        <select id="vibe">
          <option>Quiet confidence</option>
          <option>Soft dominance</option>
          <option>Emotional elegance</option>
          <option>Siren after dark</option>
          <option>Boardroom luxe calm</option>
        </select>
      </div>
    </div>

    <div class="actions">
      <button id="build">Build Prompt</button>
      <button id="render">Generate Image</button>
      <button class="secondary" id="copy">Copy Prompt</button>
      <button class="secondary" id="download">Download Image</button>
    </div>

    <div class="out" id="promptOut" placeholder="Prompt will appear here…"></div>
    <div class="imgbox"><img id="preview" alt="preview"/></div>
  </section>
</main>

<script>
// 1) set this to your deployed API (Fly/Render). Never put your key in the front-end.
const API_BASE = "https://YOUR-API-HOST"; // e.g. https://kelani-api.fly.dev

// 2) data options
const outfits = [
  "Street Siren — fitted white bodysuit, high-waisted stone-wash ripped jeans, silver hoops, minimalist jewelry",
  "Boardroom Luxe — satin champagne blouse, wide-leg ivory trousers, gold watch, sleek heels, low bun",
  "Soft Lounge — blush silk robe, plush slippers, dainty necklace, rosé glass in hand",
  "After-Dark Glam — espresso velvet mini dress, glossy lips, gold heels, perfume mist moment",
  "Soft Street Luxe — oversized cropped trench, tailored trousers, bralette, silver stilettos"
];
const hair = [
  "Waist-length braids cascading over one shoulder",
  "Low sleek bun with a clean middle part",
  "Voluminous curls grazing her collarbone",
  "Soft straight blowout with a side part",
  "Half-up, half-down twist, romantic siren feel"
];
const scenes = [
  "Luxury downtown street — blush marble buildings, rose-gold storefront reflections, soft midday light",
  "Penthouse vanity glow — golden-hour window light, velvet vanity chair, flickering candle",
  "Cafe corner chic — champagne-toned upscale cafe, espresso marble table, glass teacup",
  "Rooftop after dark — city lights bokeh, silk slip catching the wind",
  "Solo brunch lounge — blush day lounge, glass of rosé, city view backdrop"
];
const poses = [
  "The Side Glance Glow — leaning on marble, calm smirk",
  "The Drape & Drift — walking slowly, coat draped (ignore coat if outfit has none)",
  "The Earring Adjustment — hand grazing gold hoop, serene allure",
  "The Seated Command — poised cross-leg, chin slightly lifted",
  "The Shoulder Look-Back — candlelit mystery"
];
const makeups = [
  "Soft glam with blush glow, radiant skin",
  "Fresh matte skin, defined liner, nude gloss",
  "Dewy skin, subtle shimmer on lids, glossy lip",
  "Evening glam: smoked espresso liner, luminous highlight",
  "No-makeup makeup, hydrated skin, soft mascara"
];

const $ = s => document.querySelector(s);
function fill(id, arr){ const el=$(id); el.innerHTML=arr.map(x=>`<option>${x}</option>`).join(""); }
fill("#outfit", outfits); fill("#hair", hair); fill("#scene", scenes); fill("#pose", poses); fill("#makeup", makeups);

function buildPrompt(){
  const prompt = `
Ultra-realistic cinematic portrait of **Kélani Vaughn** — timeless, feminine Black woman exuding soft power and quiet confidence. 
Consistent facial structure and proportions; elegant poise; refined details.

Outfit: ${$("#outfit").value}.
Hair: ${$("#hair").value}. Makeup: ${$("#makeup").value}.
Scene: ${$("#scene").value}. Pose: ${$("#pose").value}. Vibe: ${$("#vibe").value}.

Brand aesthetic: blush, rose-gold, champagne, espresso; textures of silk/velvet/shimmer dust; gentle golden-hour glow; subtle sparkle clusters; soft vignette; clean background space for captions.

Camera & quality: full-frame, shallow depth of field, soft specular highlights, realistic skin texture, filmic lighting, 4k detail, natural color, no artifacts, no duplicate faces, hands anatomically correct.

Notes: Do NOT add a sweater unless specified. Keep jewelry minimalist and tasteful. Preserve character consistency across renders.
  `.trim();
  $("#promptOut").textContent = prompt;
  return prompt;
}

$("#build").onclick = () => buildPrompt();

$("#copy").onclick = async () => {
  const p = $("#promptOut").textContent.trim();
  if(!p) return;
  await navigator.clipboard.writeText(p);
  alert("Prompt copied ✨");
};

$("#render").onclick = async () => {
  const prompt = buildPrompt();
  const btn = $("#render");
  btn.disabled = true; btn.textContent = "Rendering…";
  try{
    const res = await fetch(API_BASE + "/image", {
      method:"POST",
      headers:{"Content-Type":"application/json"},
      body: JSON.stringify({ prompt, size: "1024x1024" })
    });
    const json = await res.json();
    $("#preview").src = "data:image/png;base64," + json.b64_png;
  }catch(e){
    alert("Image error. Check API_BASE and server CORS.");
  }finally{
    btn.disabled=false; btn.textContent="Generate Image";
  }
};

$("#download").onclick = () => {
  const src = $("#preview").src || "";
  if(!src.startsWith("data:image")) { alert("No image yet"); return; }
  const a = document.createElement("a"); a.href = src; a.download = "kelani.png"; a.click();
};

// register service worker (offline/installable)
if('serviceWorker' in navigator){ navigator.serviceWorker.register('./sw.js'); }
</script>
</body>
</html>
