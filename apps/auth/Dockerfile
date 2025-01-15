FROM node:21-alpine AS dev

WORKDIR /usr/src/app

COPY package*.json ./
COPY pnpm-lock.yaml ./
COPY tsconfig.json tsconfig.json
COPY nest-cli.json nest-cli.json

RUN npm install -g pnpm

RUN pnpm install

COPY apps/auth apps/auth
COPY libs libs

RUN pnpm run build auth

FROM node:21-alpine AS prod

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package*.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm

RUN pnpm install --prod

COPY --from=dev /usr/src/app/dist ./dist

CMD ["node", "dist/apps/auth/main"]