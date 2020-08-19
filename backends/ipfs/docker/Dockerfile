FROM node:14.8.0-alpine

RUN mkdir -p /app && chown -R node:node /app

WORKDIR /app

COPY app/main.js /app

CMD ["node", "main.js"]