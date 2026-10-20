# Bol Chaal: website + Android app

Ek hi project se teen cheezein: **website**, **installable app (PWA)** aur **Android app (Play Store)**.

```
bolchaal/
  server/index.js     AI server (API key yahin rehti hai) + website serve karta hai
  web/                website: index.html, manifest, service worker, icons, privacy.html
  web/config.js       Android app ke liye server ka address yahan likhna hai
  assets/             icon source (Android icons banane ke liye)
  capacitor.config.json   Android app ki settings
```

## 1. Apne computer par chalana

Node.js 18 ya naya chahiye.

```bash
npm install
export ANTHROPIC_API_KEY=sk-ant-...      # Windows: set ANTHROPIC_API_KEY=...
npm start
```

Browser mein `http://localhost:3000` kholiye. Mic ke liye Chrome ya Safari use kijiye.

Key console.anthropic.com se milti hai. **Key ko kabhi `web/` folder ya app ke andar mat rakhiye.** Wo sirf server par environment variable mein rehni chahiye.

Zaroori settings (sab environment variables):

| Naam | Matlab | Default |
|---|---|---|
| `ANTHROPIC_API_KEY` | aapki API key | (zaroori) |
| `ANTHROPIC_MODEL` | kaunsa model | `claude-haiku-5-5` (tez aur sasta) |
| `RATE_PER_MINUTE` | ek IP se ek minute mein kitne messages | 20 |
| `RATE_PER_DAY` | ek IP se ek din mein kitne messages | 300 |
| `ALLOWED_ORIGINS` | Android app ke liye allowed addresses | capacitor/localhost |

Har message par API ka kharcha aata hai. Shuru mein rate limit kam rakhiye aur Anthropic console mein monthly spend limit laga dijiye.

## 2. Website live karna (internet par)

Kisi bhi Node hosting par chalega (Render, Railway, Fly.io, apna VPS). Steps lagbhag ek jaise hain:

1. Project ko GitHub par daaliye.
2. Hosting par naya "Web Service" banaiye, build command `npm install --omit=dev`, start command `npm start`.
3. Environment variable `ANTHROPIC_API_KEY` set kijiye.
4. Mile hue `https://...` address par site khul jayegi. Apna domain bhi jod sakte hain.

HTTPS zaroori hai (mic aur app install dono ke liye). Hosting wale ye khud dete hain.

Phone par Chrome mein site kholkar menu se **Install app / Add to Home screen** karne par ye app jaisi chalti hai.

## 3. Android app (Play Store)

Pehle section 2 poora kijiye, kyunki app ko ek live server chahiye.

**Zaroori cheezein:** Android Studio, JDK 17, aur Google Play developer account (ek baar ka fees lagta hai).

1. `web/config.js` mein apna live address likhiye:
   ```js
   window.BOLCHAAL_API = 'https://aapka-server.example.com';
   ```
2. `capacitor.config.json` mein `appId` badaliye (jaise `com.aapkanaam.bolchaal`). Ye Play Store par unique rehna chahiye aur baad mein badla nahi ja sakta.
3. Android project banaiye:
   ```bash
   npm install
   npx cap add android
   npm run icons          # assets/ se Android icons banata hai
   npx cap sync android
   npx cap open android   # Android Studio khulega
   ```
4. `android/app/src/main/AndroidManifest.xml` mein ye line `<manifest>` ke andar jodiye (mic ke liye):
   ```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
   ```
5. Android Studio mein phone ya emulator par **Run** karke test kijiye. Mic dabane par permission poochhegi.
6. Release ke liye: **Build > Generate Signed Bundle / APK > Android App Bundle (.aab)**. Signing key (keystore) banakar **surakshit jagah backup rakhiye**. Ise khone par app update nahi ho payegi.
7. Play Console mein naya app banaiye, `.aab` upload kijiye.

**Play Store ke liye taiyari:**
- Privacy policy ka public link: `web/privacy.html` ko edit karke ([YOUR NAME], [EMAIL], [DATE] bhariye), site ke saath live kijiye, aur link `https://aapka-server.example.com/privacy` Play Console mein dijiye.
- "Data safety" form bharna padega. Sach batayiye: mic se voice device par text banti hai, likha hua text server aur AI service ko jata hai, koi account nahi, progress sirf phone mein.
- Store listing: naam, chhota aur lamba description, 512x512 icon (`web/icon-512.png`), screenshots, aur content rating questionnaire.
- Play Console ki current requirements (naye developer account ke liye testing rules, target Android version) badalti rehti hain, isliye submit karne se pehle unka latest page zaroor dekhiye.

Nayi version ke baad: `web/` mein badlav karke `npx cap sync android` chalaiye, phir nayi `.aab` banaiye (version code badhana mat bhooliye).

## Kaise kaam karta hai

- Website aur app ek hi `web/index.html` chalate hain.
- App ya site `POST /api/chat` ko level, topic aur chat ka history bhejti hai. Prompt server banata hai, isliye koi bahar se is server ko free AI ki tarah use nahi kar sakta.
- Server kuch bhi save ya log nahi karta. Progress, words aur settings user ke phone ke storage mein rehte hain.
- Mic: Android app mein `@capacitor-community/speech-recognition` plugin, website par browser ki speech recognition. Dono mein `en-IN` use hota hai.

## Aage ke vichar

- Login aur cloud mein progress save (taki phone badalne par bhi streak bani rahe).
- Premium plan ya daily free limit, taki API ka kharcha nikal sake.
- Pronunciation score, IELTS/interview mock mode, roz ka naya topic.
- iPhone app (Capacitor se `npx cap add ios`, Mac aur Apple developer account chahiye).
