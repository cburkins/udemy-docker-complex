# Client (React frontend) PROD
FROM node:alpine as build
WORKDIR '/app'
# Copying package.json ahead of time means that "npm install" is only dependent on package.json, not other content files
COPY ./package.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx
EXPOSE 3000
# Overwrite the containers default Nginx configuration with our own
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
# Copy our React static content (from build command) to Nginx's default content location
COPY --from=build /app/build /usr/share/nginx/html
