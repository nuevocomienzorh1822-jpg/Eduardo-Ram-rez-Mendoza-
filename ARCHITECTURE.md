# AppForge AI - Architecture Document

## System Overview

AppForge AI is a multi-layered system designed to generate full-stack applications from natural language descriptions.

## Layer 1: User Interface (React + Vite)

### Pages

1. **Dashboard**
   - Recent projects
   - Create new project button
   - Project status overview
   - Activity feed
   - Error logs
   - Deployment history

2. **New Project**
   - Natural language input form
   - Requirements analysis
   - Editable build plan

3. **Project View**
   - Construction chat
   - Build plan with progress
   - Code editor with file explorer
   - Live preview panel
   - Database admin
   - AI agents management
   - Backend functions
   - Integrations
   - GitHub integration
   - Deployment flow

### Components

- Sidebar navigation
- Header with user menu
- Chat interface
- Code editor
- Preview panel
- Progress tracker
- Entity builder
- Deployment status

## Layer 2: Backend API (Node.js)

### Core Services

- **ProjectService**: CRUD operations for projects
- **BuildService**: Orchestrate build processes
- **DatabaseService**: Manage database schema
- **CodeGeneratorService**: Generate code from specs
- **AuthService**: User authentication and authorization
- **DeploymentService**: Handle deployments

### API Endpoints

```
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout

GET    /api/projects
POST   /api/projects
GET    /api/projects/:id
PUT    /api/projects/:id
DELETE /api/projects/:id

GET    /api/projects/:id/files
GET    /api/projects/:id/files/:path
POST   /api/projects/:id/files
PUT    /api/projects/:id/files/:path
DELETE /api/projects/:id/files/:path

GET    /api/projects/:id/build
POST   /api/projects/:id/build
GET    /api/projects/:id/build/logs

GET    /api/projects/:id/entities
POST   /api/projects/:id/entities
PUT    /api/projects/:id/entities/:entityId
DELETE /api/projects/:id/entities/:entityId

GET    /api/projects/:id/agents
POST   /api/projects/:id/agents
PUT    /api/projects/:id/agents/:agentId
DELETE /api/projects/:id/agents/:agentId

GET    /api/projects/:id/deploy
POST   /api/projects/:id/deploy
GET    /api/projects/:id/deploy/logs
```

## Layer 3: AI Agent System

### Orchestrator

Coordinates all agent activities. Routes requests to appropriate agents.

### Specialized Agents

1. **RequirementsAgent**
   - Parses natural language input
   - Identifies missing requirements
   - Creates specification document

2. **ArchitectAgent**
   - Designs system architecture
   - Defines data flow
   - Plans component structure

3. **DatabaseAgent**
   - Creates entity definitions
   - Defines relationships
   - Generates schema
   - Sets up permissions

4. **BackendAgent**
   - Generates API endpoints
   - Creates services
   - Implements business logic
   - Handles error cases

5. **FrontendAgent**
   - Generates React components
   - Creates pages
   - Implements styling
   - Handles state management

6. **AIAgentAgent**
   - Creates custom AI agents
   - Configures tools
   - Sets permissions

7. **TestAgent**
   - Generates unit tests
   - Creates integration tests
   - Runs test suites
   - Reports coverage

8. **DebugAgent**
   - Analyzes errors
   - Suggests fixes
   - Applies corrections
   - Re-runs tests

9. **DeployAgent**
   - Validates code
   - Builds project
   - Deploys to server
   - Verifies deployment

## Layer 4: Data Layer

### Entities

```typescript
User {
  id: UUID
  email: string
  password: string (hashed)
  name: string
  avatar?: string
  createdAt: timestamp
  updatedAt: timestamp
}

Project {
  id: UUID
  userId: UUID
  name: string
  description: string
  status: 'draft' | 'building' | 'ready' | 'deployed'
  specifications: JSON
  architecture: JSON
  buildPlan: JSON[]
  buildProgress: number
  createdAt: timestamp
  updatedAt: timestamp
}

ProjectFile {
  id: UUID
  projectId: UUID
  path: string
  content: text
  language: string
  createdAt: timestamp
  updatedAt: timestamp
}

Conversation {
  id: UUID
  projectId: UUID
  createdAt: timestamp
}

Message {
  id: UUID
  conversationId: UUID
  role: 'user' | 'assistant'
  content: text
  metadata: JSON?
  createdAt: timestamp
}

Build {
  id: UUID
  projectId: UUID
  status: 'pending' | 'in-progress' | 'success' | 'failed'
  startedAt: timestamp
  completedAt?: timestamp
  error?: text
}

BuildTask {
  id: UUID
  buildId: UUID
  name: string
  status: 'pending' | 'in-progress' | 'success' | 'failed'
  order: number
  startedAt?: timestamp
  completedAt?: timestamp
  output?: text
  error?: text
}

Agent {
  id: UUID
  projectId: UUID
  name: string
  instructions: text
  model: string
  tools: JSON
  permissions: JSON
  context: JSON?
  createdAt: timestamp
}

AgentExecution {
  id: UUID
  agentId: UUID
  input: text
  output: text
  status: 'pending' | 'running' | 'success' | 'failed'
  startedAt: timestamp
  completedAt?: timestamp
  error?: text
}

Integration {
  id: UUID
  projectId: UUID
  type: string (e.g., 'github', 'stripe', 'sendgrid')
  config: JSON
  active: boolean
  createdAt: timestamp
}

Deployment {
  id: UUID
  projectId: UUID
  status: 'validating' | 'testing' | 'building' | 'deploying' | 'verifying' | 'success' | 'failed'
  version: string
  url?: string
  startedAt: timestamp
  completedAt?: timestamp
  error?: text
}

DeploymentLog {
  id: UUID
  deploymentId: UUID
  level: 'info' | 'warning' | 'error'
  message: text
  createdAt: timestamp
}

ErrorLog {
  id: UUID
  projectId: UUID
  level: 'warning' | 'error' | 'critical'
  message: text
  stack?: text
  context: JSON?
  resolvedAt?: timestamp
  createdAt: timestamp
}

GitRepository {
  id: UUID
  projectId: UUID
  owner: string
  repo: string
  branch: string
  token: string (encrypted)
  lastSyncedAt?: timestamp
  createdAt: timestamp
}
```

## Security Model

- Authentication: JWT tokens
- Authorization: Role-based access control (RBAC)
- Data: Encrypted at rest and in transit
- Agent Permissions: Least privilege principle
- Audit: All operations logged

## Deployment Strategy

1. **Development**: Local Docker containers
2. **Staging**: Cloud staging environment
3. **Production**: Auto-scaling cloud infrastructure
4. **Monitoring**: Real-time logs and alerts

## Scalability

- Microservices-ready architecture
- Stateless API design
- Database connection pooling
- Caching layer (Redis)
- Message queue for async tasks (Bull)
