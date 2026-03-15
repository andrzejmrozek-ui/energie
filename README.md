# ⚡ Energie — Rozúčtování energií

Webová aplikace (PWA) pro rozúčtování energií mezi spolubydlícími.  
Data se synchronizují přes Firebase — všichni vidí totéž v reálném čase.

---

## 🚀 Jak zprovoznit (10 minut)

### 1. Vytvoř Firebase projekt (zdarma)

1. Jdi na **https://console.firebase.google.com**
2. Klikni **„Add project"** → pojmenuj ho (třeba `energie-app`) → vytvoř
3. V levém menu jdi na **Build → Firestore Database**
4. Klikni **„Create database"** → vyber **europe-west1** (nebo nejbližší) → **Start in test mode** → Create
5. V nastavení projektu (⚙ ikona nahoře) → **General** → scroll dolů → **Your apps** → klikni na **Web** (`</>`)
6. Pojmenuj app (třeba `energie-web`) → **Register app**
7. Zkopíruj `firebaseConfig` objekt — vypadá takto:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "energie-app-xxxxx.firebaseapp.com",
  projectId: "energie-app-xxxxx",
  storageBucket: "energie-app-xxxxx.firebasestorage.app",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

### 2. Vlož Firebase config do aplikace

Otevři soubor **`index.html`** a najdi blok:

```js
const firebaseConfig = {
  apiKey: "TVUJ_API_KEY",
  ...
};
```

Nahraď ho svým configem z kroku 1.

### 3. Zabezpeč Firestore (důležité!)

V Firebase Console → **Firestore Database → Rules** nahraď pravidla:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /houses/{doc} {
      allow read, write: if true;
    }
  }
}
```

> ⚠️ Toto je jednoduché pravidlo bez autentizace — funguje pro domácí použití.
> Pro vyšší bezpečnost můžeš přidat Firebase Authentication později.

### 4. Nahraj na GitHub Pages

```bash
# Vytvoř nový repo na GitHubu (třeba "energie")
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/TVUJ_USERNAME/energie.git
git branch -M main
git push -u origin main
```

Na GitHubu jdi do **Settings → Pages**:
- Source: **Deploy from a branch**
- Branch: **main** / **(root)**
- Save

Za pár minut bude appka na: **https://TVUJ_USERNAME.github.io/energie/**

### 5. Přidej si na telefon

Na telefonu otevři URL v prohlížeči:

- **Android (Chrome)**: Menu ⋮ → „Přidat na plochu" / „Instalovat aplikaci"
- **iPhone (Safari)**: Sdílení □↑ → „Přidat na plochu"

---

## 📱 Jak to funguje

**Při prvním otevření** si každý vybere, kdo je.  
Appka si zapamatuje volbu v prohlížeči (nemusíš se „přihlašovat" pokaždé).

### Majitel (správce)
- **Přehled**: vidí dluhy všech spolubydlících, zálohy, faktury
- **Zálohy**: zaznamenává přijaté zálohy (nebo si je zadávají spolubydlící sami)
- **Faktury**: zadává faktury za energie (elektřina, plyn, voda...)
- **Nastavení**: spravuje lidi, **individuální zálohy na osobu**, od kdy sledovat

### Spolubydlící
- **Přehled**: vidí svůj dluh na zálohách, faktury za energie, skutečné vyúčtování
- **Zaplatit**: sám si zaznamená platbu zálohy
- **Historie**: timeline všech plateb a faktur (jako Settle Up)

### Individuální zálohy
Každý spolubydlící může mít **jinou výši zálohy** (např. jeden platí 3 000 Kč, druhý 2 500 Kč).  
Nastaví se v **Nastavení → klik na ✎ u spolubydlícího**.

### Synchronizace
- Data se ukládají do Firebase — **všichni vidí totéž v reálném čase**
- Zelená tečka vpravo nahoře = online a synchronizováno
- Oranžová = ukládám
- Červená = offline (data se uloží lokálně a synchronizují po obnovení připojení)

---

## 📁 Soubory

```
energie-pwa/
├── index.html       ← hlavní aplikace (vše v jednom souboru)
├── manifest.json    ← PWA manifest
├── sw.js            ← service worker (offline podpora)
├── icon-192.png     ← ikona 192×192
├── icon-512.png     ← ikona 512×512
└── README.md        ← tento soubor
```

---

## ❓ FAQ

**Musím platit za Firebase?**  
Ne. Spark (free) plán stačí na tisíce čtení/zápisů denně — víc než dost pro domácnost.

**Co když nemám internet?**  
Appka funguje offline díky service workeru. Data se uloží lokálně a synchronizují se až bude připojení.

**Jak přidám/odeberu spolubydlícího?**  
Přihlaš se jako majitel → Nastavení → + Přidat nebo ✕ odebrat.

**Jak změním zálohu jednomu spolubydlícímu?**  
Nastavení → klikni ✎ u daného spolubydlícího → uprav „Záloha (Kč/měs.)" → Uložit.
