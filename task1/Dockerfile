ARG NODE_VERSION = 24-alpine
FROM node-${NODE_VERSION} AS base

WORKDIR /app
#clean npm install prod
COPY package*.json ./

RUN npm ci --production
#installing necessary dependencies for build froma pplication dependencies package
FROM deps AS base
#copy code
COPY . .
#building the application from installed dependencies package
RUN npm build

FROM node-${NODE_VERSION} AS runner

WORKDIR /app
#use lib build or dist depending on production requirement
COPY --from=build /app/dist ./dist \
     --from=build /app/node_modules /node_modules \
     package*.json ./


#5000 depending on service type
EXPOSE 3000
CMD ["node", "dist/server.js"] 