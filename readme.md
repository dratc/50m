# সম্পূর্ণ Project Master Documentation
## ৫০টি মডিউলভিত্তিক Business Management Application

**ডকুমেন্টের নাম:** `DOCUMENTATION.md`  
**ডকুমেন্টের উদ্দেশ্য:** এই ফাইলটি হবে পুরো প্রজেক্টের প্রধান পরিকল্পনা, আর্কিটেকচার, AI development rulebook, module development guide, testing standard এবং deployment guide। নতুন AI chat/session শুরু হলেও এই ডকুমেন্ট পড়ে কাজের নিয়ম ও প্রজেক্টের কাঠামো বোঝা যাবে।

**ডকুমেন্টের অবস্থা:** Master planning specification  
**প্রস্তাবিত Technology Stack:** PHP 8.3+, SQLite, HTML5, CSS3, Vanilla JavaScript  
**Architecture:** Modular Monolith  
**Project Scale:** ৫০টি স্বতন্ত্র Feature Module, একটি কেন্দ্রীয় Core ও একটি কেন্দ্রীয় SQLite database  
**প্রধান লক্ষ্য:** Lightweight, maintainable, secure, modular, consistent এবং ভবিষ্যতে সম্প্রসারণযোগ্য business management application।

> গুরুত্বপূর্ণ: এই ডকুমেন্ট একটি পরিকল্পনা ও বাধ্যতামূলক কাজের নির্দেশিকা। কোনো ফিচার বাস্তবে তৈরি, পরীক্ষা বা deploy না করা পর্যন্ত তাকে completed বলা যাবে না। AI-কে source code পরীক্ষা করে প্রমাণসহ status জানাতে হবে।

---

# সূচিপত্র

1. Project Vision ও লক্ষ্য
2. Technology Stack ও সীমাবদ্ধতা
3. Architecture Principles
4. প্রস্তাবিত Root Folder Structure
5. Core Foundation
6. Central Database Architecture
7. Module Ownership ও Database Safety
8. Internal API Architecture
9. Authentication ও Permission System
10. Unified Frontend ও Design System
11. Module Auto-Discovery ও Registry
12. Standard Module Folder Structure
13. ৫০টি Module পরিকল্পনা ও তালিকা তৈরির পদ্ধতি
14. পূর্ণ Development Roadmap
15. একটি Module তৈরির বাধ্যতামূলক Workflow
16. AI-কে Module-এর কাজ কীভাবে বুঝিয়ে দেবেন
17. Copy-ready Module Development Prompt
18. Change Control ও Approval Policy
19. Security Requirements
20. Testing ও Quality Assurance
21. Documentation ও Project Memory
22. Git, Backup ও Recovery
23. Deployment ও Operations
24. Project Status ও Completion Criteria
25. ঝুঁকি, সিদ্ধান্ত ও নিষিদ্ধ পদ্ধতি
26. Project শুরু করার প্রথম কাজ
27. Module Request Template
28. Final Acceptance Checklist

---

# 1. Project Vision ও লক্ষ্য

একটি একক Business Management Application তৈরি করা হবে, যার মধ্যে ৫০টি আলাদা business feature/module থাকবে। প্রতিটি Module তার নিজস্ব business logic, page, action, service, permission এবং database schema পরিচালনা করবে। কিন্তু সব Module একটি shared application foundation ব্যবহার করবে।

## 1.1 মূল লক্ষ্য

- ৫০টি Module-কে একই application-এর মধ্যে পরিচালনা করা।
- একটি কেন্দ্রীয় SQLite database file ব্যবহার করা।
- একটি কেন্দ্রীয় database connection ব্যবহার করা।
- সব Module-এর জন্য একই Login, Authentication ও Permission system।
- সব Module-এর জন্য একই Header, Sidebar, Navigation ও App Layout।
- একটি unified frontend design system বজায় রাখা।
- Module-গুলোর মধ্যে সরাসরি private database access নয়; Internal API দিয়ে যোগাযোগ।
- নতুন Module যোগ করতে Core বা পুরোনো Module-এর অপ্রয়োজনীয় পরিবর্তন না লাগা।
- AI দিয়ে ধাপে ধাপে development করা, যাতে আগের কাজ নষ্ট না হয়।
- প্রতিটি পরিবর্তন পরীক্ষা, নথিভুক্ত ও version control করা।

## 1.2 Non-goals ও সতর্কতা

- ৫০টি Module একসঙ্গে generate করা হবে না।
- শুধু UI তৈরি হলেই Module complete হবে না।
- AI-এর দাবি বা অনুমানকে test evidence ছাড়া সত্য ধরে নেওয়া যাবে না।
- SQLite-কে একাধিক আলাদা database file-এ ভাগ করা হবে না, যদি না Project Owner ভবিষ্যতে architecture পরিবর্তনের অনুমোদন দেন।
- অপ্রয়োজনীয় framework, package, build system বা paid API যোগ করা যাবে না।
- Browser-side JavaScript দিয়ে database-এ সরাসরি access করা যাবে না।

---

# 2. Technology Stack ও সীমাবদ্ধতা

| Layer | Technology | নিয়ম |
|---|---|---|
| Backend | PHP 8.3+ | Native PHP; প্রয়োজন ছাড়া framework নয় |
| Database | SQLite | একটি central `.sqlite`/`.db` file |
| Database access | PDO SQLite | Prepared statements বাধ্যতামূলক |
| Frontend | HTML5 | Semantic markup |
| Styling | CSS3 | Central design system |
| Client scripting | Vanilla JavaScript | প্রয়োজন ছাড়া framework নয় |
| Web server | Nginx / HestiaCP | Production environment |
| Version control | Git | নিয়মিত commit ও rollback |
| Documentation | Markdown | Root-level source of truth |

## 2.1 Dependency policy

নতুন Composer package, JavaScript library, CSS framework, icon library বা external service যোগ করার আগে:

1. কেন প্রয়োজন তা ব্যাখ্যা করতে হবে।
2. Native PHP/JS দিয়ে করা যায় কি না যাচাই করতে হবে।
3. Resource, security, licensing ও maintenance impact জানাতে হবে।
4. Project Owner-এর অনুমোদন নিতে হবে।

## 2.2 Lightweight policy

- PHP request যতটা সম্ভব ছোট ও efficient রাখতে হবে।
- অপ্রয়োজনীয় background worker বা polling নয়।
- Database query-তে প্রয়োজনীয় column-ই select করতে হবে।
- Pagination ব্যবহার করতে হবে।
- বড় report-এ filtering ও date range দিতে হবে।
- JavaScript bundle বা framework runtime যোগ করা যাবে না, যদি না অনুমোদিত হয়।

---

# 3. Architecture Principles

## 3.1 Modular Monolith

প্রজেক্টটি হবে Modular Monolith। অর্থাৎ একটি deployable PHP application থাকবে, কিন্তু ভিতরে Module অনুযায়ী দায়িত্ব আলাদা থাকবে।

```text
Browser
  |
  v
Public Entry Point
  |
  v
Core Bootstrap / Router
  |
  +---- Authentication / Permission
  |
  +---- Shared Layout / Design System
  |
  +---- Internal API Registry
  |
  +---- Module Registry / Loader
  |
  +---- Module 01 ... Module 50
  |
  v
Central Database Connection
  |
  v
One SQLite Database File
```

