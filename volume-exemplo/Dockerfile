FROM node:22

LABEL maintainer="Malu Vicentini"

COPY . /var/www

WORKDIR /var/www

RUN npm install

EXPOSE 3000

ENTRYPOINT ["npm","start"]


