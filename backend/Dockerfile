FROM node:18-alpine

WORKDIR /app

ENV NODE_ENV=production

COPY package*.json ./
RUN npm install --omit=dev && npm cache clean --force

COPY . .

ENV PORT=8080
EXPOSE 8080

CMD ["npm", "start"]

