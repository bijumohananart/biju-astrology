# ഡിപ്ലോയ്മെന്റ് ഗൈഡ് (തുടക്കക്കാർക്ക്)
**അവസ്ഥ:** ഇവിടെ നെറ്റ്‌വർക്ക് ഇല്ലായിരുന്നതിനാൽ Swiss Ephemeris ഇൻസ്റ്റാൾ/ടെസ്റ്റ് ചെയ്തിട്ടില്ല. താഴെ പറയുന്ന ഘട്ടങ്ങൾ താങ്കളുടെ കമ്പ്യൂട്ടറിൽ ഓടിച്ച് പരിശോധിക്കണം. പാസ്‌വേഡോ API രഹസ്യങ്ങളോ ചാറ്റിൽ പങ്കിടരുത്.

## 0. ലൈസൻസ് (ആദ്യം തീരുമാനിക്കുക)
Swiss Ephemeris: AGPL-3.0 അല്ലെങ്കിൽ Astrodienst-ന്റെ വാണിജ്യ ലൈസൻസ്. ഔദ്യോഗിക വിവരം: https://www.astro.com/swisseph/ (ഇവിടെ നിന്ന് സ്വയം സ്ഥിരീകരിക്കുക). `swisseph` npm പാക്കേജിന്റെ പേരും ലൈസൻസും പതിപ്പും npmjs.com-ൽ പരിശോധിക്കുക; `package.json`-ൽ ഇപ്പോൾ `"*"` ആണ്, ഇൻസ്റ്റാളിനു ശേഷം പതിപ്പ് പിൻ ചെയ്യുക.

## 1. കമ്പ്യൂട്ടറിൽ ഓടിക്കാൻ
1. Node.js **20 അല്ലെങ്കിൽ പുതിയത്** (`node -v`), C++ കമ്പൈലർ + Python 3 (native പാക്കേജ് ബിൽഡിന്).
2. `cd biju-backend && npm install`
3. എഫെമെറിസ് ഫയലുകൾ: https://www.astro.com/swisseph/swephinfo_e.htm എന്ന ഔദ്യോഗിക പേജിലെ ഡൗൺലോഡ് ലിങ്കിൽ നിന്ന് `sepl_18.se1`, `semo_18.se1`, `seas_18.se1` എടുത്ത് `biju-backend/ephe/` ഫോൾഡറിൽ ഇടുക (ഫയൽ പേരും URL-ഉം അവിടെ സ്ഥിരീകരിക്കുക; Dockerfile-ലെ URL ഞാൻ പരിശോധിച്ചിട്ടില്ല). ഫയലുകൾക്കും AGPL/വാണിജ്യ ലൈസൻസ് ബാധകം.
4. `npm run test:pure` (ലൈബ്രറി ഇല്ലാതെ ഓടും), തുടർന്ന് `npm test`. എഫെമെറിസ് ഇല്ലെങ്കിൽ `swisseph.test.js` "skipped" എന്ന് കാണിക്കും — വിജയമായി കണക്കാക്കരുത്.
5. `ALLOWED_ORIGINS=http://localhost:5500 npm start`, തുടർന്ന് `http://localhost:8080/health` → `{"ok":true,...}` വേണം.
6. ശരിയായ ചാർട്ട്: `curl -X POST localhost:8080/api/chart -H "Content-Type: application/json" -d '{"y":1990,"mo":5,"d":5,"h":6,"mi":30,"utcOffsetHours":5.5,"lat":8.52,"lon":76.94}'` — `"ephemeris":"swisseph-files"` ഉണ്ടെന്ന് ഉറപ്പാക്കുക.

## 2. റഫറൻസ് പരിശോധന
Jagannatha Hora അല്ലെങ്കിൽ Swiss Ephemeris-ന്റെ ഔദ്യോഗിക ഔട്ട്പുട്ട് (ലാഹിരി, മീൻ നോഡ്, Whole Sign) ഉപയോഗിച്ച് `test/reference.json` പൂരിപ്പിക്കുക: `[{"name":"..","input":{y,mo,d,h,mi,utcOffsetHours,lat,lon},"planets":[9 സിദ്ധാന്ത രേഖാംശങ്ങൾ: സൂര്യൻ,ചന്ദ്രൻ,ചൊവ്വ,ബുധൻ,വ്യാഴം,ശുക്രൻ,ശനി,രാഹു,കേതു],"ascendant":0,"tolerance":0.01}]`. `npm test` ഓടിച്ച് `RECORD` വരികളിലെ വ്യത്യാസങ്ങൾ സൂക്ഷിക്കുക.

## 3. ഓൺലൈൻ ഡിപ്ലോയ് (Render ഉദാഹരണം)
1. GitHub-ൽ പുതിയ (private) റിപ്പോ ഉണ്ടാക്കി `biju-backend/` ഉള്ളടക്കം പുഷ് ചെയ്യുക (`.gitignore` ഉള്ളതിനാൽ ephe, .env പോകില്ല).
2. render.com-ൽ New → Web Service → റിപ്പോ തിരഞ്ഞെടുക്കുക → Runtime: **Docker**.
3. Environment: `ALLOWED_ORIGINS` = താങ്കളുടെ വെബ്സൈറ്റിന്റെ URL (ഉദാ. `https://yoursite.com`). `PORT` Render സ്വയം നൽകും.
4. Deploy. ബിൽഡ് ലോഗിൽ ephemeris ഡൗൺലോഡ് പിശകില്ലെന്ന് നോക്കുക.
5. `https://<നിങ്ങളുടെ-സർവീസ്>.onrender.com/health` തുറക്കുക → `ok:true` വേണം. തുടർന്ന് മുകളിലെ curl (localhost മാറ്റി) ഓടിക്കുക.

## 4. വെബ്സൈറ്റുമായി ബന്ധിപ്പിക്കൽ
`biju-astrology.html`-ൽ `<body>`-ന് തൊട്ടുതാഴെ `window.BIJU_API="";` എന്ന വരി കണ്ടെത്തി URL നൽകുക: `window.BIJU_API="https://<സർവീസ്>.onrender.com";`
**പരിമിതി:** ഇപ്പോഴത്തെ claude.ai ഹോസ്റ്റ് ചെയ്ത പേജ് പുറത്തേക്കുള്ള അഭ്യർത്ഥനകൾ തടയുന്നു, അതിനാൽ അവിടെ ബന്ധം പ്രവർത്തിക്കില്ല. ഫയൽ സ്വന്തം ഹോസ്റ്റിംഗിൽ (Netlify, Cloudflare Pages, GitHub Pages) വച്ച് `ALLOWED_ORIGINS` അതിലേക്ക് സജ്ജമാക്കണം. ബന്ധം പരാജയപ്പെട്ടാൽ പേജ് ഏകദേശ ഗണിതത്തിൽ തിരിച്ചുപോകുകയും അത് വ്യക്തമായി കാണിക്കുകയും ചെയ്യും.
