# PERFORMANCE OPTIMIZATION

Otimize sistematicamente a performance do código em $ARGUMENTS, com medições precisas e melhorias mensuráveis.

## FASE 1: PROFILING E BASELINE

### 1.1 Setup de Profiling

```bash
# Node.js profiling
node --prof app.js
node --prof-process isolate-*.log > profile.txt

# Browser profiling
performance.mark('myFunction-start');
// código
performance.mark('myFunction-end');
performance.measure('myFunction', 'myFunction-start', 'myFunction-end');

# Python profiling
python -m cProfile -o profile.stats app.py

# Ruby profiling
ruby-prof app.rb

# Java profiling
java -Xprof App

# PHP profiling
php -d xdebug.profiler_enable=1 app.php

# Go profiling
go test -cpuprofile=cpu.prof
go tool pprof cpu.prof
```

### 1.2 Métricas Baseline

```markdown
## Medições Iniciais - [Data/Hora]

### Performance Metrics
| Métrica | Valor | Target | Gap |
|---------|-------|--------|-----|
| Response Time (p50) | [X]ms | [Y]ms | [Z]% |
| Response Time (p95) | [X]ms | [Y]ms | [Z]% |
| Response Time (p99) | [X]ms | [Y]ms | [Z]% |
| Throughput | [X] req/s | [Y] req/s | [Z]% |
| CPU Usage | [X]% | <[Y]% | [Z]% |
| Memory Usage | [X]MB | <[Y]MB | [Z]MB |
| DB Query Time | [X]ms | <[Y]ms | [Z]ms |

### Identificação de Bottlenecks
1. **[Component/Function]**: [X]ms ([Y]% do total)
2. **[Component/Function]**: [X]ms ([Y]% do total)
3. **[Component/Function]**: [X]ms ([Y]% do total)
```

### 1.3 Ferramentas de Análise

```javascript
// Instrumentação customizada
class PerformanceMonitor {
  constructor() {
    this.metrics = new Map();
  }

  start(label) {
    this.metrics.set(label, {
      start: performance.now(),
      memory: process.memoryUsage()
    });
  }

  end(label) {
    const start = this.metrics.get(label);
    if (!start) return;

    const duration = performance.now() - start.start;
    const memoryDelta = process.memoryUsage().heapUsed - start.memory.heapUsed;

    console.log(`${label}: ${duration.toFixed(2)}ms, Memory: ${(memoryDelta / 1024 / 1024).toFixed(2)}MB`);
    return { duration, memoryDelta };
  }
}

const perf = new PerformanceMonitor();
```

## FASE 2: ANÁLISE DETALHADA

### 2.1 Análise de Complexidade Algorítmica

```markdown
## Big-O Analysis

### Algoritmos Identificados
| Função | Complexidade Atual | Complexidade Ideal | Impacto |
|--------|-------------------|-------------------|---------|
| [função1] | O(n²) | O(n log n) | [X]ms saved |
| [função2] | O(n³) | O(n²) | [Y]ms saved |
| [função3] | O(2ⁿ) | O(n²) | [Z]ms saved |

### Exemplos de Otimização
```javascript
// ❌ ANTES: O(n²)
function findDuplicates(arr) {
  const duplicates = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j] && !duplicates.includes(arr[i])) {
        duplicates.push(arr[i]);
      }
    }
  }
  return duplicates;
}

// ✅ DEPOIS: O(n)
function findDuplicates(arr) {
  const seen = new Set();
  const duplicates = new Set();
  
  for (const item of arr) {
    if (seen.has(item)) {
      duplicates.add(item);
    }
    seen.add(item);
  }
  
  return Array.from(duplicates);
}
```

### 2.2 Database Query Analysis

```sql
-- Query Plan Analysis
EXPLAIN ANALYZE
SELECT [columns]
FROM [table]
WHERE [conditions];

-- Index Usage
SELECT 
  schemaname,
  tablename,
  indexname,
  idx_scan,
  idx_tup_read,
  idx_tup_fetch
