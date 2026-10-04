---
name: "api-developer"
description: "REST and GraphQL API design and development expert. Use when building APIs, designing endpoints, implementing authentication, or documenting APIs. Covers Express, Fastify, Next.js API routes, GraphQL with Apollo, OpenAPI/Swagger, rate limiting, JWT/OAuth authentication, and API testing."
compatibility: pi
---

# API Developer

Comprehensive API design and development for modern web applications.

## Required Safety Baseline

Treat the examples below as conceptual sketches, not production-ready code. Before copying or adapting any snippet:

- Inspect the actual framework, auth model, project conventions, runtime, and deployment configuration.
- Validate and normalize every untrusted parameter/body against a schema; enforce authorization on the server for every resource and action. Authentication alone does not authorize access.
- Use framework-supported session/JWT verification and validate issuer, audience, expiry, and required claims. Never accept a role or owner identity from client input.
- Return stable, sanitized error responses; log diagnostic detail only on the server and never leak secrets or stack traces.
- Make writes safe under retries where side effects or duplicate requests can cause harm; test unauthorized, invalid, missing, and duplicate cases.
- Consult the `vibe-security` skill for auth, user data, credentials, database rules, payments, or other sensitive flows.

## Core Workflows

### 1. REST API with Express

```typescript
import express from 'express';
const app = express();

app.use(express.json());

// GET endpoint
app.get('/api/users', async (req, res) => {
  const users = await db.users.findMany();
  res.json(users);
});

// GET with params
app.get('/api/users/:id', async (req, res) => {
  const user = await db.users.findUnique({
    where: { id: req.params.id }
  });
  if (!user) return res.status(404).json({ error: 'Not found' });
  res.json(user);
});

// POST endpoint
app.post('/api/users', async (req, res) => {
  const { name, email } = req.body;
  const user = await db.users.create({
    data: { name, email }
  });
  res.status(201).json(user);
});
```

### 2. GraphQL with Apollo

```typescript
import { ApolloServer, gql } from '@apollo/server';

const typeDefs = gql`
  type User {
    id: ID!
    name: String!
    email: String!
    posts: [Post!]!
  }
  
  type Query {
    users: [User!]!
    user(id: ID!): User
  }
  
  type Mutation {
    createUser(name: String!, email: String!): User!
  }
`;

const resolvers = {
  Query: {
    users: () => db.users.findMany(),
    user: (_, { id }) => db.users.findUnique({ where: { id } }),
  },
  Mutation: {
    createUser: (_, { name, email }) => 
      db.users.create({ data: { name, email } }),
  },
};
```

### 3. Authentication

```typescript
import jwt from 'jsonwebtoken';

// Generate token
function generateToken(user) {
  return jwt.sign(
    { userId: user.id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '7d' }
  );
}

// Verify middleware
function authMiddleware(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ error: 'No token' });
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch {
    res.status(401).json({ invalid: 'Invalid token' });
  }
}
```

### 4. Rate Limiting

```typescript
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP
  message: { error: 'Too many requests' }
});

app.use('/api/', limiter);
```

### 5. OpenAPI Documentation

```yaml
# openapi.yaml
openapi: 3.0.0
paths:
  /api/users:
    get:
      summary: List users
      parameters:
        - name: limit
          in: query
          schema:
            type: integer
      responses:
        '200':
          description: User list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
```

## Best Practices

1. **Use HTTP methods** correctly: GET, POST, PUT, DELETE
2. **Status codes**: 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 404 Not Found, 500 Error
3. **Version APIs**: `/api/v1/users`
4. **Paginate**: `?page=1&limit=20`
5. **Filter & sort**: `?sort=-created_at&status=active`

## Quick Reference

```typescript
// Express setup
const app = express();
app.use(express.json());

// Routes
app.get('/path', handler);
app.post('/path', handler);
app.put('/path/:id', handler);
app.delete('/path/:id', handler);

// Error handling
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});
```