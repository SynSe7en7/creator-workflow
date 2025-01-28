# System Architecture - AI Creator Workflow Platform

## 1. Technology Stack

### 1.1 Core Technologies
```typescript
interface TechnologyStack {
  frontend: {
    framework: 'Next.js 14';
    language: 'TypeScript';
    styling: 'Tailwind CSS';
    stateManagement: {
      clientState: 'Jotai';
      serverState: 'TanStack Query';
    };
    workflowEngine: 'ReactFlow';
    editor: 'TipTap';
    components: 'shadcn/ui';
  };
  
  infrastructure: {
    compute: 'Cloud Run';
    storage: 'Cloud Storage';
    cdn: 'Cloud CDN';
    networking: 'Cloud Load Balancing';
    monitoring: 'Cloud Monitoring';
    logging: 'Cloud Logging';
  };
  
  database: {
    primary: 'Supabase (PostgreSQL)';
    vectorStore: 'pgvector';
    cache: 'Cloud Memorystore';
  };
  
  ai: {
    contentGeneration: 'Gemini 1.5 Pro';
    vectorization: 'pgvector';
    development: 'Claude 3.5 Sonnet';
  };
  
  security: {
    secretsManagement: 'Secret Manager';
    authentication: 'Supabase Auth';
    networkSecurity: 'Cloud Armor';
  };
}
```

### 1.2 State Management Architecture
```typescript
interface StateManagement {
  atoms: {
    user: {
      atom: 'userAtom';
      storage: true;
      type: 'User | null';
    };
    workflow: {
      atom: 'workflowAtom';
      storage: false;
      type: 'WorkflowState';
    };
    ui: {
      atom: 'uiAtom';
      storage: true;
      type: 'UIState';
    };
  };
  
  queries: {
    workflow: {
      key: ['workflow', 'id'];
      staleTime: 60000;
      cacheTime: 300000;
    };
    research: {
      key: ['research', 'query'];
      staleTime: 30000;
      cacheTime: 300000;
    };
  };
}
```

### 1.3 GCP Service Configuration
```typescript
interface GCPConfiguration {
  cloudRun: {
    region: 'us-central1';
    scaling: {
      minInstances: 1;
      maxInstances: 10;
      concurrency: 80;
    };
    memory: '1Gi';
    cpu: '1';
  };
  
  cloudStorage: {
    buckets: {
      assets: 'creator-workflow-assets';
      backups: 'creator-workflow-backups';
      temp: 'creator-workflow-temp';
    };
    lifecycle: {
      tempRetention: '7d';
      backupRetention: '30d';
    };
  };
  
  memorystore: {
    tier: 'basic';
    size: '1GB';
    version: 'Redis 6.x';
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
  type TEXT NOT NULL,
  content TEXT NOT NULL,
  metadata JSONB DEFAULT '{}'::jsonb,
  embedding vector(512),
  created_at TIMESTAMPTZ DEFAULT NOW()
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
interface ComponentArchitecture {
  providers: {
    jotai: 'JotaiProvider';
    query: 'QueryClientProvider';
    supabase: 'SupabaseProvider';
  };
  
  state: {
    atoms: Record<string, Atom<any>>;
    queries: Record<string, QueryConfig>;
    persistence: AtomStorage;
  };
  
  hooks: {
    useWorkflow: WorkflowHook;
    useResearch: ResearchHook;
    useContent: ContentHook;
  };
}

interface ServiceArchitecture {
  frontend: {
    service: 'Cloud Run';
    cdn: 'Cloud CDN';
    assets: 'Cloud Storage';
  };
  
  api: {
    service: 'Cloud Run';
    endpoints: {
      workflow: WorkflowAPI;
      content: ContentAPI;
      research: ResearchAPI;
    };
  };
  
  backgroundJobs: {
    service: 'Cloud Tasks';
    queues: {
      research: ResearchQueue;
      generation: GenerationQueue;
      publishing: PublishingQueue;
    };
  };
}
```

