# System Architecture - AI Creator Workflow Platform

## 1. Technology Stack

### 1.1 Core Technologies
```typescript
interface TechnologyStack {
  frontend: {
    framework: 'Next.js 14';
    language: 'TypeScript';
    styling: 'Tailwind CSS';
    stateManagement: 'Zustand';
    workflowEngine: 'ReactFlow';
    editor: 'TipTap';
    components: 'shadcn/ui';
  };
  
  backend: {
    database: 'Supabase (PostgreSQL)';
    vectorStore: 'pgvector';
    auth: 'Supabase Auth';
    storage: 'Supabase Storage';
    caching: 'Redis';
    deployment: 'GCP Cloud Run';
  };
  
  ai: {
    contentGeneration: 'Gemini 1.5 Pro';
    vectorization: 'pgvector';
    development: 'Claude 3.5 Sonnet';
  };
}
```

## 2. Database Schema

### 2.1 Core Tables
```sql
-- Enable Extensions
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "vector";

-- Users
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email TEXT UNIQUE NOT NULL,
  metadata JSONB DEFAULT '{}'::jsonb,
  settings JSONB DEFAULT '{}'::jsonb,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Workflows
CREATE TABLE workflows (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  description TEXT,
  nodes JSONB NOT NULL,
  edges JSONB NOT NULL,
  metadata JSONB DEFAULT '{}'::jsonb,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Content
CREATE TABLE content (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  workflow_id UUID REFERENCES workflows(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  type TEXT NOT NULL,
  content TEXT NOT NULL,
  metadata JSONB DEFAULT '{}'::jsonb,
  embedding vector(512),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Research Sources
CREATE TABLE research_sources (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  url TEXT,
  title TEXT,
  content TEXT,
  metadata JSONB DEFAULT '{}'::jsonb,
  embedding vector(512),
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

## 3. Application Components

### 3.1 Core Components
```typescript
// Workflow Engine
interface WorkflowEngine {
  canvas: {
    nodes: Node[];
    edges: Edge[];
    onNodesChange: (changes: NodeChange[]) => void;
    onEdgesChange: (changes: EdgeChange[]) => void;
    onConnect: (params: Connection) => void;
  };
  
  execution: {
    executeNode: (nodeId: string) => Promise<void>;
    executeWorkflow: (workflowId: string) => Promise<void>;
    status: ExecutionStatus;
    history: ExecutionHistory[];
  };
}

// Content Management
interface ContentSystem {
  editor: {
    content: string;
    type: ContentType;
    onChange: (content: string) => void;
    onSave: () => Promise<void>;
    format: FormatOptions;
  };
  
  generator: {
    generateContent: (prompt: string, context: Context) => Promise<string>;
    analyzeContent: (content: string) => Promise<Analysis>;
    improveContent: (content: string, goals: Goals) => Promise<string>;
  };
}

// Research System
interface ResearchSystem {
  scraper: {
    scrapeUrl: (url: string) => Promise<ScrapedContent>;
    extractContent: (html: string) => Promise<ParsedContent>;
    validateSource: (url: string) => Promise<ValidationResult>;
  };
  
  vectorStorage: {
    storeVector: (content: string, metadata: any) => Promise<string>;
    searchSimilar: (query: string, limit?: number) => Promise<SearchResult[]>;
    updateVector: (id: string, content: string) => Promise<void>;
  };
}
```

### 3.2 Component Dependencies
```typescript
interface ComponentDependencies {
  workflow: {
    requires: ['ReactFlow', 'Zustand', 'AI Service'];
    optional: ['Analytics'];
  };
  
  content: {
    requires: ['TipTap', 'AI Service', 'Vector Storage'];
    optional: ['Templates'];
  };
  
  research: {
    requires: ['Vector Storage', 'AI Service'];
    optional: ['Cache'];
  };
}
```

## 4. API Structure

### 4.1 REST Endpoints
```typescript
interface APIRoutes {
  workflows: {
    GET: '/api/workflows' // List workflows
    POST: '/api/workflows' // Create workflow
    GET: '/api/workflows/:id' // Get workflow
    PUT: '/api/workflows/:id' // Update workflow
    DELETE: '/api/workflows/:id' // Delete workflow
  };
  
  content: {
    GET: '/api/content' // List content
    POST: '/api/content' // Create content
    GET: '/api/content/:id' // Get content
    PUT: '/api/content/:id' // Update content
    DELETE: '/api/content/:id' // Delete content
  };
  
  research: {
    POST: '/api/research/scrape' // Scrape URL
    POST: '/api/research/search' // Vector search
    GET: '/api/research/sources' // List sources
  };
}
```

### 4.2 API Types
```typescript
interface APITypes {
  request: {
    headers: {
      authorization: string;
      'content-type': 'application/json';
    };
    body: unknown;
  };
  
  response: {
    status: number;
    data: unknown;
    error?: APIError;
  };
}
```

## 5. State Management

### 5.1 Global State
```typescript
interface GlobalState {
  user: User | null;
  currentWorkflow: Workflow | null;
  settings: AppSettings;
  ui: UIState;
}

interface UIState {
  sidebarOpen: boolean;
  currentView: ViewType;
  modal: ModalState | null;
  notifications: Notification[];
}
```

### 5.2 Local State
```typescript
interface LocalState {
  workflowCanvas: {
    selected: Selection;
    zoom: number;
    position: Position;
  };
  
  editor: {
    content: string;
    selection: Range | null;
    history: HistoryState;
  };
}
```

## 6. Security Implementation

### 6.1 Authentication
```typescript
interface AuthSystem {
  methods: {
    signIn: (email: string, password: string) => Promise<Session>;
    signUp: (email: string, password: string) => Promise<User>;
    signOut: () => Promise<void>;
  };
  
  session: {
    getSession: () => Promise<Session | null>;
    refreshSession: () => Promise<Session>;
    onAuthStateChange: (callback: AuthChangeCallback) => Unsubscribe;
  };
}
```

### 6.2 Authorization
```typescript
interface AuthorizationSystem {
  roles: ['user', 'admin'];
  
  permissions: {
    workflows: ['create', 'read', 'update', 'delete'];
    content: ['create', 'read', 'update', 'delete'];
    research: ['create', 'read'];
  };
  
  checks: {
    canAccess: (resource: string, action: string) => boolean;
    canModify: (resource: string, userId: string) => boolean;
  };
}
```

## 7. Error Handling

### 7.1 Error Types
```typescript
interface ErrorSystem {
  types: {
    APIError: typeof Error;
    ValidationError: typeof Error;
    AuthError: typeof Error;
    WorkflowError: typeof Error;
  };
  
  handling: {
    captureError: (error: Error) => void;
    logError: (error: Error) => void;
    displayError: (error: Error) => void;
  };
}
```

### 7.2 Recovery Strategies
```typescript
interface RecoveryStrategies {
  workflow: {
    saveState: () => Promise<void>;
    restoreState: () => Promise<void>;
    rollback: (steps: number) => Promise<void>;
  };
  
  content: {
    autoSave: () => Promise<void>;
    recoverDraft: () => Promise<string>;
    validateContent: () => Promise<ValidationResult>;
  };
}
```