# Build env
FROM node:lts-alpine as build-npm-stage
LABEL maintainer="Mickael BARON"
ARG script_name=build

WORKDIR /polldle-vue
COPY package*.json ./
RUN npm install
COPY public ./public
COPY src ./src
COPY vue.config.js ./
COPY .env.production ./
COPY .env.subpath ./

RUN npm run $script_name

# Run env
FROM nginx:stable-alpine
COPY --from=build-npm-stage /polldle-vue/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]