# Super Market Matcher - Workshop Practical

Welcome to the **Super Market Matcher** workshop! 🎉

This is a hands-on practical where you'll build a web application that helps shoppers find the best supermarket prices and deals across Dutch supermarkets (Jumbo, Albert Heijn, and Plus).

## 📋 Workshop Overview

**Goal:** Build a small web app with back-end APIs, front-end UI, NoSQL database, tests, and documentation.

**What You'll Build:**
- 🔍 Product search functionality
- 📊 Price comparison across supermarkets
- 📈 Historical price tracking and charts
- 💰 Deal detection and lower-than-normal price alerts
- 📱 Responsive web interface

**Key Learning Objectives:**
- Working with NoSQL databases (MongoDB/CosmosDB/Firestore)
- Building RESTful APIs
- Creating interactive front-end interfaces
- Writing unit and integration tests
- Documenting code and APIs

## 🛠️ Technical Requirements

### Constraints
- **Backend:** Any language/framework (Node.js, Python, .NET, etc.)
- **Frontend:** Web only (React, Vue, Angular, or any framework)
- **Database:** **NoSQL** (MongoDB, CosmosDB, Firestore, etc.)
- **Documentation:** Clear README with setup steps
- **Testing:** Unit tests required (integration tests are bonus)

### Recommended Stacks

Choose one track based on your preference:

**Track A (JavaScript/TypeScript):**
- Node.js + Express/Fastify
- MongoDB
- React/Vite
- Chart.js

**Track B (Python):**
- FastAPI
- MongoDB
- Vue/React
- Chart.js

**Track C (.NET):**
- ASP.NET Core Minimal APIs
- Cosmos DB
- React
- Chart.js

## 🗂️ Suggested Project Structure

```
/app
  /backend
    src/
    tests/
  /frontend
    src/
    tests/
  /docs
  /data (synthetic dataset)
  docker-compose.yml (optional)
```

## 🚀 Getting Started

### 1. Set Up Your Environment

1. Clone this repository
2. Choose your tech stack (Track A, B, or C)
3. Install required dependencies
4. Set up your NoSQL database (local MongoDB, MongoDB Atlas, or Docker)

### 2. Review the Specification

Read the complete workshop specification in [Outline.md](./Outline.md) which includes:
- Detailed feature requirements
- Data model design
- API specifications
- UI/UX guidelines
- Testing requirements
- Example code snippets

### 3. Build Your Application

Follow the acceptance criteria in [Outline.md](./Outline.md) to implement:

**Core Features:**
1. **Data Ingestion** - Import product prices from provided JSON datasets
2. **Search** - Find products by name, brand, or category
3. **Product Page** - Display price metrics, charts, and comparisons
4. **Home Page** - Show deals and lower-than-normal prices

**Bonus Features:**
- Integration tests
- Docker setup
- CI/CD pipeline
- Additional stretch goals (see Outline.md)

## 📊 Key Features to Implement

### 1. Price Metrics
- Current cheapest price across stores
- Lowest price ever
- Lowest price in last 6 months
- Highest price
- Price trends and charts

### 2. Deal Detection
- Items with lower-than-normal prices (≥10% below 6-month average)
- Best deals ranked by discount percentage

### 3. Price Comparison
- Real-time comparison across Albert Heijn, Jumbo, and Plus
- Historical price tracking per store
- Visual charts for price trends

## 🧪 Testing Requirements

**Must Have (Unit Tests):**
- ✅ Metric calculations (cheapest, lowest, highest)
- ✅ Search functionality (case-insensitive, partial match)
- ✅ API endpoints (correct responses, error handling)

**Bonus (Integration Tests):**
- ✅ End-to-end API tests with test database
- ✅ Frontend E2E tests (Cypress, Playwright)

## 📚 Resources

- **Full Specification:** [Outline.md](./Outline.md)
- **Data Model:** See NoSQL schema in Outline.md
- **API Reference:** See API specifications in Outline.md
- **Example Code:** Metric calculation examples in Outline.md

## ✅ Acceptance Criteria

**Functional:**
- [ ] Search returns relevant products
- [ ] Product page shows correct price stats and chart
- [ ] Home page shows lower-than-normal items and best deals
- [ ] API returns consistent formats and handles empty states
- [ ] Data model supports multi-store daily prices

**Non-functional:**
- [ ] Uses a NoSQL database
- [ ] Includes unit tests (bonus: integration tests)
- [ ] Has a clear README with setup steps and how to run/test
- [ ] Reasonable error handling and input validation
- [ ] Front-end responsive enough to demo

## 🎯 Workshop Tips

1. **Start Small:** Begin with basic data models and a simple API
2. **Use Seed Data:** Leverage the provided JSON datasets in `/data` (see Outline.md)
3. **Test Early:** Write tests as you build features
4. **Document Often:** Keep your README updated with setup instructions
5. **Ask Questions:** Don't hesitate to discuss design decisions with your team

## 🚢 Deployment (Optional)

If time allows, consider deploying your app:
- Backend: Render, Railway, Azure App Service, AWS
- Database: MongoDB Atlas, Azure Cosmos DB
- Frontend: Vercel, Netlify, GitHub Pages

## 🧩 Stretch Goals

If you complete the core features, try these additional challenges:
- 🛒 Compare basket feature (total cost per store)
- 🔔 Price alert subscriptions
- 🔐 User authentication for favorites
- 📱 PWA with offline support
- 🌍 Internationalization (NL/EN)
- ⚡ Caching and rate limiting

## 📖 Complete Documentation

For the complete, detailed workshop specification including:
- Data model schemas
- API endpoint specifications
- Example metric calculations
- Synthetic dataset structure
- UI/UX guidelines
- And much more...

**👉 See [Outline.md](./Outline.md)**

## 📄 License

This is a workshop practical project. Feel free to use and modify as needed.

---

**Happy Coding!** 🚀 If you have questions or need clarification on any requirements, refer to the detailed [Outline.md](./Outline.md) or ask your workshop facilitator.