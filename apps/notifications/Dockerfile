FROM node:21-alpine AS dev

WORKDIR /usr/src/app

COPY package*.json ./
COPY pnpm-lock.yaml ./
COPY tsconfig.json tsconfig.json
COPY nest-cli.json nest-cli.json

RUN npm install -g pnpm

RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install

COPY apps/notifications apps/notifications
COPY libs libs

RUN pnpm run build notifications

FROM node:21-alpine AS prod

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package*.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm

RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --prod

COPY --from=dev /usr/src/app/dist ./dist

CMD ["node", "dist/apps/notifications/main"]