# CLAUDE.md - Frontend Application

## 🎯 Objetivo do Projeto

[Descrição do frontend - exemplo abaixo]

Esta é a aplicação frontend de [nome do produto], construída com React/Next.js. Oferece uma interface moderna, responsiva e acessível para os usuários interagirem com nossos serviços. O foco está em performance, usabilidade e uma experiência excepcional em todos os dispositivos.

**Principais características:**

- SPA/SSR com roteamento otimizado
- Design responsivo mobile-first
- PWA com funcionamento offline
- Internacionalização (i18n)
- Acessibilidade WCAG 2.1 AA

## 🏗️ Arquitetura Frontend

### Stack Tecnológica

- **Framework**: React 18.x / Next.js 14.x
- **Language**: TypeScript 5.x
- **Styling**: Tailwind CSS / Styled Components / CSS Modules
- **State Management**: Zustand / Redux Toolkit / TanStack Query
- **Forms**: React Hook Form + Zod
- **Testing**:
  - **Unit/Integration**: Jest + React Testing Library
  - **E2E/Regression**: Playwright
  - **Visual Regression**: Playwright + Screenshots
- **Build Tool**: Vite / Next.js / Webpack
- **Package Manager**: npm / pnpm / yarn
- **Linting**: ESLint + Prettier
- **CI/CD**: GitHub Actions + Vercel/Netlify

### Estrutura de Pastas Detalhada

```
src/
├── app/                    # Next.js 13+ app directory
│   ├── (auth)/            # Auth group routes
│   ├── (dashboard)/       # Dashboard routes
│   ├── api/               # API routes (if using Next.js)
│   └── layout.tsx         # Root layout
├── components/
│   ├── common/            # Shared components
│   │   ├── Button/
│   │   ├── Input/
│   │   └── Modal/
│   ├── features/          # Feature-specific components
│   │   ├── auth/
│   │   ├── products/
│   │   └── checkout/
│   └── layouts/           # Layout components
├── hooks/                 # Custom React hooks
├── lib/                   # Utility libraries
│   ├── api/              # API client
│   ├── auth/             # Auth utilities
│   └── utils/            # Helper functions
├── stores/               # State management
├── styles/               # Global styles
├── types/                # TypeScript types
└── tests/                # Test files
```

### Component Architecture

```
┌─────────────────────────────────────────────┐
│                  App Shell                   │
│  ┌─────────────────────────────────────┐   │
│  │            Navigation                │   │
│  └─────────────────────────────────────┘   │
│  ┌─────────────────────────────────────┐   │
│  │                                     │   │
│  │          Page Component            │   │
│  │    ┌─────────────────────────┐    │   │
│  │    │   Feature Components    │    │   │
│  │    │  ┌───────┐  ┌───────┐  │    │   │
│  │    │  │ Card  │  │ List  │  │    │   │
│  │    │  └───────┘  └───────┘  │    │   │
│  │    └─────────────────────────┘    │   │
│  │                                     │   │
│  └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

## 📋 Padrões e Convenções Frontend

### Component Standards

```typescript
// Component file structure
components/
└── Button/
    ├── Button.tsx          # Component logic
    ├── Button.styles.ts    # Styled components/styles
    ├── Button.types.ts     # TypeScript interfaces
    ├── Button.test.tsx     # Component tests
    ├── Button.stories.tsx  # Storybook stories
    └── index.ts           # Public exports

// Component template
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'danger';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  onClick?: () => void;
  children: React.ReactNode;
}

export const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  size = 'md',
  disabled = false,
  loading = false,
  onClick,
  children
}) => {
  // Component implementation
};
```

### State Management Patterns

```typescript
// Zustand store example
interface UserStore {
  user: User | null;
  isLoading: boolean;
  error: string | null;
  login: (credentials: LoginDTO) => Promise<void>;
  logout: () => void;
  updateProfile: (data: UpdateProfileDTO) => Promise<void>;
}

