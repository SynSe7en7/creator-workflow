# AI Development Rules & Context
## Creator Workflow Platform

You are assisting in the development of an AI-powered content creation workflow platform. The platform helps technical thought leaders maintain consistent brand voice while efficiently creating and distributing content across platforms. Your role is to assist in writing clean, performant code while maintaining our established architecture and best practices.

## Project Context

### Core Purpose
- Visual workflow automation for content creation
- AI-powered content generation and research
- Brand voice consistency maintenance
- Multi-platform content distribution
- Technical thought leadership development

### Primary User
- Technical founder/CTO building in public
- Focus on AI, Product Development, Entrepreneurship
- Goal: 50K followers in 12 months
- Platforms: LinkedIn, Twitter
- Content Types: Technical articles, threads, documentation

## Technology Requirements

### Core Stack
Always use these specific technologies and their latest STABLE versions:
```typescript
const requiredTech = {
  framework: 'Next.js 14 (App Router)',
  language: 'TypeScript 5.x',
  styling: 'Tailwind CSS',
  components: 'shadcn/ui',
  stateManagement: 'Zustand',
  workflow: 'ReactFlow',
  editor: 'TipTap',
  database: 'Supabase + pgvector',
  ai: 'Gemini 1.5 Pro',
  infrastructure: 'GCP (Cloud Run)'
};
```

### Package Rules
1. Always check for latest STABLE versions
2. Prefer official packages over community alternatives
3. Verify TypeScript support
4. Check bundle size impact
5. Verify license compatibility
6. Confirm GCP/Cloud Run compatibility

## Development Rules

### Code Standards
1. TypeScript:
```typescript
// Always use strict type checking
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true
  }
}
```

2. Performance:
- Implement React Server Components where appropriate
- Use proper code splitting
- Implement proper caching strategies
- Optimize asset loading
- Use proper Suspense boundaries

3. State Management:
```typescript
// Use Zustand for global state
interface Store {
  // Always type your store
  state: State;
  actions: Actions;
}

// Implement proper selectors
const useStore = create<Store>((set) => ({
  // Always implement atomic updates
  // Always consider persistence needs
}));
```

4. Components:
```typescript
// Always use TypeScript for components
interface ComponentProps {
  // Explicitly type all props
}

// Use functional components with proper typing
const Component: React.FC<ComponentProps> = ({ prop }) => {
  // Implement proper error boundaries
  // Use proper loading states
  // Consider accessibility
};
```

### Best Practices

1. Architecture:
- Follow established folder structure
- Maintain separation of concerns
- Implement proper error boundaries
- Use proper loading states
- Follow accessibility guidelines

2. Performance:
- Implement proper memoization
- Use lightweight alternatives when possible
- Optimize bundle size
- Implement proper caching
- Monitor performance metrics

3. Security:
- Follow GCP security best practices
- Implement proper authentication
- Use Secret Manager for sensitive data
- Implement proper CORS policies
- Follow least privilege principle

## AI Integration Rules

### Gemini Integration
```typescript
// Always use the official Google AI SDK
import { GoogleGenerativeAI } from '@google/generative-ai';

// Implement proper error handling
try {
  const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
  const model = genAI.getGenerativeModel({ model: 'gemini-1.5-pro' });
} catch (error) {
  // Proper error handling
}
```

### Vector Operations
```typescript
// Always use pgvector with proper indexing
const vectorQuery = `
  SELECT *
  FROM content
  WHERE embedding <-> $1 < $2
  ORDER BY embedding <-> $1
  LIMIT $3
`;
```

## Testing Requirements

1. Implementation:
- Write unit tests for all functions
- Implement integration tests
- Use proper mocking strategies
- Test error scenarios
- Verify type safety

2. Coverage:
```typescript
// Maintain minimum coverage requirements
const coverageThresholds = {
  statements: 80,
  branches: 70,
  functions: 80,
  lines: 80
};
```

## Documentation Requirements

1. Code Documentation:
- Document all functions
- Explain complex logic
- Document type definitions
- Include usage examples
- Document edge cases

2. Comments:
```typescript
// Use JSDoc for function documentation
/**
 * Function description
 * @param {Type} param - Parameter description
 * @returns {Type} Return value description
 * @throws {Error} Error description
 */
```

## Error Handling

1. Implementation:
```typescript
// Always implement proper error handling
try {
  // Operation
} catch (error) {
  // Proper error typing
  if (error instanceof SpecificError) {
    // Specific handling
  }
  // Proper error logging
  // User-friendly error messages
}
```

2. Error Types:
- Define proper error types
- Implement error boundaries
- Handle async errors
- Provide user feedback
- Log errors appropriately

## Performance Rules

1. Optimization:
- Use React Server Components
- Implement proper caching
- Optimize asset loading
- Minimize bundle size
- Use proper lazy loading

2. Monitoring:
- Implement performance metrics
- Monitor API response times
- Track resource usage
- Monitor error rates
- Track user metrics

## Final Notes

Always:
1. Follow established architecture
2. Maintain type safety
3. Consider performance implications
4. Implement proper error handling
5. Write comprehensive tests
6. Document your code
7. Consider security implications
8. Follow accessibility guidelines

Never:
1. Bypass type checking
2. Ignore error handling
3. Skip documentation
4. Ignore performance implications
5. Bypass security measures
6. Violate architectural patterns
7. Use deprecated methods
8. Ignore accessibility