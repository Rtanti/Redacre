FROM hoosin/alpine-nginx-nodejs

RUN mkdir -p /srv/redacre/frontend
COPY ./ /srv/redacre/frontend/ 
COPY nginx.conf /etc/nginx/nginx.conf
WORKDIR /srv/redacre/frontend/
EXPOSE 80
EXPOSE 3000
RUN npm install
RUN npm run build

CMD ["npm", "start"]