## 3.2 Separation of concerns

- Core: application lifecycle, routing, module loading, shared infrastructure।
- Shared: Auth, Permission, Layout ও reusable UI।
- Frontend: global CSS, JS, design tokens ও assets।
- Modules: নির্দিষ্ট business domain-এর functionality।
- Database: central connection ও database file-এর protected location।
- Documentation: architecture, rules, API, status ও operational guidance।

## 3.3 Dependency direction

- Module → Core-এর public interfaces ব্যবহার করতে পারবে।
- Module → Shared services/components ব্যবহার করতে পারবে।
- Module A → Module B-এর data নিতে হলে Internal API ব্যবহার করবে।
- Module A → Module B-এর private PHP file include করবে না।
- Module A → Module B-এর private database table query করবে না।
- Core → নির্দিষ্ট কোনো business Module-এর private implementation-এর উপর নির্ভরশীল হবে না।

---

# 4. প্রস্তাবিত Root Folder Structure

নিচের structure হলো target structure। বর্তমান project-এ আগে থেকেই কাজ করা structure থাকলে AI সেটি পরীক্ষা করে migration plan দেবে; অনুমতি ছাড়া সব ফাইল নতুন structure-এ সরাবে না।

```text
MY-PROJECT/
│
├── Core/
│   ├── Bootstrap.php
│   ├── Router.php
│   ├── ModuleLoader.php
│   ├── ModuleRegistry.php
│   ├── ApiRegistry.php
│   ├── InternalApi.php
│   ├── Request.php
│   ├── Response.php
│   ├── ErrorHandler.php
│   ├── Logger.php
│   └── Exceptions/
│
├── Database/                     # AI direct file-edit protected
│   ├── Connection.php
│   ├── DatabaseManager.php
│   └── database.sqlite
│
├── Shared/
│   ├── Auth/
│   ├── Permission/
│   ├── Layout/
│   ├── UI/
│   ├── Components/
│   └── Services/
│
├── Frontend/
│   ├── DesignSystem/
│   ├── CSS/
│   ├── JS/
│   └── Assets/
│       ├── icons/
│       ├── images/
│       └── fonts/
│
├── Modules/
│   ├── Module01/
│   ├── Module02/
│   ├── Module03/
│   └── ... Module50/
│
├── Config/
├── Routes/
├── Public/
├── Storage/
│   ├── logs/
│   ├── cache/
│   ├── uploads/
│   └── backups/
│
├── Tests/
├── Documentation/
│
├── AI_RULES.md
├── PROJECT_MASTER_PLAN.md
├── PROJECT_ROADMAP.md
├── PROJECT_STATUS.md
├── MODULE_REGISTRY.md
├── ARCHITECTURE.md
├── DATABASE_RULES.md
├── INTERNAL_API.md
├── UI_DESIGN_SYSTEM.md
├── SECURITY_RULES.md
├── TESTING_RULES.md
├── DEVELOPMENT_WORKFLOW.md
├── CHANGELOG.md
├── README.md
└── index.php
```

## 4.1 Folder ownership

| Folder | Purpose | AI edit policy |
|---|---|---|
| `Core/` | Shared application engine | Explicit approval required |
| `Database/` | Central connection/database protected area | AI direct file edit prohibited |
| `Shared/` | Auth, permission, layout, shared components | Explicit approval required |
| `Frontend/` | Global design system and assets | Explicit approval required |
| `Modules/<Name>/` | Individual module implementation | Only when that module is assigned |
| `Config/` | Environment/application configuration | Approval required |
| `Routes/` | Global routes | Approval required |
| `Storage/` | Runtime data, logs, uploads, backups | Never casually edit/delete |
| `Tests/` | Test suite | Module test files may be added in assigned scope |
| `Documentation/` | Supporting documents | Update only relevant documents, preserve history |

---

# 5. Core Foundation

Core আগে তৈরি ও test করতে হবে। ৫০টি Module তৈরি হওয়ার পরে Core-এর design পরিবর্তন করা ব্যয়বহুল হবে।

## 5.1 Required Core services

1. Bootstrap: application initialization।
2. Router: URL ও request route resolution।
3. ModuleLoader: enabled Module load করা।
4. ModuleRegistry: Module metadata ও status।
5. ApiRegistry: Internal API action registration।
6. InternalApi: registered action dispatch।
7. Request: validated request data abstraction।
8. Response: consistent response format।
9. ErrorHandler: exception ও error handling।
10. Logger: structured application logging।
11. Config loader: environment-specific settings।
12. Service container বা lightweight service registry, যদি সত্যিই প্রয়োজন হয়।

## 5.2 Core acceptance criteria

- Application clean start করতে পারে।
- Invalid route-এ controlled 404 দেয়।
- Disabled Module load হয় না।
- Missing Module-এর কারণে পুরো application crash করে না।
- Duplicate Module ID বা API action detect করে।
- Permission ছাড়া protected action execute হয় না।
- Error response-এ production-এর sensitive stack trace প্রকাশ করে না।
- Core-এর public interfaces documentation-এ লেখা থাকে।

---

# 6. Central Database Architecture

## 6.1 একটিমাত্র database

পুরো application-এর business data একটি central SQLite database file-এ থাকবে।

উদাহরণ:

```text
Database/database.sqlite
```

সব Module একই central PDO SQLite connection ব্যবহার করবে। কোনো Module নিজের database file তৈরি করবে না।

## 6.2 Connection policy

- Database connection তৈরি করবে Core/Database infrastructure।
- Module-কে shared connection বা approved database service inject/pass করা হবে।
- Module নিজে নতুন PDO connection তৈরি করবে না।
- Database credentials/configuration source code-এ hardcode করা যাবে না।
- PDO prepared statements বাধ্যতামূলক।
- Database errors log হবে; user-কে sensitive query বা file path দেখানো যাবে না।

## 6.3 Table naming

প্রতিটি Module-এর table-এ unique prefix থাকবে। উদাহরণ:

- `acc_` — Accounts
- `emp_` — Employees
- `att_` — Attendance

নামগুলো চূড়ান্ত Module Registry-তে নির্ধারিত হবে। একই table name অন্য Module ব্যবহার করতে পারবে না।

## 6.4 Module table ownership

- Accounts Module শুধু `acc_` tables পরিচালনা করবে।
- Employees Module শুধু `emp_` tables পরিচালনা করবে।
- Attendance Module শুধু `att_` tables পরিচালনা করবে।
- কোনো Module অন্য Module-এর table alter/drop/delete করতে পারবে না।
- Cross-module data দরকার হলে Internal API call করবে।
- Shared infrastructure tables Core/Shared ownership-এর অধীনে থাকবে।

## 6.5 Migration policy

প্রতিটি Module নিজের migration PHP file রাখবে, যেমন:

```text
Modules/Accounts/Migrations/
  001_create_accounts_tables.php
  002_add_account_status.php
```

Migration system-এ থাকতে হবে:

- Unique migration ID
- Applied migration tracking
- Transaction যেখানে সম্ভব
- Before/after schema validation
- Safe rollback plan বা forward-fix plan
- Backup requirement for destructive changes
- Duplicate execution prevention