const useUserStore = create<UserStore>((set) => ({
  user: null,
  isLoading: false,
  error: null,
  
  login: async (credentials) => {
    set({ isLoading: true, error: null });
    try {
      const user = await authApi.login(credentials);
      set({ user, isLoading: false });
    } catch (error) {
      set({ error: error.message, isLoading: false });
    }
  },
  
  logout: () => {
    authApi.logout();
    set({ user: null });
  }
}));
```

### API Integration

```typescript
// API client setup
const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
});

// Request interceptor
apiClient.interceptors.request.use((config) => {
  const token = getAuthToken();
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Response interceptor
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      await refreshToken();
    }
    return Promise.reject(error);
  }
);

// React Query setup
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5 minutes
      cacheTime: 10 * 60 * 1000, // 10 minutes
      retry: 3,
      refetchOnWindowFocus: false
    }
  }
});
```

### Styling Conventions

```typescript
// Tailwind CSS with custom design system
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          900: '#1e3a8a'
        },
        gray: {
          // Custom gray scale
        }
      },
      spacing: {
        '18': '4.5rem',
        '88': '22rem'
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.3s ease-out'
      }
    }
  }
};

// Component styling example
<div className="
  container mx-auto px-4
  md:px-6 lg:px-8
  py-8 md:py-12
  grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3
  gap-4 md:gap-6
">
  {/* Content */}
</div>
```

## 🔧 Comandos Frontend Específicos

### Development

```bash
# Development server
npm run dev                 # Start dev server
npm run dev:https          # Start with HTTPS
npm run dev:mobile         # Start with mobile device access

# Build commands
npm run build              # Production build
npm run build:analyze      # Analyze bundle size
npm run build:profile      # Build with profiling

# Type checking
npm run type-check         # Check TypeScript types
npm run type-check:watch   # Watch mode

# Linting & Formatting
npm run lint              # Lint code
npm run lint:fix          # Fix linting issues
npm run format            # Format with Prettier
npm run format:check      # Check formatting

# Testing
npm run test              # Run all tests
npm run test:unit         # Unit tests only
npm run test:integration  # Integration tests
npm run test:e2e          # Playwright E2E tests
npm run test:e2e:ui       # Playwright with UI mode
npm run test:e2e:debug    # Playwright debug mode
npm run test:regression   # Visual regression tests
npm run test:coverage     # Generate coverage
npm run test:watch        # Watch mode

# Playwright specific
npm run playwright:install # Install browsers
npm run playwright:report  # Show last test report
npm run playwright:codegen # Generate tests

# Storybook
npm run storybook         # Start Storybook
npm run build:storybook   # Build Storybook
```

### Code Generation

```bash
# Generate component
npm run generate:component Button

# Generate page
npm run generate:page products

# Generate hook
npm run generate:hook useAuth

# Generate API client
npm run generate:api-client
```

## ⚠️ Áreas Críticas - Frontend

### Performance Optimization

```typescript
// Critical rendering path
- Lazy load below-the-fold content
- Preload critical fonts
- Inline critical CSS
- Defer non-critical scripts

// Image optimization
import Image from 'next/image';

<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={600}
  priority // LCP image
  placeholder="blur"
  blurDataURL={shimmer}
/>

// Code splitting
const DashboardModule = lazy(() => 
  import(/* webpackChunkName: "dashboard" */ './Dashboard')
);

// Bundle optimization
- Tree shaking enabled
- Minimize JS/CSS
- Compress assets (gzip/brotli)
- Use CDN for static assets
```

### State Management Critical Areas

```typescript
// Authentication state - stores/auth.store.ts
- Token refresh logic
- Session persistence
- Protected route handling
- Permission checks

// Cart state - stores/cart.store.ts
- Local storage sync
- Price calculations
- Stock validation
- Checkout flow