FROM pg_stat_user_indexes
ORDER BY idx_scan;
```

#### Problemas Comuns Identificados

- [ ] **N+1 Queries**

  ```javascript
  // ❌ ANTES: N+1 queries
  const users = await User.findAll();
  for (const user of users) {
    user.posts = await Post.findAll({ where: { userId: user.id } });
  }

  // ✅ DEPOIS: 1 query
  const users = await User.findAll({
    include: [{
      model: Post,
      as: 'posts'
    }]
  });
  ```

- [ ] **Missing Indexes**

  ```sql
  -- Criar índices para colunas frequentemente filtradas
  CREATE INDEX idx_users_email ON users(email);
  CREATE INDEX idx_posts_user_id_created_at ON posts(user_id, created_at DESC);
  ```

- [ ] **Unnecessary Data Fetching**

  ```javascript
  // ❌ ANTES: Buscando todos os campos
  const users = await User.findAll();

  // ✅ DEPOIS: Apenas campos necessários
  const users = await User.findAll({
    attributes: ['id', 'name', 'email']
  });
  ```

### 2.3 Memory Analysis

```javascript
// Detectar memory leaks
const heapSnapshot1 = process.memoryUsage();

// ... executar operação suspeita várias vezes ...

const heapSnapshot2 = process.memoryUsage();
const delta = {
  rss: heapSnapshot2.rss - heapSnapshot1.rss,
  heapTotal: heapSnapshot2.heapTotal - heapSnapshot1.heapTotal,
  heapUsed: heapSnapshot2.heapUsed - heapSnapshot1.heapUsed,
  external: heapSnapshot2.external - heapSnapshot1.external
};

console.log('Memory Delta:', delta);
```

#### Problemas de Memória Comuns

- [ ] **Event Listeners não removidos**
- [ ] **Closures retendo referências**
- [ ] **Caches sem limite**
- [ ] **Buffers grandes não liberados**

## FASE 3: OTIMIZAÇÕES IMPLEMENTADAS

### 3.1 Otimizações de Algoritmo

```markdown
## Otimização 1: [Nome]

### Problema
[Descrição do bottleneck]

### Solução
```javascript
// Implementação otimizada
[código]
```

### Resultados

- Tempo de execução: [antes]ms → [depois]ms (-[X]%)
- Uso de memória: [antes]MB → [depois]MB (-[Y]%)
- Complexidade: O([antes]) → O([depois])

```

### 3.2 Otimizações de I/O
```javascript
// Batching de operações
// ❌ ANTES: Múltiplas operações síncronas
for (const item of items) {
  await processItem(item);
}

// ✅ DEPOIS: Processamento em lote
const BATCH_SIZE = 100;
for (let i = 0; i < items.length; i += BATCH_SIZE) {
  const batch = items.slice(i, i + BATCH_SIZE);
  await Promise.all(batch.map(item => processItem(item)));
}

// Caching estratégico
const cache = new Map();
const CACHE_TTL = 60 * 60 * 1000; // 1 hora

async function getCachedData(key) {
  const cached = cache.get(key);
  if (cached && Date.now() - cached.timestamp < CACHE_TTL) {
    return cached.data;
  }

  const data = await fetchData(key);
  cache.set(key, { data, timestamp: Date.now() });
  return data;
}
```

### 3.3 Otimizações de Renderização (Frontend)

```javascript
// React optimizations
// ❌ ANTES: Re-render desnecessário
const ExpensiveComponent = ({ data, onUpdate }) => {
  return <div>{/* complex render */}</div>;
};

// ✅ DEPOIS: Memorização
const ExpensiveComponent = React.memo(({ data, onUpdate }) => {
  return <div>{/* complex render */}</div>;
}, (prevProps, nextProps) => {
  return prevProps.data.id === nextProps.data.id;
});

// Virtual scrolling para listas grandes
import { FixedSizeList } from 'react-window';

