# Demander un devis

> Demandez un devis pour embaucher un salarié en Italie. Trois étapes courtes, réponse en un jour ouvré.


<section> <span class="eyebrow">Demander un devis</span> <h1 id="dites-nous-qui-vous-voulez-recruter">Dites-nous qui vous voulez recruter</h1> <p class="lede">Trois étapes courtes. Nous répondons en un jour ouvré avec le coût complet et les grandes lignes du contrat.</p> </section> <section> <form id="lead" class="panel" style="display:grid;gap:18px;max-width:640px" novalidate> <p id="stepinfo" class="tag" style="justify-self:start">Étape 1 sur 3</p> <fieldset data-step="1" style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="email">E-mail professionnel<input id="email" type="email" autocomplete="email" required></label> <label for="company">Entreprise<input id="company" autocomplete="organization"></label> </fieldset> <fieldset data-step="2" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="service">De quoi avez-vous besoin ?<select id="service"><option>Employer of Record</option><option>Paie (nous avons une société italienne)</option><option>Visa de travail / carte bleue</option><option>Recrutement</option></select></label> <label for="count">Combien de personnes en Italie ?<input id="count" type="number" min="1" value="1"></label> <label for="city">Ville ou région<input id="city" placeholder="Milan, Turin, télétravail…"></label> <label for="salary">Salaire brut annuel prévu (EUR)<input id="salary" type="number" step="1000"></label> </fieldset> <fieldset data-step="3" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="name">Votre nom<input id="name" autocomplete="name"></label> <label for="start">Date de démarrage prévue<input id="start" type="date"></label> <label for="notes">Autre chose ?<textarea id="notes" rows="4"></textarea></label> <label style="display:flex;gap:10px;align-items:flex-start;font-weight:400"><input id="consent" type="checkbox" style="width:auto;margin-top:5px">J'accepte qu'EmbaucherEnItalie traite mes données pour répondre à cette demande (politique de confidentialité).</label> </fieldset> <p id="msg" role="status" style="margin:0;color:var(--accent)"></p> <div class="cta"><button type="button" class="btn ghost" id="back" hidden>Retour</button><button type="submit" class="btn" id="next">Continuer</button></div> </form> </section> <script>
// Il passo 1 salva subito l'email (lead abbandonato recuperabile dal nurturing). Endpoint del worker: /api/lead.
const f = document.getElementById('lead'); let step = 1;
// Oggetto dalla pagina partner: ?subject=Partner precompila le note.
{ const sj = new URLSearchParams(location.search).get('subject'); if (sj) document.getElementById('notes').value = '[' + sj.slice(0, 40) + '] '; }
const sets = [...f.querySelectorAll('fieldset')];
const data = () => Object.fromEntries([...f.querySelectorAll('input,select,textarea')].map(e => [e.id, e.type === 'checkbox' ? e.checked : e.value]));
async function save(final){ try { await fetch('/api/lead', {method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({...data(), step, final, brand:'embaucherenitalie'})}); } catch(e){} }
function show(){ sets.forEach(s => s.hidden = +s.dataset.step !== step);
  document.getElementById('stepinfo').textContent = `Étape ${step} sur 3`;
  document.getElementById('back').hidden = step === 1;
  document.getElementById('next').textContent = step === 3 ? 'Envoyer la demande' : 'Continuer'; }
// Ripresa dal link nelle email: ?resume=<token> precompila i campi e salta al passo successivo.
(async () => { const tok = new URLSearchParams(location.search).get('resume'); if (!tok) return;
  try { const r = await fetch('/api/lead/' + encodeURIComponent(tok)); if (!r.ok) return; const d = await r.json();
    for (const [k, v] of Object.entries(d)) { const el = document.getElementById(k); if (el && v != null) el.value = v; }
    step = d.step || 2; show(); } catch(e){} })();
document.getElementById('back').onclick = () => { step--; show(); };
f.addEventListener('submit', e => { e.preventDefault(); const m = document.getElementById('msg'); m.textContent = '';
  if (step === 1 && !document.getElementById('email').checkValidity()) { m.textContent = 'Saisissez un e-mail professionnel valide pour continuer.'; return; }
  if (step === 3 && !document.getElementById('consent').checked) { m.textContent = 'Cochez la case de consentement pour que nous puissions répondre.'; return; }
  save(step === 3);
  if (step < 3) { step++; show(); } else { f.querySelectorAll('fieldset,.cta').forEach(x => x.hidden = true); m.textContent = 'Merci. Nous vous répondons en un jour ouvré.'; } });
</script>


---
Source: https://embaucherenitalie.com/contact.html
