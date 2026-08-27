# CyberScut — găzduire pe GitHub Pages + editor de articole

## 1. Publicare

1. Încarcă tot conținutul acestui folder în root-ul repo-ului `ValentinRo/Bugs`.
2. Settings → Pages → Source: branch `main`, folder `/ (root)`.
3. Site-ul apare la `https://valentinro.github.io/Bugs`.

Domeniu propriu (opțional): adaugă un fișier `CNAME` cu `cyberscut.ro`, apoi la registrar
setează recordurile A către IP-urile GitHub Pages. Activează HTTPS din Settings → Pages.

## 2. Editorul de articole (Sveltia CMS)

Editorul se deschide la `/admin/` — de exemplu `https://valentinro.github.io/Bugs/admin/`.
Scrii articolul într-o interfață de browser, iar la salvare fișierul `.md` este commit-uit
în repo. Nu există server sau bază de date: suprafața de atac este contul tău GitHub.

### Autentificare — pas obligatoriu, o singură dată

Sveltia CMS are nevoie de un serviciu mic de autentificare OAuth (nu poate vorbi direct cu
GitHub din browser). Varianta recomandată de proiect:

1. Deployează `sveltia-cms-auth` (Cloudflare Worker, plan gratuit) — repo:
   `https://github.com/sveltia/sveltia-cms-auth`.
2. Creează un GitHub OAuth App (Settings → Developer settings → OAuth Apps), cu
   Authorization callback URL = URL-ul worker-ului + `/callback`.
3. Pune `GITHUB_CLIENT_ID` și `GITHUB_CLIENT_SECRET` ca variabile în worker.
4. În `admin/config.yml`, sub `backend:`, adaugă linia:
   `base_url: https://<worker-ul-tau>.workers.dev`

Activează 2FA pe contul GitHub — el este singura cheie către conținut.

## 3. Cum apar articolele pe site

- `content/blog/*.md` — fișierele scrise din editor (front matter YAML + Markdown).
- `content-index.js` — lista fișierelor publicate. **După fiecare articol nou, adaugă o
  linie cu numele fișierului aici.** (GitHub Pages nu poate lista un folder singur.)
- `blog.html` — citește lista și afișează cardurile.
- `post.html?slug=<nume-fisier-fara-.md>` — randează articolul.

Ciornele (`draft: true` în front matter) nu apar în listă.

## 4. Documentele contractuale

`docs/` conține acordul de autorizare a testării (complet și simplificat) și politica de
confidențialitate, oferite ca descărcare în pagina de contact. Butonul de plată se
activează doar după bifarea confirmării că au fost citite și semnate.

Link-ul de plată din `contact.html` este `#` — înlocuiește-l cu link-ul Stripe/PayPal real.