### 3.2 State Management Implementation
```typescript
interface StateImplementation {
  atoms: {
    user: atom<User | null>(null);
    workflow: atom<WorkflowState>({
      nodes: [],
      edges: [],
      selected: null
    });
    ui: atom<UIState>({
      theme: 'light',
      sidebarOpen: true
    });
  };
  
  queries: {
    useWorkflow: UseQueryResult<Workflow>;
    useResearch: UseQueryResult<ResearchData>;
    useContent: UseQueryResult<Content>;
  };
}
```

## 4. API Structure

### 4.1 REST Endpoints
```typescript
interface APIEndpoints {
  workflow: {
    list: 'GET /api/workflows';
    create: 'POST /api/workflows';
    get: 'GET /api/workflows/:id';
    update: 'PUT /api/workflows/:id';
    delete: 'DELETE /api/workflows/:id';
  };
  
  content: {
    create: 'POST /api/content';
    get: 'GET /api/content/:id';
    update: 'PUT /api/content/:id';
    delete: 'DELETE /api/content/:id';
  };
  
  research: {
    search: 'POST /api/research/search';
    extract: 'POST /api/research/extract';
    store: 'POST /api/research/store';
  };
}

## 6. Security Implementation

### 6.1 GCP Security Configuration
```typescript
interface SecurityConfiguration {
  cloudArmor: {
    rules: {
      ddosProtection: true;
      rateLimit: {
        rate: '100/min';
        banDuration: '5m';
      };
      allowedRegions: string[];
    };
  };
  
  secretManager: {
    secrets: {
      apiKeys: Secret;
      databaseCredentials: Secret;
      serviceAccounts: Secret;
    };
    rotation: {
      enabled: true;
      period: '30d';
    };
  };
  
  iap: {
    enabled: true;
    oauthConfig: OAuthConfig;
  };
}
```

## 7. Deployment Architecture

### 7.1 GCP Deployment
```typescript
interface DeploymentArchitecture {
  environments: {
    development: {
      project: 'creator-workflow-dev';
      region: 'us-central1';
    };
    staging: {
      project: 'creator-workflow-staging';
      region: 'us-central1';
    };
    production: {
      project: 'creator-workflow-prod';
      region: 'us-central1';
    };
  };
  
  services: {
    frontend: {
      service: 'Cloud Run';
      domain: string;
      ssl: true;
    };
    api: {
      service: 'Cloud Run';
      domain: string;
      ssl: true;
    };
  };
  
  networking: {
    vpc: 'creator-workflow-vpc';
    subnets: {
      primary: string;
      secondary: string;
    };
    connectors: {
      serverless: 'vpc-access-connector';
      memorystore: 'redis-connector';
    };
  };
}
```

### 7.2 CI/CD Pipeline
```typescript
interface CICDPipeline {
  provider: 'Cloud Build';
  triggers: {
    main: BuildTrigger;
    staging: BuildTrigger;
    production: BuildTrigger;
  };
  
  steps: {
    test: TestStep;
    build: BuildStep;
    deploy: DeployStep;
    notify: NotificationStep;
  };
  
  artifacts: {
    storage: 'Cloud Storage';
    registry: 'Artifact Registry';
  };
}
```

## 8. Monitoring & Logging

### 8.1 Monitoring Configuration
```typescript
interface MonitoringConfiguration {
  metrics: {
    service: 'Cloud Monitoring';
    custom: {
      workflowExecution: Metric;
      aiLatency: Metric;
      userActions: Metric;
    };
  };
  
  logging: {
    service: 'Cloud Logging';
    sinks: {
      application: LogSink;
      security: LogSink;
      audit: LogSink;
    };
  };
  
  alerting: {
    service: 'Cloud Monitoring';
    policies: {
      error: AlertPolicy;
      performance: AlertPolicy;
      security: AlertPolicy;
    };
    notifications: {
      email: boolean;
      slack: boolean;
      pagerduty: boolean;
    };
  };
}
```
