FROM node:20

WORKDIR /usr/src/app
COPY package*.json ./
# Use npm ci when a lockfile is present for reproducible installs,
# otherwise fall back to a production install so the Docker build doesn't fail.
RUN if [ -f package-lock.json ]; then npm ci --omit=dev; else npm install --production --no-audit --no-fund; fi
COPY . .
RUN npm run build

# This makes sure traffic is only sent to this container when its healthy (vendure started)
HEALTHCHECK --interval=1m --timeout=5s --start-interval=1s --start-period=20s --retries=3 CMD curl -f http://localhost:3000/health || exit 1