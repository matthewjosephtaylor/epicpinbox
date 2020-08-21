FROM node:14.8.0-alpine

RUN mkdir -p /app && chown -R node:node /app

WORKDIR /app

ARG APP_CID
ARG IPFS_PROXY
ADD "${IPFS_PROXY}/ipfs/${APP_CID}" /app/main.js

ARG APP_CHECKSUM
RUN echo "${APP_CHECKSUM}  /app/main.js" | sha256sum -c

CMD ["node", "main.js"]