SQLite-এর সব DDL operation সবসময় transactional বা সহজে reversible নয়। তাই destructive migration-এর আগে backup, explicit approval এবং tested recovery plan বাধ্যতামূলক।

## 6.6 Database folder protection বনাম runtime permission

দুটি বিষয় আলাদা:

**AI file-edit restriction:** AI `Database/` folder-এর source/config/database files edit, replace, rename বা delete করবে না।

**Runtime application permission:** PHP application-কে SQLite database file এবং প্রয়োজনীয় journal/WAL files লেখার permission দিতে হতে পারে। এই permission AI-এর file-edit access নয়। Production-এ OS user, folder permission ও backup policy দিয়ে নিয়ন্ত্রণ করতে হবে।

---

# 7. Module Ownership ও Database Safety

প্রতিটি Module-এর database logic থাকবে তার নিজের folder-এ। তবে central database connection ব্যবহার করবে।

## 7.1 Allowed

- নিজের prefix-এর table create করা।
- নিজের prefix-এর column/index যোগ করা।
- নিজের data insert/update/delete করা।
- নিজের migrations লেখা ও test করা।
- অন্য Module-এর approved Internal API call করা।

## 7.2 Not allowed

- অন্য Module-এর private table সরাসরি query করা।
- অন্য Module-এর table alter/drop করা।
- `Database/` folder-এর file edit করা।
- database file replace করা।
- migration history মুছে ফেলা।
- data loss ঘটাতে পারে এমন operation approval ছাড়া চালানো।

## 7.3 Cross-module relationship

এক Module-এর data অন্য Module-এ দেখাতে হলে API contract ব্যবহার করতে হবে। প্রয়োজন হলে stable business identifier ব্যবহার হবে। Foreign key ব্যবহার করা হবে কি না তা architecture decision হিসেবে নির্ধারণ করতে হবে; cross-module private schema dependency তৈরি করা যাবে না।

---

# 8. Internal API Architecture

Module-to-Module communication-এর জন্য application-এর ভিতরের PHP Internal API ব্যবহার হবে। এটি primary inter-module interface; প্রতিটি Module-এর জন্য আলাদা public REST service বানানো লক্ষ্য নয়।

## 8.1 API action example

```text
employees.list
employees.get
employees.find_by_id
accounts.get_balance
attendance.get_daily_summary
```

এগুলো কেবল উদাহরণ; বাস্তব action name Module Registry ও API documentation-এ নিবন্ধিত হবে।

## 8.2 API requirements

প্রতিটি action-এর জন্য নির্ধারণ করতে হবে:

- Unique action name
- Owning Module
- Purpose
- Input schema
- Output schema
- Required permission
- Validation rules
- Error codes
- Data sensitivity
- Calling Module / allowed caller, যদি সীমাবদ্ধতা প্রয়োজন হয়

## 8.3 Standard response shape

Success:

```json
{
  "success": true,
  "data": {},
  "message": null,
  "error": null
}
```

Failure:

```json
{
  "success": false,
  "data": null,
  "message": "Request could not be completed.",
  "error": {
    "code": "VALIDATION_ERROR",
    "details": {}
  }
}
```

Internal response-এ PHP array/object ব্যবহার করা যেতে পারে; JSON shape public boundary বা logging/debugging contract-এ প্রয়োজনমতো serialize হবে।

## 8.4 API security

- প্রতিটি protected action permission check করবে।
- Input validate ও normalize করতে হবে।
- Caller identity/context নির্ভরযোগ্যভাবে pass করতে হবে।
- User-supplied module/action name দিয়ে arbitrary file include করা যাবে না।
- Unknown action reject করতে হবে।
- Internal API error-এ private data leak করা যাবে না।
- API action registration duplicate হলে application startup/test-এ error দিতে হবে।

---

# 9. Authentication ও Permission System

সব Module একই central Auth ও Permission infrastructure ব্যবহার করবে।

## 9.1 Authentication

- Login / Logout
- Secure password hashing (`password_hash`, `password_verify`)
- Session regeneration after login
- Secure cookie settings in production
- Session timeout policy
- Account status checks
- Password reset flow only if specified
- Login attempt throttling or equivalent protection

## 9.2 Authorization / RBAC

Role-based access control ব্যবহার করা হবে। Permission শুধু Module visibility-তে সীমাবদ্ধ নয়; page, action, API এবং sensitive operation-এও enforce করতে হবে।

উদাহরণ permission:

```text
accounts.view
accounts.create
accounts.update
accounts.delete
accounts.approve
```

## 9.3 Permission policy

- Default deny।
- নতুন Module-এর permission auto-register হতে পারে, কিন্তু default role access explicit policy অনুযায়ী দিতে হবে।
- Admin access-ও code-level permission enforcement এড়িয়ে যাবে না, যদি না documented super-admin policy থাকে।
- UI-তে button hide করা security control নয়; backend-এও permission check বাধ্যতামূলক।

---

# 10. Unified Frontend ও Design System

৫০টি Module-এর UI যেন একই application-এর অংশ মনে হয়। তাই Global Design System ও Shared Layout কেন্দ্রীয়ভাবে থাকবে।

## 10.1 Shared layout

```text
Shared/
  Layout/
    AppLayout.php
    Header.php
    Sidebar.php
    Footer.php
    Breadcrumb.php
```

Module নিজস্ব Header, Sidebar বা AppLayout বানাবে না।

## 10.2 Frontend structure

```text
Frontend/
  DesignSystem/
    tokens.css
    colors.css
    typography.css
    spacing.css
    responsive.css
  CSS/
    reset.css
    global.css
    layout.css
    components.css
    utilities.css
    themes.css
  JS/
    app.js
    sidebar.js
    modal.js
    dropdown.js
    table.js
    notifications.js
    theme.js
  Assets/
    icons/
    images/
    fonts/
```

## 10.3 Shared UI components

প্রয়োজন অনুযায়ী reusable components:

- Button
- Input
- Select
- Checkbox
- Modal
- Dropdown
- Table / DataTable
- Pagination
- Badge
- Card
- Alert
- SearchBar
- FilterBar
- PageHeader
- EmptyState
- LoadingState
- ConfirmationDialog

## 10.4 Frontend rules

- সব Module central design tokens ব্যবহার করবে।
- Font, typography, spacing, radius, shadow ও color একই standard অনুসরণ করবে।
- Icon-এর জন্য approved icon set বা project-এর existing icon method ব্যবহার করতে হবে।
- Emoji-কে functional icon হিসেবে ব্যবহার করা যাবে না।
- Random color, random font ও inconsistent button style নয়।
- Responsive layout বাধ্যতামূলক।
- Module-specific CSS প্রয়োজন হলে শুধু সেই Module-এর scope-এর মধ্যে রাখতে হবে।
- Global CSS, Shared UI বা AppLayout পরিবর্তনের প্রয়োজন হলে আগে approval চাইতে হবে।
- Shared component-এর bug থাকলে assigned Module-এর মধ্যে workaround ছড়িয়ে না দিয়ে change proposal দিতে হবে।

---

# 11. Module Auto-Discovery ও Registry

প্রতিটি Module-এর metadata standard format-এ থাকবে। Core Module folder scan করে enabled Module আবিষ্কার ও register করবে।

