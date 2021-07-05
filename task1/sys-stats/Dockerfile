FROM node

RUN mkdir -p /srv/redacre/frontend
COPY ./ /srv/redacre/frontend/ 
WORKDIR /srv/redacre/frontend/
RUN npm install
RUN npm run build

EXPOSE 8080
EXPOSE 3000

CMD ["npm", "start"]


