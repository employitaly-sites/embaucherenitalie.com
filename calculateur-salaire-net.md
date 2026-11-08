# Calculateur de salaire net en Italie

> Du brut au net en Italie en 2026 : cotisations salariales, impôt sur le revenu (IRPEF) avec crédits d'impôt, surtaxes régionales et communales, pour Milan, Rome, Turin, Bologne et Naples.


<section> <span class="eyebrow">Calculateur de net</span> <h1 id="ce-que-votre-salarie-touche-en-italie">Ce que votre salarié touche en Italie</h1> <p class="lede">Saisissez le brut annuel et la ville. L'estimation applique les règles 2026 : cotisations salariales, impôt sur le revenu avec crédits d'impôt et surtaxes locales. Utile pour comparer une offre italienne à un net français.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Salaire brut annuel, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">Ville<select id="city"><option value="Milan">Milan</option><option value="Rome">Rome</option><option value="Turin">Turin</option><option value="Bologna">Bologne</option><option value="Naples">Naples</option></select></label> <label for="inst">Versé en<select id="inst"><option value="13">13 mensualités</option><option value="14">14 mensualités</option><option value="12">12 (13e/14e lissées)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Poste</th><th class="num">Par an</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="comment-fonctionne-le-calcul">Comment fonctionne le calcul</h2> <ol> <li>Le salarié paie 9,19 % de cotisations sur son brut, plus 1 % sur la part au-delà de 56 224 € par an.</li> <li>L'impôt sur le revenu (IRPEF) s'applique au reste : 23 % jusqu'à 28 000 €, 33 % de 28 000 à 50 000 €, 43 % au-delà. Pas de quotient familial : l'impôt est individuel.</li> <li>Des crédits d'impôt pour salariés réduisent l'impôt. Ils sont plus élevés sur les bas salaires et s'annulent à 50 000 €. Entre 20 000 et 40 000 €, un crédit supplémentaire allant jusqu'à 1 000 € s'applique.</li> <li>Les surtaxes régionale et communale s'ajoutent, sauf si le revenu est sous le seuil d'exonération de la ville.</li> </ol> <p>Rien de tout cela ne change ce que vous payez comme employeur : voir le <a href="calculateur.html">calculateur du coût employeur</a>.</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Lombardie","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Lazio","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Piémont","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0.8% to 1.2% by band; 0.8% used","exempt":11790},"Bologna":{"region":"Emilia-Romagna","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Campanie","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−' : '') + Math.round(Math.abs(n)).toLocaleString('fr-FR') + ' €';
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['Salaire brut', g], ['Cotisations salariales', -inps], ['Impôt sur le revenu (IRPEF) après crédits', -irpef], [`Surtaxe régionale (${c.region})`, -reg], ['Surtaxe communale', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>Salaire net</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `Environ ${eur(net / n)} net sur chacun des ${n} bulletins mensuels (${(net / g * 100).toFixed(0)} % du brut).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://embaucherenitalie.com/calculateur-salaire-net.html