## 11.1 Module metadata

প্রতিটি Module-এর `module.php` বা অনুমোদিত manifest-এ থাকতে পারে:

- Module ID
- Name
- Version
- Description
- Entry point
- Enabled status
- Dependencies
- Navigation metadata
- API actions
- Permission definitions
- Migration directory

## 11.2 Auto-discovery rules

- শুধু valid Module folder load হবে।
- Disabled Module route/API expose করবে না।
- Missing dependency detect হবে।
- Circular dependency detect হবে।
- Duplicate ID detect হবে।
- Invalid manifest graceful error দেবে।
- Module load failure log হবে।
- Core কোনো Module-এর private business logic জানবে না।

---

# 12. Standard Module Folder Structure

প্রতিটি Module একই standard pattern অনুসরণ করবে।

```text
Modules/
└── Accounts/
    ├── module.php
    ├── config.php
    ├── database.php
    ├── api.php
    ├── permissions.php
    ├── README.md
    │
    ├── Pages/
    ├── Actions/
    ├── Services/
    ├── Repositories/
    ├── Migrations/
    ├── Assets/
    │   ├── css/
    │   └── js/
    └── Tests/
```

প্রতিটি ফাইল সব Module-এ বাধ্যতামূলক হবে কি না তা Template-এ নির্ধারণ করতে হবে। অপ্রয়োজনীয় empty file তৈরি করা যাবে না।

## 12.1 Folder responsibility

- `module.php`: metadata/registration।
- `config.php`: Module-specific configuration; secret নয়।
- `database.php`: নিজের table/migration registration বা database setup logic।
- `api.php`: Internal API action registration।
- `permissions.php`: Module permission definitions।
- `Pages/`: Page/controller presentation entry points।
- `Actions/`: Request action handlers।
- `Services/`: Business logic।
- `Repositories/`: নিজের table-এর data access।
- `Migrations/`: নিজের schema changes।
- `Assets/`: Scoped module CSS/JS।
- `Tests/`: Module-specific tests।
- `README.md`: Module documentation।

---

# 13. ৫০টি Module পরিকল্পনা ও তালিকা তৈরির পদ্ধতি

৫০টি Module-এর সঠিক নাম ও scope Project Owner-এর কাছ থেকে নিশ্চিত করতে হবে। AI নিজে থেকে ৫০টি business feature ধরে নিয়ে বাস্তব project scope হিসেবে স্থায়ী করবে না।

## 13.1 Module Registry-তে প্রয়োজনীয় fields

| Field | অর্থ |
|---|---|
| Module ID | স্থায়ী unique ID |
| Module Name | Module-এর নাম |
| Folder | Folder name |
| Purpose | Module কী কাজ করে |
| Owner | Business/technical ownership |
| Tables | নিজের table prefix ও table list |
| API Actions | প্রকাশিত Internal API |
| Permissions | Required permissions |
| Dependencies | অন্য Module-এর API dependency |
| Priority | Critical / High / Medium / Low |
| Status | Planned / In Progress / Testing / Complete |
| Version | Module version |
| Test Status | Not Run / Passed / Failed |
| Notes | সিদ্ধান্ত ও সীমাবদ্ধতা |

## 13.2 Dependency planning

Module development order নির্ধারণের আগে dependency map তৈরি করতে হবে। যেসব Module অন্য Module-এর API ব্যবহার করবে, তাদের provider Module আগে তৈরি অথবা API contract আগে স্থির করতে হবে।

## 13.3 Pilot modules

প্রথমে Accounts, Employees ও Attendance-এর মতো representative Module দিয়ে architecture pilot করা যেতে পারে—তবে এগুলো কেবল প্রস্তাবিত pilot, Project Owner-এর scope confirmation প্রয়োজন।

Pilot-এর উদ্দেশ্য:
- Core ও Module integration যাচাই।
- Database ownership যাচাই।
- Internal API যাচাই।
- Permission enforcement যাচাই।
- Unified UI যাচাই।
- Migration ও testing workflow যাচাই।

Pilot-এ architecture সমস্যা থাকলে ৫০টি Module-এ ছড়িয়ে দেওয়ার আগে ঠিক করতে হবে।

---

# 14. পূর্ণ Development Roadmap

## Phase 0 — Existing Project Audit ও Scope Freeze

কাজ:
1. বর্তমান source code ও folder structure inspect।
2. Existing database ও schema inspect।
3. Existing working features চিহ্নিত।
4. Broken/incomplete features আলাদা করা।
5. Project Owner-এর ৫০টি Module list সংগ্রহ।
6. Existing code preserve করার migration plan তৈরি।
7. Git baseline commit ও backup।

Deliverables:
- `PROJECT_AUDIT.md`
- `MODULE_REGISTRY.md`
- `DEPENDENCY_MAP.md`
- Updated `PROJECT_STATUS.md`

Exit criteria:
- Current state verified।
- Scope agreed।
- Destructive changes-এর তালিকা approved।

## Phase 1 — Core Foundation

কাজ:
- Bootstrap
- Router
- Module Loader
- Module Registry
- API Registry
- Internal API dispatcher skeleton
- Request/Response
- Error handling
- Logger
- Config loader

Exit criteria:
- Test Module discover ও load হয়।
- Invalid Module safely reject হয়।
- Duplicate registration detect হয়।
- Basic route ও error handling pass করে।

## Phase 2 — Central Database

কাজ:
- Central PDO SQLite connection
- Database manager
- Migration runner
- Migration history
- Table ownership conventions
- Backup/restore procedure
- Integrity test

Exit criteria:
- একটি central DB file ব্যবহার হচ্ছে।
- একাধিক Module একই connection ব্যবহার করতে পারে।
- Module migration শুধু নিজের table prefix পরিবর্তন করে।
- Backup restore test pass।

## Phase 3 — Auth, Users ও Permissions

কাজ:
- Login/Logout
- User management
- Roles
- Permissions
- Session security
- CSRF infrastructure
- Audit events

Exit criteria:
- Anonymous user protected page/action access করতে পারে না।
- Role ও permission server-side enforce হয়।
- Session security test pass।

## Phase 4 — Unified Frontend

কাজ:
- Design tokens
- Global CSS
- AppLayout
- Header/Sidebar
- Shared UI components
- Responsive navigation
- Notification foundation

Exit criteria:
- Test Module shared layout-এ render হয়।
- Mobile/desktop layout যাচাই।
- Module-specific duplicate global layout নেই।

## Phase 5 — Internal API ও Module Template

কাজ:
- API registration
- Input validation
- Permission enforcement
- Standard response/error contract
- Service invocation
- Module manifest/template
- API documentation

Exit criteria:
- একটি Module অন্য Module-এর data API দিয়ে নিতে পারে।
- Private table direct access test fail/blocked হয়।
- Unknown action reject হয়।

## Phase 6 — Pilot Modules

কাজ:
- Project Owner-approved ২–৩টি pilot Module
- Module migrations
- Backend logic
- API actions
- Permissions
- Frontend pages
- Tests
- Documentation

Exit criteria:
- End-to-end workflow pass।
- Cross-module integration pass।
- Shared UI consistency pass।
- Architecture gaps documented and resolved।

## Phase 7 — Remaining Modules (Batch Development)

প্রস্তাবিত batch:

