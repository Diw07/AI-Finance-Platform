# AI Finance Platform

An intelligent personal finance management system that helps users track expenses, manage budgets, and make better financial decisions using machine learning and artificial intelligence.

## What It Does

This platform automates the tedious parts of personal finance management. Upload a receipt, and the system extracts transaction details automatically. Add expenses manually, and it categorizes them intelligently. Set budgets, and get insights on your spending patterns with predictions for future expenses.

The AI components include receipt OCR, automatic transaction categorization, spending prediction models, and personalized financial advice generation.

## Table of Contents

- [Core Features](#core-features)
- [Technology Stack](#technology-stack)
- [System Architecture](#system-architecture)
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Reference](#api-reference)
- [Machine Learning Models](#machine-learning-models)
- [Deployment](#deployment)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [Troubleshooting](#troubleshooting)

## Core Features

**Transaction Management**
- Create, edit, and delete transactions
- Upload receipt images for automatic data extraction
- Export transaction history to CSV or PDF
- Set up recurring transactions for bills and income

**Account Management**
- Support for multiple account types (checking, savings, credit cards)
- Track balances across all accounts
- Categorize transactions automatically

**Budget Tracking**
- Set monthly budgets by category
- Visual progress indicators
- Alerts when approaching budget limits
- Historical budget analysis

**AI-Powered Insights**
- Automatic transaction categorization using NLP
- Spending predictions using LSTM neural networks
- Personalized financial advice from LLM
- Pattern recognition in spending behavior

**User Experience**
- Dark and light theme support
- Responsive design for mobile and desktop
- Real-time data updates
- Clean, accessible interface

## Technology Stack

### Frontend
- React 18 with TypeScript for type safety
- Vite for fast development and optimized builds
- Tailwind CSS for styling
- Radix UI for accessible component primitives
- Recharts for data visualization
- Clerk for authentication
- React Router for navigation

### Backend
- Node.js with Express framework
- PostgreSQL for data persistence
- Knex.js for database queries and migrations
- JWT for stateless authentication
- Multer for file upload handling
- PDFKit for report generation

### ML Service
- Python 3.11 with FastAPI
- TensorFlow and Keras for neural networks
- scikit-learn for traditional ML algorithms
- NLTK for natural language processing
- Google Gemini API for OCR and language models
- Pillow for image preprocessing

## System Architecture

The application follows a microservices architecture with three main components:

```
┌─────────────┐
│   Frontend  │ (React SPA)
│   Port 5173 │
└──────┬──────┘
       │
       │ REST API
       │
┌──────▼──────┐       ┌─────────────┐
│   Backend   │◄─────►│ PostgreSQL  │
│   Port 5000 │       │  Database   │
└──────┬──────┘       └─────────────┘
       │
       │ HTTP
       │
┌──────▼──────┐       ┌─────────────┐
│ ML Service  │◄─────►│ Gemini API  │
│  Port 8000  │       └─────────────┘
└─────────────┘
```

The frontend communicates with the backend via REST APIs. The backend handles business logic and database operations, then forwards ML-related requests to the Python service. This separation allows each service to scale independently and use the most appropriate technology for its purpose.

## Getting Started

### Prerequisites

You'll need these installed:
- Node.js (v18 or higher)
- Python 3.11
- PostgreSQL (v12 or higher)
- Git

### API Keys

Sign up for free accounts to get API keys:
- Clerk (https://clerk.com) for authentication
- Google AI Studio (https://ai.google.dev) for Gemini API

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/ai-finance-platform.git
cd ai-finance-platform
```

### 2. Set Up the Database

Open PostgreSQL and create the database:

```sql
CREATE DATABASE finance_ai;
```

### 3. Configure Environment Variables

Copy the example files and add your credentials:

```bash
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env
cp mlservice/.env.example mlservice/.env
```

Edit each `.env` file with your actual values.

### 4. Install Dependencies

Backend:
```bash
cd backend
npm install
npm run migrate
```

Frontend:
```bash
cd frontend
npm install
```

ML Service:
```bash
cd mlservice
pip install -r requirements.txt
```

## Configuration

### Backend Environment Variables

Create `backend/.env`:

```
PORT=5000
NODE_ENV=development
DATABASE_URL=postgresql://postgres:YOUR_PASSWORD@localhost:5432/finance_ai
CLERK_SECRET_KEY=your_clerk_secret_key
ML_SERVICE_URL=http://localhost:8000
JWT_SECRET=your_random_secret_string
```

### Frontend Environment Variables

Create `frontend/.env`:

```
VITE_API_URL=http://localhost:5000/api
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
```

### ML Service Environment Variables

Create `mlservice/.env`:

```
GEMINI_API_KEY=your_gemini_api_key
ML_SERVICE_PORT=8000
MODEL_PATH=../models
UPLOAD_DIR=uploads
```

## Running the Application

You need three terminal windows running simultaneously:

**Terminal 1 - Backend**
```bash
cd backend
npm run dev
```

**Terminal 2 - ML Service**
```bash
cd mlservice
python main.py
```

**Terminal 3 - Frontend**
```bash
cd frontend
npm run dev
```

The application will be available at `http://localhost:5173`

Alternatively, on Windows you can use the provided startup script:
```bash
start-demo.bat
```

## API Reference

### Authentication Endpoints

```
POST   /api/auth/register    Create new user account
POST   /api/auth/login       Authenticate user
GET    /api/auth/me          Get current user details
```

### Transaction Endpoints

```
GET    /api/transactions              Get all transactions
POST   /api/transactions              Create new transaction
PUT    /api/transactions/:id          Update transaction
DELETE /api/transactions/:id          Delete transaction
POST   /api/transactions/uploadReceipt Upload receipt for OCR
GET    /api/transactions/export       Export to CSV/PDF
```

### Account Endpoints

```
GET    /api/accounts         Get all accounts
POST   /api/accounts         Create new account
PUT    /api/accounts/:id     Update account
DELETE /api/accounts/:id     Delete account
```

### Budget Endpoints

```
GET    /api/budgets          Get all budgets
POST   /api/budgets          Create new budget
PUT    /api/budgets/:id      Update budget
DELETE /api/budgets/:id      Delete budget
GET    /api/budgets/analysis Get budget analytics
```

### AI Service Endpoints

```
POST   /api/ai/categorize     Categorize transaction text
POST   /api/ai/train          Retrain categorization model
GET    /api/ai/predict        Get spending predictions
POST   /api/ai/advice/stream  Get financial advice (streaming)
GET    /api/ai/insights       Get spending insights
```

## Machine Learning Models

The system uses several ML models working together:

**Receipt OCR**
- Uses Google Gemini Vision API
- Extracts merchant name, total amount, date, and line items
- Handles various receipt formats and image qualities
- Accuracy above 95% on clear images

**Transaction Categorization**
- Random Forest classifier with 1000+ training samples
- TF-IDF vectorization for text features
- Supports 10+ expense categories
- Learns from user corrections to improve accuracy

**Spending Prediction**
- LSTM neural network with 2 layers (50 units each)
- Trained on 12 months of historical data
- Generates 30-day forecasts by category
- Uses time-series patterns and seasonal trends

**Financial Advice**
- Google Gemini LLM for natural language generation
- Takes predictions and spending patterns as context
- Provides actionable recommendations
- Streams responses for better UX

Model files are stored in the `models/` directory and loaded at startup. The categorization model retrains automatically when users correct categories.

## Deployment

Detailed deployment instructions are available in `DEPLOYMENT.md`. Quick summary:

**Option 1: Heroku + Vercel**
- Deploy backend and ML service to Heroku
- Use Heroku Postgres for database
- Deploy frontend to Vercel
- Configure environment variables in each platform

**Option 2: AWS**
- Frontend on S3 + CloudFront
- Backend on EC2 or ECS
- Database on RDS
- ML service on EC2 with Docker

**Option 3: Railway**
- Simplest option with auto-deployment
- Connect your GitHub repository
- Railway detects services automatically
- Add environment variables in dashboard

See `DEPLOYMENT.md` for step-by-step instructions.

## Project Structure

```
ai-finance-platform/
├── frontend/                 
│   ├── src/
│   │   ├── components/       UI components
│   │   ├── pages/            Page components
│   │   ├── contexts/         React contexts
│   │   ├── lib/              Utilities and API client
│   │   └── types/            TypeScript definitions
│   └── public/               Static assets
│
├── backend/                  
│   ├── routes/               API route handlers
│   ├── middleware/           Auth and validation
│   ├── migrations/           Database schema
│   ├── jobs/                 Background tasks
│   └── config/               Configuration
│
├── mlservice/                
│   ├── main.py               FastAPI application
│   ├── ocr_service.py        Receipt OCR
│   ├── categorization_service.py  Transaction categorization
│   ├── prediction_service.py      Spending predictions
│   └── advice_service.py     Financial advice
│
├── models/                   Pre-trained ML models
│   ├── category_rf.pkl       Random Forest classifier
│   ├── spend_rnn.h5          LSTM model
│   └── *.pkl                 Supporting artifacts
│
└── others/                   Documentation and guides
```

## Contributing

Contributions are welcome. Please follow these guidelines:

1. Fork the repository
2. Create a feature branch from `main`
3. Make your changes with clear commit messages
4. Add tests if applicable
5. Submit a pull request with a description of your changes

For major changes, open an issue first to discuss what you'd like to change.

## Troubleshooting

**Database connection fails**

Check that PostgreSQL is running and the credentials in `backend/.env` are correct. Verify the database exists:
```bash
psql -U postgres -l | grep finance_ai
```

**ML service won't start**

Ensure Python 3.11 is installed and all dependencies are present:
```bash
python --version
pip list
```

If packages are missing, reinstall:
```bash
pip install -r requirements.txt
```

**Port already in use**

Find and kill the process using the port:
```bash
# Windows
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# Linux/Mac
lsof -ti:5000 | xargs kill -9
```

**Frontend build errors**

Clear the cache and reinstall:
```bash
rm -rf node_modules package-lock.json
npm install
```

For more issues, check `SETUP.md` or open an issue on GitHub.

## Documentation

Additional documentation:
- `SETUP.md` - Detailed setup instructions
- `QUICKSTART.md` - 5-minute quick start guide
- `DEPLOYMENT.md` - Production deployment guide
- `GITHUB_SETUP.md` - Push to GitHub safely
- `others/` - Interview resources and additional docs

## License

MIT License - see LICENSE file for details.

---

Built with React, Node.js, Python, TensorFlow, and Google Gemini AI.
