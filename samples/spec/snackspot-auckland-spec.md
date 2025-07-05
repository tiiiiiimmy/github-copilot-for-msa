# SnackSpot Auckland - Technical Specification

## 📋 Project Overview

**Project Name:** SnackSpot Auckland  
**Type:** Mobile Web Application (PWA)  
**Launch Location:** Auckland, New Zealand  
**Target Audience:** Inclusive snack community - casual, welcoming to all ages  

### Vision Statement
A gamified mobile webapp where Auckland's snack lovers discover, share, and celebrate amazing treats through beautiful sharing cards and progressive map unlocking system.

## 🎯 Core Features

### 1. Map-Based Discovery System
- **Interactive Map View**: Google Maps integration showing snack locations with custom pins
- **Location-Based Search**: "chocolate croissants" → map with nearby spots
- **Geolocation Integration**: Automatically show user's current location
- **Pin Clustering**: Group nearby locations to avoid map clutter

### 2. Category Browsing System
- **Core Categories**: Sweet, Savory, International, Allergy-Free
- **Visual Category Tiles**: Instagram-worthy design with representative images
- **User-Generated Categories**: Level 2+ users can add new categories
- **Tag System**: Users can add custom tags (#crunchy, #nostalgic, #late-night-craving)

### 3. Gamification & Leveling System
- **Progressive Area Unlocking**:
  - Level 1: Neighborhood (2-3 mile radius)
  - Level 2: Extended area (5-7 mile radius)
  - Level 3: Greater Auckland
  - Level 4: Auckland Region
  - Level 5: North Island
  - Level 6: All New Zealand

- **Level-Up Tasks**:
  - Rate 10 snacks (Level 1→2)
  - Share first snack discovery with photo (Level 1→2)
  - Visit 3 different spots on map (Level 2→3)
  - Get 5 helpful votes on contributions (Level 2→3)
  - Complete category challenges (Level 3→4)

- **Badge System**:
  - Neighborhood Navigator
  - City Connoisseur
  - Regional Snack Explorer
  - National Taste Maker
  - Category Creator (for adding new categories)

### 4. Social Sharing Features
- **Beautiful Sharing Cards**: Instagram-story-style cards with:
  - High-quality snack photo
  - Location information
  - User rating/review snippet
  - App branding/watermark
  - Direct link to app

- **Social Media Integration**:
  - Generate posters for Instagram, Facebook, Twitter
  - "Found this gem at..." template designs
  - Shareable links that direct to app

### 5. Community Interaction
- **RedNote-Style Comments**: Authentic conversation flows
- **Rating System**: 5-star rating with optional reviews
- **User Profiles**: Basic profiles with level, badges, contributions
- **Following System**: Simple user following (Level 3+ feature)

### 6. Content Management
- **Data Sources**:
  - Web scraping from NZ supermarket websites (Countdown, New World, PAK'nSAVE)
  - User-generated content and discoveries
  - Admin/founder seeded content

- **Content Moderation**:
  - User reporting system
  - Basic content filtering
  - Manual review for new categories

## 🏗️ Technical Architecture

### Frontend (Mobile Web App)
- **Framework**: React + TypeScript
- **Styling**: Tailwind CSS for responsive design
- **PWA Features**: Service workers, offline caching, installable
- **Maps**: Google Maps JavaScript API
- **Image Handling**: Cloudinary for optimization and CDN
- **State Management**: React Context + useReducer

### Backend (API Server)
- **Framework**: Node.js + Express.js
- **Database**: PostgreSQL with PostGIS for geospatial data
- **Authentication**: JWT tokens with refresh mechanism
- **File Storage**: AWS S3 for user-uploaded images
- **Web Scraping**: Puppeteer for supermarket data collection

### Database Schema

#### Users Table
```sql
- id (UUID, Primary Key)
- username (String, Unique)
- email (String, Unique)
- password_hash (String)
- level (Integer, Default: 1)
- experience_points (Integer, Default: 0)
- location (Point - PostGIS)
- created_at (Timestamp)
- updated_at (Timestamp)
```

#### Snacks Table
```sql
- id (UUID, Primary Key)
- name (String)
- description (Text)
- category_id (UUID, Foreign Key)
- image_url (String)
- user_id (UUID, Foreign Key - contributor)
- location (Point - PostGIS)
- shop_name (String)
- shop_address (String)
- average_rating (Decimal)
- total_ratings (Integer)
- created_at (Timestamp)
- data_source (Enum: 'user', 'scraped', 'seeded')
```

#### Categories Table
```sql
- id (UUID, Primary Key)
- name (String, Unique)
- description (Text)
- icon_url (String)
- created_by (UUID, Foreign Key - null for default categories)
- is_active (Boolean, Default: true)
- created_at (Timestamp)
```

#### Ratings Table
```sql
- id (UUID, Primary Key)
- user_id (UUID, Foreign Key)
- snack_id (UUID, Foreign Key)
- rating (Integer, 1-5)
- comment (Text)
- helpful_votes (Integer, Default: 0)
- created_at (Timestamp)
```

#### User_Badges Table
```sql
- id (UUID, Primary Key)
- user_id (UUID, Foreign Key)
- badge_type (String)
- earned_at (Timestamp)
```

## 🎨 User Experience Flow

### First-Time User Journey
1. **Landing**: Open app → Location permission request
2. **Onboarding**: 3-slide tutorial about leveling and map unlocking
3. **Discovery**: See immediate snacks near them (from scraped data)
4. **Engagement**: Browse categories, read comments
5. **Contribution**: Rate first snack, start leveling up

### Core User Flows

#### Snack Discovery Flow
1. User opens app
2. Map shows current location with nearby snack pins
3. User can:
   - Search for specific snacks
   - Browse by categories
   - Filter by ratings/distance
   - View trending snacks nearby

#### Snack Sharing Flow
1. User finds a great snack
2. Takes photo or selects from gallery
3. Adds location (auto-detected or manual)
4. Selects category and adds tags
5. Writes review/description
6. Publishes to community
7. Generates sharing card for social media

#### Level-Up Flow
1. User completes qualifying actions
2. Progress bar fills up
3. Level-up celebration animation
4. New area unlocks on map with visual expansion
5. Badge notification
6. "Welcome to [Area Name] snacking!" message

## 🔧 Technical Requirements

### Performance Requirements
- **Load Time**: < 3 seconds on 3G connection
- **Map Rendering**: < 2 seconds for 100 pins
- **Image Upload**: < 10 seconds for 5MB image
- **Offline Capability**: Basic browsing of cached content

### Security Requirements
- **Authentication**: Secure JWT implementation
- **Data Validation**: Input sanitization and validation
- **Image Security**: Virus scanning for uploads
- **API Rate Limiting**: Prevent abuse of scraping endpoints

### Scalability Considerations
- **Database**: Connection pooling, query optimization
- **CDN**: Global distribution for images
- **Caching**: Redis for frequently accessed data
- **Load Balancing**: Horizontal scaling capability

## 🚀 Development Phases

### Phase 1: MVP (Months 1-2)
- Basic map view with snack pins
- Core categories (Sweet, Savory, International, Allergy-Free)
- Simple user registration and authentication
- Basic rating system
- Level 1-2 unlocking (neighborhood to extended area)
- Web scraping for initial data seeding

### Phase 2: Social Features (Months 3-4)
- Beautiful sharing card generation
- RedNote-style comment system
- Badge system implementation
- Enhanced user profiles
- Level 3-4 unlocking (Greater Auckland)

### Phase 3: Advanced Features (Months 5-6)
- User-generated categories
- Following system
- Advanced search and filtering
- Level 5-6 unlocking (North Island, All NZ)
- Performance optimization and PWA features

### Phase 4: Growth & Expansion (Months 7+)
- Enhanced gamification features
- Social media integrations
- Analytics and insights
- Expansion to other NZ cities
- Community management tools

## 🎯 Success Metrics

### User Engagement
- Monthly Active Users (MAU)
- Average session duration
- User retention rates (Day 1, Day 7, Day 30)
- Number of snacks shared per user

### Content Quality
- Average rating per snack
- Comments per snack
- User-generated vs. scraped content ratio
- Category diversity

### Social Impact
- Sharing card generation rate
- Social media click-through rates
- New user acquisition via shares
- Community interaction levels

## 🛠️ Development Tools & Setup

### Development Environment
- **Version Control**: Git with GitHub
- **Package Manager**: npm/yarn
- **Build Tools**: Vite for fast development
- **Testing**: Jest + React Testing Library
- **Linting**: ESLint + Prettier
- **CI/CD**: GitHub Actions

### Third-Party Services
- **Maps**: Google Maps Platform
- **Storage**: AWS S3
- **CDN**: Cloudinary
- **Analytics**: Google Analytics 4
- **Error Tracking**: Sentry
- **Push Notifications**: Firebase Cloud Messaging

## 🌟 Future Enhancements

### Advanced Features
- AI-powered snack recommendations
- Augmented reality snack discovery
- Integration with food delivery services
- Snack nutrition information
- Community events and challenges

### Expansion Opportunities
- Restaurant and cafe integration
- International expansion
- Brand partnerships
- Loyalty programs
- E-commerce integration

---

**Document Version**: 1.0  
**Last Updated**: July 2025  
**Next Review**: Before Phase 2 Development