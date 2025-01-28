# Instructions for AI Creator Workflow Platform

## Overview

This document combines system architecture, technical requirements, and product requirements for building an AI-powered creator workflow platform. The platform will be built on GCP infrastructure with Supabase for database management.

## Table of Contents
1. Product Vision & Requirements
2. System Architecture
3. Technical Implementation
4. Development Timeline
5. Infrastructure Setup
6. Security & Monitoring

## 1. Product Vision & Requirements

### 1.1 Vision Statement
Create a visual workflow automation platform leveraging GCP and AI services to help technical thought leaders maintain consistent brand voice while efficiently creating and distributing content across platforms.

### 1.2 Target User Profile
Primary User (Initial Phase):
- Technical founder/CTO
- Building in public
- Focus: AI, Product Development, Entrepreneurship
- Goal: 50K followers in 12 months
- Platforms: LinkedIn, Twitter

### 1.3 Success Metrics
```typescript
interface SuccessMetrics {
  growth: {
    followers: '50K in 12 months';
    engagement: '> 5%';
    contentOutput: '5-7 pieces/week';
  };
  
  platform: {
    uptime: '99.95%';
    responseTime: '< 500ms';
    errorRate: '< 1%';
    userRetention: '> 90%';
  };
  
  ai: {
    geminiLatency: '< 3s';
    voiceAccuracy: '> 95%';
    contentQuality: '> 90%';
  };
}
```

## 2. System Architecture

### 2.1 Technology Stack
```typescript
interface TechnologyStack {
  frontend: {
    framework: 'Next.js 14';
    language: 'TypeScript';
    styling: 'Tailwind CSS';
    stateManagement: 'Jotai';
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
}
```

### 2.2 Database Schema
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

## 3. Technical Implementation

### 3.1 Development Environment Setup
```typescript
interface DevEnvironment {
  tools: {
    ide: 'Cursor';
    aiAssistant: 'Claude 3.5 Sonnet';
    cloudTools: 'gcloud CLI';
  };
  
  configuration: {
    gcloudCli: {
      install: true;
      configure: true;
      auth: true;
    };
    supabase: {
      cli: true;
      types: true;
    };
  };
}
```

### 3.2 GCP Configuration
```typescript
interface GCPConfiguration {
  cloudRun: {
    services: {
      frontend: {
        name: 'creator-workflow-frontend';
        port: 3000;
        env: EnvConfig;
      };
      api: {
        name: 'creator-workflow-api';
        port: 8080;
        env: EnvConfig;
      };
    };
    
    configuration: {
      memory: '1Gi';
      cpu: '1';
      concurrency: 80;
      timeout: '300s';
    };
  };
}
```

## 4. Development Timeline

### 4.1: Foundation
- GCP project setup
- Supabase database setup
- Basic workflow canvas
- Authentication system
- Basic storage implementation

### 4.2: Core Features
- Gemini API integration
- Vector storage implementation
- Content editor integration
- Research system setup
- Basic analytics

### 4.3: Advanced Features
- Workflow automation
- Performance optimization
- Advanced analytics
- Platform integrations
- Testing & refinement

## 5. Core Features Implementation

### 5.1 Workflow System
```typescript
interface WorkflowSystem {
  canvas: {
    engine: 'ReactFlow';
    features: {
      dragAndDrop: true;
      realTimePreview: true;
      customNodes: true;
    };
    storage: {
      service: 'Cloud Storage';
      backup: true;
      versioning: true;
    };
  };
  
  execution: {
    service: 'Cloud Run';
    features: {
      parallelProcessing: true;
      errorHandling: true;
      monitoring: true;
    };
  };
}
```

### 5.2 AI Integration
```typescript
interface AISystem {
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

## 6. Security & Monitoring

### 6.1 Security Implementation
```typescript
interface SecurityConfig {
  cloudArmor: {
    policies: {
      ddos: PolicyConfig;
      rateLimit: PolicyConfig;
      geoRestriction: PolicyConfig;
    };
  };
  
  secretManager: {
    secrets: {
      apiKeys: SecretConfig;
      dbCreds: SecretConfig;
      tokens: SecretConfig;
    };
  };
}
```

### 6.2 Monitoring Setup
```typescript
interface MonitoringSetup {
  metrics: {
    custom: CustomMetric[];
    alerts: AlertConfig[];
    dashboards: DashboardConfig[];
  };
  
  logging: {
    sinks: {
      application: SinkConfig;
      security: SinkConfig;
      audit: SinkConfig;
    };
  };
}
```

## 7. Performance Requirements

### 7.1 System Performance
```typescript
interface PerformanceRequirements {
  cloudRun: {
    responseTime: '< 500ms';
    concurrency: 80;
    availability: '99.95%';
  };
  
  storage: {
    uploadSpeed: '> 10MB/s';
    downloadSpeed: '> 50MB/s';
    availability: '99.9%';
  };
  
  ai: {
    responseTime: '< 3s';
    streamingLatency: '< 500ms';
    accuracy: '> 95%';
  };
}
```

### 7.2 User Experience
```typescript
interface UXRequirements {
  interaction: {
    pageLoad: '< 2s';
    workflowResponse: '< 1s';
    editorLatency: '< 100ms';
  };
  
  reliability: {
    errorRate: '< 1%';
    dataLoss: '0%';
    uptime: '99.9%';
  };
}
```
