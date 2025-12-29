FROM node:14

WORKDIR /app

COPY package*.json .

RUN npm install

COPY . .

# 런타임 환경변수
ARG DEFAULT_PORT=80


ENV PORT=$DEFAULT_PORT

EXPOSE $PORT

# VOLUME ["/app/node_modules", "/app/temp"]

CMD ["npm", "start"]