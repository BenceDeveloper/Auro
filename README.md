# AURO - Film Közösségi Platform
## Teljes Projektdokumentáció

---

## 📋 Tartalomjegyzék
1. [Projektáttekintés](#projektáttekintés)
2. [Technológiai Stack](#technológiai-stack)
3. [Rendszerarchitektúra](#rendszerarchitektúra)
4. [Funkcionális Modulok](#funkcionális-modulok)
5. [Telepítés és Futtatás](#telepítés-és-futtatás)
6. [API Dokumentáció](#api-dokumentáció)
7. [Fejlesztői Iránymutatások](#fejlesztői-iránymutatások)
8. [Teljesítmény és Optimalizálás](#teljesítmény-és-optimalizálás)

---

## 🎯 Projektáttekintés

### Összefoglalás
Az **AURO** egy modern, többrétegű film közösségi platform, amely lehetővé teszi a felhasználók számára:
- Filmek böngészése és keresése
- Filmek megosztása és értékelése
- Közösségi interakciók (kommentelés, üzenetküldés)
- Baráti kapcsolatok kezelése
- Személyi kedvenc filmek gyűjtése

### Cél és Célközönség
Az alkalmazás szórakoztatóipari rajongók, kritikusok és casual felhasználók számára készült, akik szeretnének filmes tapasztalataikat megosztani egy közösség része.

### Kulcs Jellemzők
- ✅ Real-time üzenetküldés WebSocket-tel
- ✅ JWT token-alapú biztonságos autentikáció
- ✅ Fájlfeltöltés és médiamanagment
- ✅ Szűrés és keresés funkcionalitás
- ✅ Responsive design mobil és asztali eszközökhöz
- ✅ Containerizált microservices architecture

---

## 🛠 Technológiai Stack

### Frontend
| Technológia | Verzió | Célfeladat |
|-------------|--------|-----------|
| **React** | 19.2.0 | UI komponensek és state management |
| **Vite** | 5.x | Gyors fejlesztői szerver és bundler |
| **Tailwind CSS** | 4.1.18 | Utility-first CSS keretrendszer |
| **DaisyUI** | 5.5.18 | Előépített Tailwind komponensek |
| **React Router** | 7.13.0 | Kliens oldali routing |
| **Axios** | 1.13.5 | HTTP kliens |
| **React Slick** | 0.31.0 | Carousel/slider komponensek |
| **Lucide React** | 0.563.0 | SVG ikon könyvtár |

**Build Tool:** Vite fejlesztői szerver (HMR támogatás)
**Stílus:** Tailwind CSS utility classes + DaisyUI komponensek

### Backend (Python - FastAPI)
| Technológia | Verzió | Célfeladat |
|-------------|--------|-----------|
| **FastAPI** | 0.104.1 | Async REST API keretrendszer |
| **Uvicorn** | 0.24.0 | ASGI szerver |
| **SQLAlchemy** | 2.0.23 | ORM és adatbázis absztrakció |
| **Alembic** | 1.12.1 | Adatbázis verziókezelés |
| **Pydantic** | 2.5.0 | Adatvalidáció és schema |
| **WebSockets** | 12.0 | Real-time kommunikáció |
| **Asyncmy** | 0.2.9 | Async MySQL driver |
| **Python-multipart** | 0.0.6 | Fájlfeltöltés kezelés |

**Autentikáció:** JWT tokenek (python-jose)
**Biztonsági:** BCrypt jelszóhasználát

### Backend (PHP)
| Technológia | Verzió | Célfeladat |
|-------------|--------|-----------|
| **PHP** | 8.x | Fő nyelvezet |
| **Composer** | 2.x | Függőségkezelő |
| **Firebase JWT** | Latest | JWT token kezelés |
| **PHPMailer** | Latest | Email küldés |
| **PDO MySQL** | Native | Adatbázis kapcsolat |

**Funkciók:** Google OAuth integrációval, email verifikáció, profil kezelés

### Adatbázis
- **MySQL** 8.0
- **Adatbázis név:** cinehub
- **Karakterkészlet:** UTF-8MB4 (multilingual támogatás)

### Infrastruktúra
- **Containerizáció:** Docker & Docker Compose
- **Verziókezelés:** Git
- **Port Konfiguráció:**
  - Frontend: `5173` (Vite dev server)
  - FastAPI Backend: `8000`
  - PHP Backend: `8081`
  - MySQL: `3306`

---

## 🏗 Rendszerarchitektúra

### Magas szintű felépítés

```
┌─────────────────────────────────────────────────────────────┐
│                     AURO Platform                            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────────────┐      ┌──────────────────────┐    │
│  │   React + Vite       │      │  Tailwind + DaisyUI  │    │
│  │   Frontend (5173)    │◄────►│  Responsive Design   │    │
│  └──────────┬───────────┘      └──────────────────────┘    │
│             │                                               │
│             │ HTTPS/API Calls (Axios)                      │
│             ▼                                               │
│  ┌─────────────────────────────────────────────────────┐  │
│  │           API Gateway & Load Balancing              │  │
│  └─────────────┬──────────────────────────┬────────────┘  │
│                │                          │                │
│        ┌───────▼────────┐      ┌──────────▼────────┐      │
│        │   FastAPI      │      │   PHP Backend      │      │
│        │ (8000) - Core  │      │ (8081) - Auth      │      │
│        └───┬────────────┘      └──────────┬────────┘      │
│            │                             │                │
│            │  Movies, Comments,          │ JWT            │
│            │  Favorites, Messages,       │ Email Verify   │
│            │  Friends, WebSocket         │ OAuth Google   │
│            │                             │                │
│        ┌───┴─────────────────────────────┴────────┐       │
│        │                                           │       │
│        └──────────────────┬──────────────────────┘       │
│                          │                              │
│                ┌─────────▼─────────┐                   │
│                │    MySQL 8.0      │                   │
│                │   (cinehub db)    │                   │
│                └───────────────────┘                   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Microservices Megközelítés
- **PHP mikro-szervíz:** Autentikációra és felhasználó-kezelésre optimalizált
- **FastAPI szervíz:** Üzleti logika és content kezelésre optimalizált
- **Shared Database:** Központi adattárolás (táblaszigetesítés)

### Communication Patterns
1. **JWT-alapú autentikáció:** Tokenek hitelesítésére
2. **REST API:** Szinkron kommunikáció filmek, kommentek, kedvencek között
3. **WebSocket:** Real-time üzenetküldés

---

## 📦 Funkcionális Modulok

### 1. Film Kezelés (Movies Module)

#### Végpontok
```
GET  /movies/list_movies           - Filmek listázása szűrésekkel
GET  /movies/one_movie/{id}        - Egy film részletei
GET  /movies/top_movie             - Top 10 film az elmúlt 3 évből
GET  /movies/movie_actors/{id}     - Film szereplői
GET  /movies/movie_crew/{id}       - Film stábja (fejlesztés alatt)
```

#### Szűrési Lehetőségek
- **Cím szerinti keresés** - Fuzzy matching támogatás
- **Műfaj szerinti szűrés** - 19 kategória (Akció, Drama, Horror, stb.)
- **Oldalazás** - Lapozható eredmények

### 2. Komment Rendszer (Comments Module)

#### Végpontok
```
POST   /comments/create            - Új hozzászólás létrehozása
GET    /comments/{movie_id}        - Film összes hozzászólása
PUT    /comments/{comment_id}      - Hozzászólás módosítása
DELETE /comments/{comment_id}      - Hozzászólás törlése
```

#### Funkciók
- Felhasználó autentikáció szükséges
- Szülő/gyermek hozzászólások (threading)
- Timestamp követés (created_at, updated_at)
- Tartalom validáció és szanitizálás

### 3. Kedvenyek Kezelése (Favorites Module)

#### Végpontok
```
POST   /favorites/add/{movie_id}   - Film hozzáadása kedvencekhez
DELETE /favorites/{movie_id}       - Film eltávolítása kedvencekből
GET    /favorites/get              - Felhasználó kedvencei
GET    /favorites/check/{movie_id} - Ellenőrzés: kedvenc-e
```

#### Funkciók
- Gyors filmek hozzáadása/eltávolítása
- Kedvenc filmek listázása felhasználónként
- Cached favorit állapot frontend-en

### 4. Üzenetküldés (Real-time Messages)

#### Technológia: WebSocket
```python
WebSocket Endpoints:
ws://localhost:8000/ws/chat/{user_id}
```

#### Funkciók
- Real-time 1:1 beszélgetések
- Üzenet előzmények
- User listing (online status)
- Üzenetkézbesítési acknowledgement

### 5. Baráti Kapcsolatok (Friends Module)

#### Végpontok
```
POST   /friends/send_request/{user_id}    - Barátkérés küldése
POST   /friends/accept/{user_id}          - Barátkérés elfogadása
POST   /friends/decline/{user_id}         - Barátkérés elutasítása
DELETE /friends/{user_id}                 - Barát eltávolítása
GET    /friends/list                      - Barátok listája
GET    /friends/requests                  - Függőben lévő kérések
```

#### Adatmodell
```python
class Friendship:
    user_id: int
    friend_id: int
    status: Literal["pending", "accepted", "blocked"]
    created_at: datetime
    accepted_at: Optional[datetime]
```

### 6. Autentikáció (PHP Backend)

#### Végpontok
```
POST   /register                   - Új felhasználó regisztrálása
POST   /login                      - Felhasználó bejelentkezés
POST   /email-verify               - Email cím verifikálása
POST   /google-oauth               - Google OAuth bejelentkezés
POST   /refresh-token              - Token frissítése
```

#### Biztonsági Funkcionalitások
- **JWT Tokenek:** Access + Refresh token párok
- **Email Verifikáció:** Biztonságos regisztrálás
- **Google OAuth 2.0:** Harmadik fél autentikáció
- **Jelszó hasitás:** BCrypt + salt
- **CORS Védelem:** Corss-origin requests korlátozása

---

## 🚀 Telepítés és Futtatás

### Előfeltételek
```
- Docker Desktop (Windows/Mac) vagy Docker Engine (Linux)
- Docker Compose 1.29+
- Git
- Szabad portok: 5173, 8000, 8081, 3306
```

### Automatizált Telepítés (Ajánlott)

#### 1. Repository klónozása
```bash
git clone <repository-url>
cd Auro
```

#### 2. Docker Compose indítása
```bash
docker-compose up -d
```

Ez automatikusan:
- MySQL adatbázist elindít (`cinehub.sql` inicializálásával)
- PHP backend szervezet (`port 8081`)
- FastAPI backend szServlet (`port 8000`)
- React frontend szerver (`port 5173`)

#### 3. Kész!
```
Frontend:   http://localhost:5173
PHP API:    http://localhost:8081
FastAPI:    http://localhost:8000
```

### Lokális Fejlesztési Telepítés

#### Frontend
```bash
cd Frontend
npm install
npm run dev
# Elérhető: http://localhost:5173
```

#### FastAPI Backend
```bash
cd FastAPI_backend
python -m venv venv
# Windows: .\venv\Scripts\activate
# Linux/Mac: source venv/bin/activate

pip install -r requirements.txt
uvicorn main:app --reload
# Elérhető: http://localhost:8000
```

#### PHP Backend
```bash
cd PHPBackend
composer install
php -S localhost:8081
```

#### Adatbázis
```bash
# MySQL inicializáció
mysql -u root -p < Database/cinehub.sql
```

---

## 📡 API Dokumentáció

### FastAPI Auto-Generated Dokumentáció
```
http://localhost:8000/docs        (Swagger UI)
http://localhost:8000/redoc       (ReDoc)
```

### Autentikációs Flow

#### 1. Felhasználó Regisztrációja (PHP)
```http
POST /register HTTP/1.1
Host: localhost:8081
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePass123",
  "name": "John Doe"
}

Response:
{
  "success": true,
  "message": "Registration successful. Please verify your email."
}
```

#### 2. Email Verifikálása
```http
POST /email-verify HTTP/1.1
Host: localhost:8081
Content-Type: application/json

{
  "email": "user@example.com",
  "code": "123456"
}

Response:
{
  "success": true,
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIs..."
}
```

#### 3. API Hívás JWT Token-nel
```http
GET /movies/list_movies HTTP/1.1
Host: localhost:8000
Authorization: Bearer <access_token>

Response:
{
  "data": [
    {
      "id": 1,
      "title": "Inception",
      "genre": "Sci-Fi",
      "rating": 8.8,
      "poster": "url/to/poster.jpg"
    }
  ]
}
```

### Hibakezelés

#### Standard Error Response
```json
{
  "detail": "Hiba leírása",
  "error_code": "AUTH_001",
  "timestamp": "2024-04-14T10:30:00Z"
}
```

#### HTTP Status Kódok
| Kód | Jelentés | Akció |
|-----|----------|-------|
| 200 | OK | Sikeres kérés |
| 201 | Created | Erőforrás létrehozva |
| 400 | Bad Request | Érvénytelen input |
| 401 | Unauthorized | Hiányzó/érvénytelen token |
| 403 | Forbidden | Nincs jogosultság |
| 404 | Not Found | Erőforrás nem létezik |
| 500 | Server Error | Szerver hiba |

---

## 👨‍💻 Fejlesztői Iránymutatások

### Kódstruktúra Konvenciók

#### Backend (FastAPI)
```
FastAPI_backend/
├── main.py                 # Alkalmazás belépési pont
├── routers/               # Route kezelők
│   ├── movies.py
│   ├── comments.py
│   ├── favorites.py
│   ├── messages.py
│   └── friends.py
├── models/                # SQLAlchemy adatmodellek
├── schemas/               # Pydantic validáció schemák
├── services/              # Üzleti logika
├── database/              # Adatbázis konfigurálás
└── static/                # Feltöltött fájlok
```

#### Frontend (React)
```
Frontend/src/
├── main.jsx              # React belépési pont
├── Api/                  # HTTP kliens
│   ├── AxiosInstance.js
│   ├── MovieService.js
│   ├── CommentService.js
│   └── ProfileService.js
├── AuthCheck/           # Autentikációs context
│   ├── AuthContext.jsx
│   └── MovieContext.jsx
├── Components/          # Újrafelhasználható komponensek
│   ├── Navbar/
│   ├── MovieCard/
│   ├── Comment/
│   └── Filter/
├── Pages/              # Oldalkomponensek
│   ├── Auth/
│   ├── Home/
│   ├── Movies/
│   ├── Profile/
│   └── Chat/
└── MainLayout/         # Layout komponens
```

### Egy új feature hozzáadása

#### 1. Adatmodell létrehozása
```python
# models/NewFeature.py
from sqlalchemy import Column, Integer, String, DateTime
from datetime import datetime

class NewFeature(Base):
    __tablename__ = "new_features"
    
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey("users.id"))
    title = Column(String(255))
    created_at = Column(DateTime, default=datetime.utcnow)
```

#### 2. Schema validáció
```python
# schemas/NewFeature.py
from pydantic import BaseModel

class NewFeatureCreate(BaseModel):
    title: str

class NewFeatureOut(BaseModel):
    id: int
    title: str
    class Config:
        from_attributes = True
```

#### 3. Service/Logika
```python
# services/newfeature_services.py
async def create_feature(db, user_id, title):
    feature = NewFeature(user_id=user_id, title=title)
    db.add(feature)
    await db.commit()
    return feature
```

#### 4. Router/Endpoint
```python
# routers/newfeature.py
from fastapi import APIRouter, Depends

router = APIRouter()

@router.post("/create")
async def create(data: NewFeatureCreate):
    return await create_feature(db, user_id, data.title)
```

#### 5. Frontend komponens
```jsx
// src/Components/NewFeature/NewFeatureForm.jsx
import axios from 'axios'

export function NewFeatureForm() {
  const handleSubmit = async (e) => {
    const response = await axios.post('/api/newfeature/create', {
      title: 'Test'
    })
  }
  return (<form onSubmit={handleSubmit}>...</form>)
}
```

### Tesztelési Stratégia

#### Backend tesztek
```bash
# FastAPI tesztek (pytest)
pytest FastAPI_backend/tests/

# Frontend tesztek (ha van)
npm test
```

### Verbose Logging
```python
# FastAPI-ban
import logging
logger = logging.getLogger(__name__)
logger.info("Szöveges üzenet")
```

---

## ⚡ Teljesítmény és Optimalizálás

### Frontend Optimalizálások

#### 1. Code Splitting
- React Router `lazy()` és `Suspense` komponensekkel
- Nagyobb bundleméret redukciója

#### 2. Tailwind CSS Purge
```javascript
// tailwind.config.js
content: ['./src/**/*.{js,jsx}']
// Csak felhasznált CSS osztályok kerülnek build-be
```

#### 3. Image Lazy Loading
```jsx
<img src="/poster.jpg" loading="lazy" />
```

#### 4. API Call Batching
- Axios `Promise.all()` több kéréshez

### Backend Optimalizálások

#### 1. Async Processing
```python
# FastAPI async endpoints
@router.get("/slow_operation")
async def slow_operation():
    result = await some_async_operation()
    return result
```

#### 2. Database Query Optimization
```python
# SQLAlchemy selectinload()
from sqlalchemy.orm import selectinload
query = select(Movie).options(selectinload(Movie.comments))
```

#### 3. Caching Stratégia
```python
# Redis cache (szükség esetén implementálható)
from functools import lru_cache

@lru_cache(maxsize=128)
async def get_top_movies():
    # Cache-d eredmény
    pass
```

#### 4. Rate Limiting
- Implementálható: `slowapi` library-vel

### Adatbázis Optimalizálások

#### 1. Indexek
```sql
CREATE INDEX idx_movie_title ON movies(title);
CREATE INDEX idx_user_email ON users(email);
```

#### 2. Query Optimization
- N+1 probléma elkerülése join-okkal
- Prepared statements

---

## 📊 Deployment Konfigurációk

### Produktion Considerations

#### 1. Environment Variables
```bash
# .env.production
DATABASE_URL=mysql+pymysql://user:pass@db_host/cinehub
JWT_SECRET_KEY=your-super-secret-key
CORS_ORIGINS=https://yourdomain.com
```

#### 2. Security Headers
```python
# FastAPI CORS
origins = [
    "https://yourdomain.com",
    "https://www.yourdomain.com"
]
```

#### 3. SSL/TLS
- Nginx reverse proxy SSL termination-nal

#### 4. Database Backups
```bash
# Automatizált MySQL backup
mysqldump -u root -p cinehub > backup.sql
```

#### 5. Load Balancing
- Nginx upstream load balancing
- Horizontal scaling

---

## 🐛 Troubleshooting

### Gyakori Problémák

#### Port már használatban van
```bash
# Keresés: kik használják a port-okat
netstat -ano | findstr :8000  # Windows
lsof -i :8000                 # Linux/Mac
```

#### Database Connection Error
```python
# Ellenőrzés: DATABASE_URL
echo $DATABASE_URL
# mysql+pymysql://root:@db:3306/cinehub
```

#### CORS Hiba
```javascript
// Frontend: ellenőrízd az origins listát
// FastAPI-ban: add hozzá frontend URL-t az origins-hoz
```

---

## 📚 Referenciák és Linkek

- **FastAPI dokumentáció:** https://fastapi.tiangolo.com
- **React dokumentáció:** https://react.dev
- **Tailwind CSS:** https://tailwindcss.com
- **SQLAlchemy:** https://sqlalchemy.org
- **Docker dokumentáció:** https://docs.docker.com

---

## 🔄 Dokkumentációkezelés

| Verzió | Dátum | Módosítások |
|--------|-------|-----------|
| 1.0 | 2026.03.10 | Kezdeti dokumentáció |

---

## 📝 Szerzői Megjegyzés

Ez a dokumentáció egy teljes körű útmutató az **AURO Film Közösségi Platform** projekthez. A rendszer modern web fejlesztési praktikákat alkalmaz, beleértve:

- **Async/await** programozást Python-ban
- **REST API** tervezési mintákat
- **JWT** biztonsági tokeneket
- **WebSocket** real-time kommunikációt
- **Docker**-ből izolált microservices-eket
- **Responsive UI**-t React és Tailwind CSS-sel

**A projekt ideális referencia modern webalkalmazás-fejlesztéshez.**
---

**Utolsó módosítás:** 2026. április 12.
**Fejlesztő:** Bence
