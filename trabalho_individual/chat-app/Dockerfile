FROM node:16

WORKDIR /chat-app

COPY . /chat-app/

RUN yarn

COPY . /chat-app/

EXPOSE 7001

CMD yarn watch