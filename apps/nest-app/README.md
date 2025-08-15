# NestJS App

A simple NestJS application with TypeScript that provides a GET endpoint at the root route.

## Features

- GET `/` - Returns "Hello from NestJS!"
- Runs on port 4000
- TypeScript support
- Hot reload in development mode

## Installation

```bash
yarn install
```

## Running the App

### Development Mode (with hot reload)
```bash
yarn start:dev
```

### Production Mode
```bash
yarn build
yarn start:prod
```

## API Endpoints

- `GET /` - Returns a greeting message

## Port

The application runs on port 4000 by default.

## Project Structure

```
src/
├── main.ts          # Application entry point
├── app.module.ts    # Root module
└── app.controller.ts # Main controller with root route
```
