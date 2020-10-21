# CICD Kurs med Norkart

Målet med dette kurset er å vise hvordan du kan raskt få en kart-webapplikasjon til å kjøre live. Ved å få appen raskt ut i produksjon og ved å videreutvikle den med 'Continuous Integration' og 'Continuous Delivery' (CICD) prinsippet, kan nye features og bug-fixes effektivt og kontinuerlig integreres i appen din. Her får du en enkel mal på en React applikasjon med et mapbox kart. Denne skal du deploye til github pages slik at dere lett kan vise andre det kule dere lager.

Når dere er ferdige her skal dere ha deres egen lenke til en live react kart-nettside slik som dette: https://norkart.github.io/webkurs2020-CICD-React/.

---

## STEG 0: Forutsetninger

Før dere starter må dere ha noe programvare installert:

1. **Git**. Følg instruksjonene som gjelder for ditt OS her: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git. Sjekk at git er instalert:

```
   git --version
```

2. **Github**. Lag deg en bruker her https://github.com/

3. **Node.js med npm**. Det annbefales å bruke node version manager for å installere node siden dette lar deg enkelt bytte mellom node versjoner. For **Windows**: https://docs.microsoft.com/en-us/windows/nodejs/setup-on-windows. For **Mac/Linux**:
   https://www.stanleyulili.com/node/how-to-install-node-and-npm-on-mac-or-linux/. Sjekk at node og npm er installert:

```
   node --version
```

```
    npm --version
```

4. **En code editor (vs code annbefales)**. https://code.visualstudio.com/download

---

## STEG 1: Klon og kjør Prosjektet

1. I terminalen. Finn fram til fillokasjonen hvor dere vil lagre prosjektet og klon dette repoet:

```
   git clone https://github.com/Norkart/webkurs2020-CICD-React.git
```

2. Installer npm pakkene til prosjektet. De relevante pakkene kan sees i `package.json` filen i prosjektet. Vi bruker for eksempel `mapbox-gl` biblioteket til å vise kart på nettsiden. Dette vil i tillegg installere `gh-pages` som brukes til å deploye nettsiden.

```
   npm install
```

3. Kjør opp prosjektet lokalt:

```
   npm start
```

Dette bør åpne browseren din på http://localhost:3000/react-bedpress.

HURRA! Du kan nå kalle deg for en React-utvikler!

---

## STEG 2: Push koden til ditt eget github repo

Du har nå en enkel mal som du kan bygge videre på. For å begynne å jobbe videre med prosjektet og for at du skal kunne deploye til din egen github-pages, trenger du å flytte koden over på ditt eget github repository.

1. Lag deg et nytt repository på https://github.com/. Gjerne kall repositoriet **webkurs2020-CICD-React**. Velg public (hvis du har github pro (gratis for studenter) kan du velge å gjøre repoet private. Hvis ikke må det være public for at github-pages skal virke). Ikke initialiser med README, .gitignore eller licence.
   ![new repo](public/Images/github_new_repo.png)

2. I terminalen. Sørg for at du er inni prosjektet som du klonet og kjørte i forrige steg. `ctrl c` for å stoppe appen hvis den fortsatt kjører.

3. Endre git 'origin' til dit nye repository:

```
   git remote set-url origin http://github.com/{{YOUR_GITHUB_USERNAME}}/{{YOUR_REPO_NAME}}
```

Sjekk at du har byttet origin ved å skrive

```
   git remote -v
```

Da skal du se pathen til repoet ditt.

4. Dytt koden opp til ditt repository:

```
   git add .
   git commit -m'initial commit'
```

(Github har endret navnet på det som tidligere het 'master' til 'main')

```
   git push origin main
```

Koden din skal nå være 'pushet' til ditt nye repo.

---

## STEG 3: Få nettsiden til å kjøre på github-pages.

1. Åpne prosjektet i vs code. (skriv `code .` i terminalen )
2. Endre homepage i `package.json` til din egen url: `"homepage": "http://{{YOUR_GITHUB_USERNAME}}.github.io/{{YOUR_GITHUB_PROJECT}}",`
   ![package json](public/Images/packagejsonhome.png)

3. Deploy appen til github pages:

```
   npm run deploy
```

Denne kommandoen vil lage en branch i repoet ditt som heter gh-pages. Du kan kjøre denne kommandoen siden kildekoden har definert den i scripts i package.json. I tillegg, har du allerede installert pakken `gh-pages` når du kjørte kommandoen `npm install`.

![package json](public/Images/packagejsonscript.png)

4. Aktiver github pages i github repoet ditt. Gå til settings og skroll ned til GitHub pages section. Velg source `gh-pages`

![activate github pages](public/Images/activate-gh-pages.png)

5. Sjekk om nettsiden din kjører på: `http://github.com/{{YOUR_GITHUB_USERNAME}}/{{YOUR_REPO_NAME}}`

Hurra! nettsiden din er live :D

## STEG 4: Utvikle en super cool react-app med continuous deployment!

1. Gjør endringer i koden (start for eksempel med å oppdatere kartets zoom nivå, start koordinater eller bakgrunnskart).
2. Push oppdateringene dine til git og deploy endringene

```
git add .
git commit -m 'Your commit message'
git push origin main
npm run deploy
```

Endringene du gjør vil automatisk oppdateres på nettsiden din! Happy coding :D
Videre gir vi deg 2 mulige utfordringer men du står fritt til å gjøre noe helt annet hvis du har en kul ide til appen. Mulige utfordringer:

1. OPTION 1: Set opp automatisk deploy av appen trigget når main branchen oppdaterses.
2. OPTION 2: Lag en meny komponent for å bytte bakgrunnskart.