const BigList = ({ items }) => (
  <FixedSizeList
    height={600}
    itemCount={items.length}
    itemSize={50}
    width="100%"
  >
    {({ index, style }) => (
      <div style={style}>
        {items[index].name}
      </div>
    )}
  </FixedSizeList>
);
```

### 3.4 Otimizações de Build

```javascript
// Webpack optimizations
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10
        }
      }
    },
    minimizer: [
      new TerserPlugin({
        parallel: true,
        terserOptions: {
          compress: {
            drop_console: true
          }
        }
      })
    ]
  }
};

// Lazy loading
const HeavyComponent = lazy(() => import('./HeavyComponent'));
```

## FASE 4: VALIDAÇÃO E MÉTRICAS

### 4.1 Testes de Performance

```javascript
// Performance regression tests
describe('Performance Tests', () => {
  it('should process 10k items in under 100ms', async () => {
    const items = generateItems(10000);
    const start = performance.now();
    
    await processItems(items);
    
    const duration = performance.now() - start;
    expect(duration).toBeLessThan(100);
  });

  it('should not exceed memory limit', () => {
    const initialMemory = process.memoryUsage().heapUsed;
    
    // Process large dataset
    processLargeDataset();
    
    const finalMemory = process.memoryUsage().heapUsed;
    const memoryIncrease = (finalMemory - initialMemory) / 1024 / 1024;
    
    expect(memoryIncrease).toBeLessThan(50); // Max 50MB increase
  });
});
```

### 4.2 Métricas Pós-Otimização

```markdown
## Resultados Finais - [Data/Hora]

### Comparação Before/After
| Métrica | Antes | Depois | Melhoria |
|---------|-------|--------|----------|
| Response Time (p50) | [X]ms | [Y]ms | -[Z]% ✅ |
| Response Time (p95) | [X]ms | [Y]ms | -[Z]% ✅ |
| Response Time (p99) | [X]ms | [Y]ms | -[Z]% ✅ |
| Throughput | [X] req/s | [Y] req/s | +[Z]% ✅ |
| CPU Usage | [X]% | [Y]% | -[Z]% ✅ |
| Memory Usage | [X]MB | [Y]MB | -[Z]MB ✅ |

### Gráficos de Performance
```

Response Time (ms)
│
│     Before                After
│   ┌─────────┐          ┌─────────┐
│   │         │          │         │
│   │   250   │          │   95    │
│   │         │          │         │
│   └─────────┘          └─────────┘
└──────────────────────────────────────
        p50                  p50

Throughput (req/s)
│
│                          ┌─────────┐
│                          │   450   │
│   ┌─────────┐           │         │
│   │   180   │           └─────────┘
│   │         │              After
│   └─────────┘
│     Before
└──────────────────────────────────────

```

### Load Test Results
```bash
# Before optimization
wrk -t12 -c400 -d30s http://localhost:3000/api/endpoint
  Latency    250.43ms   89.12ms   1.23s
  Req/Sec    15.23      8.94      25.00

# After optimization  
wrk -t12 -c400 -d30s http://localhost:3000/api/endpoint
  Latency     95.17ms   42.38ms   432.91ms
  Req/Sec    37.82     12.47      50.00
```

```

## FASE 5: DOCUMENTAÇÃO E MONITORAMENTO

### 5.1 Documentação das Mudanças
```markdown
## Otimizações Implementadas

### 1. Algorithm Optimization
**Arquivo**: `src/services/DataProcessor.js`
**Mudança**: Substituído nested loops por hash maps
**Impacto**: -75% no tempo de processamento
**Trade-off**: +10MB uso de memória (aceitável)

### 2. Database Optimization
**Queries Otimizadas**: 5
**Índices Criados**: 3
**Impacto**: -60% no tempo médio de query
**Trade-off**: +50MB storage (índices)

### 3. Caching Implementation
**Tipo**: Redis com TTL adaptativo
**Hit Rate**: 85%
**Impacto**: -40% load no database
**Trade-off**: Complexidade adicional

