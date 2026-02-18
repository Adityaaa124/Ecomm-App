# Stage 1 : Build Stage
FROM node:22-alpine AS builder

#  set the working directory in the application
WORKDIR /app

# copy package files
COPY package*.json ./
COPY prisma ./prisma/

#  install all the dependencies
RUN npm install

# generate prisma client
RUN npx prisma generate

# copy the rest
COPY . .

# building the application
RUN npm run build

# Stage 2 : Production Stage

FROM node:22-alpine AS production

WORKDIR /app

# copy package files (for metadata)
COPY package*.json ./

# copy installed deps from builder to avoid reinstalling dev deps
COPY --from=builder /app/node_modules ./node_modules

# copy the built application and prisma schema from the builder
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/prisma ./prisma

# expose port
EXPOSE 3001

# the command to run the dist
CMD ["node", "dist/app.js"]
