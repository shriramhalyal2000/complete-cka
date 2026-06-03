ARG NODE_VERSION=24-alpine

# 1. Base stage for shared configuration
FROM node:${NODE_VERSION} AS base
WORKDIR /app
COPY package*.json ./

# 2. Build stage (Installs ALL dependencies and builds the app)
FROM base AS build
RUN npm ci 
COPY . .
# Note: Ensure "build" is defined in your package.json scripts (npm run build)
RUN npm run build

# 3. Production dependencies stage (Only keeps runtime dependencies)
FROM base AS prod-deps
RUN npm ci --only=production

# 4. Final runner stage (Ultra-lean production image)
FROM node:${NODE_VERSION} AS runner
ENV NODE_ENV=production
WORKDIR /app

# Copy compiled code from the build stage
COPY --from=build /app/dist ./dist

# Copy ONLY production dependencies from the prod-deps stage
COPY --from=prod-deps /app/node_modules ./node_modules
COPY package*.json ./

EXPOSE 3000
CMD ["node", "dist/server.js"]