// UI state - stores/ui.store.ts
- Modal management
- Toast notifications
- Loading states
- Error boundaries
```

### Security Considerations

```typescript
// XSS Prevention
// ❌ NEVER: dangerouslySetInnerHTML with user input
<div dangerouslySetInnerHTML={{ __html: userInput }} />

// ✅ ALWAYS: Sanitize if needed
import DOMPurify from 'dompurify';
<div dangerouslySetInnerHTML={{ 
  __html: DOMPurify.sanitize(content) 
}} />

// Content Security Policy
<meta
  http-equiv="Content-Security-Policy"
  content="default-src 'self'; script-src 'self' 'unsafe-inline';"
/>

// Secure storage
- Never store sensitive data in localStorage
- Use httpOnly cookies for auth tokens
- Encrypt sensitive data in sessionStorage
```

## 🎨 Design System

### Component Library

```typescript
// Design tokens
const tokens = {
  colors: {
    primary: '#3B82F6',
    secondary: '#10B981',
    danger: '#EF4444',
    warning: '#F59E0B',
    info: '#3B82F6',
    
    // Semantic colors
    text: {
      primary: '#111827',
      secondary: '#6B7280',
      disabled: '#9CA3AF'
    },
    
    background: {
      primary: '#FFFFFF',
      secondary: '#F9FAFB',
      tertiary: '#F3F4F6'
    }
  },
  
  typography: {
    fontFamily: {
      sans: ['Inter', 'system-ui', 'sans-serif'],
      mono: ['Fira Code', 'monospace']
    },
    
    fontSize: {
      xs: '0.75rem',    // 12px
      sm: '0.875rem',   // 14px
      base: '1rem',     // 16px
      lg: '1.125rem',   // 18px
      xl: '1.25rem',    // 20px
      '2xl': '1.5rem',  // 24px
      '3xl': '1.875rem' // 30px
    }
  },
  
  spacing: {
    xs: '0.5rem',  // 8px
    sm: '1rem',    // 16px
    md: '1.5rem',  // 24px
    lg: '2rem',    // 32px
    xl: '3rem'     // 48px
  },
  
  breakpoints: {
    sm: '640px',
    md: '768px',
    lg: '1024px',
    xl: '1280px',
    '2xl': '1536px'
  }
};
```

### Accessibility Standards

```typescript
// ARIA labels and roles
<button
  aria-label="Close dialog"
  aria-pressed={isPressed}
  role="button"
  tabIndex={0}
>

// Keyboard navigation
const handleKeyDown = (e: KeyboardEvent) => {
  switch(e.key) {
    case 'Enter':
    case ' ':
      handleClick();
      break;
    case 'Escape':
      handleClose();
      break;
  }
};

// Focus management
useEffect(() => {
  if (isOpen) {
    firstFocusableElement?.focus();
  }
  return () => {
    triggerElement?.focus();
  };
}, [isOpen]);

// Screen reader announcements
<div role="status" aria-live="polite" aria-atomic="true">
  {message}
</div>
```

## 🐛 Debugging Frontend

### Browser DevTools

```typescript
// Performance profiling
performance.mark('component-start');
// Component render
performance.mark('component-end');
performance.measure('component-render', 'component-start', 'component-end');

// React DevTools profiler
<Profiler id="Navigation" onRender={onRenderCallback}>
  <Navigation />
</Profiler>

// Console debugging helpers
window.DEBUG = {
  store: useStore.getState(),
  clearCache: () => queryClient.clear(),
  user: () => console.table(useStore.getState().user)
};
```

### Error Boundaries

```typescript
class ErrorBoundary extends Component {
  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    // Log to error reporting service
    errorReportingService.log({
      error,
      errorInfo,
      url: window.location.href,
      userAgent: navigator.userAgent
    });
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback />;
    }
    return this.props.children;
  }
}
```

### Development Tools

```bash
# React DevTools
- Component tree inspection
- Props/State debugging
- Profiler for performance

