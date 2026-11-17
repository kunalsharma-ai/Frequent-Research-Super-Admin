# Frontend: build with Vite on Node 24, then serve the static files with nginx.
#   docker build -t fr-frontend ./frontend                      (with search-engine pages, needs about 15 minutes)
#   docker build -t fr-frontend --build-arg PRERENDER=0 ./frontend   (quick build, app only)

FROM node:24-bookworm-slim AS build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --no-audit --no-fund
# Chromium is only needed at build time, to write one HTML file per page for search engines (scripts/prerender.cjs)
ARG PRERENDER=1
RUN if [ "$PRERENDER" = "1" ]; then npx playwright install --with-deps chromium; fi
COPY . .
RUN npm run build && if [ "$PRERENDER" = "1" ]; then npm run prerender; fi

FROM nginx:1.29-alpine
ENV BACKEND_URL=http://backend:8787
COPY nginx/default.conf.template /etc/nginx/templates/default.conf.template
COPY nginx/security-headers.conf /etc/nginx/snippets/security-headers.conf
COPY --from=build /app/dist /usr/share/nginx/html
RUN mkdir -p /etc/nginx/fr \
 && mv /usr/share/nginx/html/.nginx/*.conf /etc/nginx/fr/ \
 && rm -rf /usr/share/nginx/html/.nginx /usr/share/nginx/html/.shell.html
EXPOSE 80
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://127.0.0.1/healthz || exit 1
