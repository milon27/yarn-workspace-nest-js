# Yarn Workspace with NestJS

A modern monorepo setup using Yarn workspaces with a NestJS application and shared TypeScript packages, featuring hot reload and cross-workspace imports.

## 🚀 Features

- **Monorepo Architecture** - Multiple packages in a single repository
- **NestJS Application** - Full-featured backend with TypeScript
- **Shared Packages** - Reusable types and enums across workspaces
- **Hot Reload** - Instant updates during development
- **No Build Steps** - Direct TypeScript imports in development
- **Yarn Workspaces** - Efficient dependency management

## 📁 Project Structure

```
yarn-w-nest/
├── apps/
│   ├── nest-app/           # Main NestJS application
│   │   ├── src/
│   │   │   ├── main.ts     # Entry point (port 4000)
│   │   │   ├── app.module.ts
│   │   │   └── app.controller.ts
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── nest-cli.json
│   │   └── webpack.config.js
│   └── tryme/              # Shared types package
│       ├── src/
│       │   └── index.ts    # Shared enums and types
│       ├── package.json
│       └── tsconfig.json
├── package.json             # Root workspace configuration
├── yarn.lock
└── README.md
```

## 🛠️ Prerequisites

- **Node.js** (v18 or higher)
- **Yarn** (v4.9.2 or higher)
- **TypeScript** knowledge

## 🚀 Quick Start

### 1. Install Dependencies

```bash
# From root directory
yarn install
```

### 2. Start Development Server

```bash
# Start NestJS app in development mode
yarn web start:dev
```

### 3. Access Your App

- **URL**: http://localhost:4000
- **API Endpoint**: GET `/` returns "Hello from NestJS!"

## 📖 Available Commands

### From Root Directory

```bash
# Start NestJS app in development mode (with hot reload)
yarn web start:dev

# Build the application
yarn web build

# Start in production mode
yarn web start

# Run tests (if configured)
yarn web test
```

### From NestJS App Directory

```bash
cd apps/nest-app

# Development mode
yarn start:dev

# Production mode
yarn start

# Build
yarn build
```

## 🔧 Development Workflow

### 1. Cross-Workspace Imports

Import shared types directly from other packages:

```typescript
// In apps/nest-app/src/main.ts
import { SitePromotionRewardType } from '@mono/tryme';

console.log(SitePromotionRewardType.Item); // 'Item'
```

### 2. Hot Reload

- Edit files in `apps/nest-app/src/` - App reloads automatically
- Edit files in `apps/tryme/src/` - Changes reflect immediately
- No manual build steps required

### 3. Adding New Packages

1. Create new directory in `apps/`
2. Add `package.json` with unique name (e.g., `@mono/new-package`)
3. Add to workspace dependencies where needed
4. Import and use immediately

## 🏗️ Architecture Details

### Webpack Configuration

The NestJS app uses webpack for development to enable:
- Direct TypeScript imports from workspaces
- No compilation steps for shared packages
- Seamless hot reload across workspaces

```javascript
// apps/nest-app/webpack.config.js
resolve: {
  alias: {
    '@mono/tryme': path.resolve(__dirname, '../tryme/src/index.ts'),
  },
}
```

### Workspace Configuration

```json
// Root package.json
{
  "workspaces": ["apps/*"],
  "scripts": {
    "web": "yarn workspace @mono/app"
  }
}
```

## 📦 Package Details

### @mono/app (NestJS Application)

- **Port**: 4000
- **Framework**: NestJS with TypeScript
- **Features**: REST API, hot reload, webpack bundling
- **Dependencies**: NestJS core, Express, RxJS

### @mono/tryme (Shared Types)

- **Purpose**: Shared TypeScript types and enums
- **Build**: Not required (direct import)
- **Usage**: Import types across workspaces

## 🔍 Troubleshooting

### Common Issues

1. **Import Errors**: Ensure package names match in `package.json`
2. **Port Conflicts**: Change port in `apps/nest-app/src/main.ts`
3. **Hot Reload Not Working**: Check webpack configuration
4. **TypeScript Errors**: Verify `tsconfig.json` paths

### Debug Commands

```bash
# Check workspace status
yarn workspaces list

# View workspace info
yarn workspace @mono/app info

# Clean and reinstall
rm -rf node_modules apps/*/node_modules
yarn install
```

## 🧪 Testing

```bash
# Run tests (when configured)
yarn web test

# Run tests in watch mode
yarn web test:watch
```

## 🚀 Production

```bash
# Build for production
yarn web build

# Start production server
yarn web start:prod
```

## 📚 Learning Resources

- [NestJS Documentation](https://docs.nestjs.com/)
- [Yarn Workspaces](https://yarnpkg.com/features/workspaces)
- [TypeScript](https://www.typescriptlang.org/)
- [Webpack](https://webpack.js.org/)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 🆘 Support

If you encounter any issues:
1. Check the troubleshooting section
2. Review the webpack configuration
3. Verify workspace dependencies
4. Check TypeScript configuration

---

**Happy Coding! 🎉**

Built with ❤️ using NestJS, TypeScript, and Yarn Workspaces.
