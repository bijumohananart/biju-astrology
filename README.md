# BIJU ASTROLOGY ബാക്കെൻഡ്
Swiss Ephemeris (npm `swisseph`) + Express. **ഇതുവരെ ഇൻസ്റ്റാൾ ചെയ്ത് പ്രവർത്തിപ്പിച്ചിട്ടില്ല.**
## ലൈസൻസ്
Swiss Ephemeris: AGPL-3.0 അല്ലെങ്കിൽ Astrodienst-ന്റെ വാണിജ്യ (Professional) ലൈസൻസ്. സേവനം പൊതുജനത്തിന് നൽകുന്നുവെങ്കിൽ AGPL പ്രകാരം ബാക്കെൻഡിന്റെ സോഴ്സ് ലഭ്യമാക്കണം; അല്ലെങ്കിൽ വാണിജ്യ ലൈസൻസ് വാങ്ങുക. തീരുമാനം താങ്കളുടേതാണ്.
## സജ്ജീകരണം
1. Node 20+ ഉള്ള സെർവർ (Render, Railway, Fly.io, VPS).
2. `npm install && npm test`
3. `ephe/` ഫോൾഡറിൽ Swiss Ephemeris ഫയലുകൾ (sepl_18.se1, semo_18.se1, seas_18.se1) ഇടുക; ഇല്ലെങ്കിൽ പ്രതികരണത്തിൽ `ephemeris: "moshier-fallback"` എന്നു കാണിക്കും.
4. `ALLOWED_ORIGINS=<താങ്കളുടെ സൈറ്റ് URL> EPHE_PATH=./ephe npm start`
5. HTTPS ഉറപ്പാക്കുക; ജനനവിവരങ്ങൾ ലോഗ് ചെയ്യരുത്.
ഡിപ്ലോയ് ഗൈഡ്: DEPLOYMENT.md കാണുക.
`POST /api/chart` {y,mo,d,h,mi,utcOffsetHours,lat,lon} → ഗ്രഹങ്ങൾ (നിരയന), ലഗ്നം, അയനാംശം, ക്രമീകരണങ്ങൾ.
## ഡിപ്ലോയ് (Docker / Render)
`Dockerfile` native `swisseph` കമ്പൈൽ ചെയ്യുകയും എഫെമെറിസ് ഫയലുകൾ ഡൗൺലോഡ് ചെയ്യുകയും ചെയ്യും. Render-ൽ ഈ ഫോൾഡർ Git റിപ്പോയായി കണക്ട് ചെയ്ത് `ALLOWED_ORIGINS` സെറ്റ് ചെയ്യുക. `/health` 200 നൽകിയാൽ മാത്രം Swiss Ephemeris സജ്ജമാണ്. ഫയലുകൾ ഇല്ലെങ്കിൽ `/api/chart` 503 നൽകും (നിശബ്ദ ഏകദേശ ഗണിതം ഇല്ല) — വികസനത്തിന് മാത്രം `ALLOW_MOSHIER=1`.