# Redux DevTools (if using Redux)
- Action history
- State time-travel
- Action dispatching

# Playwright Inspector
- Step-by-step debugging
- Element selectors
- Network inspection

# Network debugging
- Check API calls
- Inspect request/response
- Mock API responses
```

### Playwright Debugging

```typescript
// Debug specific test
npx playwright test checkout.spec.ts --debug

// Use Inspector
await page.pause(); // Pauses execution

// Slow down execution
const browser = await chromium.launch({
  slowMo: 1000 // 1 second between actions
});

// Save trace for debugging
const context = await browser.newContext({
  recordVideo: { dir: 'videos/' },
  recordHar: { path: 'network.har' }
});

// Debug selectors
await page.locator('button').highlight();
const count = await page.locator('.item').count();
console.log(`Found ${count} items`);

// Wait strategies
await page.waitForLoadState('networkidle');
await page.waitForSelector('.dynamic-content');
await page.waitForFunction(() => document.ready);
```

### Visual Regression Debugging

```bash
# Update baseline screenshots
npm run test:regression -- --update-snapshots

# Compare specific component
npm run test:regression -- --grep "button"

# Generate diff report
npm run test:regression -- --reporter=html
```

## 🚀 Build & Deploy

### Build Optimization

```javascript
// next.config.js
module.exports = {
  swcMinify: true,
  compiler: {
    removeConsole: process.env.NODE_ENV === 'production'
  },
  images: {
    domains: ['cdn.example.com'],
    formats: ['image/avif', 'image/webp']
  },
  experimental: {
    optimizeCss: true
  }
};

// Webpack optimization
optimization: {
  splitChunks: {
    chunks: 'all',
    cacheGroups: {
      vendor: {
        test: /[\\/]node_modules[\\/]/,
        name: 'vendors',
        priority: 10
      },
      common: {
        minChunks: 2,
        priority: 5,
        reuseExistingChunk: true
      }
    }
  }
}
```

### Environment Configuration

```bash
# .env.local
NEXT_PUBLIC_API_URL=http://localhost:3001
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_GA_ID=UA-XXXXXXXX

# .env.production
NEXT_PUBLIC_API_URL=https://api.example.com
NEXT_PUBLIC_APP_URL=https://app.example.com
NEXT_PUBLIC_GA_ID=UA-YYYYYYYY
NEXT_PUBLIC_SENTRY_DSN=https://xxx@sentry.io/yyy
```

### Deployment Checklist

```markdown
Pre-deployment:
- [ ] Build passes locally
- [ ] All tests passing (unit, integration, E2E)
- [ ] Visual regression tests passing
- [ ] Lighthouse score > 90
- [ ] Bundle size within budget
- [ ] No console errors
- [ ] Environment variables set
- [ ] SEO meta tags updated
- [ ] Sitemap generated
- [ ] Robots.txt configured
- [ ] Accessibility audit passed

Post-deployment:
- [ ] Smoke tests pass
- [ ] Run Playwright E2E in production
- [ ] Visual regression against production
- [ ] Analytics working
- [ ] Error tracking active
- [ ] Performance monitoring
- [ ] SSL certificate valid
```

### Automated Testing in CI/CD

```yaml
# Example GitHub Actions workflow
name: Frontend Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node
        uses: actions/setup-node@v3
        
      - name: Install dependencies
        run: npm ci
        
      - name: Run unit tests
        run: npm run test:unit
        
      - name: Run integration tests
        run: npm run test:integration
        
      - name: Install Playwright
        run: npx playwright install --with-deps
        
      - name: Run E2E tests
        run: npm run test:e2e
        
      - name: Run visual regression
        run: npm run test:regression
        
      - name: Upload test artifacts
        if: failure()
        uses: actions/upload-artifact@v3
        with:
          name: test-results
          path: |
            playwright-report/
            test-results/
            e2e/screenshots/
