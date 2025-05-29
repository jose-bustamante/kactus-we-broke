# syntax=docker/dockerfile:1

### Step 1: Build the Next.js app
FROM --platform=linux/arm/v7 node:18-alpine AS builder

WORKDIR /app

# No need for ARG if hardcoding
ENV NEXT_FORCE_SWCPPURE=true

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy source files
COPY . .

# Run build with explicit env var
RUN NEXT_FORCE_SWCPPURE=true npm run build

### Step 2: Lightweight production image
FROM --platform=linux/arm/v7 node:18-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# Copy build outputs from builder
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/package*.json ./

# Install only production dependencies
RUN npm install --omit=dev

EXPOSE 3000

CMD ["npm", "start"]
