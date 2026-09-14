FROM node:20-slim
# Switched from `node:20` (full Debian) to `node:20-slim`: the full image
# ships hundreds of OS packages this app never uses (ImageMagick, Kerberos,
# full kernel headers/tools, etc.), each one a potential source of
# HIGH/CRITICAL CVEs from Debian's own patch cycle. slim keeps the same
# Debian base and glibc (so pg/redis native bits behave identically) but
# drops that unused surface, which is what was driving the 805-finding
# Trivy result down to a normal handful.

WORKDIR /app

# node:20 ships an older bundled npm whose vendored node-tar has several
# HIGH/CRITICAL CVEs (CVE-2026-23745, -23950, -24842, -26960, -29786,
# -31802, -59874, -73566). These live at
# /usr/local/lib/node_modules/npm/node_modules/tar - not in this project's
# own dependencies - so `npm audit fix` in the app has no effect on them.
# Upgrading npm replaces its bundled tar with a patched copy.
#
# Pinned (not @latest): npm 12.x now requires Node >=22, which this base
# image doesn't have. npm 11.x still supports Node 20 (^20.17.0 || >=22.9.0)
# and 11.19.1+ bundles a tar release that clears the CVE list above.
# Bump this pin, don't drop it, whenever a newer 11.x closes a future CVE.
RUN npm install -g npm@11.19.1

COPY package*.json ./
RUN npm install

COPY . .

CMD ["node", "server.js"]
