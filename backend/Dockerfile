FROM node:20

WORKDIR /app

# node:20 ships an older bundled npm whose vendored node-tar has several
# HIGH/CRITICAL CVEs (CVE-2026-23745, -23950, -24842, -26960, -29786,
# -31802, -59874, -73566). These live at
# /usr/local/lib/node_modules/npm/node_modules/tar - not in this project's
# own dependencies - so `npm audit fix` in the app has no effect on them.
# Upgrading npm itself replaces its bundled tar with a patched copy.
RUN npm install -g npm@latest

COPY package*.json ./
RUN npm install

COPY . .

CMD ["node", "server.js"]
