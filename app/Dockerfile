FROM node:hydrogen-alpine AS builder


# Corepack means a tool for managing package managers.
RUN corepack enable pnpm

WORKDIR /app
COPY ./package.json ./

RUN npm install --silent

COPY . .

RUN npm run build

# NGINX Configuration
FROM nginx:stable-alpine-perl AS production

RUN rm -rf /usr/share/nginx/html/*

COPY --from=builder /app/dist/ /usr/share/nginx/html/
COPY --from=builder /app/.nginx/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

# Start Nginx to serve the application
CMD ["nginx", "-g", "daemon off;"]