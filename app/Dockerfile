# 本番環境用のイメージはスリムなものを使う
FROM node:lts-bullseye-slim

WORKDIR /app

COPY package*.json ./

COPY yarn.lock ./

RUN yarn install --frozen-lockfile --production

COPY . .

EXPOSE 3000

CMD ["yarn", "start"]
