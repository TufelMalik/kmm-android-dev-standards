# Web Development Best Practices

> **Based on Official Guidelines & Industry Standards:**  
> - [PHP-FIG Standards (PSR-1, PSR-12)](https://www.php-fig.org/)
> - [OWASP Security Guidelines](https://owasp.org/)
> - [W3C Web Standards](https://www.w3.org/standards/)
> - [MDN Web Docs Best Practices](https://developer.mozilla.org/en-US/docs/Web)
> - [Twelve-Factor App Methodology](https://12factor.net/)
> - [REST API Design Guidelines](https://restfulapi.net/)

This rule defines comprehensive best practices for web development across PHP and other web technologies, following senior developer standards.

---

## Core Mandatory Rules for Senior Engineers

> **CRITICAL**: These are not "nice-to-have" — they are **mandatory** if you want clean, stable, production-grade web development. Every serious senior engineer and every team that cares about scale, reliability, and maintainability follows these rules.

### 1. Architecture Rules

**1.1 Keep the codebase modular**
- Each module does **one job**
- No "God files", no 2,000-line controllers
- If a file becomes messy, you **split it**

**1.2 Separate layers clearly**
- Frontend, backend, database, and business logic must **not blur together**
- ❌ No SQL in your UI layer
- ❌ No UI logic in your API controllers
- ✅ Clear separation: Presentation → Controller → Service → Repository → Database

**1.3 Consistent project structure**
- Every feature should follow the **same folder pattern**
- Any dev should be able to navigate blindfolded
- Document your structure and enforce it

---

### 2. Code Quality Rules

**2.1 DRY: Don't Repeat Yourself**
- Repeated logic = **guaranteed bugs**
- Extract common code into reusable functions/classes

**2.2 Use type systems where possible**
- TypeScript for web frontends
- DTOs on backend
- Strict schemas everywhere (Zod, Yup, JSON Schema)

**2.3 Write predictable, readable code**
- **Readable > clever**
- If someone can't understand your code in 10 seconds, it's bad
- Self-documenting code with clear naming

---

### 3. API Design Rules

**3.1 REST must follow standards**
Use proper HTTP verbs:
- `GET` = fetch data (idempotent, no side effects)
- `POST` = create new resource
- `PUT` = update entire resource (idempotent)
- `PATCH` = partial update
- `DELETE` = remove resource

**3.2 Version your APIs**
- **Never break old clients**
- Use versioning: `/api/v1/`, `/api/v2/`
- Deprecate gradually with clear warnings

**3.3 Validate every input**
- **Never trust client values**
- Validate server-side **100%** of the time
- Use validation libraries, not manual checks

---

### 4. Security Rules

**4.1 Never store plain passwords**
- Use `bcrypt` or `argon2`
- ❌ Never use MD5, SHA1, or plain text

**4.2 Always use HTTPS**
- **No exceptions** in production
- Force HTTPS redirects
- Use HSTS headers

**4.3 Sanitize all inputs**
- Prevent SQL injection → Use prepared statements
- Prevent XSS → Escape outputs
- Prevent CSRF → Use tokens

**4.4 Use environment variables**
- ❌ Never hardcode secrets
- ✅ Use `.env` files (never commit to git)
- Use secret management tools in production

---

### 5. Performance Rules

**5.1 Cache aggressively**
- **CDN → Browser → Server → Database**
- Senior devs treat caching as a **first-class feature**
- Cache invalidation strategy is critical

**5.2 Reduce asset size**
- Compress images (WebP, AVIF)
- Minify CSS/JS
- Use lazy loading
- Implement code splitting

**5.3 Avoid N+1 database queries**
- Use **joins** or **eager loading**
- Monitor slow queries
- Add proper indexes

---

### 6. Frontend Rules

**6.1 Use a design system**
- Buttons, inputs, colors — **everything must be consistent**
- Build a component library
- Enforce design tokens

**6.2 Make it responsive from day one**
- ❌ Don't try to "fix mobile later"
- Mobile-first approach
- Test on real devices

**6.3 Follow accessibility standards**
- **ARIA** labels
- Keyboard navigation
- Contrast ratios (WCAG 2.1 AA minimum)
- Screen reader compatibility

---

### 7. Git & Workflow Rules

**7.1 Small PRs (Pull Requests)**
- Huge commits slow reviews and cause bugs
- Keep PRs under 400 lines when possible
- One feature/fix per PR

**7.2 Use proper branches**
- `main` → production-ready code
- `dev` → integration branch
- `feature/*` → new features
- `bugfix/*` → bug fixes
- `hotfix/*` → urgent production fixes

**7.3 Every PR must pass**
- ✅ Linting
- ✅ Tests
- ✅ Build
- ✅ Code review (minimum 1 approval)

---

### 8. Testing Rules

**8.1 Unit test core logic**
- Edge cases **must be covered**
- Aim for >80% coverage on critical paths
- Fast, isolated, deterministic tests

**8.2 Integration tests for APIs**
- Test real database interactions
- Mock external services only
- Verify request/response contracts

**8.3 End-to-end tests for flows**
- Users don't care about your units
- They care if the **flow works**
- Test critical user journeys

---

### 9. Deployment Rules

**9.1 Everything is automated**
- **CI/CD must build, test, lint, and deploy**
- Zero manual steps
- Reproducible builds

**9.2 No manual server changes**
- Infrastructure as code (Terraform, Pulumi, CloudFormation)
- All config in version control
- Immutable infrastructure

**9.3 Rollback-friendly**
- Every release must be **reversible in seconds**
- Blue-green deployments
- Feature flags for gradual rollouts

---

### 10. Monitoring Rules

**10.1 Use logs, don't guess**
- **Structured logs** with correlation IDs
- Centralized logging (ELK, CloudWatch, Datadog)
- Log levels used correctly

**10.2 Track performance and errors**
- Error tracking (Sentry, Rollbar, Bugsnag)
- APM tools (Datadog, New Relic)
- Real User Monitoring (RUM)

**10.3 Add health checks**
- Every service must tell you if it's **alive**
- `/health` and `/ready` endpoints
- Monitor dependencies (DB, cache, external APIs)

---

## Detailed Implementation Guide

The following sections provide detailed implementation guidance for each of these mandatory rules.

---

## 1. Code Organization & Architecture

### 1.1 MVC/MVVM Pattern
**Always** use a clear separation of concerns:
- **Models**: Data layer, database interactions, business logic
- **Views**: Presentation layer, HTML/templates only
- **Controllers**: Request handling, routing logic
- **Services**: Reusable business logic

**Example Structure:**
```
project/
├── app/
│   ├── Controllers/
│   ├── Models/
│   ├── Services/
│   ├── Middleware/
│   └── Validators/
├── config/
├── public/
│   ├── css/
│   ├── js/
│   └── assets/
├── resources/
│   └── views/
├── routes/
├── storage/
│   ├── logs/
│   └── cache/
├── tests/
└── vendor/
```

### 1.2 Dependency Injection
Use dependency injection for better testability and maintainability:

```php
// ❌ Bad: Hard-coded dependencies
class UserController {
    public function index() {
        $db = new Database();
        return $db->getUsers();
    }
}

// ✅ Good: Dependency injection
class UserController {
    private UserRepository $userRepository;
    
    public function __construct(UserRepository $userRepository) {
        $this->userRepository = $userRepository;
    }
    
    public function index() {
        return $this->userRepository->getAll();
    }
}
```

---

## 2. Security Best Practices (OWASP)

### 2.1 SQL Injection Prevention
**ALWAYS** use prepared statements, **NEVER** concatenate SQL queries:

```php
// ❌ CRITICAL: Vulnerable to SQL injection
$userId = $_GET['id'];
$query = "SELECT * FROM users WHERE id = $userId";

// ✅ CORRECT: Using prepared statements
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = :id");
$stmt->execute(['id' => $userId]);
```

### 2.2 XSS (Cross-Site Scripting) Prevention
**ALWAYS** escape output, **NEVER** trust user input:

```php
// ❌ Vulnerable to XSS
echo "<div>" . $_POST['username'] . "</div>";

// ✅ Escaped output
echo "<div>" . htmlspecialchars($_POST['username'], ENT_QUOTES, 'UTF-8') . "</div>";
```

### 2.3 CSRF Protection
**ALWAYS** implement CSRF tokens for state-changing operations:

```php
// Generate token
$_SESSION['csrf_token'] = bin2hex(random_bytes(32));

// Validate token
if (!hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'])) {
    die('CSRF token validation failed');
}
```

### 2.4 Password Security
**ALWAYS** use modern hashing algorithms:

```php
// ❌ Never use MD5 or SHA1
$password = md5($userPassword);

// ✅ Use password_hash and password_verify
$hashedPassword = password_hash($userPassword, PASSWORD_ARGON2ID);
$isValid = password_verify($userPassword, $hashedPassword);
```

### 2.5 Input Validation & Sanitization
**ALWAYS** validate and sanitize all inputs:

```php
// Validation
$email = filter_var($_POST['email'], FILTER_VALIDATE_EMAIL);
if ($email === false) {
    throw new ValidationException('Invalid email');
}

// Sanitization
$username = filter_var($_POST['username'], FILTER_SANITIZE_STRING);
```

---

## 3. Database Best Practices

### 3.1 Use ORM or Query Builder
Prefer ORM (Eloquent, Doctrine) or query builders over raw SQL:

```php
// ✅ Using Eloquent ORM
$users = User::where('status', 'active')
    ->orderBy('created_at', 'desc')
    ->limit(10)
    ->get();

// ✅ Using PDO Query Builder
$users = $db->table('users')
    ->where('status', 'active')
    ->orderBy('created_at', 'DESC')
    ->limit(10)
    ->get();
```

### 3.2 Database Transactions
Use transactions for complex operations:

```php
try {
    $db->beginTransaction();
    
    $order = Order::create($orderData);
    $order->items()->createMany($items);
    $payment = Payment::process($paymentData);
    
    $db->commit();
} catch (Exception $e) {
    $db->rollBack();
    throw $e;
}
```

### 3.3 Index Optimization
**ALWAYS** add indexes on frequently queried columns:

```sql
-- Add indexes for performance
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_products_category_status ON products(category_id, status);
```

### 3.4 N+1 Query Prevention
Use eager loading to prevent N+1 queries:

```php
// ❌ N+1 problem
$users = User::all();
foreach ($users as $user) {
    echo $user->profile->bio; // Triggers a query for each user
}

// ✅ Eager loading
$users = User::with('profile')->get();
foreach ($users as $user) {
    echo $user->profile->bio; // No additional queries
}
```

---

## 4. Naming Conventions

### 4.1 PHP Naming Conventions (PSR-1, PSR-12)

#### Classes & Interfaces
```php
// ✅ PascalCase
class UserController {}
interface PaymentGatewayInterface {}
abstract class BaseModel {}
```

#### Methods & Functions
```php
// ✅ camelCase
public function getUserById($id) {}
private function validateEmailAddress($email) {}
```

#### Variables
```php
// ✅ camelCase
$userId = 123;
$isAuthenticated = true;
$userProfile = [];
```

#### Constants
```php
// ✅ UPPER_SNAKE_CASE
const MAX_LOGIN_ATTEMPTS = 5;
const API_BASE_URL = 'https://api.example.com';
```

### 4.2 Database Naming Conventions

#### Table Names
```sql
-- ✅ Plural, snake_case
users
product_categories
order_items
```

#### Column Names
```sql
-- ✅ Singular, snake_case
id
user_id
created_at
email_verified_at
```

#### Foreign Keys
```sql
-- ✅ Format: {referenced_table_singular}_id
user_id (references users.id)
product_id (references products.id)
```

### 4.3 File Naming

```
UserController.php          // ✅ PascalCase for classes
user-profile.blade.php      // ✅ kebab-case for views
userHelpers.php            // ✅ camelCase for utility files
config.php                 // ✅ lowercase for config files
```

---

## 5. Error Handling & Logging

### 5.1 Exception Handling
Use try-catch blocks and custom exceptions:

```php
// Custom exceptions
class ValidationException extends Exception {}
class DatabaseException extends Exception {}

// Proper error handling
try {
    $user = $this->userRepository->find($id);
    if (!$user) {
        throw new NotFoundException("User not found");
    }
    return $user;
} catch (NotFoundException $e) {
    $logger->warning("User not found: " . $id);
    return response()->json(['error' => $e->getMessage()], 404);
} catch (Exception $e) {
    $logger->error("Unexpected error: " . $e->getMessage());
    return response()->json(['error' => 'Internal server error'], 500);
}
```

### 5.2 Logging Standards
Use proper log levels and structured logging:

```php
// PSR-3 Log Levels
$logger->emergency("System is unusable");
$logger->alert("Immediate action required");
$logger->critical("Critical condition");
$logger->error("Runtime error", ['user_id' => $userId]);
$logger->warning("Deprecated API usage");
$logger->notice("Normal but significant");
$logger->info("User logged in", ['ip' => $ip]);
$logger->debug("Debug information", ['query' => $sql]);
```

### 5.3 Never Expose Sensitive Information

```php
// ❌ Never do this
catch (Exception $e) {
    echo $e->getMessage(); // May expose sensitive data
    echo $e->getTraceAsString(); // Exposes file paths
}

// ✅ Safe error handling
catch (Exception $e) {
    $logger->error($e->getMessage(), [
        'trace' => $e->getTraceAsString(),
        'user_id' => auth()->id()
    ]);
    return response()->json(['error' => 'An error occurred'], 500);
}
```

---

## 6. API Development

### 6.1 RESTful API Design

#### HTTP Methods
```
GET     /api/users          - List all users
GET     /api/users/{id}     - Get specific user
POST    /api/users          - Create new user
PUT     /api/users/{id}     - Update entire user
PATCH   /api/users/{id}     - Update partial user
DELETE  /api/users/{id}     - Delete user
```

#### Response Structure
```json
// ✅ Success response
{
    "success": true,
    "data": {
        "id": 1,
        "name": "John Doe",
        "email": "john@example.com"
    },
    "message": "User retrieved successfully"
}

// ✅ Error response
{
    "success": false,
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Invalid input data",
        "errors": {
            "email": ["Email is required", "Email must be valid"]
        }
    }
}
```

### 6.2 API Versioning
**ALWAYS** version your APIs:

```php
// ✅ URL versioning
Route::prefix('api/v1')->group(function () {
    Route::get('/users', [UserController::class, 'index']);
});

// ✅ Header versioning
if ($request->header('API-Version') === '2.0') {
    // Use v2 logic
}
```

### 6.3 Rate Limiting
Implement rate limiting to prevent abuse:

```php
// Middleware example
Route::middleware(['throttle:60,1'])->group(function () {
    Route::get('/api/users', [UserController::class, 'index']);
});
```

### 6.4 Authentication & Authorization
Use industry-standard authentication:

```php
// ✅ JWT Authentication
$token = auth()->attempt($credentials);

// ✅ OAuth 2.0
$user = Socialite::driver('google')->user();

// ✅ API Keys
$apiKey = $request->header('X-API-Key');
if (!$this->isValidApiKey($apiKey)) {
    abort(401, 'Invalid API key');
}
```

---

## 7. Performance Optimization

### 7.1 Caching Strategy
Implement multi-layer caching:

```php
// ✅ Cache frequently accessed data
$users = Cache::remember('users.active', 3600, function () {
    return User::where('status', 'active')->get();
});

// ✅ Cache invalidation
Cache::forget('users.active');
Cache::tags(['users', 'profiles'])->flush();
```

### 7.2 Query Optimization
```php
// ✅ Select only needed columns
User::select(['id', 'name', 'email'])->get();

// ✅ Use chunks for large datasets
User::chunk(100, function ($users) {
    foreach ($users as $user) {
        // Process user
    }
});

// ✅ Use database indexing
Schema::table('users', function ($table) {
    $table->index('email');
    $table->index(['status', 'created_at']);
});
```

### 7.3 Asset Optimization
```php
// Minify CSS/JS
// Use CDN for static assets
// Implement lazy loading
// Enable GZIP compression

// .htaccess example
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
</IfModule>
```

### 7.4 Database Connection Pooling
```php
// ✅ Use persistent connections
$pdo = new PDO(
    'mysql:host=localhost;dbname=mydb',
    'username',
    'password',
    [PDO::ATTR_PERSISTENT => true]
);
```

---

## 8. Code Quality & Standards

### 8.1 PSR Standards Compliance
Follow PHP-FIG standards:
- **PSR-1**: Basic Coding Standard
- **PSR-12**: Extended Coding Style
- **PSR-4**: Autoloading Standard
- **PSR-7**: HTTP Message Interface

### 8.2 Type Declarations
**ALWAYS** use strict types and type hints:

```php
declare(strict_types=1);

class UserService {
    public function createUser(
        string $name,
        string $email,
        int $age
    ): User {
        // Implementation
    }
    
    public function findUserById(int $id): ?User {
        return User::find($id);
    }
}
```

### 8.3 Code Documentation
Use PHPDoc for all classes and methods:

```php
/**
 * User repository for managing user data
 *
 * @package App\Repositories
 */
class UserRepository {
    /**
     * Find user by email address
     *
     * @param string $email User's email address
     * @return User|null User object or null if not found
     * @throws DatabaseException If database connection fails
     */
    public function findByEmail(string $email): ?User {
        // Implementation
    }
}
```

### 8.4 Code Formatting
Use consistent formatting (PSR-12):

```php
// ✅ Correct formatting
class UserController extends Controller
{
    public function store(Request $request): JsonResponse
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users',
        ]);

        $user = User::create($validated);

        return response()->json($user, 201);
    }
}
```

---

## 9. Environment Configuration

### 9.1 Environment Variables
**NEVER** hardcode sensitive data:

```php
// ❌ Never hardcode credentials
$db = new PDO('mysql:host=localhost', 'root', 'password123');

// ✅ Use environment variables
$db = new PDO(
    'mysql:host=' . getenv('DB_HOST'),
    getenv('DB_USER'),
    getenv('DB_PASSWORD')
);
```

### 9.2 .env File Structure
```env
# Application
APP_NAME="My Application"
APP_ENV=production
APP_DEBUG=false
APP_URL=https://example.com

# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=myapp
DB_USERNAME=dbuser
DB_PASSWORD=securepassword

# Cache
CACHE_DRIVER=redis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# Mail
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null

# API Keys (Never commit to git)
STRIPE_KEY=pk_test_xxxxx
STRIPE_SECRET=sk_test_xxxxx
```

### 9.3 .gitignore Configuration
```gitignore
# ✅ Always ignore sensitive files
.env
.env.local
.env.production
/vendor/
/node_modules/
/storage/*.key
composer.lock (optional)
```

---

## 10. Testing

### 10.1 Unit Tests
Write tests for all business logic:

```php
use PHPUnit\Framework\TestCase;

class UserServiceTest extends TestCase
{
    /** @test */
    public function it_creates_user_with_valid_data(): void
    {
        $service = new UserService();
        
        $user = $service->createUser([
            'name' => 'John Doe',
            'email' => 'john@example.com'
        ]);
        
        $this->assertInstanceOf(User::class, $user);
        $this->assertEquals('John Doe', $user->name);
    }
    
    /** @test */
    public function it_throws_exception_with_invalid_email(): void
    {
        $this->expectException(ValidationException::class);
        
        $service = new UserService();
        $service->createUser([
            'name' => 'John',
            'email' => 'invalid-email'
        ]);
    }
}
```

### 10.2 Integration Tests
Test database interactions:

```php
class UserRepositoryTest extends TestCase
{
    use DatabaseTransactions;
    
    /** @test */
    public function it_retrieves_user_from_database(): void
    {
        $user = User::factory()->create([
            'email' => 'test@example.com'
        ]);
        
        $repository = new UserRepository();
        $found = $repository->findByEmail('test@example.com');
        
        $this->assertEquals($user->id, $found->id);
    }
}
```

### 10.3 Test Coverage
Aim for **>80% code coverage** on critical paths:

```bash
# Generate coverage report
phpunit --coverage-html coverage/
```

---

## 11. Session & Cookie Management

### 11.1 Secure Session Configuration
```php
// ✅ Secure session settings
ini_set('session.cookie_httponly', 1);
ini_set('session.cookie_secure', 1); // HTTPS only
ini_set('session.cookie_samesite', 'Strict');
ini_set('session.use_strict_mode', 1);

session_start([
    'cookie_lifetime' => 0,
    'cookie_secure' => true,
    'cookie_httponly' => true,
    'cookie_samesite' => 'Strict'
]);
```

### 11.2 Session Regeneration
Regenerate session ID after authentication:

```php
// ✅ Prevent session fixation
if ($this->authenticate($credentials)) {
    session_regenerate_id(true);
    $_SESSION['user_id'] = $user->id;
}
```

---

## 12. Common Mistakes to Avoid

### ❌ Don't Do This:
1. **Using deprecated MySQL functions** (`mysql_*` instead of `mysqli_*` or PDO)
2. **Not validating file uploads** (check MIME type, size, extension)
3. **Storing passwords in plain text**
4. **Using `eval()` or `exec()` with user input**
5. **Not implementing HTTPS** (especially for authentication)
6. **Ignoring CORS issues** (configure properly)
7. **Not sanitizing file names** before storage
8. **Using `SELECT *`** (only select needed columns)
9. **Not implementing pagination** for large datasets
10. **Mixing business logic with presentation**

### ✅ Do This:
1. Use **prepared statements** for all database queries
2. Implement **rate limiting** on all API endpoints
3. Use **environment variables** for configuration
4. Implement **proper error handling** and logging
5. Use **HTTPS** for all production environments
6. Implement **CORS** headers properly
7. Use **content security policies (CSP)**
8. Implement **backup strategies** for databases
9. Use **version control** (Git) for all code
10. Document your **API endpoints** (use Swagger/OpenAPI)

---

## 13. Deployment Best Practices

### 13.1 Pre-Deployment Checklist
- [ ] All environment variables configured
- [ ] Database migrations tested
- [ ] SSL certificate installed and configured
- [ ] All tests passing
- [ ] Error logging configured
- [ ] Backup strategy in place
- [ ] Monitoring tools configured
- [ ] Security headers configured
- [ ] Performance optimization applied
- [ ] Documentation updated

### 13.2 Security Headers
```php
// .htaccess or via PHP
header("X-Frame-Options: DENY");
header("X-Content-Type-Options: nosniff");
header("X-XSS-Protection: 1; mode=block");
header("Strict-Transport-Security: max-age=31536000; includeSubDomains");
header("Content-Security-Policy: default-src 'self'");
header("Referrer-Policy: no-referrer-when-downgrade");
```

### 13.3 Automated Deployment
Use CI/CD pipelines:

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install dependencies
        run: composer install --no-dev --optimize-autoloader
      - name: Run tests
        run: ./vendor/bin/phpunit
      - name: Deploy
        run: ./deploy.sh
```

---

## 14. Monitoring & Maintenance

### 14.1 Application Monitoring
- Monitor server resources (CPU, memory, disk)
- Monitor application logs for errors
- Track API response times
- Monitor database query performance
- Set up alerts for critical errors

### 14.2 Database Maintenance
```sql
-- Regular maintenance tasks
OPTIMIZE TABLE users;
ANALYZE TABLE orders;

-- Monitor slow queries
SHOW PROCESSLIST;
SHOW FULL PROCESSLIST;

-- Check table integrity
CHECK TABLE users;
```

### 14.3 Backup Strategy
```bash
# Daily database backup
mysqldump -u username -p database_name > backup_$(date +%Y%m%d).sql

# Weekly full backup
tar -czf backup_$(date +%Y%m%d).tar.gz /path/to/project/
```

---

## 15. Core Web Vitals & Performance Metrics

> **CRITICAL**: Google ranks websites based on Core Web Vitals. Production sites are automatically judged on these metrics.

### 15.1 Core Web Vitals Requirements

**LCP (Largest Contentful Paint) < 2.5s**
- Measures loading performance
- Must occur within 2.5 seconds of page start
- Affects 75% of page visits

**FID/INP (First Input Delay / Interaction to Next Paint) < 200ms**
- Measures interactivity
- Time from user interaction to browser response
- Must be under 200ms for good UX

**CLS (Cumulative Layout Shift) < 0.1**
- Measures visual stability
- No unexpected layout shifts
- Score must be below 0.1

**TTFB (Time to First Byte) < 200ms**
- Measures server response time
- First byte must arrive within 200ms
- Critical for perceived performance

### 15.2 Practical Optimization Rules

#### Optimize LCP
```html
<!-- ✅ Preload hero images -->
<link rel="preload" as="image" href="/hero.webp" fetchpriority="high">

<!-- ✅ Use modern image formats -->
<picture>
  <source srcset="hero.avif" type="image/avif">
  <source srcset="hero.webp" type="image/webp">
  <img src="hero.jpg" alt="Hero" loading="eager">
</picture>

<!-- ✅ Prioritize critical CSS -->
<style>
  /* Critical above-the-fold CSS inline */
  .hero { display: flex; }
</style>
<link rel="preload" href="/styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

#### Optimize FID/INP
```javascript
// ✅ Use passive event listeners
element.addEventListener('scroll', handler, { passive: true });

// ✅ Debounce expensive operations
const debouncedSearch = debounce(searchFunction, 300);

// ✅ Use requestIdleCallback for non-critical tasks
requestIdleCallback(() => {
  // Analytics, prefetching, etc.
});

// ✅ Code splitting for large bundles
const HeavyComponent = lazy(() => import('./HeavyComponent'));
```

#### Optimize CLS
```css
/* ✅ Reserve space for images */
img {
  aspect-ratio: 16 / 9;
  width: 100%;
  height: auto;
}

/* ✅ Reserve space for ads/embeds */
.ad-slot {
  min-height: 250px;
}

/* ❌ Never inject content above existing content without reserved space */
```

```html
<!-- ✅ Always specify dimensions -->
<img src="image.jpg" width="800" height="600" alt="Description">
```

#### Optimize TTFB
```php
// ✅ Use server-side caching
$cached = Cache::remember('homepage', 3600, function () {
    return view('home')->render();
});

// ✅ Use CDN for static assets
// Configure CDN headers
header('Cache-Control: public, max-age=31536000, immutable');

// ✅ Enable HTTP/2
// Configure in Apache/Nginx
```

### 15.3 Performance Budget
Set and enforce performance budgets:

```javascript
// performance-budget.json
{
  "budgets": [
    {
      "resourceSizes": [
        { "resourceType": "script", "budget": 300 },
        { "resourceType": "stylesheet", "budget": 100 },
        { "resourceType": "image", "budget": 500 },
        { "resourceType": "font", "budget": 100 }
      ],
      "timings": [
        { "metric": "interactive", "budget": 3000 },
        { "metric": "first-contentful-paint", "budget": 1500 }
      ]
    }
  ]
}
```

### 15.4 Monitoring Core Web Vitals
```javascript
// ✅ Use web-vitals library
import {onCLS, onFID, onLCP, onINP, onTTFB} from 'web-vitals';

function sendToAnalytics(metric) {
  fetch('/analytics', {
    method: 'POST',
    body: JSON.stringify(metric),
    keepalive: true
  });
}

onCLS(sendToAnalytics);
onFID(sendToAnalytics);
onLCP(sendToAnalytics);
onINP(sendToAnalytics);
onTTFB(sendToAnalytics);
```

---

## 16. SEO & Web Indexing

> **MANDATORY**: Even non-marketing sites need SEO basics for discoverability, usability, and link previews.

### 16.1 Canonical Tags
Prevent duplicate content issues:

```html
<!-- ✅ Always specify canonical URL -->
<link rel="canonical" href="https://example.com/product/shoes" />

<!-- ✅ For paginated content -->
<link rel="canonical" href="https://example.com/blog" />
<link rel="next" href="https://example.com/blog?page=2" />
```

### 16.2 Open Graph & Twitter Cards
```html
<!-- Open Graph (Facebook, LinkedIn) -->
<meta property="og:title" content="Product Name - Your Store" />
<meta property="og:description" content="Product description here" />
<meta property="og:image" content="https://example.com/image.jpg" />
<meta property="og:url" content="https://example.com/product/123" />
<meta property="og:type" content="product" />
<meta property="og:site_name" content="Your Store" />

<!-- Twitter Cards -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site" content="@yourhandle" />
<meta name="twitter:title" content="Product Name" />
<meta name="twitter:description" content="Product description" />
<meta name="twitter:image" content="https://example.com/image.jpg" />
```

### 16.3 Structured Data (JSON-LD)
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Product Name",
  "image": "https://example.com/image.jpg",
  "description": "Product description",
  "brand": {
    "@type": "Brand",
    "name": "Brand Name"
  },
  "offers": {
    "@type": "Offer",
    "price": "99.99",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock"
  }
}
</script>
```

### 16.4 Sitemap Generation
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2024-01-01</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://example.com/products</loc>
    <lastmod>2024-01-01</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
</urlset>
```

```php
// ✅ Automatic sitemap generation
Route::get('/sitemap.xml', function () {
    $urls = Page::all();
    return response()->view('sitemap', compact('urls'))
        ->header('Content-Type', 'application/xml');
});
```

### 16.5 robots.txt Best Practices
```
User-agent: *
Allow: /
Disallow: /admin/
Disallow: /private/
Disallow: /api/
Disallow: /*.json$

Sitemap: https://example.com/sitemap.xml
```

### 16.6 Meta Description Rules
```html
<!-- ✅ Unique, descriptive, 150-160 characters -->
<meta name="description" content="Buy premium running shoes with free shipping. Shop top brands like Nike, Adidas, and more at competitive prices.">

<!-- ❌ Too short or generic -->
<meta name="description" content="Shoes">

<!-- ❌ Duplicate across pages -->
```

### 16.7 Semantic HTML & Heading Hierarchy
```html
<!-- ✅ Proper heading hierarchy -->
<h1>Main Page Title</h1>
  <h2>Section Title</h2>
    <h3>Subsection</h3>
  <h2>Another Section</h2>

<!-- ❌ Don't skip levels -->
<h1>Title</h1>
  <h4>Wrong - skipped h2 and h3</h4>

<!-- ✅ Use semantic HTML -->
<article>
  <header>
    <h1>Article Title</h1>
    <time datetime="2024-01-01">January 1, 2024</time>
  </header>
  <main>
    <p>Content here...</p>
  </main>
  <footer>
    <p>Author information</p>
  </footer>
</article>
```

---

## 17. Accessibility Compliance (Legal Requirement)

> **LEGAL**: WCAG 2.1 AA compliance is **law** in many countries. Missing this fails enterprise-level standards.

### 17.1 WCAG 2.1 AA Compliance Checklist
- [ ] All images have alt text
- [ ] Color contrast ratio ≥ 4.5:1 for text
- [ ] Keyboard navigation works everywhere
- [ ] Focus indicators are visible
- [ ] Forms have proper labels
- [ ] ARIA attributes used correctly
- [ ] No content flashes more than 3 times per second
- [ ] Captions for all video content
- [ ] Transcripts for audio content

### 17.2 Keyboard Navigation
```javascript
// ✅ Ensure keyboard accessibility
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') {
    closeModal();
  }
  if (e.key === 'Enter' || e.key === ' ') {
    // Activate clickable elements
  }
});

// ✅ Proper tab order
<button tabindex="0">Primary Action</button>
<button tabindex="0">Secondary Action</button>
<div tabindex="-1">Not keyboard accessible</div>
```

### 17.3 Screen Reader Compatibility
```html
<!-- ✅ ARIA labels -->
<button aria-label="Close dialog">×</button>
<nav aria-label="Main navigation">
  <ul>...</ul>
</nav>

<!-- ✅ ARIA live regions for dynamic content -->
<div aria-live="polite" aria-atomic="true">
  Item added to cart
</div>

<!-- ✅ Skip links -->
<a href="#main-content" class="skip-link">Skip to main content</a>
```

### 17.4 Focus Indicators
```css
/* ✅ Visible focus indicators */
:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* ❌ Never remove focus without alternative */
:focus {
  outline: none; /* WRONG */
}

/* ✅ Custom focus styles */
button:focus-visible {
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.5);
}
```

### 17.5 Color Contrast Requirements
```css
/* ✅ WCAG AA requires 4.5:1 for normal text */
.text {
  color: #333333; /* on white background = 12.6:1 ✅ */
}

/* ✅ AAA requires 7:1 for normal text */
.text-aaa {
  color: #000000; /* on white background = 21:1 ✅ */
}

/* ❌ Insufficient contrast */
.bad-text {
  color: #999999; /* on white background = 2.9:1 ❌ */
}

/* ✅ Large text (18pt+) requires 3:1 minimum */
.large-text {
  font-size: 18pt;
  color: #767676; /* on white = 4.5:1 ✅ */
}
```

### 17.6 Form Accessibility
```html
<!-- ✅ Proper form labels -->
<label for="email">Email Address</label>
<input 
  type="email" 
  id="email" 
  name="email"
  aria-required="true"
  aria-describedby="email-help"
>
<span id="email-help">We'll never share your email</span>

<!-- ✅ Error messaging -->
<input 
  type="email" 
  id="email"
  aria-invalid="true"
  aria-describedby="email-error"
>
<span id="email-error" role="alert">
  Please enter a valid email address
</span>
```

### 17.7 Media Accessibility
```html
<!-- ✅ Video with captions -->
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track kind="captions" src="captions.vtt" srclang="en" label="English">
  <track kind="descriptions" src="descriptions.vtt" srclang="en">
</video>

<!-- ✅ Provide transcripts -->
<audio controls>
  <source src="podcast.mp3" type="audio/mpeg">
</audio>
<a href="/transcript.pdf">Download Transcript</a>
```

---

## 18. Legal & Compliance Requirements

> **MANDATORY**: Professional teams always include compliance standards.

### 18.1 GDPR (EU)
```php
// ✅ User consent management
class ConsentManager {
    public function recordConsent(User $user, array $categories) {
        UserConsent::create([
            'user_id' => $user->id,
            'analytics' => $categories['analytics'] ?? false,
            'marketing' => $categories['marketing'] ?? false,
            'necessary' => true, // Always true
            'ip_address' => request()->ip(),
            'user_agent' => request()->userAgent(),
            'consented_at' => now()
        ]);
    }
    
    public function exportUserData(User $user) {
        // GDPR Article 20: Right to data portability
        return [
            'personal_data' => $user->toArray(),
            'orders' => $user->orders,
            'activity_log' => $user->activities
        ];
    }
    
    public function anonymizeUser(User $user) {
        // GDPR Article 17: Right to be forgotten
        $user->update([
            'name' => 'Deleted User',
            'email' => 'deleted_' . $user->id . '@example.com',
            'phone' => null,
            'address' => null
        ]);
    }
}
```

### 18.2 Cookie Consent Banner
```html
<!-- ✅ Proper cookie consent -->
<div id="cookie-banner">
  <h2>We use cookies</h2>
  <p>We use necessary cookies to make our site work. We'd also like to set optional cookies to improve your experience.</p>
  
  <div class="cookie-categories">
    <label>
      <input type="checkbox" checked disabled> Necessary
    </label>
    <label>
      <input type="checkbox" name="analytics"> Analytics
    </label>
    <label>
      <input type="checkbox" name="marketing"> Marketing
    </label>
  </div>
  
  <button onclick="acceptSelected()">Accept Selected</button>
  <button onclick="acceptAll()">Accept All</button>
  <button onclick="rejectAll()">Reject All</button>
</div>
```

### 18.3 Privacy Policy Requirements
Must include:
- What data you collect
- Why you collect it
- How long you store it
- Who you share it with
- User rights (access, deletion, portability)
- How to contact you
- Cookie policy
- Data breach notification process

### 18.4 CCPA (California Consumer Privacy Act)
```php
// ✅ "Do Not Sell My Personal Information" link
Route::get('/do-not-sell', function () {
    return view('dnsmpi');
});

// ✅ Honor opt-out requests
if (request()->header('Sec-GPC') === '1') {
    // Global Privacy Control detected
    $user->doNotSell();
}
```

### 18.5 PCI-DSS (Payment Card Industry)
```php
// ✅ Never store sensitive card data
// ❌ NEVER do this:
$card = [
    'number' => $_POST['card_number'], // ILLEGAL
    'cvv' => $_POST['cvv'], // ILLEGAL
];

// ✅ Use payment processor tokens
$payment = Stripe::token()->create([
    'card' => [
        'number' => $request->card_number,
        'exp_month' => $request->exp_month,
        'exp_year' => $request->exp_year,
        'cvc' => $request->cvc,
    ],
]);

// Store only the token
$user->update(['stripe_token' => $payment->id]);
```

### 18.6 Data Retention Policy
```php
// ✅ Auto-delete old data
Schedule::daily(function () {
    // Delete logs older than 90 days
    Log::where('created_at', '<', now()->subDays(90))->delete();
    
    // Anonymize inactive users after 2 years
    User::where('last_login_at', '<', now()->subYears(2))
        ->each(fn($user) => $user->anonymize());
});
```

---

## 19. Deployment Architecture Standards

### 19.1 Deployment Strategies

#### Blue-Green Deployment
```yaml
# ✅ Two identical environments
blue_environment:
  version: v1.0.0
  status: live
  traffic: 100%

green_environment:
  version: v1.1.0
  status: staging
  traffic: 0%

# After testing green:
# 1. Switch traffic from blue to green
# 2. Keep blue as backup for instant rollback
```

#### Canary Deployment
```yaml
# ✅ Gradual rollout
deployment:
  v1.0.0: 90%  # Stable version
  v1.1.0: 10%  # New version (canary)

# If metrics are good:
# Increase canary to 25%, then 50%, then 100%
```

### 19.2 Edge Caching Strategy
```php
// ✅ Cache-Control headers
header('Cache-Control: public, max-age=3600, s-maxage=86400');

// CDN-specific headers
header('CDN-Cache-Control: max-age=86400');
header('Cloudflare-CDN-Cache-Control: max-age=86400');

// Surrogate keys for purging
header('Surrogate-Key: product-123 category-shoes');
```

### 19.3 Load Balancer Configuration
```nginx
# ✅ Layer 7 (Application) Load Balancing
upstream backend {
    least_conn;  # Intelligent routing
    server app1.example.com:8080 weight=3;
    server app2.example.com:8080 weight=2;
    server app3.example.com:8080 backup;
}

server {
    listen 443 ssl http2;
    
    location / {
        proxy_pass http://backend;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    
    # Health checks
    location /health {
        access_log off;
        return 200 "healthy\n";
    }
}
```

### 19.4 Zero-Downtime Database Migrations
```php
// ✅ Use Expand-Contract pattern

// Step 1: Expand - Add new column (non-breaking)
Schema::table('users', function ($table) {
    $table->string('full_name')->nullable();
});

// Step 2: Write to both old and new
$user->first_name = $firstName;
$user->last_name = $lastName;
$user->full_name = "$firstName $lastName"; // Also write to new column

// Step 3: Backfill data
User::chunk(100, function ($users) {
    foreach ($users as $user) {
        $user->full_name = "{$user->first_name} {$user->last_name}";
        $user->save();
    }
});

// Step 4: Switch reads to new column
// Step 5: Contract - Remove old columns (after verification)
Schema::table('users', function ($table) {
    $table->dropColumn(['first_name', 'last_name']);
});
```

### 19.5 Feature Flags
```php
// ✅ Feature flag system
class FeatureFlag {
    public static function isEnabled(string $flag, ?User $user = null): bool {
        $config = cache()->remember("feature:$flag", 60, function () use ($flag) {
            return FeatureConfig::where('name', $flag)->first();
        });
        
        if (!$config || !$config->enabled) {
            return false;
        }
        
        // Percentage rollout
        if ($config->percentage < 100) {
            return (crc32($user?->id ?? request()->ip()) % 100) < $config->percentage;
        }
        
        return true;
    }
}

// Usage
if (FeatureFlag::isEnabled('new_checkout')) {
    return $this->newCheckoutFlow();
}
return $this->oldCheckoutFlow();
```

### 19.6 Scaling Decision Framework

#### Horizontal vs Vertical Scaling
```
Horizontal (Scale Out):
✅ Better for: Web servers, stateless applications
✅ Unlimited scaling potential
✅ Better fault tolerance
❌ More complex architecture

Vertical (Scale Up):
✅ Better for: Databases, monolithic apps
✅ Simpler architecture
❌ Hardware limits
❌ Single point of failure
```

#### Serverless vs Container Decision
```
Use Serverless when:
✅ Unpredictable traffic patterns
✅ Event-driven workloads
✅ Want zero server management

Use Containers when:
✅ Consistent traffic
✅ Need full control
✅ Long-running processes
✅ Complex dependencies
```

---

## 20. Logging, Observability & Tracing

### 20.1 Distributed Tracing (OpenTelemetry)
```php
// ✅ Trace requests across services
use OpenTelemetry\API\Trace\Tracer;

$tracer = Globals::tracerProvider()->getTracer('my-service');

$span = $tracer->spanBuilder('process-payment')
    ->setSpanKind(SpanKind::KIND_SERVER)
    ->startSpan();

$span->setAttribute('user.id', $userId);
$span->setAttribute('amount', $amount);

try {
    $result = $paymentService->process($amount);
    $span->setStatus(StatusCode::STATUS_OK);
} catch (Exception $e) {
    $span->setStatus(StatusCode::STATUS_ERROR, $e->getMessage());
    $span->recordException($e);
    throw $e;
} finally {
    $span->end();
}
```

### 20.2 Structured Logging with Correlation IDs
```php
// ✅ Add correlation ID to all logs
class LoggingMiddleware {
    public function handle($request, $next) {
        $correlationId = $request->header('X-Correlation-ID') 
            ?? (string) Str::uuid();
        
        $request->headers->set('X-Correlation-ID', $correlationId);
        
        Log::withContext([
            'correlation_id' => $correlationId,
            'user_id' => auth()->id(),
            'ip' => $request->ip()
        ]);
        
        return $next($request);
    }
}

// All subsequent logs will include these fields
Log::info('Payment processed', [
    'amount' => $amount,
    'payment_id' => $payment->id
    // correlation_id automatically included
]);
```

### 20.3 Metrics (Prometheus)
```php
// ✅ Expose metrics endpoint
Route::get('/metrics', function () {
    $metrics = [
        'http_requests_total' => Counter::get('http_requests'),
        'http_request_duration_seconds' => Histogram::get('http_duration'),
        'active_users' => Gauge::get('active_users'),
    ];
    
    return response($metrics)->header('Content-Type', 'text/plain');
});

// Track metrics
Metrics::increment('http_requests_total', ['method' => 'POST', 'endpoint' => '/api/users']);
Metrics::histogram('http_request_duration_seconds', $duration);
Metrics::gauge('active_users', User::online()->count());
```

### 20.4 Dashboards & Visualization
```javascript
// ✅ Grafana dashboard configuration
{
  "dashboard": {
    "title": "Application Metrics",
    "panels": [
      {
        "title": "Request Rate",
        "targets": [
          {
            "expr": "rate(http_requests_total[5m])"
          }
        ]
      },
      {
        "title": "Error Rate",
        "targets": [
          {
            "expr": "rate(http_requests_total{status=~\"5..\"}[5m])"
          }
        ]
      },
      {
        "title": "95th Percentile Latency",
        "targets": [
          {
            "expr": "histogram_quantile(0.95, http_request_duration_seconds_bucket)"
          }
        ]
      }
    ]
  }
}
```

### 20.5 Error Budgets & SLOs
```php
// ✅ Define Service Level Objectives
class SLO {
    const AVAILABILITY = 99.9;  // 99.9% uptime
    const LATENCY_P95 = 200;    // 95th percentile < 200ms
    const ERROR_RATE = 0.1;     // < 0.1% error rate
    
    public static function calculateErrorBudget(): float {
        $totalMinutes = 30 * 24 * 60; // 30 days
        $allowedDowntime = $totalMinutes * (1 - self::AVAILABILITY / 100);
        $actualDowntime = self::getActualDowntime();
        
        return $allowedDowntime - $actualDowntime;
    }
}
```

---

## 21. Frontend Build System Standards

### 21.1 Bundler Configuration
```javascript
// ✅ Webpack/Vite optimization
export default {
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor': ['react', 'react-dom'],
          'ui': ['@mui/material'],
          'utils': ['lodash', 'date-fns']
        }
      }
    },
    // Chunk size warnings
    chunkSizeWarningLimit: 500,
  },
  // Code splitting
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10
        }
      }
    }
  }
}
```

### 21.2 Tree Shaking
```javascript
// ✅ Named imports for tree shaking
import { debounce } from 'lodash-es';

// ❌ Imports entire library
import _ from 'lodash';

// ✅ Mark side-effect-free packages
{
  "name": "my-package",
  "sideEffects": false  // Enable aggressive tree shaking
}

// ✅ Or specify specific files with side effects
{
  "sideEffects": ["*.css", "*.scss"]
}
```

### 21.3 Import Conventions
```javascript
// ✅ Use absolute imports with path aliases
import { Button } from '@/components/Button';
import { formatDate } from '@/utils/date';

// ❌ Avoid deep relative imports
import { Button } from '../../../components/Button';

// tsconfig.json / vite.config.js
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"],
      "@components/*": ["./src/components/*"],
      "@utils/*": ["./src/utils/*"]
    }
  }
}
```

### 21.4 Remove Unused Dependencies
```bash
# ✅ Audit dependencies regularly
npm audit
npx depcheck
npx npm-check-updates

# ✅ Remove unused packages
npm remove unused-package

# ✅ Check bundle size
npx webpack-bundle-analyzer
```

### 21.5 Component Reusability Patterns
```javascript
// ✅ Compound components pattern
const Card = ({ children }) => <div className="card">{children}</div>;
Card.Header = ({ children }) => <div className="card-header">{children}</div>;
Card.Body = ({ children }) => <div className="card-body">{children}</div>;

// Usage
<Card>
  <Card.Header>Title</Card.Header>
  <Card.Body>Content</Card.Body>
</Card>

// ✅ Render props pattern for flexibility
const DataFetcher = ({ url, render }) => {
  const [data, setData] = useState(null);
  useEffect(() => fetch(url).then(r => r.json()).then(setData), [url]);
  return render(data);
};
```

---

## 22. Backend Scalability Principles

### 22.1 Event-Driven Architecture
```php
// ✅ Use events for decoupling
class OrderPlaced {
    public function __construct(public Order $order) {}
}

// Dispatch event
event(new OrderPlaced($order));

// Multiple listeners can react
class SendOrderConfirmation {
    public function handle(OrderPlaced $event) {
        Mail::to($event->order->user)->send(new OrderConfirmationEmail($event->order));
    }
}

class UpdateInventory {
    public function handle(OrderPlaced $event) {
        foreach ($event->order->items as $item) {
            $item->product->decrement('stock', $item->quantity);
        }
    }
}

class NotifyWarehouse {
    public function handle(OrderPlaced $event) {
        Http::post(config('warehouse.url'), $event->order->toArray());
    }
}
```

### 22.2 Message Queues
```php
// ✅ Use queues for async processing
// RabbitMQ / SQS / Redis Queue
class ProcessLargeExport implements ShouldQueue {
    public function handle() {
        $data = User::all();
        Excel::create($data);
        // Email when complete
    }
}

// Dispatch to queue
ProcessLargeExport::dispatch();

// ✅ Configure queue workers
// config/queue.php
'connections' => [
    'sqs' => [
        'driver' => 'sqs',
        'key' => env('AWS_ACCESS_KEY_ID'),
        'secret' => env('AWS_SECRET_ACCESS_KEY'),
        'queue' => env('SQS_QUEUE_URL'),
        'region' => 'us-east-1',
    ],
    'redis' => [
        'driver' => 'redis',
        'connection' => 'default',
        'queue' => 'default',
        'retry_after' => 90,
    ],
]
```

### 22.3 Rate Limiting Strategies
```php
// ✅ Multiple rate limiting tiers
class RateLimitMiddleware {
    protected array $limits = [
        'free' => 100,    // 100 requests/hour
        'pro' => 1000,    // 1000 requests/hour
        'enterprise' => 10000
    ];
    
    public function handle($request, $next) {
        $user = $request->user();
        $limit = $this->limits[$user->plan] ?? 100;
        
        $key = 'rate_limit:' . $user->id;
        $current = Cache::increment($key);
        
        if ($current === 1) {
            Cache::expire($key, 3600);
        }
        
        if ($current > $limit) {
            return response()->json([
                'error' => 'Rate limit exceeded'
            ], 429);
        }
        
        return $next($request);
    }
}
```

### 22.4 Idempotency Keys
```php
// ✅ Prevent duplicate operations
class PaymentController {
    public function process(Request $request) {
        $idempotencyKey = $request->header('Idempotency-Key');
        
        if (!$idempotencyKey) {
            return response()->json(['error' => 'Idempotency-Key required'], 400);
        }
        
        // Check if already processed
        $cached = Cache::get("idempotency:$idempotencyKey");
        if ($cached) {
            return response()->json($cached);
        }
        
        // Process payment
        $result = $this->paymentService->charge($request->amount);
        
        // Cache result for 24 hours
        Cache::put("idempotency:$idempotencyKey", $result, 86400);
        
        return response()->json($result);
    }
}
```

### 22.5 Retry & Backoff Policies
```php
// ✅ Exponential backoff
class ApiClient {
    public function requestWithRetry(string $url, int $maxRetries = 3): mixed {
        $attempt = 0;
        
        while ($attempt < $maxRetries) {
            try {
                return Http::timeout(10)->get($url)->json();
            } catch (RequestException $e) {
                $attempt++;
                
                if ($attempt >= $maxRetries) {
                    throw $e;
                }
                
                // Exponential backoff: 1s, 2s, 4s
                $delay = pow(2, $attempt);
                sleep($delay);
                
                Log::warning("API request failed, retrying in {$delay}s", [
                    'url' => $url,
                    'attempt' => $attempt
                ]);
            }
        }
    }
}
```

### 22.6 Circuit Breaker Pattern
```php
// ✅ Prevent cascading failures
class CircuitBreaker {
    private int $failureCount = 0;
    private int $threshold = 5;
    private bool $open = false;
    private int $timeout = 60; // seconds
    
    public function call(callable $operation): mixed {
        $key = 'circuit:' . md5(serialize($operation));
        
        if ($this->isOpen($key)) {
            throw new CircuitBreakerOpenException('Circuit is open');
        }
        
        try {
            $result = $operation();
            $this->onSuccess($key);
            return $result;
        } catch (Exception $e) {
            $this->onFailure($key);
            throw $e;
        }
    }
    
    private function onFailure(string $key): void {
        $count = Cache::increment("$key:failures");
        
        if ($count >= $this->threshold) {
            Cache::put("$key:open", true, $this->timeout);
        }
    }
    
    private function isOpen(string $key): bool {
        return Cache::get("$key:open", false);
    }
}
```

---

## 23. File Upload & Media Handling

### 23.1 File Scanning for Malware
```php
// ✅ Scan uploads for malware
use Xenolope\Quahog\Client as ClamAVClient;

class FileUploadController {
    public function upload(Request $request) {
        $file = $request->file('upload');
        
        // Scan with ClamAV
        $clam = new ClamAVClient('unix:///var/run/clamav/clamd.sock');
        $result = $clam->scanFile($file->getRealPath());
        
        if ($result['status'] !== 'OK') {
            Log::alert('Malware detected', [
                'file' => $file->getClientOriginalName(),
                'virus' => $result['reason']
            ]);
            
            return response()->json([
                'error' => 'File rejected: security threat detected'
            ], 400);
        }
        
        // Continue with upload
    }
}
```

### 23.2 MIME Type Validation
```php
// ✅ Validate MIME type server-side
public function upload(Request $request) {
    $file = $request->file('upload');
    
    // ❌ Don't trust client-provided MIME type
    $clientMime = $file->getClientMimeType(); // Can be spoofed
    
    // ✅ Detect actual MIME type
    $actualMime = $file->getMimeType(); // Uses finfo
    
    $allowedMimes = ['image/jpeg', 'image/png', 'image/webp', 'application/pdf'];
    
    if (!in_array($actualMime, $allowedMimes)) {
        return response()->json([
            'error' => 'Invalid file type'
        ], 400);
    }
}
```

### 23.3 File Size Limits
```php
// ✅ Enforce size limits
public function upload(Request $request) {
    $request->validate([
        'file' => 'required|file|max:10240', // 10MB in kilobytes
        'image' => 'required|image|max:5120', // 5MB for images
    ]);
    
    // Additional check
    $file = $request->file('upload');
    if ($file->getSize() > 10 * 1024 * 1024) {
        return response()->json(['error' => 'File too large'], 413);
    }
}

// php.ini configuration
upload_max_filesize = 10M
post_max_size = 10M
```

### 23.4 Chunked Uploads
```javascript
// ✅ Client-side chunked upload
async function uploadLargeFile(file) {
  const chunkSize = 5 * 1024 * 1024; // 5MB chunks
  const chunks = Math.ceil(file.size / chunkSize);
  const uploadId = generateUUID();
  
  for (let i = 0; i < chunks; i++) {
    const start = i * chunkSize;
    const end = Math.min(start + chunkSize, file.size);
    const chunk = file.slice(start, end);
    
    const formData = new FormData();
    formData.append('chunk', chunk);
    formData.append('chunk_number', i);
    formData.append('total_chunks', chunks);
    formData.append('upload_id', uploadId);
    
    await fetch('/api/upload/chunk', {
      method: 'POST',
      body: formData
    });
  }
  
  // Finalize upload
  await fetch('/api/upload/finalize', {
    method: 'POST',
    body: JSON.stringify({ upload_id: uploadId })
  });
}
```

```php
// ✅ Server-side chunk handling
public function uploadChunk(Request $request) {
    $uploadId = $request->upload_id;
    $chunkNumber = $request->chunk_number;
    $totalChunks = $request->total_chunks;
    
    // Store chunk
    $path = storage_path("uploads/temp/$uploadId");
    if (!file_exists($path)) {
        mkdir($path, 0755, true);
    }
    
    $request->file('chunk')->move($path, "chunk_$chunkNumber");
    
    return response()->json(['success' => true]);
}

public function finalizeUpload(Request $request) {
    $uploadId = $request->upload_id;
    $path = storage_path("uploads/temp/$uploadId");
    
    // Combine all chunks
    $finalFile = fopen(storage_path("uploads/$uploadId"), 'wb');
    
    $chunks = glob("$path/chunk_*");
    sort($chunks, SORT_NATURAL);
    
    foreach ($chunks as $chunk) {
        fwrite($finalFile, file_get_contents($chunk));
        unlink($chunk);
    }
    
    fclose($finalFile);
    rmdir($path);
    
    return response()->json(['file' => $uploadId]);
}
```

### 23.5 CDN Storage vs Server Storage
```php
// ✅ Use CDN for static assets
class FileUploadService {
    public function store(UploadedFile $file, string $visibility = 'public'): string {
        // Determine storage based on file type and size
        if ($file->getSize() > 1024 * 1024) { // > 1MB
            // Store in CDN (S3, Cloudflare R2)
            $path = Storage::disk('s3')->putFile('uploads', $file, $visibility);
            return Storage::disk('s3')->url($path);
        } else {
            // Small files on local server
            $path = $file->store('uploads', 'public');
            return asset("storage/$path");
        }
    }
}

// config/filesystems.php
's3' => [
    'driver' => 's3',
    'key' => env('AWS_ACCESS_KEY_ID'),
    'secret' => env('AWS_SECRET_ACCESS_KEY'),
    'region' => env('AWS_DEFAULT_REGION'),
    'bucket' => env('AWS_BUCKET'),
    'url' => env('AWS_URL'),
    'endpoint' => env('AWS_ENDPOINT'),
    'use_path_style_endpoint' => false,
],
```

### 23.6 Image Optimization on Upload
```php
// ✅ Optimize images automatically
use Intervention\Image\Facades\Image;

public function uploadImage(Request $request) {
    $file = $request->file('image');
    
    // Create optimized versions
    $image = Image::make($file);
    
    // Original (max 2000px)
    $image->resize(2000, 2000, function ($constraint) {
        $constraint->aspectRatio();
        $constraint->upsize();
    })->save(storage_path('app/public/images/original.jpg'), 85);
    
    // Thumbnail (300px)
    $image->fit(300, 300)
        ->save(storage_path('app/public/images/thumb.jpg'), 80);
    
    // Convert to WebP
    $image->encode('webp', 85)
        ->save(storage_path('app/public/images/original.webp'));
    
    return response()->json([
        'original' => asset('storage/images/original.jpg'),
        'thumbnail' => asset('storage/images/thumb.jpg'),
        'webp' => asset('storage/images/original.webp')
    ]);
}
```

---

## Summary

Senior developers follow these principles:

1. **Security First**: Never trust user input, always validate and sanitize
2. **Code Quality**: Follow PSR standards, use type hints, write tests
3. **Performance**: Implement caching, optimize queries, use indexes
4. **Maintainability**: Use MVC architecture, dependency injection, proper naming
5. **Documentation**: Document code, APIs, and deployment procedures
6. **Error Handling**: Proper logging, exception handling, no sensitive data exposure
7. **Best Practices**: Follow industry standards (OWASP, PSR, REST)
8. **DevOps**: Use environment variables, automated testing, CI/CD pipelines
9. **Modularity**: Keep code DRY, split large files, clear separation of concerns
10. **Monitoring**: Structured logging, health checks, performance tracking
11. **Core Web Vitals**: Meet Google's performance standards (LCP, FID/INP, CLS, TTFB)
12. **SEO & Accessibility**: Proper metadata, semantic HTML, WCAG 2.1 AA compliance
13. **Legal Compliance**: GDPR, CCPA, PCI-DSS, proper consent management
14. **Scalability**: Event-driven architecture, message queues, circuit breakers
15. **Observability**: Distributed tracing, metrics, dashboards, SLOs

**Remember**: Good code is secure, performant, maintainable, well-documented, **and compliant**. These are not optional — they are the **foundation of professional web development**.

---

## Quick Reference

### Performance Targets
- **LCP**: < 2.5s
- **FID/INP**: < 200ms  
- **CLS**: < 0.1
- **TTFB**: < 200ms

### Legal Requirements
- ✅ GDPR consent management
- ✅ CCPA "Do Not Sell" compliance
- ✅ Cookie consent banner
- ✅ Privacy policy
- ❌ Never store CVV/full card numbers

### Accessibility
- ✅ WCAG 2.1 AA minimum
- ✅ Contrast ratio ≥ 4.5:1
- ✅ Keyboard navigation
- ✅ ARIA labels
- ✅ Captions for video

### Deployment
- ✅ Blue-green or canary deployments
- ✅ Feature flags for gradual rollouts
- ✅ Zero-downtime migrations
- ✅ Automated CI/CD pipelines
- ✅ Health check endpoints

### Scalability
- ✅ Event-driven architecture  
- ✅ Message queues (SQS, RabbitMQ)
- ✅ Circuit breakers for resilience
- ✅ Idempotency keys for critical operations
- ✅ Distributed tracing with OpenTelemetry

---

*This guide is based on official standards from Google (Core Web Vitals), OWASP (Security), W3C (Web Standards), PHP-FIG (PSR), WCAG 2.1 (Accessibility), and industry best practices. Following these rules ensures production-grade, enterprise-level web applications.*
