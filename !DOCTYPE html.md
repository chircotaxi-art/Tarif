<!DOCTYPE html>  
<html lang="fr">  
<head>  
<meta charset="utf-8">  
<title>Calcul Tarif</title>  
<meta name="viewport" content="width=device-width,initial-scale=1">  
<style>  
  body { font-family: Arial, sans-serif; margin: 20px; max-width: 560px; }  
  label { display:block; margin-top:12px; }  
  input[type="number"], select { padding:6px; width:100%; box-sizing:border-box; }  
  input[type="checkbox"] { margin-left:8px; transform:scale(1.05); }  
  #total { font-weight:700; margin-top:18px; font-size:1.25em; }  
  #message { color:#b00020; margin-top:10px; font-weight:700; }  
  #details { margin-top:14px; white-space:pre-line; background:#f8f8f8; padding:10px; border-radius:6px; color:#222; }  
  button { margin-top:14px; padding:8px 12px; border-radius:6px; cursor:pointer; }  
  .muted { color:#555; font-size:0.95em; }  
</style>  
</head>  
<body>  
  
<h2>Calcul du tarif</h2>  
  
<form id="tarifForm" onsubmit="return false;">  
  <label>Tarif kilométrique :  
    <select id="tarifKm">  
      <option value="1.20" selected>1.20 €/km</option>  
      <option value="1.22">1.22 €/km</option>  
      <option value="1.07">1.07 €/km</option>  
    </select>  
  </label>  
  
  <label>Kilomètres :  
    <input type="number" id="km" min="0" step="0.1" value="0" />  
  </label>  
  
  <label><input type="checkbox" id="hdj" /> HDJ / Chimio</label>  
  <label><input type="checkbox" id="nuit" /> Tarifs de nuit</label>  
  <label><input type="checkbox" id="metropole" /> Métropole</label>  
  
  <div id="message" role="alert" aria-live="polite"></div>  
  <div id="total">Total : 0.00 €</div>  
  <div id="details" class="muted"></div>  
  
  <button type="button" id="resetBtn">Réinitialiser</button>  
</form>  
  
<script>  
const kmInput = document.getElementById('km');  
const tarifKmSelect = document.getElementById('tarifKm');  
const hdjCheckbox = document.getElementById('hdj');  
const nuitCheckbox = document.getElementById('nuit');  
const metropoleCheckbox = document.getElementById('metropole');  
const messageDiv = document.getElementById('message');  
const totalDiv = document.getElementById('total');  
const detailsDiv = document.getElementById('details');  
const resetBtn = document.getElementById('resetBtn');  
  
function fmt(n){ return Number(n).toFixed(2); }  
  
function calculerTarif(){  
  messageDiv.textContent = "";  
  detailsDiv.textContent = "";  
  
  let km = parseFloat(kmInput.value);  
  if (isNaN(km)) km = 0;  
  let tarifKm = parseFloat(tarifKmSelect.value);  
  if (isNaN(tarifKm) || tarifKm <= 0) tarifKm = 1.2;  
  
  // Validation : on élève l'erreur si km < 4  
  if (km < 4) {  
    messageDiv.textContent = "Erreur : le kilométrage doit être ≥ 4 km pour effectuer le calcul.";  
    totalDiv.textContent = "Total : 0.00 €";  
    detailsDiv.textContent = "Aucune opération effectuée. Renseigne au moins 4 km.";  
    return;  
  }  
  
  const kmEffectif = km - 4;  
  let multiplicateurHDJ = 1;  
  if (hdjCheckbox.checked) multiplicateurHDJ = km < 50 ? 1.25 : 1.5;  
  
  // Calculs étape par étape  
  const tarifApplique = tarifKm * multiplicateurHDJ;  
  const montantKm = kmEffectif * tarifApplique;  
  const apresBase = montantKm + 13;  
  let total = apresBase;  
  
  let details = "";  
  details += `Kilométrage saisi : ${fmt(km)} km\n`;  
  details += `Kilométrage facturable (km - 4) : ${fmt(kmEffectif)} km\n`;  
  details += `Tarif kilométrique choisi : ${fmt(tarifKm)} €/km\n`;  
  if (hdjCheckbox.checked) {  
    details += `HDJ activé → multiplicateur HDJ : ${multiplicateurHDJ}\n`;  
  } else {  
    details += `HDJ non activé → multiplicateur HDJ : 1\n`;  
  }  
  details += `Tarif appliqué par km (tarifKm × multiplicateurHDJ) : ${fmt(tarifApplique)} €/km\n`;  
  details += `Montant km = ${fmt(kmEffectif)} × ${fmt(tarifApplique)} = ${fmt(montantKm)} €\n`;  
  details += `Ajout fixe = + 13 € → sous-total = ${fmt(apresBase)} €\n`;  
  
  if (metropoleCheckbox.checked) {  
    total += 15;  
    details += `Ajout Métropole = +15 € → sous-total = ${fmt(total)} €\n`;  
  } else {  
    details += `Pas d'ajout Métropole\n`;  
  }  
  
  if (nuitCheckbox.checked) {  
    total *= 1.5;  
    details += `Tarif de nuit appliqué : × 1.5 → total final = ${fmt(total)} €\n`;  
  } else {  
    details += `Pas de tarif de nuit → total final = ${fmt(total)} €\n`;  
  }  
  
  totalDiv.textContent = "Total : " + fmt(total) + " €";  
  detailsDiv.textContent = details;  
}  
  
[kmInput, tarifKmSelect, hdjCheckbox, nuitCheckbox, metropoleCheckbox].forEach(el=>{  
  el.addEventListener('input', calculerTarif);  
  el.addEventListener('change', calculerTarif);  
});  
  
resetBtn.addEventListener('click', () => {  
  kmInput.value = 0;  
  tarifKmSelect.value = "1.20";  
  hdjCheckbox.checked = false;  
  nuitCheckbox.checked = false;  
  metropoleCheckbox.checked = false;  
  messageDiv.textContent = "";  
  totalDiv.textContent = "Total : 0.00 €";  
  detailsDiv.textContent = "";  
});  
  
calculerTarif();  
</script>  
  
</body>  
</html>  
