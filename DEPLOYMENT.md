# Deployment

Hosting setup for [NoctaDev Studio](https://noctadevstudio.com/) using GitHub Pages and a Namecheap domain.

## Publish on GitHub Pages

These are manual instructions. Creating the files does not enable Pages or change DNS.

1. Commit and push the website to `main` in `noctadevstudio/noctadevstudio-website`.
2. In the GitHub account/organization **Settings → Pages**, verify ownership of `noctadevstudio.com` with GitHub's supplied TXT record. Keep that record afterward. See [domain verification](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages).
3. In repository **Settings → Pages**, set **Build and deployment → Deploy from a branch**, then `main`, `/ (root)`, and save.
4. Under **Custom domain**, save `noctadevstudio.com`. The tracked `CNAME` already contains it. Configure this before pointing DNS to GitHub.
5. Set the DNS records below. Wait for the DNS check and certificate availability, then select **Enforce HTTPS**. DNS propagation may take up to 24 hours; certificate issuance can also take time.
6. Check deployment in the repository's Actions tab and verify both hostnames below.

## Namecheap DNS

Open **Domain List → Manage → Advanced DNS → Host Records** for `noctadevstudio.com`. If another provider hosts authoritative DNS, edit records there instead. Do not switch nameservers just for this setup.

Use TTL **Automatic**:

| Type | Host | Value |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `noctadevstudio.github.io` |

The CNAME uses the owner hostname without a scheme or repository path. With the apex domain configured in Pages, GitHub redirects `www.noctadevstudio.com` to `noctadevstudio.com`.

Save a copy of existing DNS first. Replace only conflicting **website routing** records for `@` and `www`, such as parking, URL redirects, or obsolete A/AAAA/CNAME records. Do not add wildcard records. IPv6 is optional; review existing AAAA records pointing elsewhere before HTTPS setup.

**Preserve email records:** MX, SPF/TXT, DKIM, DMARC, Zoho verification, mail subdomains, and all other email-related records. Apex TXT records coexist with A records. Do not replace the DNS zone or remove unrelated records. If restrictive CAA records exist, consult GitHub's HTTPS documentation before changes.

Sources:

- [GitHub: manage a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
- [GitHub: HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https)
- [Namecheap: link a domain to GitHub Pages](https://www.namecheap.com/support/knowledgebase/article.aspx/9645/2208/how-do-i-link-my-domain-to-github-pages/)

### Verify publication

- `https://noctadevstudio.com/` loads with a valid certificate.
- `https://www.noctadevstudio.com/` redirects to the apex with a valid certificate.
- Both HTTP addresses redirect to HTTPS after enforcement.
- Styles, script, images, `robots.txt`, and `sitemap.xml` load.
- Apps/Studio navigation, Playlogr link, keyboard focus, and narrow/wide layouts work.
- Optional DNS checks: `nslookup noctadevstudio.com` and `nslookup -type=CNAME www.noctadevstudio.com`.