| Batch | Scope | Exit condition |
|---|---|---|
| Batch 1 | Module 01–10 | সব Module tested ও documented |
| Batch 2 | Module 11–20 | Dependency ও integration pass |
| Batch 3 | Module 21–30 | Regression tests pass |
| Batch 4 | Module 31–40 | Shared reports/API integration pass |
| Batch 5 | Module 41–50 | Full module registry complete |

প্রতিটি Batch-এর শেষে:
- Integration testing
- Database migration review
- Permission review
- UI consistency review
- Regression testing
- Documentation update
- Git commit/tag

## Phase 8 — Cross-module Integration

কাজ:
- Dashboard widgets
- Global search
- Shared notifications
- Cross-module reports
- API dependency validation
- Navigation integration
- Activity feed

Exit criteria:
- Integration workflow test pass।
- No unauthorized direct database coupling।
- Dashboard data API দিয়ে আসে।

## Phase 9 — Security, Performance ও Quality Assurance

কাজ:
- SQL injection tests
- XSS tests
- CSRF tests
- Permission bypass tests
- File upload tests
- Session tests
- Audit log review
- SQLite integrity test
- Backup restore drill
- Performance profiling
- Regression suite

Exit criteria:
- Critical/High security issues unresolved নেই।
- Restore drill pass।
- Test report available।

## Phase 10 — Production Deployment ও Maintenance

কাজ:
- Production config
- HestiaCP/Nginx setup
- PHP extensions verification
- SQLite permissions
- HTTPS
- Backup schedule
- Log rotation
- Monitoring/health check
- Deployment documentation
- Rollback plan

Exit criteria:
- Production smoke tests pass।
- Backup ও recovery tested।
- Deployment ও rollback documented।

---

# 15. একটি Module তৈরির বাধ্যতামূলক Workflow

AI-কে কোনো Module-এর কাজ দিলে নিচের workflow অনুসরণ করতে হবে।

## Step 1 — Read project context

AI প্রথমে পড়বে:
- `DOCUMENTATION.md`
- `AI_RULES.md`
- `PROJECT_MASTER_PLAN.md`
- `PROJECT_ROADMAP.md`
- `PROJECT_STATUS.md`
- `MODULE_REGISTRY.md`
- `ARCHITECTURE.md`
- `DATABASE_RULES.md`
- `INTERNAL_API.md`
- `UI_DESIGN_SYSTEM.md`
- Assigned Module-এর `README.md`, যদি থাকে।

সব ফাইল না থাকলে বানিয়ে আছে বলে দাবি করা যাবে না। কোনটি অনুপস্থিত তা জানাতে হবে।

## Step 2 — Inspect current code

- Assigned Module-এর existing files inspect।
- Core-এর public interfaces inspect (read-only unless approved)।
- Shared UI/API/Database service-এর available contract inspect।
- Relevant tests inspect।
- Git status inspect।
- Existing working behavior preserve করতে হবে।

## Step 3 — Requirement analysis

- User-এর request ভেঙে functional requirements লিখবে।
- Non-functional requirements লিখবে।
- User roles ও permissions নির্ধারণ করবে।
- Input/output ও validation লিখবে।
- Database entities ও ownership লিখবে।
- API dependencies লিখবে।
- UI pages/states লিখবে।
- Error/edge cases লিখবে।

কোনো গুরুত্বপূর্ণ business rule অস্পষ্ট হলে অনুমান করে implementation নয়; একটি নির্দিষ্ট clarification question করবে।

## Step 4 — Impact analysis

- কোন files পরিবর্তন হবে তা তালিকা করবে।
- কোন table create/alter হবে তা তালিকা করবে।
- কোন API action যোগ হবে তা তালিকা করবে।
- অন্য Module-এর উপর impact আছে কি না জানাবে।
- Core/Shared/Frontend/Database protected files পরিবর্তনের প্রয়োজন হলে আগে approval চাইবে।

## Step 5 — Implementation plan

Coding-এর আগে সংক্ষিপ্ত plan দেবে:
1. Files to create
2. Files to modify
3. Database changes
4. API changes
5. Permission changes
6. UI changes
7. Tests
8. Risks/rollback

সাধারণ, সীমাবদ্ধ Module task হলে plan সংক্ষিপ্ত হতে পারে। বড় বা destructive task হলে explicit approval নিতে হবে।

## Step 6 — Implement within scope

- শুধু অনুমোদিত Module folder-এ কাজ করবে।
- Shared API ও UI reuse করবে।
- অন্য Module-এর private code/table ব্যবহার করবে না।
- Central database connection ব্যবহার করবে।
- Prepared statement ব্যবহার করবে।
- Permission server-side enforce করবে।
- Scoped CSS/JS ব্যবহার করবে।
- Existing files অকারণে rewrite করবে না।

## Step 7 — Test

- PHP syntax check
- Migration test
- API test
- Permission test
- Validation test
- CRUD/business workflow test
- UI responsive check
- Regression test
- Error/empty/loading states test

যে test বাস্তবে চালানো হয়নি, সেটিকে “Not run” বলতে হবে।

## Step 8 — Documentation update

Assigned Module-এর `README.md` আপডেট করবে। প্রয়োজন হলে:
- `MODULE_REGISTRY.md`
- `DEPENDENCY_MAP.md`
- `INTERNAL_API.md`
- `DATABASE_SCHEMA.md`
- `PROJECT_STATUS.md`
- `CHANGELOG.md`

Project-level file পরিবর্তন করার অনুমতি না থাকলে proposed changes আলাদা report-এ দেবে।

## Step 9 — Completion report

শেষে জানাবে:
- কী তৈরি হয়েছে
- কোন files create/modify হয়েছে
- কোন tables/migrations যোগ হয়েছে
- কোন API actions যোগ হয়েছে
- কোন permissions যোগ হয়েছে
- কোন tests চালানো হয়েছে ও ফলাফল
- কোন tests চালানো হয়নি
- Known issues
- Approval প্রয়োজন এমন pending change
- Next recommended task

---

# 16. AI-কে Module-এর কাজ কীভাবে বুঝাবেন

শুধু “Accounts Module বানাও” বললে AI যথেষ্ট business context পাবে না। প্রতিবার Module-এর কাজ দেওয়ার সময় নিচের তথ্যগুলো দিতে হবে:

1. Module name ও উদ্দেশ্য।
2. কারা ব্যবহার করবে।
3. কী কী কাজ করতে পারবে।
4. প্রতিটি কাজের business rules।
5. কোন data রাখতে হবে।
6. কোন report/filter/export দরকার।
7. কোন অন্য Module-এর সঙ্গে integration লাগবে।
8. কোন roles-এর কী permission থাকবে।
9. UI-তে কোন page, form, table, modal দরকার।
10. Validation ও error behavior।
11. Existing system-এর কোন behavior preserve করতে হবে।
12. Acceptance criteria—কখন কাজ complete ধরা হবে।

## 16.1 Module Request-এর সঠিক উদাহরণ

ভালো নির্দেশনা:

