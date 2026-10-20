# Netlify par trial deploy (pehle ye)

Ye version website + AI dono Netlify par chalata hai. Server ki zaroorat nahi, Netlify Functions AI sambhalte hain.

## Steps

1. **GitHub par project daaliye.** GitHub par naya repository banaiye (private rakh sakte hain) aur `bolchaal` folder ki saari files upload kijiye. `node_modules` folder mat daaliye.
2. **Netlify par site banaiye.** netlify.com par sign up karke "Add new site" > "Import an existing project" > GitHub chuniye aur ye repository select kijiye.
3. **Build settings.** Netlify khud `netlify.toml` padh leta hai. Build command khaali chhod dijiye. Publish folder `web` dikhna chahiye.
4. **API key daaliye.** Site settings > Environment variables mein:
   - `ANTHROPIC_API_KEY` = aapki key
   - `ANTHROPIC_MODEL` = `claude-haiku-5-5` (optional, ye default hi hai)
   - `RATE_PER_MINUTE` = `20` (optional)
   - `RATE_PER_DAY` = `300` (optional)
5. **Deploy karke chalaiye.** Deploy hone ke baad diya gaya `https://...netlify.app` address phone par kholiye. Chat karke dekhiye.
6. **Phone par app jaisa install:** Chrome mein site kholkar menu > "Add to Home screen".

## Trial ke liye check list

- [ ] Site khulti hai aur "Start talking" par AI ka pehla message aata hai
- [ ] Typing se baat hoti hai, galti par "Say it like this" dikhta hai
- [ ] Hindi button se Hindi anuvaad aata hai
- [ ] Phone ke Chrome mein mic dabane par bolne ki permission poochhti hai aur text aata hai
- [ ] Words, Progress tab chalte hain aur page refresh karne par progress rehti hai

## Dhyan dene wali baatein

- **Mic:** Chrome aur Safari par mic kaam karta hai. Netlify par HTTPS hota hai, isliye permission milti hai.
- **Rate limit:** Netlify Blobs mein counter rakha hai. Agar storage fail ho to request chal jaati hai (taaki learners ruke nahi), aur error log hota hai.
- **Netlify limits:** free plan mein function calls aur bandwidth ki limit hai. Ye Netlify ke pricing page par check kar lijiye, kyunki ye badalte rehte hain.
- **Kharcha:** har message par Anthropic API ka paisa lagta hai. Console mein spend limit zaroor lagaiye.
- **Android app baad mein:** `web/config.js` mein `window.BOLCHAAL_API = 'https://aapki-site.netlify.app'` likhna hoga, phir Play Store wala step.
