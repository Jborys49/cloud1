# Etap build (tworzenie statycznych plików)
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
RUN npm prune --production

# Etap produkcyjny (uruchamianie statycznej aplikacji)
FROM node:22-alpine
WORKDIR /app

# Instalujemy serwer do hostowania plików (serve)
RUN npm install -g serve

# Kopiujemy tylko build
COPY --from=builder /app/build ./build

# Zmienna środowiskowa z backendem
ENV PUBLIC_API_BASE_URL=http://<BACKEND_IP>:8081

# Wystawiamy port
EXPOSE 3000

# Uruchamiamy serwer statyczny
CMD [ "serve", "-s", "build", "-l", "3000" ]