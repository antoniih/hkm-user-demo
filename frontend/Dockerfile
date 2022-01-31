FROM node:17-alpine

RUN apk --no-cache add ca-certificates
RUN apk --no-cache add git
WORKDIR /root/
RUN git clone https://github.com/kmikulski/blog-keycloak.git
WORKDIR /root/blog-keycloak/keycloak-react-part1
RUN npm install
CMD ["npm", "start"]