## OPTION 1: Automatisk deploy av app

For å oppdattere nettsiden må vi manuelt kjøre **npm run deploy** etter å ha endret koden. Hadde det ikke vært greit å automatisert dette slik at nettsiden oppdatteres hver gang main-branchen oppdateres? Dette kan vi gjøre ved hjelp av Github Actions:

1. Generer access token for å deploye nettsiden gjennom Github Actions

For å gi Github Actions tilgang til å lese og deploye repoet vårt, trenger vi ett access token. Gå til https://github.com/settings/tokens og trykk 'Generate new token'.
<br>
<br>

![generate github token](public/Images/github-deploy-token.png)

<br>
<br>

Gi tokenet et navn, f.eks 'deploy-access', og huk av på 'repo'. Klikk så på 'Generate token' og kopier verdien.

<img src="public/Images/github-example-token.png" alt="secret" width="600"/>

<br>
<br>
<br>

2. Lag en secret som kan brukes av Github Actions
   <br >
   <br >
   <br >
   <img src="public/Images/github-create-secret.png" alt="secret" width="600"/>
   <br>
   <br>
   <br>

For å la Github Actions hente tokenet vi nettop lagde, trenger vi en sectet. Trykk på 'New Secret' og gi den et navn, for eksempel 'ACTIONS_DEPLOY_ACCESS_TOKEN', og kopier inn verdien fra forrige steg.

3. Sett opp github actions:
   <br >
   <br >
   <img src="public/Images/create-github-actions.png" alt="create github action" width="400"/>

<br>
<br>

Ved å følge pilene på bildet ovenfor, genererer vi en enkel løype som bygger og tester applikasjonen hver gang vi oppdatterer main-branchen. Vi vil i tillegg deploye applikasjonen, og legger derfor til enda ett steg helt nederst:

<br>

<pre>
    - name: Deploy
      run: |
        git config --global user.name $user_name
        git config --global user.email $user_email
        git remote set-url origin https://${github_token}@github.com/${repository}
        npm run deploy
      env:
        user_name: 'Ettellerannet'
        user_email: 'Ettellerannet@epost.no'
        github_token: ${{ secrets.ACTIONS_DEPLOY_ACCESS_TOKEN }}
        repository: ${{ github.repository }}
</pre>

Navn- og epost-informasjon for Github trenger ikke være deres eget, så derfor fyller vi bare inn etellerannet her. 'ACTIONS_DEPLOY_ACCESS_TOKEN' er secreten vi lagde i forrige steg. Dersom dere valgte eget navn må dere huske å bytte navnet her.

I tillegg til å legge til dette steget, må vi gjøre noen endringer:

1. Fjern **10.x** og **12.x** fra node-version listen (vi vil bare bygge og deploye én gang)

2. Fjern **run: npm test** steget, da dette steget ikke vil passere ettersom vi ikke har noen tester.

Filen blir altså seende slik ut:

<pre>
name: Node.js CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12.x]

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: npm run build --if-present
    - name: Deploy
      run: |
        git remote set-url origin https://${github_token}@github.com/${repository}
        npm run deploy
      env:
        github_token: ${{ secrets.ACTIONS_DEPLOY_ACCESS_TOKEN }}
        repository: ${{ github.repository }}
</pre>

Klikk på commit, og gjerne gjør noen enkle endringer i koden for å se at nettsiden endrer seg når ny kode dyttes til main-branchen! :D


## OPTION 2: Bytt bakgrunnskart
I dette steget skal du lage en enkel meny for å bytte bakgrunnskartet. Mapbox dokumentasjonen forklarer hvordan det kan gjøres med html og javascript: https://docs.mapbox.com/mapbox-gl-js/example/setstyle/. Med noen få tweeks, kan du få til det samme i din React applikasjon. Her får du noen tips til hvordan å løse oppgaven. Spør gjerne om hjelp hvis du sitter fast.
![activate github pages](public/Images/SwapBackground.png)

Det kan være enklest å begynne med å lage menyen i MapboxGLMap komponenten. Når den virker slik som du ønsker kan den flyttes til en egen komponent for ryddighetsskyld. Et løsningforslag på denne oppgaven kan sees i **changeBackgroundLayer** branchen. 

De forskjellige bakgrunnene som finnes har følgende ider: "streets-v11", "light-v10", "dark-v10" og "satellite-v9". Bakgrunnskartet settes ved å sette style til `mapbox://styles/mapbox/${backgroundLayerID}`. Det kan være lurt å lagre den foreløpig valgte mapstylen som en state i komponenten (https://reactjs.org/docs/hooks-state.html).

Bakgrunnskartet endres ved et kall til `map.setStyle('mapbox://styles/mapbox/${backgroundLayerID})`. For å få til dette, kan du bruke en `useEffect` hook (https://reactjs.org/docs/hooks-effect.html).

Menyen for valg av bakgrunnskart kan med fordel være en egen komponent. Prøv å skil ut koden for menyen til en egen komponent ved å sende relevant data eller funksjoner som props fra MapboxGLMap komponenten. Løsningsforslag for dette finner du i branchen **changeBackgroundLayerUsingProps**  på github.


### NB

- Mapbox tokenet til dette prosjektet vil utløpe etter et par uker. Du kan enkelt lage din egen token på https://account.mapbox.com/access-tokens. Det er gratis helt frem til du får veldig stor traffikk mot appen din. Bytt ut tokenen din i,'.env' filen for å ta i bruk din egen token. Det annbefales ikke å legge tokenet i kode på et public github repo (bruk private eller la være å pushe .env fila til git). I Mapbox Studio kan du også lage custom kart (f.eks dark mode med rosa vann) som du kan vise på nettsiden.
