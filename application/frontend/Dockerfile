

# ============================================
# Stage 1: Angular build
# ============================================
FROM node:22-alpine AS build

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci

COPY . .

RUN npm run build

# ============================================
# Stage 2: Non-root Nginx runtime
# ============================================
FROM nginxinc/nginx-unprivileged:alpine AS runtime

COPY --from=build --chown=nginx:nginx \
    /app/dist/frontend/ \
    /usr/share/nginx/html/

COPY --chown=nginx:nginx nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 8080

USER nginx

CMD ["nginx", "-g", "daemon off;"]


