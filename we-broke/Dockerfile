# syntax=docker/dockerfile:1

### Step 1: Build the Next.js app
FROM --platform=$BUILDPLATFORM node:18-alpine AS builder

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the source code
COPY . .

# Build the Next.js app
RUN npm run build

### Step 2: Create a lightweight production image
FROM --platform=$BUILDPLATFORM node:18-alpine AS runner

WORKDIR /app

# Set NODE_ENV to production
ENV NODE_ENV=production

# Set ARM architecture config
ENV npm_config_arch=arm


# Copy only the necessary output from the builder
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./

# Expose the port Next.js uses
EXPOSE 3000

# Start the Next.js server
CMD ["npm", "start"]
