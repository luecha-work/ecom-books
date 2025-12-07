# Ecom Book

Full-stack e-commerce project composed of a Next.js storefront/admin dashboard and a NestJS API backed by PostgreSQL. Use this README to spin up both apps locally or in Docker and to see where to change environment/config values.

## Project layout
- `nextjs-ecom-fontend` – Next.js 13 front end using TypeScript, MUI, Redux Toolkit/Zustand, and axios. API base URL is defined in `nextjs-ecom-fontend/service/abstract.service.tsx`.
- `nestjs-ecom-backend` – NestJS 7 API using TypeORM, PostgreSQL, JWT auth, file uploads, scheduling, and Swagger docs.

## Prerequisites
- Node.js 18+ and Yarn or npm
- Docker (for the compose workflows)
- PostgreSQL (if running the API without Docker)

## Backend setup (`nestjs-ecom-backend`)
1) Create an `.env` file (or edit the existing one) with your values:
```
PORT=1000
CORS_ORIGIN=http://localhost:3000
ORIGIN_CORS_PORT=3000
BASE_URL=http://localhost:1000/api
PROFILE_PATH=uploads/profile
PRODUCTS_PATH=uploads/products
PRODUCTS_PAYMENT_PATH=uploads/payment
VIDEO_PATH=uploads/videos
JWT_KEY=changeme
JWT_SECRET=changeme
EXPIRESIN=3600
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=postgres
DB_DATABASE=ecom
```
2) Install deps and start the API:
```
cd nestjs-ecom-backend
yarn install
yarn start:dev
```
3) Swagger docs are served from `http://localhost:1000/pe-shop-api/docs` (also saved to `nestjs-ecom-backend/swagger-docs.json`). The generated TypeORM connection currently uses a connection string in `src/app.module.ts`; point it to your DB or swap in the env vars above before running locally.

## Frontend setup (`nextjs-ecom-fontend`)
1) Point the client to your API by changing `base_url` in `nextjs-ecom-fontend/service/abstract.service.tsx` (e.g., `http://localhost:1000/api` when running the backend locally).
2) Install deps and start the dev server:
```
cd nextjs-ecom-fontend
yarn install
yarn dev
```
3) The app runs on `http://localhost:3000`.

## Docker workflows
- API + Postgres: from `nestjs-ecom-backend`, run `docker-compose up --build`. The API listens on port 1000 and Postgres on 5432. Ensure your `.env` matches these values.
- Front end: from `nextjs-ecom-fontend`, run `docker-compose up --build` to serve the Next.js app on port 3000. Update the client `base_url` to reach the backend container/host.

## Data, uploads, and seeds
- Sample SQL: `nestjs-ecom-backend/data-demo.sql` can be loaded into your Postgres instance.
- File storage: upload paths are controlled by the `*_PATH` variables and default to the `nestjs-ecom-backend/uploads` directory.

## Helpful commands
- Backend: `yarn test`, `yarn test:e2e`, `yarn lint`, `yarn build`
- Frontend: `yarn lint`, `yarn build`, `yarn start` (after building)
