# Video Code Development Session

## Project Overview
Created a simple NestJS application with TypeScript in a Yarn workspace setup, featuring cross-workspace imports and hot reload functionality.

## Initial Setup
- **Project**: `yarn-w-nest` - A monorepo using Yarn workspaces
- **Structure**: 
  ```
  yarn-w-nest/
  ├── apps/
  │   ├── nest-app/     # Main NestJS application
  │   └── tryme/        # Shared types/enums package
  ├── package.json       # Root workspace configuration
  └── yarn.lock
  ```

## Development Process

### 1. Creating the NestJS App
**Request**: Create a simple NestJS app with TypeScript in apps folder, with a GET request at root route running on port 4000.

**Files Created**:
- `apps/nest-app/package.json` - Dependencies and scripts
- `apps/nest-app/tsconfig.json` - TypeScript configuration
- `apps/nest-app/nest-cli.json` - NestJS CLI configuration
- `apps/nest-app/src/main.ts` - Entry point (port 4000)
- `apps/nest-app/src/app.module.ts` - Root module
- `apps/nest-app/src/app.controller.ts` - Controller with root GET route
- `apps/nest-app/README.md` - Initial documentation

**Key Features**:
- GET `/` route returning "Hello from NestJS!"
- Runs on port 4000
- TypeScript support
- Hot reload capability

### 2. Workspace Script Configuration
**Issue**: User wanted to run `yarn web start:dev` instead of `yarn workspace @mono/app start:dev`

**Solution**: Added root-level scripts in `package.json`:
```json
"scripts": {
  "web": "yarn workspace @mono/app"
}
```

**Usage**: 
- `yarn web start:dev` - Start in development mode
- `yarn web build` - Build the app
- `yarn web start` - Start in production mode

### 3. Cross-Workspace Import Challenge
**Problem**: Importing from `@mono/tryme` package resulted in ES module syntax errors:
```
SyntaxError: Unexpected token 'export'
```

**Root Cause**: Node.js trying to load TypeScript files directly in CommonJS mode.

**Attempted Solutions**:
1. **Build step approach** - Added TypeScript compilation (rejected - defeats watch mode purpose)
2. **Direct TypeScript imports** - Used relative paths (caused linter errors)
3. **Webpack configuration** - Final solution that worked

### 4. Webpack Solution Implementation
**Final Solution**: Configured NestJS to use webpack for development, enabling:
- Direct TypeScript imports from workspaces
- No build steps required
- Hot reload across workspaces
- Proper module resolution

**Files Modified**:
- `apps/nest-app/nest-cli.json` - Added webpack configuration
- `apps/nest-app/webpack.config.js` - Webpack config with workspace aliases
- `apps/nest-app/package.json` - Added webpack and ts-loader dependencies

**Webpack Configuration**:
```javascript
resolve: {
  alias: {
    '@mono/tryme': path.resolve(__dirname, '../tryme/src/index.ts'),
  },
}
```

## Final Working Configuration

### Root package.json
```json
{
  "name": "yarn-w-nest",
  "private": true,
  "workspaces": ["apps/*"],
  "scripts": {
    "web": "yarn workspace @mono/app"
  }
}
```

### NestJS App (apps/nest-app/)
- **Port**: 4000
- **Import**: `import { SitePromotionRewardType } from '@mono/tryme';`
- **Hot Reload**: ✅ Working
- **Cross-workspace imports**: ✅ Working

### Tryme Package (apps/tryme/)
- **Content**: TypeScript enums and types
- **Build**: Not required
- **Import**: Direct from source

## Key Learnings

1. **Yarn Workspaces**: Use `yarn workspace <package-name> <command>` syntax
2. **TypeScript in Workspaces**: Webpack configuration enables direct TypeScript imports
3. **NestJS Development**: Webpack mode provides better development experience for monorepos
4. **Module Resolution**: Aliases in webpack config solve workspace import issues

## Commands Used

```bash
# From root directory
yarn web start:dev    # Start NestJS app in development mode
yarn web build        # Build the app
yarn web start        # Start in production mode

# From nest-app directory
yarn start:dev        # Direct start
yarn install          # Install dependencies
```

## Final Status
✅ **NestJS app running on port 4000**
✅ **Cross-workspace imports working**
✅ **Hot reload functional**
✅ **No build steps required**
✅ **TypeScript support across workspaces**

The application successfully demonstrates a monorepo setup with NestJS, Yarn workspaces, and seamless TypeScript development experience.