“Accounts Module-এ cash/bank account create, edit, deactivate, opening balance, current balance এবং transaction history রাখতে হবে। Accounts Officer transaction entry করতে পারবে; Manager approve করতে পারবে; Admin সব দেখতে পারবে। Employees Module থেকে employee ID/name দরকার হলে Internal API ব্যবহার করবে। Accounts Module শুধু `acc_` prefix-এর table পরিচালনা করবে। Shared AppLayout ও existing Design System ব্যবহার করবে। আগে requirement analysis ও implementation plan দেবে; approval ছাড়া Core, Shared, Database বা অন্য Module পরিবর্তন করবে না।”

দুর্বল নির্দেশনা:

“একটা সুন্দর Accounts System বানাও।”

---

# 17. Copy-ready Module Development Prompt

প্রতিবার নতুন Module-এর কাজ দেওয়ার সময় নিচের prompt কপি করে তথ্য পূরণ করবেন।

```text
তুমি এই Project-এর Senior PHP Architect, Business Analyst,
UI Developer এবং QA Engineer হিসেবে কাজ করবে।

প্রথমে Project Root-এর DOCUMENTATION.md এবং AI_RULES.md পড়বে।
তারপর সংশ্লিষ্ট Architecture, Database, API, UI এবং Module
documentation পড়বে।

PROJECT RULES:
- PHP 8.3+, SQLite, HTML, CSS, Vanilla JavaScript।
- Modular Monolith Architecture।
- একটি central SQLite database file।
- একটি central database connection।
- Module-to-Module communication শুধু Internal API দিয়ে।
- প্রতিটি Module শুধু নিজের table পরিচালনা করবে।
- Database/ folder-এর কোনো file সরাসরি edit/delete/replace করবে না।
- Core, Shared, Frontend global files বা অন্য Module-এ
  explicit approval ছাড়া কোনো পরিবর্তন করবে না।
- Existing functionality নষ্ট বা অপ্রয়োজনীয় rewrite করবে না।
- Shared AppLayout, Header, Sidebar ও Design System ব্যবহার করবে।
- অপ্রয়োজনীয় framework/package যোগ করবে না।

ASSIGNED MODULE:
Name: [MODULE NAME]
Folder: Modules/[MODULE FOLDER]

MODULE PURPOSE:
[এই Module কেন প্রয়োজন এবং কী কাজ করবে]

USERS / ROLES:
[কারা ব্যবহার করবে]

FEATURES:
1. [Feature 1]
2. [Feature 2]
3. [Feature 3]

BUSINESS RULES:
[প্রতিটি কাজের নিয়ম]

DATABASE REQUIREMENTS:
[কী কী data রাখতে হবে]
Table prefix: [যেমন acc_]
এই Module-এর বাইরে কোনো table modify করা যাবে না।

INTERNAL API / INTEGRATION:
[কোন Module থেকে কী data লাগবে]
অন্য Module-এর private table সরাসরি query করা যাবে না।

PERMISSIONS:
[কোন role কী করতে পারবে]

FRONTEND:
[প্রয়োজনীয় pages, forms, tables, filters, reports]
Existing global design system ব্যবহার করতে হবে।

VALIDATION & ERROR HANDLING:
[Required fields, duplicate rules, limits, error cases]

ACCEPTANCE CRITERIA:
[কোন কোন test/behavior pass করলে complete হবে]

REQUIRED WORKFLOW:
1. Documentation পড়ো।
2. Existing source code inspect করো।
3. Requirements ও dependencies analyze করো।
4. Files, database, API, permission ও UI impact report দাও।
5. Implementation plan দাও।
6. Scope-এর বাইরে পরিবর্তন লাগলে আগে approval চাও।
7. Approved scope-এর মধ্যে implement করো।
8. Tests চালাও।
9. Module README ও relevant documentation update করো।
10. Changed files ও test evidence সহ completion report দাও।

IMPORTANT:
প্রথমে সরাসরি coding শুরু করবে না।
প্রথম response-এ Analysis, Missing Information,
Dependencies, Impact এবং Implementation Plan দেবে।
Approval প্রয়োজন হলে approval-এর জন্য অপেক্ষা করবে।
```

## 17.1 ছোট কাজের জন্য সংক্ষিপ্ত Prompt

ছোট Bug Fix বা UI adjustment-এর জন্য:

```text
Assigned Module: [MODULE NAME]
Task: [নির্দিষ্ট কাজ]

প্রথমে relevant documentation ও existing code inspect করো।
শুধু assigned Module-এর মধ্যে কাজ করো।
Core, Database, Shared, global Frontend বা অন্য Module
পরিবর্তনের প্রয়োজন হলে আগে approval চাও।
Existing behavior preserve করো।
Fix করার পর relevant tests ও regression tests চালাও।
Module README এবং PROJECT_STATUS প্রয়োজন অনুযায়ী update করো।
কোন files পরিবর্তন হয়েছে এবং কোন tests pass/fail হয়েছে জানাও।
```

---

# 18. Change Control ও Approval Policy

AI যেন নিজের সিদ্ধান্তে architecture পরিবর্তন না করে।

## 18.1 Approval বাধ্যতামূলক

নিচের কাজের আগে Project Owner-এর অনুমতি লাগবে:

- Core-এর public interface পরিবর্তন।
- Central database connection পরিবর্তন।
- `Database/` folder-এর file পরিবর্তন।
- Existing database file replace বা delete।
- অন্য Module-এর table/schema পরিবর্তন।
- Shared Auth/Permission পরিবর্তন।
- Shared Header/Sidebar/AppLayout পরিবর্তন।
- Global CSS/Design System পরিবর্তন।
- নতুন framework বা dependency যোগ।
- Existing data migration/delete।
- API contract breaking change।
- Production configuration বা deployment access পরিবর্তন।

## 18.2 Approval request format

```text
APPROVAL REQUIRED

Reason:
কেন পরিবর্তন প্রয়োজন।

Affected files:
কোন কোন file পরিবর্তন করতে হবে।

Affected modules:
কোন Module-এ প্রভাব পড়বে।

Database impact:
কোন table/schema/data পরিবর্তন হবে।

Risk:
সম্ভাব্য সমস্যা।

Alternative:
অন্যভাবে করা সম্ভব কি না।

Rollback:
সমস্যা হলে কীভাবে আগের অবস্থায় ফিরবে।

Please approve before implementation.
```

## 18.3 No silent destructive action

- `DROP TABLE`, `DROP COLUMN`, bulk delete, file replacement বা mass rename approval ছাড়া নয়।
- Destructive migration-এর আগে verified backup লাগবে।
- Production data-এর উপর test migration চালানো যাবে না।
- AI নিজে থেকে user data reset করতে পারবে না।

---

# 19. Security Requirements

নিচের বিষয়গুলো পুরো application-এ বাধ্যতামূলক।

## 19.1 Backend

- PDO prepared statements।
- Input validation ও normalization।
- Output escaping।
- Server-side authorization।
- Secure session management।
- CSRF token for state-changing requests।
- File upload MIME/size/extension validation।
- Upload file executable path-এর বাইরে রাখা।
- Sensitive error detail production user-কে না দেখানো।
- Secrets source control-এ commit না করা।

## 19.2 Frontend

- User data HTML-এ render করার আগে escape।
- Untrusted string-কে `innerHTML`-এ বসানো এড়িয়ে চলা।
- Form validation backend-এ পুনরায় করতে হবে।
- Hidden button/field-কে permission security হিসেবে গণ্য করা যাবে না।

