🚗 RouteShare — Production-Grade Carpooling SaaS
A full-stack carpooling platform built with React, TypeScript, Node.js, PostgreSQL, Redis, and Socket.IO.


Tech Stack
Layer	Technology
Frontend	React 18, TypeScript, Tailwind CSS, Redux Toolkit, React Query, Socket.IO Client
Backend	Node.js, Express.js, TypeScript, Socket.IO
Database	PostgreSQL 15 (Prisma ORM)
Cache	Redis 7
Auth	JWT + Refresh Token rotation
DevOps	Docker, Nginx, GitHub Actions CI/CD
Design Patterns
Repository Pattern — BaseRepository<T> with per-entity implementations
Service Layer — business logic isolated from controllers
Strategy Pattern — RouteOverlapStrategy, PriceOptimizationStrategy for ride matching
Observer Pattern — NotificationBus (EventEmitter) + Socket.IO for real-time events
Factory Pattern — VehicleFactory for vehicle creation and validation
Singleton Pattern — Database, RedisClient instances

routeshare/
├── apps/
│   ├── api/                   # Express + Node.js backend
│   │   ├── src/
│   │   │   ├── config/        # DB (Singleton), Redis (Singleton)
│   │   │   ├── middleware/    # auth, validate, error
│   │   │   ├── modules/
│   │   │   │   ├── auth/      # JWT + refresh token
│   │   │   │   ├── rides/     # CRUD + Strategy matching
│   │   │   │   ├── requests/  # seat request workflow
│   │   │   │   ├── notifications/ # Observer + Socket.IO
│   │   │   │   ├── carbon/    # CO₂ calculation + leaderboard
│   │   │   │   ├── vehicles/  # Factory Pattern
│   │   │   │   ├── ratings/   # reviews + aggregation
│   │   │   │   ├── users/     # profile + cache
│   │   │   │   ├── sos/       # SOS + location broadcast
│   │   │   │   └── admin/     # RBAC admin endpoints
│   │   │   ├── shared/        # BaseRepository, AppError, response helpers
│   │   │   └── socket/        # Socket.IO event handlers
│   │   └── prisma/            # schema.prisma + migrations
│   └── web/                   # React + TypeScript frontend
│       └── src/
│           ├── app/           # Redux store + React Query client
│           ├── components/
│           │   ├── ui/        # 13 reusable components
│           │   ├── rides/     # RideCard, SearchForm, PostForm, RequestModal
│           │   ├── tracking/  # LiveMap, SOSButton
│           │   ├── layout/    # Sidebar, Topbar, AppLayout
│           │   └── dashboard/ # BarChart
│           ├── hooks/         # useRides, useAuth, useTracking, useNotifications, useCarbon
│           ├── modules/       # Redux slices (auth, rides, ui)
│           ├── pages/         # 10 full pages
│           ├── services/      # API client (axios + auto-refresh), socket, per-module APIs
│           └── types/         # Full TypeScript interfaces
├── docker-compose.yml
├── nginx.conf
└── .github/workflows/ci.yml
API Endpoints
Method	Endpoint	Auth	Description
POST	/api/auth/register	—	Register new user
POST	/api/auth/login	—	Login + token issue
POST	/api/auth/refresh	RefreshToken	Rotate access token
GET	/api/rides	JWT	Search rides with filters
POST	/api/rides	JWT (Driver)	Create ride listing
POST	/api/requests	JWT	Request a seat
PATCH	/api/requests/:id/accept	JWT (Driver)	Accept request
GET	/api/carbon/stats	JWT	User carbon dashboard
POST	/api/sos	JWT	Trigger SOS + location broadcast
GET	/api/admin/users	JWT (Admin)	User management
Socket Events
Event	Direction	Description
driver:location_update	Client→Server	Live GPS broadcast
ride:location	Server→Client	Location to passengers
passenger:join_ride	Client→Server	Subscribe to ride room
sos:trigger	Client→Server	Emergency broadcast
notification	Server→Client	Real-time notification
ride:chat	Bidirectional	In-ride messaging
Testing
cd apps/api && npm test          # Jest unit tests
cd apps/web && npm test          # Vitest unit tests
cd apps/api && npm run type-check
cd apps/web && npm run type-check
