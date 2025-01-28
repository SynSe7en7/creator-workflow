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

### 1.2 GCP Service Configuration
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
[Previous database schema section remains the same as it's Supabase-specific]

## 3. Application Components

### 3.1 Core Components
```typescript
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

interface AIServices {
  gemini: {
    service: 'Gemini API';
    models: {
      generation: 'gemini-1.5-pro';
      vision: 'gemini-pro-vision';
    };
    configuration: {
      temperature: 0.7;
      topK: 40;
      topP: 0.95;
    };
  };
  
  vectorization: {
    service: 'pgvector';
    dimensions: 512;
    indexType: 'ivfflat';
  };
}
```

### 3.2 Infrastructure Components
```typescript
interface InfrastructureComponents {
  networking: {
    loadBalancer: 'Cloud Load Balancing';
    cdn: 'Cloud CDN';
    security: 'Cloud Armor';
  };
  
  monitoring: {
    metrics: 'Cloud Monitoring';
    logging: 'Cloud Logging';
    tracing: 'Cloud Trace';
    alerts: 'Cloud Monitoring Alerts';
  };
  
  storage: {
    assets: 'Cloud Storage';
    cache: 'Cloud Memorystore';
    secrets: 'Secret Manager';
  };
}
```

## 4. API Structure
[Previous API structure section remains the same]

## 5. State Management
[Previous state management section remains the same]

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