```

## 📊 Performance Metrics

### Core Web Vitals

```yaml
Targets:
  - LCP (Largest Contentful Paint): < 2.5s
  - FID (First Input Delay): < 100ms
  - CLS (Cumulative Layout Shift): < 0.1
  - FCP (First Contentful Paint): < 1.8s
  - TTFB (Time to First Byte): < 600ms

Monitoring:
  - Real User Monitoring (RUM)
  - Synthetic monitoring
  - Lighthouse CI in pipeline
```

### Bundle Size Budget

```json
{
  "bundles": [
    {
      "name": "main",
      "maxSize": "200KB"
    },
    {
      "name": "vendor",
      "maxSize": "300KB"
    }
  ],
  "total": {
    "maxSize": "600KB",
    "gzip": true
  }
}
```

## 🧪 Testing Strategy

### Testing Pyramid

```
         /\
        /e2e\     (10%) - Critical user journeys (Playwright)
       /------\
      / regr. \   (15%) - Visual regression (Playwright)
     /----------\
    /  integr.  \ (25%) - Component integration
   /--------------\
  /     unit      \ (50%) - Component logic, hooks, utils
 /------------------\
```

### Test Examples

#### Unit Test (Jest + RTL)

```typescript
// Button.test.tsx
describe('Button', () => {
  it('renders with correct text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

#### Integration Test (Jest + RTL)

```typescript
// LoginForm.test.tsx
describe('LoginForm', () => {
  it('submits with valid credentials', async () => {
    const { user } = render(<LoginForm />);
    
    await user.type(screen.getByLabelText('Email'), 'test@example.com');
    await user.type(screen.getByLabelText('Password'), 'password123');
    await user.click(screen.getByRole('button', { name: 'Login' }));
    
    expect(await screen.findByText('Welcome back!')).toBeInTheDocument();
  });
});
```

#### E2E Test (Playwright)

```typescript
// e2e/checkout.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Checkout Flow', () => {
  test('completes purchase successfully', async ({ page }) => {
    // Navigate to products
    await page.goto('/products');
    
    // Add item to cart
    await page.getByRole('button', { name: 'Add to Cart' }).first().click();
    
    // Go to cart
    await page.getByRole('link', { name: 'Cart' }).click();
    
    // Proceed to checkout
    await page.getByRole('button', { name: 'Checkout' }).click();
    
    // Fill checkout form
    await page.getByLabel('Email').fill('test@example.com');
    await page.getByLabel('Card Number').fill('4242 4242 4242 4242');
    await page.getByLabel('Expiry').fill('12/25');
    await page.getByLabel('CVC').fill('123');
    
    // Complete order
    await page.getByRole('button', { name: 'Place Order' }).click();
    
    // Verify success
    await expect(page.getByText('Order Confirmed')).toBeVisible();
    await expect(page).toHaveURL(/\/order-confirmation/);
  });
});
```

#### Visual Regression Test (Playwright)

```typescript
// e2e/visual-regression.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Visual Regression', () => {
  test('homepage appearance', async ({ page }) => {
    await page.goto('/');
    await page.waitForLoadState('networkidle');
    
    // Take screenshot for comparison
    await expect(page).toHaveScreenshot('homepage.png', {
      fullPage: true,
      animations: 'disabled',
      mask: [page.locator('[data-testid="dynamic-content"]')]
    });
  });

  test('component states', async ({ page }) => {
    await page.goto('/components');
    
    // Default state
    await expect(page.getByTestId('button-primary')).toHaveScreenshot('button-default.png');
    
    // Hover state
    await page.getByTestId('button-primary').hover();
    await expect(page.getByTestId('button-primary')).toHaveScreenshot('button-hover.png');
    
    // Disabled state
    await page.getByTestId('button-disabled').scrollIntoViewIfNeeded();
    await expect(page.getByTestId('button-disabled')).toHaveScreenshot('button-disabled.png');
  });

  test('responsive layouts', async ({ page }) => {
    const viewports = [
      { name: 'mobile', width: 375, height: 667 },
      { name: 'tablet', width: 768, height: 1024 },
      { name: 'desktop', width: 1920, height: 1080 }
    ];

    for (const viewport of viewports) {
      await page.setViewportSize(viewport);
      await page.goto('/');
      await expect(page).toHaveScreenshot(`homepage-${viewport.name}.png`);
    }
  });
});
```

### Playwright Configuration

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [
    ['html'],
    ['json', { outputFile: 'test-results/results.json' }],
    ['junit', { outputFile: 'test-results/junit.xml' }]
  ],
  
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },

  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 5'] },
    },
    {
      name: 'Mobile Safari',
      use: { ...devices['iPhone 12'] },
    },
  ],

  webServer: {
    command: 'npm run dev',
    port: 3000,
    reuseExistingServer: !process.env.CI,
  },
});
```

### Visual Regression Setup

```typescript
// playwright-visual.config.ts
export default defineConfig({
  ...baseConfig,
  
  use: {
    ...baseConfig.use,
    // Visual regression specific settings
    screenshot: {
      mode: 'only-on-failure',
      fullPage: true,
    },
  },

  expect: {
    // Threshold for pixel differences
    toHaveScreenshot: { 
      threshold: 0.2,
      maxDiffPixels: 100,
      animations: 'disabled',
    },
  },

  // Store screenshots
  snapshotDir: './e2e/screenshots',
  snapshotPathTemplate: '{snapshotDir}/{testFileDir}/{testFileName}-{arg}{ext}',
});
```

### CI/CD Integration

```yaml
# .github/workflows/playwright.yml
name: Playwright Tests
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    timeout-minutes: 60
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          
      - name: Install dependencies
        run: npm ci
        
      - name: Install Playwright Browsers
        run: npx playwright install --with-deps
        
      - name: Run Playwright tests
        run: npm run test:e2e
        
      - name: Run Visual Regression tests
        run: npm run test:regression
        
      - uses: actions/upload-artifact@v3
        if: always()
        with:
          name: playwright-report
          path: playwright-report/
          retention-days: 30
          
      - uses: actions/upload-artifact@v3
        if: failure()
        with:
          name: playwright-screenshots
          path: e2e/screenshots/
          retention-days: 30