## 19.3 Audit

প্রয়োজনীয় sensitive operation-এর জন্য audit event:
- User ID
- Action
- Target record
- Timestamp
- Result
- Relevant metadata

Password, session token বা secret audit log-এ রাখা যাবে না।

---

# 20. Testing ও Quality Assurance

## 20.1 Test categories

1. Syntax tests
2. Unit tests
3. Module functional tests
4. Database migration tests
5. Internal API tests
6. Permission tests
7. Integration tests
8. UI responsive tests
9. Security tests
10. Regression tests
11. Backup/restore tests

## 20.2 Minimum per-module tests

- Valid create
- Invalid create
- Duplicate record
- Update
- Delete/deactivate
- Search/filter
- Pagination
- Permission denied
- Missing/invalid input
- Empty state
- API success
- API failure
- Cross-module API access
- Migration first run
- Migration repeated run

## 20.3 Test report format

```text
MODULE TEST REPORT

Module:
Version:
Date:

PHP Syntax: PASS / FAIL / NOT RUN
Migration: PASS / FAIL / NOT RUN
CRUD: PASS / FAIL / NOT RUN
Validation: PASS / FAIL / NOT RUN
Permissions: PASS / FAIL / NOT RUN
Internal API: PASS / FAIL / NOT RUN
UI: PASS / FAIL / NOT RUN
Regression: PASS / FAIL / NOT RUN

Known Issues:
Evidence:
Final Status: COMPLETE / INCOMPLETE / BLOCKED
```

“PASS” শুধু বাস্তবে test চালিয়ে সফল হলে লেখা যাবে।

---

# 21. Documentation ও Project Memory

Documentation হবে project-এর written source of truth।

## 21.1 Recommended root Markdown files

| File | Purpose |
|---|---|
| `DOCUMENTATION.md` | এই সম্পূর্ণ master specification |
| `README.md` | Project overview ও installation |
| `AI_RULES.md` | AI-এর বাধ্যতামূলক নিয়ম |
| `PROJECT_MASTER_PLAN.md` | Scope, goals ও architecture summary |
| `PROJECT_ROADMAP.md` | Phase ও milestone |
| `PROJECT_STATUS.md` | Verified progress |
| `PROJECT_MEMORY.md` | স্থায়ী decisions ও context |
| `ARCHITECTURE.md` | Technical architecture |
| `MODULE_REGISTRY.md` | ৫০টি Module registry |
| `DEPENDENCY_MAP.md` | Module dependencies |
| `DATABASE_RULES.md` | Database safety ও ownership |
| `DATABASE_SCHEMA.md` | Table/schema inventory |
| `MIGRATION_RULES.md` | Migration conventions |
| `INTERNAL_API.md` | API contracts |
| `FUNCTION_REGISTRY.md` | Registered actions/services |
| `UI_DESIGN_SYSTEM.md` | Frontend consistency rules |
| `COMPONENT_GUIDE.md` | Shared UI usage |
| `SECURITY_RULES.md` | Security standards |
| `PERMISSION_MATRIX.md` | Role-permission matrix |
| `TESTING_RULES.md` | QA standards |
| `DEVELOPMENT_WORKFLOW.md` | AI workflow |
| `BUG_FIX_PROTOCOL.md` | Bug diagnosis/fix process |
| `FEATURE_DEVELOPMENT.md` | New feature workflow |
| `CODE_STANDARDS.md` | Code conventions |
| `BACKUP_RESTORE.md` | Recovery procedure |
| `DEPLOYMENT.md` | Production deployment |
| `TROUBLESHOOTING.md` | Common issues |
| `CHANGELOG.md` | Version history |

সব ফাইল শুরুতেই সম্পূর্ণ লেখা বাধ্যতামূলক নয়। এই master document হলো মূল নির্দেশিকা; সংশ্লিষ্ট Phase শুরু হলে supporting document বিস্তারিত হবে।

## 21.2 Project status rules

প্রতিটি Module-এর status হবে:

- `PLANNED`
- `ANALYSIS`
- `IN_PROGRESS`
- `BLOCKED`
- `TESTING`
- `COMPLETE`
- `MAINTENANCE`

`COMPLETE` দিতে হলে code, integration, permission, tests ও documentation-এর evidence থাকতে হবে।

---

# 22. Git, Backup ও Recovery

## 22.1 Git rules

- কাজ শুরুর আগে `git status` পরীক্ষা।
- অজানা user changes overwrite করা যাবে না।
- Baseline commit রাখতে হবে।
- ছোট, logical commit করতে হবে।
- Feature branch ব্যবহার করা ভালো।
- Destructive change-এর আগে backup/commit।
- Secrets, production DB ও sensitive uploads commit করা যাবে না।

## 22.2 Backup rules

- Database backup নিয়মিত নিতে হবে।
- Backup file-এর timestamp ও version রাখতে হবে।
- Backup integrity যাচাই করতে হবে।
- শুধু backup তৈরি নয়, restore test-ও করতে হবে।
- Backup storage web root-এর বাইরে বা protected location-এ রাখতে হবে।
- Backup retention policy Project Owner নির্ধারণ করবেন।

---

# 23. Deployment ও Operations

Production deployment-এর আগে যাচাই:

- PHP 8.3+ active।
- PDO SQLite extension enabled।
- Nginx/PHP-FPM configuration valid।
- HTTPS active।
- SQLite file ও required directory-এর correct permissions।
- `Storage/` web-accessible নয়।
- Debug mode off।
- Production error log enabled।
- Backup schedule active।
- Upload restrictions configured।
- Production smoke test complete।
- Rollback procedure documented।

HestiaCP environment-এ actual PHP-FPM user, file ownership ও permission inspect না করে permission command অনুমান করে দেওয়া যাবে না।

---

# 24. Project Status ও Completion Criteria

## 24.1 Project-level status report

প্রতি review-তে নিচের format:

```text
PROJECT STATUS REPORT

Report Date:
Current Phase:
Overall Status:

Completed:
- Verified completed items

In Progress:
- Currently active work

Pending:
- Not started items

Blocked:
- Blocker and required decision

Tests:
- Passed
- Failed
- Not run

Architecture Compliance:
- Core boundary
- Database ownership
- Internal API
- Auth/Permission
- Shared UI

Risks:
Next 3 Actions:
Evidence / Files Inspected:
```

## 24.2 Verified vs assumed

AI অবশ্যই আলাদা করবে:

- **Verified:** source code/test/database/file inspection দিয়ে নিশ্চিত।
- **Reported:** user জানিয়েছেন, কিন্তু independently verify করা হয়নি।
- **Assumed:** সম্ভাব্য ধারণা, নিশ্চিত নয়।
- **Not checked:** পরীক্ষা করা হয়নি।

## 24.3 Module completion definition

একটি Module complete হবে কেবল যখন:

1. Requirement পূরণ হয়েছে।
2. Module registry-তে নিবন্ধিত।
3. Own-table migrations কাজ করছে।
4. Internal API contract ঠিক আছে।
5. Permissions enforce হচ্ছে।
6. Shared frontend layout ব্যবহার করছে।
7. Required workflow tests pass।
8. Regression tests pass।
9. README/documentation updated।
10. Known issues report করা হয়েছে।

