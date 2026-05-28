# Hlongwane Properties

A full-stack real estate property listing platform that allows users to list properties for sale or rent, search properties by location, and contact the agency through an integrated email system.

## Project Overview

Hlongwane Properties is a comprehensive real estate management system built with a modern MERN stack architecture. The platform enables property owners to list their houses for sale or rent, complete with detailed property information, images, and owner contact details. Users can browse properties, filter by location, and view detailed property descriptions. The system includes an automated email notification system for contact form submissions.

## Features

- **Property Listing**: Upload and list properties for sale or rent with detailed information
- **Image Management**: Automatic image upload and hosting via Cloudinary
- **Property Search**: Search properties by province/location with intelligent matching
- **Property Filtering**: Filter properties by sale/rent status
- **Property Details**: View comprehensive property information including bedrooms, bathrooms, garages, pool, pet-friendly status
- **Contact System**: Integrated contact form with automated email notifications via SendinBlue
- **Responsive Design**: Mobile-friendly interface built with Material-UI
- **State Management**: Redux for efficient client-side state management
- **RESTful API**: Well-structured API endpoints for all operations

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                         Client (React)                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Redux      │  │ Material-UI  │  │  React Router│      │
│  │   Store      │  │   Components │  │              │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└───────────────────────────┬─────────────────────────────────┘
                            │ HTTP/REST API
┌───────────────────────────┴─────────────────────────────────┐
│                    Server (Express.js)                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │House Listing │  │House Fetch   │  │Email Route   │      │
│  │   Route      │  │   Route      │  │              │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
└─────────┼────────────────┼──────────────────┼──────────────┘
          │                │                  │
┌─────────┼────────────────┼──────────────────┼──────────────┐
│         ▼                ▼                  ▼              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │  MongoDB     │  │  Cloudinary  │  │  SendinBlue  │      │
│  │  (Database)  │  │  (Images)    │  │  (Email)     │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

## Tech Stack

### Frontend
- **React 16.13.1** - UI framework
- **Material-UI 4.11.0** - Component library
- **React Router DOM 5.2.0** - Client-side routing
- **Redux 4.1.2** - State management
- **Redux Thunk 2.4.1** - Async action handling
- **Axios 0.26.1** - HTTP client

### Backend
- **Node.js 14.10.1** - Runtime environment
- **Express 4.17.1** - Web framework
- **MongoDB/Mongoose 5.10.6** - Database and ODM
- **Cloudinary 1.23.0** - Image hosting
- **Nodemailer 6.4.11** - Email service
- **Formidable 1.2.2** - Form data parsing
- **CORS 2.8.5** - Cross-origin resource sharing
- **Dotenv 8.2.0** - Environment variables

### Development Tools
- **Nodemon 2.0.4** - Auto-restart development server
- **Create React App** - React project scaffolding

## Setup Instructions

### Prerequisites
- Node.js (v14.10.1 or higher)
- MongoDB (local instance or MongoDB Atlas)
- Cloudinary account (for image hosting)
- SendinBlue account (for email services)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Real-estate-project
   ```

2. **Install server dependencies**
   ```bash
   npm install
   ```

3. **Install client dependencies**
   ```bash
   cd client
   npm install
   cd ..
   ```

4. **Configure environment variables**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` and add your credentials:
   ```env
   NODE_ENV=development
   PORT=5000
   MONGO_URI=mongodb://localhost:27017/realestate
   CLOUD_NAME=your_cloud_name
   API_KEY=your_api_key
   API_SECRET=your_api_secret
   SendinBlue__email=your_email@example.com
   SendinBlue__key=your_sendinblue_api_key
   ```

### Running the Application

**Development mode (with hot reload):**
```bash
npm run dev
```

**Production mode:**
```bash
# Build the React client
cd client
npm run build
cd ..

# Start the server
npm start
```

The application will be available at:
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000

### Deployment

The project is configured for Heroku deployment. The `heroku-postbuild` script automatically builds the React client during deployment.

## API Examples

### Base URL
```
http://localhost:5000
```

