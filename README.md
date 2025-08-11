# Data Gateway

Data Gateway is a **Laravel 10.x-based service** designed to provide a unified access point to third-party AI services (such as **OpenAI** and **Gemini**) within the Mynet ecosystem.  
It acts as a gateway, handling requests, logging, and analytics for AI interactions.

---

## Table of Contents
- [Features](#features)
- [Project Structure](#project-structure)
- [Installation](#installation)
  - [Docker Setup (Recommended)](#docker-setup-recommended)
  - [Manual Setup](#manual-setup)
- [Configuration](#configuration)
  - [Environment Variables](#environment-variables)
- [Usage](#usage)
- [API Endpoints](#api-endpoints)
- [AI Logs & Visualization](#ai-logs--visualization)
- [Testing & Quality](#testing--quality)
- [Development](#development)
- [Contributing](#contributing)
- [License](#license)

---

## Features
- Unified API for multiple AI providers (**OpenAI**, **Gemini**)
- Conversation session management
- AI request/response logging (`database/migrations/2025_08_07_090432_create_ai_logs_table.php`)
- Log filtering and visualization (`resources/views/ai_logs/index.blade.php`)
- CSV report generation for logs
- **Docker** support for local development (Redis + MySQL + App)
- Statistics page for AI usage

---

## Project Structure
```
app/Services
  ├── OpenAiService.php      # Handles OpenAI API interactions
  └── GeminiService.php      # Handles Gemini API interactions
app/Console/Commands
  ├── ChatGptStats.php       # Generates and exports AI usage stats
  └── ChatGptLogsAndStats.php# Processes and analyzes AI logs
routes/api.php               # API endpoints for OpenAI and Gemini
resources/views
  ├── ai_logs/index.blade.php # AI logs dashboard
  └── welcome.blade.php       # Main chat UI
config/mcp.php               # MCP server configuration
database/migrations          # Database schema definitions
public                       # Entry point for web requests
.env.example                 # Example environment config
```

---

## Installation

### Docker Setup (Recommended)
This project includes a `docker-compose.yml` that sets up:
- **Redis** (caching & queue)
- **MySQL 8.0** (relational database)
- **Laravel App** (served on port `8000`)

**Steps:**
```bash
# 1. Clone the repository
git clone <your-repo-url>
cd data-gateway

# 2. Copy environment file
cp .env.example .env

# 3. Set your API keys & DB credentials in .env

# 4. Build and start containers
docker-compose up -d --build

# 5. Run migrations
docker-compose exec app php artisan migrate

# 6. Access the app
# Chat UI: http://localhost:8000
# AI Logs Dashboard: http://localhost:8000/ai-logs
```

---

### Manual Setup
**Requirements:**
- PHP 8.1+
- Composer
- Node.js 18+
- MySQL 8.0
- Redis

**Steps:**
```bash
# 1. Clone repo
git clone <your-repo-url>
cd data-gateway

# 2. Install dependencies
composer install
npm install

# 3. Copy env and configure
cp .env.example .env

# 4. Generate app key
php artisan key:generate

# 5. Run migrations
php artisan migrate

# 6. Serve app
php artisan serve
```

---

## Configuration

### Environment Variables
| Variable | Description | Example |
|----------|-------------|---------|
| `APP_NAME` | Laravel app name | `Data Gateway` |
| `APP_ENV` | Environment type | `local` |
| `APP_KEY` | Laravel encryption key | _(generated)_ |
| `APP_DEBUG` | Enable debug mode | `true` |
| `APP_URL` | App base URL | `http://localhost:8000` |
| `DB_CONNECTION` | Database type | `mysql` |
| `DB_HOST` | Database host | `mysql` (Docker) or `127.0.0.1` |
| `DB_PORT` | Database port | `3306` |
| `DB_DATABASE` | Database name | `data_gateway` |
| `DB_USERNAME` | Database username | `root` |
| `DB_PASSWORD` | Database password | `password` |
| `REDIS_HOST` | Redis host | `redis` |
| `OPENAI_API_KEY` | API key for OpenAI | _your key_ |
| `GEMINI_API_KEY` | API key for Gemini | _your key_ |

---

## Usage

### Web Interface
- **Chat UI**: `/`
- **AI Logs Dashboard**: `/ai-logs` (filter, search, visualize, export CSV)

---

## API Endpoints
All endpoints require valid API keys in `.env`.

**OpenAI**
- `POST /api/openai/chat` — Send chat messages
- `GET /api/openai/models` — List available models

**Gemini**
- `POST /api/gemini/chat` — Send chat messages
- `GET /api/gemini/models` — List available models

---

## AI Logs & Visualization
- All requests/responses are logged in `ai_logs` table.
- `/ai-logs` page provides:
  - Filter by date, provider, model, session ID
  - Pie chart of provider usage
  - Classification chart of log types
  - CSV export via artisan command

---

## Testing & Quality
```bash
# Run tests
php artisan test
```

---

## Development
- Use **`develop`** branch for feature work.
- Keep `main` branch stable.

---

## Contributing
1. Fork the repository
2. Create a feature branch from `develop`
3. Commit changes
4. Open a pull request

---

## License
MIT License. See [LICENSE](LICENSE) for details.