### 4. Frontend Bundle Size
**Antes**: 2.3MB
**Depois**: 780KB
**Técnicas**: Code splitting, tree shaking, lazy loading
**Impacto**: -66% initial load time
```

### 5.2 Configurações de Produção

```javascript
// Performance-oriented production config
module.exports = {
  // Node.js optimizations
  nodeOptions: {
    '--max-old-space-size': 4096,
    '--optimize-for-size': true,
    '--gc-interval': 100
  },

  // Database connection pool
  database: {
    pool: {
      min: 5,
      max: 20,
      acquireTimeout: 30000,
      idleTimeout: 10000
    }
  },

  // Cache configuration
  cache: {
    defaultTTL: 3600,
    maxSize: '500mb',
    compression: true
  },

  // Rate limiting
  rateLimit: {
    windowMs: 15 * 60 * 1000,
    max: 100,
    standardHeaders: true,
    legacyHeaders: false
  }
};
```

### 5.3 Monitoramento Contínuo

```javascript
// APM setup (Application Performance Monitoring)
const apm = require('elastic-apm-node').start({
  serviceName: 'my-app',
  secretToken: process.env.APM_TOKEN,
  serverUrl: process.env.APM_SERVER_URL,
  captureBody: 'all',
  transactionSampleRate: 0.1
});

// Custom metrics
const prometheus = require('prom-client');

const httpDuration = new prometheus.Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status_code'],
  buckets: [0.1, 0.5, 1, 2, 5]
});

// Middleware
app.use((req, res, next) => {
  const start = Date.now();
  
  res.on('finish', () => {
    const duration = (Date.now() - start) / 1000;
    httpDuration
      .labels(req.method, req.route?.path || 'unknown', res.statusCode)
      .observe(duration);
  });
  
  next();
});
```

### 5.4 Alertas de Performance

```yaml
# Prometheus alert rules
groups:
  - name: performance
    rules:
      - alert: HighResponseTime
        expr: http_request_duration_seconds{quantile="0.95"} > 0.5
        for: 5m
        annotations:
          summary: "High response time detected"
          
      - alert: HighMemoryUsage
        expr: process_resident_memory_bytes / 1024 / 1024 > 500
        for: 5m
        annotations:
          summary: "Process using >500MB memory"
          
      - alert: HighCPUUsage
        expr: rate(process_cpu_seconds_total[5m]) > 0.8
        for: 5m
        annotations:
          summary: "CPU usage >80%"
```

## FASE 6: MANUTENÇÃO E EVOLUÇÃO

### 6.1 Performance Budget

```json
{
  "performance-budget": {
    "timings": {
      "firstContentfulPaint": 1000,
      "firstMeaningfulPaint": 1500,
      "firstCPUIdle": 3000,
      "timeToInteractive": 3500
    },
    "sizes": {
      "bundle": 300000,
      "images": 500000,
      "fonts": 100000,
      "scripts": 200000,
      "styles": 50000
    },
    "requests": {
      "total": 50,
      "js": 10,
      "css": 5,
      "images": 20
    }
  }
}
```

### 6.2 Regression Prevention

```javascript
// Performance regression test in CI/CD
// .github/workflows/performance.yml
name: Performance Tests
on: [pull_request]

jobs:
  performance:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Run performance tests
        run: |
          npm run test:performance
          npm run lighthouse:ci
          
      - name: Check bundle size
        run: |
          npm run build
          npm run size-limit
          
      - name: Comment PR
        uses: actions/github-script@v6
        with:
          script: |
            const results = require('./performance-results.json');
            const comment = `## Performance Impact
            
            | Metric | Before | After | Change |
            |--------|--------|-------|--------|
            | Bundle Size | ${results.before.bundleSize} | ${results.after.bundleSize} | ${results.change.bundleSize} |
            | Load Time | ${results.before.loadTime} | ${results.after.loadTime} | ${results.change.loadTime} |
            `;
            
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: comment
            });
```

### 6.3 Checklist de Manutenção

```markdown
## Monthly Performance Review

### Métricas para Revisar
- [ ] Response time trends
- [ ] Error rate changes
- [ ] Database slow query log
- [ ] Cache hit rates
- [ ] Memory usage patterns
- [ ] Bundle size evolution

