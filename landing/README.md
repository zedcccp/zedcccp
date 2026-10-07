# ZedCCCP landing page

A standalone, dependency-free static site. No build step, client JavaScript,
external fonts, analytics, or third-party asset requests.

## Preview

For a quick local look, open `landing/index.html` directly in a browser.
All asset paths are relative and the page does not require JavaScript.

From the repository root, with Python 3 installed:

```sh
python3 -m http.server 8080 --bind 127.0.0.1 --directory landing
```

Open http://127.0.0.1:8080. Any static HTTP server can serve this directory.
Do not serve the repository root.

## Design and copy

- Tokyo Night-inspired ink, blue, violet, cyan, and green; monospace details
  and a modal-editor motif nod to LazyVim without presenting this as a Vim distribution.
- The supplied `tokyonight_hammer_sickle.svg` artwork combines a blue geometric
  Zed mark with a coral hammer and sickle. It is shared by the favicon, header,
  hero, and footer so the mark stays consistent. It is not the official Zed logo.
- Candid copy positions the fork as a bleeding-edge, slop-forward feature-testing
  ballpit, started for smooth scrolling support, not as a stable distribution.
- A prominent thank-you credits the Zed team and upstream contributors for the
  editor and its native foundation, with a direct link to Zed and an explicit
  non-affiliation/non-endorsement statement.
- The editor is an HTML/CSS illustration, not a screenshot or interactive demo.
- Repository links point to `https://github.com/zedcccp/zedcccp`. There is no
  download CTA until a release/distribution path is confirmed.

## Eventual LXC / Cloudflare Tunnel deployment

This directory is the entire public artifact: `index.html`, `styles.css`, and
`tokyonight_hammer_sickle.svg`. Copy only those three files to the web server's document root.
No Node or Rust runtime is needed in production.

Suggested deployment shape:

```text
Visitor → Cloudflare HTTPS → Cloudflare Tunnel → static HTTP server in LXC
```

1. Provision the LXC and install a static server such as nginx or Caddy.
2. If `cloudflared` runs in the same container, bind the static server to
   `127.0.0.1:8080` and point the tunnel's public hostname at that address.
   If it runs elsewhere, explicitly configure a private listener and firewall
   access for that connector rather than exposing the origin publicly.
3. Supply tunnel credentials through the deployment environment or a protected
   file outside this repository. Never commit them.
4. Configure the hostname, HTTPS behavior, and a catch-all 404 tunnel rule.
   Serve unknown file paths as 404s; this is not a single-page application.
5. Keep HTML and the unversioned stylesheet revalidated (`Cache-Control:
   no-cache`) until asset fingerprinting is introduced.
6. Check the public hostname, mobile layout, keyboard navigation, and links
   before announcing the page.

The hostname, container placement, origin server, and tunnel configuration are
intentionally not provisioned here. Python's preview server is not the
production server.
