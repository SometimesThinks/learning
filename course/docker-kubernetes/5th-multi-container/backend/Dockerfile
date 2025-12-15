FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

ENV MONGODB_USERNAME=yun
ENV MONGODB_PASSWORD=daeyeong

CMD ["npm", "start"]