### Endpoints

#### 1. List a Property (Sale or Rent)
**POST** `/api/houseListing`

Upload a new property listing with image.

**Request:** `multipart/form-data`
```javascript
{
  "name": "John",
  "surname": "Doe",
  "idNumber": 1234567890123,
  "phoneNumber": 27123456789,
  "email": "john@example.com",
  "province": "Gauteng",
  "street": "123 Main Street",
  "sale_or_rent": "SALE",
  "housePrice": 1500000,
  "bedroomNumber": 3,
  "garages": 2,
  "pool": true,
  "bathroomNumber": 2,
  "petFriendly": true,
  "houseImage": <file>
}
```

**Response:** `200 OK`
```json
{
  "_id": "60f1b2c3d4e5f6a7b8c9d0e1",
  "owner": {
    "name": "John",
    "surname": "Doe",
    "idNumber": 1234567890123,
    "phoneNumber": 27123456789,
    "email": "john@example.com"
  },
  "house_location": {
    "province": "Gauteng",
    "street": "123 Main Street"
  },
  "house_properties": {
    "sale_or_rent": "SALE",
    "housePrice": 1500000,
    "bedroomNumber": 3,
    "garages": 2,
    "pool": true,
    "bathroomNumber": 2,
    "petFriendly": true,
    "houseImage": "https://res.cloudinary.com/..."
  },
  "__v": 0
}
```

#### 2. Get All Properties for Sale
**GET** `/house/sale`

Retrieve all properties listed for sale.

**Response:** `200 OK`
```json
[
  {
    "_id": "60f1b2c3d4e5f6a7b8c9d0e1",
    "owner": { ... },
    "house_location": { ... },
    "house_properties": {
      "sale_or_rent": "SALE",
      ...
    }
  }
]
```

#### 3. Get All Properties for Rent
**GET** `/house/rent`

Retrieve all properties listed for rent.

**Response:** `200 OK`
```json
[
  {
    "_id": "60f1b2c3d4e5f6a7b8c9d0e2",
    "owner": { ... },
    "house_location": { ... },
    "house_properties": {
      "sale_or_rent": "RENT",
      ...
    }
  }
]
```

#### 4. Get Properties by Location
**GET** `/house/:location`

Retrieve properties by province (case-insensitive).

**Example:** `/house/Gauteng` or `/house/gauteng`

**Response:** `200 OK`
```json
[
  {
    "_id": "60f1b2c3d4e5f6a7b8c9d0e1",
    "house_location": {
      "province": "Gauteng",
      "street": "123 Main Street"
    },
    ...
  }
]
```

#### 5. Get Property Details by ID
**GET** `/house/description/:id`

Retrieve detailed information about a specific property.

**Example:** `/house/description/60f1b2c3d4e5f6a7b8c9d0e1`

**Response:** `200 OK`
```json
[
  {
    "_id": "60f1b2c3d4e5f6a7b8c9d0e1",
    "owner": { ... },
    "house_location": { ... },
    "house_properties": { ... }
  }
]
```

#### 6. Search Properties
**GET** `/api/house-search/:query`

Search properties by location query. Supports partial matches.

**Example:** `/api/house-search/Johannesburg`

**Supported Locations:** Limpopo, Eastern Cape, Western Cape, Mpumalanga, Northern Cape, Pretoria, Johannesburg, Port Elizabeth

**Response:** `200 OK`
```json
[
  {
    "_id": "60f1b2c3d4e5f6a7b8c9d0e3",
    "house_location": {
      "province": "Johannesburg",
      "street": "456 Oak Avenue"
    },
    ...
  }
]
```

#### 7. Contact Form
**POST** `/api/contact`

Submit a contact form message. Sends email to agency and confirmation to sender.

**Request:** `multipart/form-data`
```javascript
{
  "email": "user@example.com",
  "subject": "Property Inquiry",
  "message": "I'm interested in the property at 123 Main Street"
}
```

**Response:** `200 OK`
```json
{
  "msg": "Message Delivered"
}
```


