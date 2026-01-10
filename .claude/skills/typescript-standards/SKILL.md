---
name: typescript-standards
description: TypeScript/JavaScript coding standards, React patterns, and type safety. Use when writing TypeScript or JavaScript code to ensure consistent style and strong typing.
---

# TypeScript Standards

TypeScript and JavaScript coding standards for TypeScript 5.x, Node.js 18+.

---

## Code Style

### Formatting

**Use Prettier**:
```bash
prettier --write .
```

Configuration (.prettierrc):
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100
}
```

### Linting

**Use ESLint with TypeScript**:
```bash
eslint --ext .ts,.tsx .
```

Configuration (flat config):
```javascript
import eslint from '@eslint/js';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  eslint.configs.recommended,
  ...tseslint.configs.strictTypeChecked,
  {
    rules: {
      '@typescript-eslint/explicit-function-return-type': 'error',
    },
  }
);
```

---

## Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| Files (components) | PascalCase | `UserProfile.tsx` |
| Files (utilities) | camelCase | `formatDate.ts` |
| Classes | PascalCase | `UserService` |
| Interfaces | PascalCase | `User` or `IUser` |
| Types | PascalCase | `UserResponse` |
| Functions | camelCase | `getUserById` |
| Variables | camelCase | `userCount` |
| Constants | UPPER_SNAKE | `MAX_RETRIES` |
| Enums | PascalCase | `UserStatus` |

```typescript
// Good
const userRepository = new UserRepository();
const isValid = validateInput(data);
const MAX_RETRY_ATTEMPTS = 3;

// Bad
const ur = new UserRepository();  // Too short
const isvalid = validateInput(data);  // Missing camelCase
```

---

## TypeScript Configuration

### Strict Mode (Required)

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "exactOptionalPropertyTypes": true,
    "forceConsistentCasingInFileNames": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

### Type Safety Rules

```typescript
// Always use explicit types for functions
function getUser(id: number): Promise<User | null> {
  // ...
}

// Use strict null checks
function processUser(user: User | null): void {
  if (!user) return;
  // user is now User (not null)
}

// Avoid `any` - use `unknown` for unknown types
function process(data: unknown): string {
  if (typeof data === 'string') {
    return data;
  }
  throw new Error('Expected string');
}
```

---

## Project Structure

```
project/
├── package.json
├── tsconfig.json
├── src/
│   ├── index.ts
│   ├── types/
│   ├── models/
│   ├── services/
│   └── utils/
├── tests/
│   ├── unit/
│   └── integration/
└── dist/
```

### Barrel Exports

```typescript
// src/models/index.ts
export { User } from './user';
export { Post } from './post';
export type { UserCreateInput } from './user';
```

---

## Type Definitions

### Interfaces vs Types

```typescript
// Interface for object shapes (extendable)
interface User {
  id: number;
  email: string;
}

interface AdminUser extends User {
  permissions: string[];
}

// Type for unions, primitives, computed types
type UserId = number | string;
type UserStatus = 'active' | 'inactive' | 'pending';
type UserWithPosts = User & { posts: Post[] };
```

### Utility Types

```typescript
type UserUpdate = Partial<User>;      // All optional
type CompleteUser = Required<User>;   // All required
type UserPreview = Pick<User, 'id' | 'name'>;
type UserWithoutId = Omit<User, 'id'>;
type UserRoles = Record<UserId, string[]>;
```

### Discriminated Unions

```typescript
type Result<T, E = Error> =
  | { success: true; data: T }
  | { success: false; error: E };

function handleResult(result: Result<User>): void {
  if (result.success) {
    console.log(result.data.name);
  } else {
    console.error(result.error.message);
  }
}
```

---

## Error Handling

### Custom Error Classes

```typescript
class AppError extends Error {
  constructor(
    message: string,
    public readonly code?: string,
    public readonly details?: Record<string, unknown>
  ) {
    super(message);
    this.name = this.constructor.name;
  }
}

class ValidationError extends AppError {
  constructor(message: string, details?: Record<string, unknown>) {
    super(message, 'VALIDATION_ERROR', details);
  }
}

class NotFoundError extends AppError {
  constructor(resource: string, id: string | number) {
    super(`${resource} with id ${id} not found`, 'NOT_FOUND');
  }
}
```

### Error Handling Patterns

```typescript
// Type-safe error handling
async function getUser(id: number): Promise<User> {
  const user = await db.users.findUnique({ where: { id } });
  if (!user) {
    throw new NotFoundError('User', id);
  }
  return user;
}

// Result type pattern
async function getUserSafe(id: number): Promise<Result<User>> {
  try {
    const user = await db.users.findUnique({ where: { id } });
    if (!user) {
      return { success: false, error: new NotFoundError('User', id) };
    }
    return { success: true, data: user };
  } catch (error) {
    return { success: false, error: error as Error };
  }
}
```

---

## Async/Await

```typescript
// Clean async/await
async function fetchUserData(userId: number): Promise<UserData> {
  const user = await getUser(userId);
  const posts = await getUserPosts(userId);
  return { user, posts };
}

// Parallel execution
async function fetchUserDataParallel(userId: number): Promise<UserData> {
  const [user, posts, followers] = await Promise.all([
    getUser(userId),
    getUserPosts(userId),
    getUserFollowers(userId),
  ]);
  return { user, posts, followers };
}
```

---

## Testing

### Jest Configuration

```javascript
export default {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/tests'],
  collectCoverageFrom: ['src/**/*.ts'],
  coverageThreshold: {
    global: { branches: 80, functions: 80, lines: 80 },
  },
};
```

### Test Structure

```typescript
import { describe, it, expect, beforeEach } from '@jest/globals';

describe('UserService', () => {
  let service: UserService;

  beforeEach(() => {
    service = new UserService();
  });

  describe('getUser', () => {
    it('should return user when found', async () => {
      const result = await service.getUser(123);
      expect(result).not.toBeNull();
      expect(result?.id).toBe(123);
    });
  });
});
```

---

## Common Patterns

### Dependency Injection

```typescript
interface UserRepository {
  findById(id: number): Promise<User | null>;
}

class UserService {
  constructor(private readonly repository: UserRepository) {}

  async getUser(id: number): Promise<User | null> {
    return this.repository.findById(id);
  }
}
```

### Builder Pattern

```typescript
class QueryBuilder<T> {
  private filters: Array<(item: T) => boolean> = [];

  where(predicate: (item: T) => boolean): this {
    this.filters.push(predicate);
    return this;
  }

  execute(items: T[]): T[] {
    return items.filter((item) => this.filters.every((f) => f(item)));
  }
}
```

---

## Anti-Patterns

| Anti-Pattern | Problem | Better Approach |
|--------------|---------|-----------------|
| `any` type | No type safety | Use `unknown` or specific types |
| Non-null assertion (`!`) | Hides potential nulls | Use proper null checks |
| `var` keyword | Hoisting issues | Use `const` or `let` |
| Callback hell | Hard to read | Use async/await |

```typescript
// Bad: Non-null assertion
function getUser(id: number): User {
  return findUser(id)!;  // Dangerous
}

// Good: Explicit handling
function getUser(id: number): User {
  const user = findUser(id);
  if (!user) throw new NotFoundError('User', id);
  return user;
}
```

---

## Tools Summary

| Tool | Purpose | Command |
|------|---------|---------|
| Prettier | Formatting | `prettier --write .` |
| ESLint | Linting | `eslint .` |
| TypeScript | Type checking | `tsc --noEmit` |
| Jest | Testing | `jest` |
| npm audit | Security | `npm audit` |
