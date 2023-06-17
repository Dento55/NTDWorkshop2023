FROM node:19-alpine

WORKDIR /usr/src/app

COPY package*.json .

RUN npm install

COPY . .

# RUN npx jest --clearCache


CMD npm test