```

## 💡 Best Practices Frontend

### Component Design

```typescript
// ✅ GOOD: Composition over configuration
<Card>
  <Card.Header>
    <Card.Title>Title</Card.Title>
  </Card.Header>
  <Card.Body>Content</Card.Body>
</Card>

// ❌ AVOID: Prop drilling
<Card 
  title="Title"
  subtitle="Subtitle"
  body="Content"
  footer="Footer"
  // ... many more props
/>
```

### Performance

```typescript
// ✅ GOOD: Memoization when needed
const ExpensiveComponent = memo(({ data }) => {
  const processedData = useMemo(() => 
    heavyProcessing(data), [data]
  );
  
  return <div>{processedData}</div>;
});

// ✅ GOOD: Debounced search
const SearchInput = () => {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 300);
  
  useEffect(() => {
    if (debouncedQuery) {
      searchAPI(debouncedQuery);
    }
  }, [debouncedQuery]);
};
```

### Accessibility

```typescript
// ✅ GOOD: Semantic HTML
<nav aria-label="Main navigation">
  <ul>
    <li><a href="/home">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>

// ✅ GOOD: Form accessibility
<form>
  <label htmlFor="email">
    Email
    <span aria-label="required">*</span>
  </label>
  <input
    id="email"
    type="email"
    required
    aria-describedby="email-error"
  />
  <span id="email-error" role="alert">
    {errors.email}
  </span>
</form>
```

---

**Última atualização**: [DATA]
**Versão**: 1.0.0
