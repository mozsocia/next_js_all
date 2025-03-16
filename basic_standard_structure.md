below i have a project structure(given below) with hono js api in  next js 14 project setup  ,
- I  use Prisma for db, zod for validation and separate file for db operation 
- i want all database related files in a parent directory
- give me all code for this files
use hono js docs to properly setup
Let's Think step by step
```
my-next-hono-app/
├── db/
│   ├── repositories/
│   │   └── post.ts
│   ├── prisma/
│   │   ├── schema.prisma
│   │   └── seed.ts
│   └── client.ts
├── server/            # All server-related logic
│   ├── handlers/
│   │   └── post.ts
│   ├── middleware/
│   │   ├── errorhandler.ts
│   │   └── auth.ts
│   └── index.ts      # Centralized Hono app setup
├── app/
│   ├── api/
│   │   └── [[...route]]/
│   │       └── route.ts # Imports server/index.ts
│   ├── layout.tsx
│   └── page.tsx
├── schemas/
│   └── post.ts
├── types/
│   └── index.ts
├── .env
└── package.json
```
---
```
my-next-hono-app/
├── db/
│   ├── repositories/          # for database operations
│   │   └── post.ts
│   ├── prisma/
│   │   ├── schema.prisma
│   │   └── seed.ts
│   └── client.ts
├── server/                   # All server-related logic
│   ├── handlers/
│   │   └── post.ts
│   ├── middleware/
│   │   ├── errorhandler.ts
│   │   └── auth.ts
│   └── index.ts              # Centralized Hono app setup
├── app/
│   ├── api/                  
│   │   └── [[...route]]/
│   │       └── route.ts      # Imports server/index.ts
│   ├── layout.tsx
│   └── page.tsx
├── services/                 # Server component business logic
│   ├── userService.ts
│   └── postService.ts
├── lib/
│   ├── api/                  # API integration layer
│   │   ├── ApiClient.ts      # API client setup (axios, fetch, etc.)
│   │   ├── endpoints.ts      # API endpoints constants
│   │   ├── requests/         # Organized API calls
│   │   │   └── postApi.ts    # Data fetching API calls
│   │   └── types.ts          # API-specific types
│   └── utils/
│       └── ...
├── schemas/              
│   └── post.ts
├── types/
│   └── index.ts
├── .env
└── package.json

```