### Ações de Manutenção
- [ ] Atualizar dependências (verificar impact)
- [ ] Revisar e otimizar queries lentas
- [ ] Limpar caches obsoletos
- [ ] Avaliar novas oportunidades de otimização
- [ ] Atualizar performance budgets

### Red Flags
- 🚨 Response time aumentou >10%
- 🚨 Memory usage crescendo consistentemente
- 🚨 Cache hit rate caiu <70%
- 🚨 Bundle size aumentou >5%
```

## CONCLUSÃO E PRÓXIMOS PASSOS

### Resumo das Melhorias

```markdown
## Performance Optimization Summary

### Conquistas Principais
✅ Response time reduzido em 62%
✅ Throughput aumentado em 150%
✅ Uso de memória reduzido em 35%
✅ Bundle size reduzido em 66%
✅ Database queries 60% mais rápidas

### ROI Estimado
- Redução de custos de infraestrutura: 40%
- Melhoria na satisfação do usuário: +15 NPS
- Redução no bounce rate: -25%
- Aumento em conversão: +8%

### Próximas Oportunidades
1. Implementar server-side rendering para critical path
2. Migrar para HTTP/3 para menor latência
3. Implementar edge computing para usuários globais
4. Avaliar WebAssembly para processamento pesado
5. Considerar micro-frontends para loading paralelo
```

### Documentação de Referência

```markdown
## Links e Recursos

### Ferramentas Utilizadas
- [Lighthouse CI](https://github.com/GoogleChrome/lighthouse-ci)
- [Clinic.js](https://clinicjs.org/)
- [0x flame graphs](https://github.com/davidmarkclements/0x)
- [Artillery.io](https://artillery.io/)

### Leitura Recomendada
- "High Performance Browser Networking" - Ilya Grigorik
- "Web Performance in Practice" - Jeremy Wagner
- "Database Performance at Scale" - Baron Schwartz

### Dashboards
- [Production Metrics](link-to-grafana)
- [APM Dashboard](link-to-apm)
- [Real User Monitoring](link-to-rum)
```

## CRITÉRIO DE SUCESSO FINAL

A otimização é considerada completa quando:

- [ ] Todas as métricas target foram atingidas
- [ ] Testes de regressão estão passando
- [ ] Documentação está completa
- [ ] Monitoramento está configurado
- [ ] Time está treinado nas mudanças
- [ ] Performance budget está definido
- [ ] CI/CD inclui checks de performance

## Adaptações por Linguagem

### JavaScript/TypeScript
- **Test Runner**: Jest, Vitest, Mocha
- **Linter**: ESLint, Biome
- **Build**: Webpack, Vite, esbuild
- **Package Manager**: npm, yarn, pnpm, bun

### PHP
- **Test Runner**: PHPUnit, Pest
- **Linter**: PHPStan, Psalm, PHPCS
- **Build**: Composer scripts
- **Package Manager**: Composer

### Java
- **Test Runner**: JUnit, TestNG
- **Linter**: SpotBugs, PMD, Checkstyle
- **Build**: Maven, Gradle
- **Package Manager**: Maven, Gradle

### Python
- **Test Runner**: pytest, unittest
- **Linter**: pylint, flake8, mypy, ruff
- **Build**: setuptools, poetry build
- **Package Manager**: pip, poetry, pipenv

### Go
- **Test Runner**: go test, testify
- **Linter**: golangci-lint, go vet
- **Build**: go build
- **Package Manager**: go mod

### Ruby
- **Test Runner**: RSpec, Minitest
- **Linter**: RuboCop, Reek
- **Build**: rake build
- **Package Manager**: Bundler

### C#/.NET
- **Test Runner**: xUnit, NUnit, MSTest
- **Linter**: Roslyn Analyzers
- **Build**: dotnet build
- **Package Manager**: NuGet

### Rust
- **Test Runner**: cargo test
- **Linter**: clippy, rustfmt
- **Build**: cargo build
- **Package Manager**: Cargo
