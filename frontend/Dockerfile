FROM node:18-alpine AS build

WORKDIR /frontend

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM node:18-alpine AS production

WORKDIR /frontend

COPY --from=build /frontend/package*.json ./
COPY --from=build /frontend/.next ./
COPY --from=build /frontend/public ./public

RUN npm ci --only=production

ENV NODE_ENV production

CMD ["npm", "start"]

EXPOSE 3000