---

# 25. ঝুঁকি, সিদ্ধান্ত ও নিষিদ্ধ পদ্ধতি

## 25.1 প্রধান ঝুঁকি

| Risk | Mitigation |
|---|---|
| AI পুরোনো code rewrite করে ফেলে | Scope restriction, Git baseline, diff review |
| Module অন্য Module-এর table ব্যবহার করে | API-only cross-module rule |
| UI inconsistency | Central design system ও shared components |
| Core বারবার পরিবর্তন | Core contract আগে freeze ও approval |
| Migration-এ data loss | Backup, review, staged test |
| Permission bypass | Server-side authorization tests |
| Documentation outdated | প্রতিটি Module completion-এ update |
| বড় task-এ context হারানো | Master docs, status, handover report |
| SQLite write/locking সমস্যা | Short transactions, indexes, workload testing |

## 25.2 নিষিদ্ধ পদ্ধতি

- সব ৫০টি Module একবারে তৈরি করা।
- অনুমতি ছাড়া Core rewrite করা।
- `Database/` folder edit করা।
- একাধিক database file তৈরি করা।
- অন্য Module-এর private table query করা।
- অন্য Module-এর PHP file include করে private logic ব্যবহার করা।
- প্রতিটি Module-এ আলাদা global CSS/Layout বানানো।
- UI-তে button hide করে permission complete দাবি করা।
- Test না চালিয়ে PASS লেখা।
- Existing data delete বা reset করা।
- User-এর business rule অনুমান করে final implementation করা।
- Documentation না দেখে নতুন AI session-এ কাজ শুরু করা।

---

# 26. Project শুরু করার প্রথম কাজ

নিচের ক্রমে শুরু করুন:

1. বর্তমান project folder ও source code audit।
2. ৫০টি Module-এর নাম ও scope চূড়ান্ত।
3. Dependency map তৈরি।
4. Root structure ও naming convention freeze।
5. AI rules ও file-edit protection setup।
6. Git repository ও baseline backup।
7. Core foundation design ও implementation।
8. Central SQLite architecture ও migration tests।
9. Auth ও Permission system।
10. Unified Frontend Design System।
11. Internal API ও Module Template।
12. Pilot Modules।
13. Pilot approval-এর পর বাকি ৪৭টি Module batch-wise।

**গুরুত্বপূর্ণ:** Existing project থাকলে audit ছাড়া নতুন করে পুরো application generate করবেন না। আগে কী আছে, কী কাজ করে, কী ভাঙা—তা যাচাই করতে হবে।

---

# 27. Module Request Template

নতুন Module assign করার সময় এই ফর্মটি পূরণ করে AI-কে দিন।

```text
MODULE REQUEST

1. Module Name:
2. Module Folder:
3. Module Purpose:
4. Main Users/Roles:
5. Features:
6. Business Rules:
7. Data Fields:
8. Table Prefix:
9. Required Pages:
10. Required Reports:
11. Search / Filters / Export:
12. Permissions:
13. Internal API Dependencies:
14. Other Modules Depending on This:
15. Validation Rules:
16. Error / Edge Cases:
17. Existing behavior to preserve:
18. Acceptance Criteria:
19. Priority:
20. Additional Notes:

AI must follow DOCUMENTATION.md and AI_RULES.md.
First analyze and propose a plan. Do not start implementation
until the required plan/approval conditions are satisfied.
```

সব field জানা না থাকলে যা জানা আছে তা দিন। AI-কে অনুপস্থিত গুরুত্বপূর্ণ business rule চিহ্নিত করে clarification চাইতে হবে; অপ্রয়োজনীয় প্রশ্ন করা যাবে না।

---

# 28. Final Acceptance Checklist

Project production-ready ঘোষণা করার আগে:

- [ ] ৫০টি Module-এর approved registry আছে।
- [ ] Core foundation stable।
- [ ] Module auto-discovery কাজ করে।
- [ ] Central SQLite database ব্যবহার হয়।
- [ ] Database ownership enforce করা হয়।
- [ ] Versioned migrations tested।
- [ ] Internal API registration ও authorization কাজ করে।
- [ ] Auth ও RBAC server-side enforce হয়।
- [ ] Shared AppLayout ও Design System সব Module-এ ব্যবহৃত।
- [ ] Module-specific UI global style ভাঙে না।
- [ ] সব Module-এর README ও registry updated।
- [ ] Critical workflows-এর integration tests pass।
- [ ] Security tests pass।
- [ ] Backup restore test pass।
- [ ] Production deployment ও rollback documented।
- [ ] Known issues ও limitations প্রকাশ করা হয়েছে।

---

# Appendix A — প্রতিটি AI Session-এর শুরুতে ব্যবহারযোগ্য Prompt

```text
এই Project-এ কাজ শুরু করার আগে Project Root-এর
DOCUMENTATION.md এবং AI_RULES.md সম্পূর্ণ পড়ো।

এরপর PROJECT_STATUS.md, MODULE_REGISTRY.md এবং
যে Module-এ কাজ করতে বলা হয়েছে তার README.md পড়ো।

প্রথমে existing code inspect করো। আগের কাজ, user changes
বা working feature নষ্ট করবে না।

Assigned scope-এর বাইরে কোনো file পরিবর্তন করবে না।
Core, Database, Shared, global Frontend বা অন্য Module-এ
পরিবর্তন প্রয়োজন হলে আগে কারণ, affected files, risks ও
rollback plan দিয়ে approval চাইবে।

প্রথমে requirement analysis, dependency analysis ও
implementation plan দেবে। তারপর অনুমোদিত কাজ implement করবে।
শেষে tests চালাবে, documentation update করবে এবং
verified completion report দেবে।

যা inspect বা test করোনি, তা complete বা pass বলে দাবি করবে না।
```

# Appendix B — Module কাজ শেষ হওয়ার পর Handover Prompt

```text
এই Module-এর কাজের handover report তৈরি করো।

জানাও:
1. Implemented features
2. Created files
3. Modified files
4. Database tables and migrations
5. Internal API actions
6. Permissions
7. Dependencies
8. Tests actually executed and their results
9. Tests not executed
10. Known bugs and limitations
11. Documentation updated
12. Pending approvals
13. Git changes/commit status, যদি যাচাই করা হয়ে থাকে
14. Recommended next task

PROJECT_STATUS.md ও Module README-এর proposed update দেখাও।
অনুমতি ছাড়া protected project files পরিবর্তন করবে না।
```

---

## শেষ নির্দেশনা

এই `DOCUMENTATION.md`-কে পুরো Project-এর প্রধান planning ও development reference হিসেবে ব্যবহার করুন। তবে বাস্তব source code, approved business requirements, migration history এবং test results-ই implementation-এর সত্যতা নির্ধারণ করবে।

প্রতিটি নতুন Module-এর ক্ষেত্রে:

**Documentation পড়ুন → Module Request দিন → Analysis নিন → Plan অনুমোদন করুন → Implement করুন → Test করুন → Document করুন → Status update করুন।**

এই নিয়ম মেনে চললে ৫০টি Module আলাদা আলাদা তৈরি হলেও পুরো application-এর Architecture, Database, Security এবং Frontend consistency বজায় রাখা অনেক সহজ হবে।
