FROM node:18-alpine AS base
RUN apk add --no-cache libc6-compat

FROM base AS deps
WORKDIR /frontend
COPY package*.json ./
RUN npm ci

FROM base AS builder
WORKDIR /frontend
COPY --from=deps /frontend/node_modules ./node_modules
COPY . .
RUN npm run build

FROM base AS runner
WORKDIR /frontend

COPY --from=deps /frontend/node_modules ./node_modules
COPY --from=builder /frontend/.next ./.next
COPY --from=builder /frontend/public ./public
COPY --from=builder /frontend/package*.json ./

RUN npm ci --only=production

ENV NODE_ENV production

CMD ["npm", "start"]

EXPOSE 3000