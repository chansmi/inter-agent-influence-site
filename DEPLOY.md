# Publishing inter-agent-influence.com

The site is static: `index.html`, `authors/`, `CNAME`, `.nojekyll`. Host it on GitHub Pages and point the Namecheap domain at it.

## 1. Put the files in a public GitHub repo

From the folder this file is in:

```
git init -b main
git add -A
git commit -m "Inter-Agent Influence site"
gh repo create chansmi/inter-agent-influence-site --public --source=. --push
```

(Without `gh`: create the empty public repo on github.com, then `git remote add origin git@github.com:chansmi/inter-agent-influence-site.git && git push -u origin main`.)

## 2. Turn on Pages with the custom domain

Repo → Settings → Pages:
- Build and deployment: Source "Deploy from a branch", branch `main`, folder `/ (root)`. Save.
- Custom domain: `inter-agent-influence.com`. Save. (The `CNAME` file in this folder already holds that name, so GitHub will not need to add a commit.)
- Leave "Enforce HTTPS" for step 4.

## 3. Namecheap DNS

Namecheap → Domain List → inter-agent-influence.com → Manage → Advanced DNS. Nameservers must be "Namecheap BasicDNS".

Delete the two parking records Namecheap adds by default (a `URL Redirect` record for `@` and a `CNAME` for `www` pointing at `parkingpage.namecheap.com`), then add:

| Type  | Host | Value                 | TTL       |
|-------|------|-----------------------|-----------|
| A     | @    | 185.199.108.153       | Automatic |
| A     | @    | 185.199.109.153       | Automatic |
| A     | @    | 185.199.110.153       | Automatic |
| A     | @    | 185.199.111.153       | Automatic |
| CNAME | www  | chansmi.github.io.    | Automatic |

Optional, for IPv6: four `AAAA` records for `@` → `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`.

Propagation is usually minutes on Namecheap, up to an hour.

## 4. HTTPS

Back in Settings → Pages, wait for the "DNS check successful" tick, then tick "Enforce HTTPS". GitHub issues the certificate; this can take up to an hour after DNS resolves. `www.inter-agent-influence.com` will redirect to the apex automatically.

## 5. Check

```
dig +short inter-agent-influence.com A        # the four 185.199.x.153 addresses
dig +short www.inter-agent-influence.com CNAME # chansmi.github.io.
curl -sI https://inter-agent-influence.com | head -3
```

## Updating the site later

Edit `index.html` (or regenerate it with `build/site.py`), commit, push. Pages redeploys in about a minute.

## Alternative without GitHub

Netlify Drop (app.netlify.com/drop): drag this folder in, then Site settings → Domain management → add `inter-agent-influence.com`, and follow Netlify's DNS instructions (an `A` record for `@` to `75.2.60.5` and a `CNAME` for `www` to the site's `*.netlify.app` name). Delete the `CNAME` file first; it is GitHub-specific.
