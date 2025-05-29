# syntax=docker/dockerfile:1

### Step 1: Build the Next.js app
FROM --platform=linux/arm/v7 node:18-alpine AS builder

WORKDIR /app

ARG NEXT_FORCE_SWCPPURE
ENV NEXT_FORCE_SWCPPURE=${NEXT_FORCE_SWCPPURE}

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy and build
COPY . .
RUN npm run build

### Step 2: Lightweight production image
FROM --platform=linux/arm/v7 node:18-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# Copy app output from builder
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/package*.json ./

# Reinstall only production dependencies for ARM
RUN npm install --omit=dev

EXPOSE 3000

CMD ["npm", "start"]
