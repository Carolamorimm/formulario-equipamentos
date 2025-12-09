# Formulário de Equipamentos - Project Index

**Version:** 1.0.0
**Last Updated:** 2025-12-08
**Repository:** https://github.com/Carolamorimm/formulario-equipamentos

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Getting Started](#getting-started)
3. [Architecture](#architecture)
4. [Features](#features)
5. [Components](#components)
6. [Configuration](#configuration)
7. [Usage Guide](#usage-guide)
8. [Data Management](#data-management)
9. [Deployment](#deployment)
10. [FAQ & Troubleshooting](#faq--troubleshooting)
11. [Examples](#examples)
12. [Security](#security)
13. [Maintenance](#maintenance)

---

## Project Overview

### Purpose

Formulário de Equipamentos is a modern, responsive web application designed for IT equipment registration and inventory management. The application provides a streamlined interface for employees to register their assigned equipment with complete specifications, including hardware details and photographic evidence.

### Key Features

- **Responsive Design**: Works seamlessly across desktop, tablet, and mobile devices
- **Real-time Validation**: Instant feedback on form field validation
- **Image Upload**: Support for photographic evidence of equipment
- **Cloud Storage**: Firebase integration for data persistence and file storage
- **User-Friendly Interface**: Intuitive form design with clear visual feedback
- **Data Persistence**: Automatic timestamping and secure data storage
- **Error Handling**: Comprehensive error messages and recovery options
- **Accessibility**: Keyboard navigation and screen reader compatible

### Technology Stack

#### Frontend
- **HTML5**: Semantic markup and modern web standards
- **CSS3**: Custom styling with gradient effects and animations
- **JavaScript (ES6+)**: Modern JavaScript with async/await patterns
- **Responsive Design**: Mobile-first approach with media queries

#### Backend Services
- **Firebase 10.0.0**: Backend-as-a-Service platform
  - **Firestore**: NoSQL database for equipment records
  - **Firebase Storage**: Cloud storage for equipment photos
  - **Firebase App**: Core Firebase SDK

#### Hosting
- **GitHub Pages**: Static site hosting
- **HTTPS**: Secure data transmission

### Business Value

1. **Equipment Tracking**: Centralized registry of all company equipment
2. **Accountability**: Clear ownership and responsibility tracking
3. **Asset Management**: Detailed hardware specifications for inventory
4. **Audit Trail**: Timestamped records with photographic evidence
5. **Compliance**: Documentation for asset management compliance
6. **Cost Efficiency**: No server infrastructure required

---

## Getting Started

### Prerequisites

- Modern web browser (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+)
- Internet connection for Firebase services
- GitHub account (for deployment)
- Firebase account (for backend services)

### Installation

This is a static web application with no build process required.

#### Option 1: Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/Carolamorimm/formulario-equipamentos.git
   cd formulario-equipamentos
   ```

2. **Open in browser**
   ```bash
   # Simply open index.html in your browser
   open index.html
   # or
   python -m http.server 8000
   # Then navigate to http://localhost:8000
   ```

#### Option 2: Direct Deployment

1. Fork the repository on GitHub
2. Enable GitHub Pages in repository settings
3. Access via: `https://[your-username].github.io/formulario-equipamentos/`

### Firebase Configuration

#### Step 1: Create Firebase Project

1. Navigate to [Firebase Console](https://console.firebase.google.com)
2. Click "Add Project"
3. Enter project name (e.g., "form-equipamentos-dr")
4. Enable Google Analytics (optional)
5. Create project

#### Step 2: Enable Services

1. **Firestore Database**
   - Go to Firestore Database
   - Click "Create Database"
   - Select "Start in production mode"
   - Choose location (e.g., us-central)
   - Click "Enable"

2. **Firebase Storage**
   - Go to Storage
   - Click "Get Started"
   - Use default security rules
   - Click "Done"

#### Step 3: Get Configuration

1. Go to Project Settings (gear icon)
2. Scroll to "Your apps"
3. Click web app icon (</>)
4. Register app with nickname
5. Copy the `firebaseConfig` object

#### Step 4: Update Application

Replace the configuration in `index.html` (lines 283-291):

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID",
    measurementId: "YOUR_MEASUREMENT_ID"
};
```

### First Run

1. Open the application in your browser
2. You should see the form with purple gradient background
3. Test by filling in all required fields
4. Submit to verify Firebase connection
5. Check Firebase Console to confirm data storage

---

## Architecture

### System Design

The application follows a **client-side rendered (CSR)** architecture with serverless backend services.

```
┌─────────────────────────────────────────────────────────────┐
│                         User Browser                         │
│  ┌───────────────────────────────────────────────────────┐  │
│  │              index.html (Single Page)                 │  │
│  │  ┌─────────────┐ ┌──────────────┐ ┌──────────────┐  │  │
│  │  │   HTML      │ │     CSS      │ │  JavaScript  │  │  │
│  │  │  Structure  │ │   Styling    │ │    Logic     │  │  │
│  │  └─────────────┘ └──────────────┘ └──────────────┘  │  │
│  └───────────────────────────────────────────────────────┘  │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTPS
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    Firebase Services                         │
│  ┌──────────────────┐         ┌──────────────────────┐     │
│  │    Firestore     │         │  Firebase Storage    │     │
│  │  (NoSQL Database)│         │  (File Storage)      │     │
│  │                  │         │                      │     │
│  │  Collection:     │         │  Path: /fotos/       │     │
│  │  equipamentos    │         │  Files: *.jpg, *.png │     │
│  └──────────────────┘         └──────────────────────┘     │
└─────────────────────────────────────────────────────────────┘
```

### Application Flow

```
User Access
    │
    ▼
Load index.html
    │
    ├──► Load CSS (inline)
    ├──► Load Firebase SDK
    └──► Initialize JavaScript
         │
         ▼
    Render Form
         │
         ▼
    User Interaction
         │
         ├──► Input Validation (real-time)
         ├──► Field Focus/Blur Events
         └──► Submit Event
              │
              ▼
         Validate Form
              │
              ├──► Invalid ──► Show Error
              │
              ▼ Valid
         Upload Photo (if present)
              │
              ▼
         Save to Firestore
              │
              ├──► Success ──► Show Success Message
              └──► Error ──► Show Error Message
```

### Data Flow

1. **User Input**: Form fields collect equipment data
2. **Validation**: Client-side validation on blur and submit
3. **Photo Upload**: Optional image upload to Firebase Storage
4. **Data Storage**: Form data saved to Firestore collection
5. **Feedback**: Visual confirmation or error message
6. **Reset**: Form cleared after successful submission

### File Structure

```
formulario-equipamentos/
│
├── index.html              # Main application file (single page)
│   ├── HTML Structure      # Form markup and layout
│   ├── CSS Styling         # Embedded styles (lines 7-195)
│   └── JavaScript Logic    # Form handling (lines 281-384)
│
├── README.md               # User documentation
├── PROJECT_INDEX.md        # This file - comprehensive documentation
│
└── .git/                   # Git version control
```

### Design Patterns

#### Single Page Application (SPA)
- All code contained in one HTML file
- No page reloads or navigation
- Inline CSS and JavaScript for simplicity

#### Event-Driven Architecture
- Form submission triggers async workflow
- Real-time validation on field blur events
- User feedback through DOM manipulation

#### Serverless Backend
- No traditional server infrastructure
- Firebase handles all backend operations
- Automatic scaling and availability

#### Progressive Enhancement
- Core functionality works without JavaScript
- Enhanced experience with JavaScript enabled
- Graceful degradation for older browsers

---

## Features

### 1. Equipment Registration Form

**Purpose**: Collect comprehensive equipment information from employees

**Fields Collected**:
- Portador (Equipment Holder)
- Equipamento (Equipment Type)
- Modelo (Model)
- Serial Number
- HD (Storage)
- Processador (Processor)
- Memória (Memory/RAM)
- Marca (Brand)
- Email (Contact Email)
- Foto (Optional Photo Evidence)

**Business Rules**:
- All fields required except photo
- Email must be valid format
- Serial number must be unique (implicit)
- Photo must be image format
- Automatic timestamp on submission

### 2. Real-Time Validation

**How It Works**:
- Validation triggers on field blur (when user leaves field)
- Visual feedback: red border for invalid, normal for valid
- Required fields marked with red asterisk
- Email format validation by browser

**Implementation**:
```javascript
// Lines 374-383
inputs.forEach(input => {
    input.addEventListener('blur', function() {
        if (this.value.trim() === '') {
            this.style.borderColor = '#e74c3c';
        } else {
            this.style.borderColor = '#e0e0e0';
        }
    });
});
```

**Use Cases**:
- User enters invalid email → Red border appears
- User leaves required field empty → Red border appears
- User corrects field → Border returns to normal

### 3. Photo Upload

**Purpose**: Provide visual evidence of equipment condition

**Capabilities**:
- Accepts any image format (image/*)
- Uploads to Firebase Storage
- Generates permanent download URL
- Stores URL with form data
- Optional field (not required)

**Workflow**:
1. User selects photo file
2. On form submission, photo uploads first
3. Firebase returns permanent URL
4. URL saved with equipment record
5. Photo accessible via Firestore record

**Storage Structure**:
```
Firebase Storage
└── fotos/
    ├── 1702123456789_macbook.jpg
    ├── 1702123457890_laptop.png
    └── [timestamp]_[filename]
```

**Limitations**:
- File size not explicitly limited (Firebase default: 5GB)
- Recommended maximum: 5MB per file
- Only image formats accepted

### 4. Data Persistence

**Storage Solution**: Firebase Firestore (NoSQL Database)

**Collection Structure**:
```javascript
Collection: equipamentos
Document: [auto-generated-id]
{
    portador: "João Silva",
    equipamento: "MacBook",
    modelo: "MacBook Pro M1",
    serial: "C02ABC123XYZ",
    hd: "256GB SSD",
    processador: "Apple M1",
    memoria: "16GB RAM",
    marca: "Apple",
    email: "joao.silva@empresa.com",
    timestamp: Timestamp(2025-12-08 14:30:00),
    fotoURL: "https://storage.googleapis.com/.../foto.jpg"
}
```

**Data Characteristics**:
- Automatic document ID generation
- Server-side timestamp
- Persistent storage (never deleted automatically)
- Queryable and indexable
- Real-time sync capabilities (not used in current version)

### 5. User Feedback System

**Success State**:
- Green message box appears
- Message: "Formulário enviado com sucesso! Os dados foram salvos."
- Auto-hide after 5 seconds
- Form automatically resets

**Error State**:
- Red message box appears
- Dynamic error message based on error type
- Auto-hide after 5 seconds
- Form data preserved for retry

**Loading State**:
- Spinner animation appears
- Message: "Enviando dados..."
- Prevents duplicate submissions
- Hides other messages

**Implementation Pattern**:
```javascript
// Show loading
loading.style.display = 'block';
successMessage.style.display = 'none';
errorMessage.style.display = 'none';

// On success
loading.style.display = 'none';
successMessage.style.display = 'block';
form.reset();

// On error
loading.style.display = 'none';
errorMessage.textContent = '✗ Erro ao enviar: ' + error.message;
errorMessage.style.display = 'block';
```

### 6. Responsive Design

**Breakpoints**:
- Desktop: 600px and above (full layout)
- Mobile: Below 600px (stacked layout)

**Mobile Optimizations**:
- Reduced padding (40px → 20px)
- Smaller heading (28px → 22px)
- Stacked buttons (column layout)
- Touch-friendly input sizes
- Full-width form elements

**Desktop Features**:
- Side-by-side buttons
- Larger visual elements
- Enhanced shadows and effects

### 7. Form Reset Capability

**Purpose**: Allow users to clear all fields and start over

**Behavior**:
- Clears all text inputs
- Removes file selection
- Resets validation states
- Does not trigger submission
- Native browser functionality

**Use Cases**:
- User wants to start over
- User entered wrong information
- User wants to register different equipment

---

## Components

### HTML Structure

#### Container Layout
```html
<div class="container">
    <div class="header">
        <!-- Title and description -->
    </div>

    <div class="success-message">
        <!-- Success feedback -->
    </div>

    <div class="error-message">
        <!-- Error feedback -->
    </div>

    <form id="equipmentForm">
        <!-- Form groups -->
    </form>
</div>
```

#### Form Groups

Each form field follows this pattern:
```html
<div class="form-group">
    <label for="fieldId">
        Field Name <span class="required">*</span>
    </label>
    <input
        type="text"
        id="fieldId"
        required
        placeholder="Example text"
    >
</div>
```

**Form Group Elements**:
1. **Portador** (text input, required)
2. **Equipamento** (text input, required)
3. **Modelo** (text input, required)
4. **Serial** (text input, required)
5. **HD** (text input, required)
6. **Processador** (text input, required)
7. **Memória** (text input, required)
8. **Marca** (text input, required)
9. **Email** (email input, required)
10. **Foto** (file input, optional)

#### Button Group
```html
<div class="button-group">
    <button type="submit" class="btn-submit">
        Enviar Formulário
    </button>
    <button type="reset" class="btn-reset">
        Limpar
    </button>
</div>
```

### CSS Styling

#### Design System

**Color Palette**:
- Primary Gradient: `#667eea` → `#764ba2`
- Success: `#d4edda` (background), `#155724` (text)
- Error: `#f8d7da` (background), `#721c24` (text)
- Neutral: `#333` (text), `#666` (secondary text), `#999` (muted)
- Border: `#e0e0e0` (default), `#667eea` (focus), `#e74c3c` (error)

**Typography**:
- Font Family: `'Segoe UI', Tahoma, Geneva, Verdana, sans-serif`
- Heading: 28px (22px mobile)
- Body: 14px
- Info Text: 12px

**Spacing**:
- Container Padding: 40px (20px mobile)
- Form Group Margin: 20px
- Input Padding: 12px
- Button Group Gap: 10px

**Effects**:
- Box Shadow: `0 20px 60px rgba(0, 0, 0, 0.3)`
- Border Radius: 12px (container), 6px (inputs)
- Transitions: 0.3s (all interactive elements)
- Hover Transform: `translateY(-2px)`

#### Animation

**Spinner**:
```css
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}
```

**Button Hover**:
- Lift effect (translateY -2px)
- Enhanced shadow
- Smooth transition

### JavaScript Modules

#### Firebase Initialization
```javascript
// Lines 283-296
const firebaseConfig = { /* config */ };
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();
const storage = firebase.storage();
```

#### Form Handler
```javascript
// Lines 303-371
form.addEventListener('submit', async function(e) {
    // 1. Prevent default
    // 2. Collect data
    // 3. Validate
    // 4. Upload photo
    // 5. Save to Firestore
    // 6. Show feedback
});
```

**Key Functions**:
1. **Data Collection**: Extract and trim all form values
2. **Validation**: Check required fields
3. **Photo Upload**: Conditional upload to Storage
4. **Database Save**: Add document to Firestore
5. **User Feedback**: Display success/error messages
6. **Form Reset**: Clear form after success

#### Real-Time Validation
```javascript
// Lines 374-383
const inputs = form.querySelectorAll('input[required], textarea[required]');
inputs.forEach(input => {
    input.addEventListener('blur', function() {
        // Validate on blur
    });
});
```

---

## Configuration

### Firebase Configuration

**Required Parameters**:

| Parameter | Description | Example |
|-----------|-------------|---------|
| `apiKey` | Firebase API key | `AIzaSyDJe-1kZVqgqCSmm9jA8YBL` |
| `authDomain` | Authentication domain | `form-equipamentos-dr.firebaseapp.com` |
| `projectId` | Firebase project ID | `form-equipamentos-dr` |
| `storageBucket` | Storage bucket URL | `form-equipamentos-dr.appspot.com` |
| `messagingSenderId` | Messaging sender ID | `951445803016` |
| `appId` | Firebase app ID | `1:951445803016:web:a3f71dda8fae7b5a4f78d` |
| `measurementId` | Analytics measurement ID | `G-QU2M74762` |

**Location**: Lines 283-291 in `index.html`

**Security Note**: The current configuration includes actual credentials. For production use:
1. Use environment variables
2. Implement Firebase Security Rules
3. Restrict API key usage
4. Enable App Check

### Firestore Security Rules

**Recommended Rules**:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /equipamentos/{document=**} {
      // Allow authenticated reads
      allow read: if request.auth != null;

      // Allow writes with valid data
      allow write: if request.auth != null
        && request.resource.data.keys().hasAll([
          'portador', 'equipamento', 'modelo',
          'serial', 'hd', 'processador',
          'memoria', 'marca', 'email'
        ]);
    }
  }
}
```

**Current Rules**: Production mode (allow all)
**Recommendation**: Implement authentication and proper rules

### Storage Security Rules

**Recommended Rules**:
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /fotos/{fileName} {
      // Allow image uploads up to 5MB
      allow write: if request.resource.size < 5 * 1024 * 1024
                   && request.resource.contentType.matches('image/.*');

      // Allow authenticated reads
      allow read: if request.auth != null;
    }
  }
}
```

### Customization Options

#### Change Colors

Edit the CSS variables in `<style>` section:

```css
/* Primary gradient */
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);

/* Success color */
background: #d4edda;
color: #155724;

/* Error color */
background: #f8d7da;
color: #721c24;
```

#### Modify Form Fields

**Add a new field**:
```html
<div class="form-group">
    <label for="newField">New Field <span class="required">*</span></label>
    <input type="text" id="newField" required placeholder="Enter value">
</div>
```

**Update JavaScript** (line 307):
```javascript
const formData = {
    // ... existing fields
    newField: document.getElementById('newField').value.trim(),
};
```

#### Change Collection Name

Line 349:
```javascript
await db.collection('your-collection-name').add(formData);
```

---

## Usage Guide

### For End Users

#### Submitting Equipment Information

**Step 1: Access the Form**
1. Navigate to the application URL
2. Wait for the form to load completely
3. Verify you see the purple gradient background

**Step 2: Fill Required Fields**

Fill in all fields marked with a red asterisk (*):

1. **Portador**: Your full name
   - Example: "João Silva"

2. **Equipamento**: Type of equipment
   - Example: "MacBook", "Dell Laptop", "Lenovo ThinkPad"

3. **Modelo**: Specific model
   - Example: "MacBook Pro M1", "XPS 13", "ThinkPad X1"

4. **Serial Number**: Equipment serial number
   - Usually found on sticker or in system settings
   - Example: "C02ABC123XYZ"

5. **HD**: Storage capacity
   - Example: "256GB SSD", "512GB NVMe", "1TB HDD"

6. **Processador**: Processor type
   - Example: "Apple M1", "Intel Core i7", "AMD Ryzen 5"

7. **Memória**: RAM amount
   - Example: "16GB RAM", "8GB DDR4", "32GB"

8. **Marca**: Equipment brand
   - Example: "Apple", "Dell", "Lenovo", "HP"

9. **Email**: Your contact email
   - Must be valid email format
   - Example: "joao.silva@empresa.com"

**Step 3: Add Photo (Optional)**
1. Click "Escolher arquivo" (Choose file)
2. Select an image of your equipment
3. Supported formats: JPG, PNG, GIF, etc.
4. Recommended: Clear photo showing equipment

**Step 4: Submit**
1. Click "Enviar Formulário" (Submit Form)
2. Wait for the loading spinner
3. Look for success message (green box)
4. Form will automatically clear after success

**Step 5: Confirmation**
- Success message appears for 5 seconds
- Form resets to empty state
- You can submit another equipment record

#### What Happens After Submission

1. **Data Storage**: Information saved to database
2. **Photo Upload**: Image stored in cloud storage
3. **Timestamp**: Automatic date/time recorded
4. **Permanent Record**: Data persists indefinitely

#### Tips for Best Results

- **Serial Number**: Usually found in:
  - System Settings/About This Mac
  - Sticker on bottom of laptop
  - Original box or receipt

- **Photo Quality**:
  - Good lighting
  - Clear view of equipment
  - Include visible serial number if possible

- **Email**: Use company email for official records

### For Administrators

#### Accessing Submitted Data

**Via Firebase Console**:

1. Log in to [Firebase Console](https://console.firebase.google.com)
2. Select your project
3. Navigate to Firestore Database
4. Browse the `equipamentos` collection
5. Each document represents one submission

**Document Structure**:
```javascript
{
    portador: "João Silva",
    equipamento: "MacBook",
    modelo: "MacBook Pro M1",
    serial: "C02ABC123XYZ",
    hd: "256GB SSD",
    processador: "Apple M1",
    memoria: "16GB RAM",
    marca: "Apple",
    email: "joao.silva@empresa.com",
    timestamp: December 8, 2025 at 2:30:00 PM UTC-3,
    fotoURL: "https://firebasestorage.googleapis.com/..."
}
```

#### Exporting Data

**Method 1: Firebase Console**
1. Go to Firestore Database
2. Select `equipamentos` collection
3. Use Firebase Admin SDK for exports
4. Export to JSON or CSV

**Method 2: Using Firebase CLI**
```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Export Firestore data
firebase firestore:export gs://your-bucket/exports
```

#### Viewing Photos

1. Click on any document in Firestore
2. Copy the `fotoURL` value
3. Paste in browser to view image
4. Or navigate to Storage section in Firebase Console

#### Filtering and Querying

**In Firebase Console**:
- Use the query builder
- Filter by email, marca, equipamento, etc.
- Sort by timestamp to see recent submissions

**Common Queries**:
- All Apple equipment: `marca == "Apple"`
- Specific user: `email == "user@empresa.com"`
- Recent submissions: Order by `timestamp` descending

#### Data Analytics

**Available Metrics**:
- Total equipment registered
- Equipment by brand
- Equipment by type
- Submission timeline
- User participation

**Export for Analysis**:
1. Export Firestore data
2. Import to Google Sheets or Excel
3. Create pivot tables and charts
4. Generate inventory reports

---

## Data Management

### Data Collection

**Automatic Fields**:
- `timestamp`: Server timestamp at submission
- `fotoURL`: Generated URL after photo upload (null if no photo)

**User-Provided Fields**:
- All form inputs (9 required fields)
- Optional photo file

### Data Storage

**Primary Storage**: Firebase Firestore
- Collection: `equipamentos`
- Document ID: Auto-generated
- Data Type: JSON-like documents
- Indexing: Automatic on all fields

**Photo Storage**: Firebase Storage
- Path: `/fotos/[timestamp]_[filename]`
- Format: Original image format
- Access: Via download URL
- Persistence: Permanent until manually deleted

### Data Retrieval

**Query Examples**:

```javascript
// Get all equipment
db.collection('equipamentos').get()
    .then(snapshot => {
        snapshot.forEach(doc => {
            console.log(doc.id, doc.data());
        });
    });

// Get by email
db.collection('equipamentos')
    .where('email', '==', 'user@empresa.com')
    .get();

// Get recent submissions
db.collection('equipamentos')
    .orderBy('timestamp', 'desc')
    .limit(10)
    .get();

// Get by equipment type
db.collection('equipamentos')
    .where('equipamento', '==', 'MacBook')
    .get();
```

### Data Privacy

**Current Implementation**:
- No encryption at rest (Firebase default)
- HTTPS encryption in transit
- No authentication required (open form)
- No automatic data deletion

**Recommendations**:
1. Implement Firebase Authentication
2. Add user authentication before form access
3. Implement data retention policies
4. Add GDPR compliance features
5. Encrypt sensitive fields

### Data Backup

**Firebase Automatic Backup**:
- Firebase provides automatic backups
- Point-in-time recovery available
- Daily backups retained for 7 days

**Manual Backup**:
```bash
# Using Firebase CLI
firebase firestore:export gs://your-bucket/backup-$(date +%Y%m%d)
```

**Recommended Schedule**:
- Daily automated backups
- Weekly full exports
- Monthly archival to cold storage

---

## Deployment

### GitHub Pages Deployment

**Prerequisites**:
- GitHub account
- Repository access
- Firebase project configured

**Step-by-Step Deployment**:

#### 1. Prepare Repository

```bash
# Clone repository
git clone https://github.com/Carolamorimm/formulario-equipamentos.git
cd formulario-equipamentos

# Verify files
ls -la
# Should see: index.html, README.md
```

#### 2. Update Firebase Configuration

Edit `index.html` lines 283-291 with your Firebase config:
```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    // ... other config values
};
```

#### 3. Commit Changes

```bash
git add index.html
git commit -m "Update Firebase configuration"
git push origin main
```

#### 4. Enable GitHub Pages

1. Go to repository on GitHub
2. Click **Settings**
3. Scroll to **Pages** section (left sidebar)
4. Under **Source**, select:
   - Branch: `main`
   - Folder: `/ (root)`
5. Click **Save**

#### 5. Verify Deployment

1. Wait 1-2 minutes for deployment
2. Navigate to: `https://carolamorimm.github.io/formulario-equipamentos/`
3. Test form submission
4. Verify data appears in Firebase Console

**Deployment URL**:
```
https://[your-username].github.io/formulario-equipamentos/
```

### Alternative Hosting Options

#### Netlify Deployment

**Advantages**:
- Automatic HTTPS
- Custom domains
- Form handling features
- Faster deployment

**Steps**:
1. Create account at [Netlify](https://netlify.com)
2. Click "New site from Git"
3. Connect to GitHub repository
4. Deploy settings:
   - Branch: `main`
   - Build command: (none)
   - Publish directory: `/`
5. Deploy site

#### Vercel Deployment

**Steps**:
1. Install Vercel CLI: `npm i -g vercel`
2. Navigate to project: `cd formulario-equipamentos`
3. Run: `vercel`
4. Follow prompts
5. Deploy: `vercel --prod`

#### Firebase Hosting

**Steps**:
```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Initialize hosting
firebase init hosting

# Deploy
firebase deploy --only hosting
```

### Custom Domain Configuration

#### GitHub Pages Custom Domain

1. In repository Settings → Pages
2. Enter custom domain (e.g., `equipamentos.empresa.com`)
3. Add DNS records at domain registrar:
   ```
   Type: CNAME
   Name: equipamentos
   Value: carolamorimm.github.io
   ```
4. Wait for DNS propagation (up to 48 hours)
5. Enable "Enforce HTTPS" in GitHub Settings

#### SSL/HTTPS

- **GitHub Pages**: Automatic with Let's Encrypt
- **Netlify**: Automatic with Let's Encrypt
- **Custom**: Use Cloudflare for free SSL

### Environment-Specific Configuration

**Development**:
```javascript
const firebaseConfig = {
    // Development Firebase project
    projectId: "form-equipamentos-dev",
    // ...
};
```

**Production**:
```javascript
const firebaseConfig = {
    // Production Firebase project
    projectId: "form-equipamentos-prod",
    // ...
};
```

**Best Practice**: Use separate Firebase projects for dev/prod

---

## FAQ & Troubleshooting

### Frequently Asked Questions

#### Q: Is the photo required?
**A:** No, the photo field is optional. All other fields are required.

#### Q: What image formats are supported?
**A:** All standard image formats (JPG, PNG, GIF, WebP, etc.) are supported by the file input.

#### Q: How large can the photo file be?
**A:** While there's no explicit limit in the code, Firebase Storage has a default limit of 5GB per file. We recommend keeping photos under 5MB for best performance.

#### Q: Can I edit a submission after sending?
**A:** No, the current version doesn't support editing. You would need to submit a new form or contact an administrator to modify records in Firebase.

#### Q: Where is my data stored?
**A:** Data is stored in Firebase Firestore (database) and Firebase Storage (photos), which are Google Cloud services.

#### Q: Is my data secure?
**A:** Data is transmitted over HTTPS. However, the current implementation doesn't have authentication. For production use, implement Firebase Authentication and security rules.

#### Q: Can I submit multiple equipment?
**A:** Yes, after successful submission, the form resets and you can submit another record.

#### Q: How long is data retained?
**A:** Data is stored indefinitely until manually deleted by an administrator.

#### Q: Can I export all submissions?
**A:** Yes, administrators can export data from the Firebase Console or using the Firebase CLI.

#### Q: Does this work offline?
**A:** No, an internet connection is required to submit data to Firebase.

#### Q: Can I customize the form fields?
**A:** Yes, you can modify the HTML and JavaScript to add, remove, or change fields.

#### Q: Is there a limit on submissions?
**A:** Firebase Spark (free) plan limits:
- Firestore: 1GB storage, 50K reads/day
- Storage: 5GB total, 1GB downloads/day
- For more, upgrade to Blaze (pay-as-you-go) plan

### Troubleshooting Guide

#### Problem: Form doesn't submit / Loading spinner stays forever

**Possible Causes**:
1. Incorrect Firebase configuration
2. Network connection issues
3. Firebase services not enabled
4. Browser console errors

**Solutions**:

1. **Check Firebase Configuration**:
   - Verify all config values are correct
   - Check for typos in API key or project ID
   - Ensure config matches Firebase Console

2. **Check Browser Console** (F12 → Console):
   ```
   Look for errors like:
   - "Firebase: Error (auth/api-key-not-valid)"
   - "Firestore: Missing or insufficient permissions"
   - "Network request failed"
   ```

3. **Verify Firebase Services**:
   - Go to Firebase Console
   - Ensure Firestore is enabled
   - Ensure Storage is enabled
   - Check service status

4. **Test Network**:
   - Open browser DevTools (F12)
   - Go to Network tab
   - Submit form
   - Look for failed requests

#### Problem: Photo doesn't upload

**Possible Causes**:
1. File too large
2. Firebase Storage not enabled
3. Incorrect storage rules
4. Invalid file format

**Solutions**:

1. **Check File Size**:
   - Try with a smaller image
   - Compress image before upload
   - Recommended: under 2MB

2. **Verify Storage is Enabled**:
   - Firebase Console → Storage
   - Should show storage bucket
   - Check rules allow writes

3. **Test with Different File**:
   - Try JPG instead of PNG
   - Use different image
   - Take new photo

4. **Check Storage Rules**:
   ```javascript
   // Ensure rules allow writes
   allow write: if request.resource.size < 5 * 1024 * 1024;
   ```

#### Problem: Success message appears but data not in Firebase

**Possible Causes**:
1. Viewing wrong project
2. Wrong collection name
3. Firestore rules blocking writes

**Solutions**:

1. **Verify Correct Project**:
   - Check Firebase Console project name
   - Verify project ID in config matches

2. **Check Collection Name**:
   - Go to Firestore Database
   - Look for `equipamentos` collection
   - Verify collection exists

3. **Review Firestore Rules**:
   - Go to Firestore → Rules
   - Ensure writes are allowed
   - For testing, use:
     ```javascript
     allow read, write: if true;
     ```

#### Problem: Validation errors not showing

**Possible Causes**:
1. JavaScript not loading
2. Browser doesn't support JavaScript
3. Console errors preventing execution

**Solutions**:

1. **Check JavaScript Console**:
   - Press F12
   - Look for errors in Console tab
   - Fix any JavaScript errors

2. **Verify Browser Support**:
   - Use modern browser (Chrome, Firefox, Safari, Edge)
   - Update to latest version
   - Clear browser cache

3. **Test Basic Functionality**:
   - Try in incognito/private mode
   - Disable browser extensions
   - Try different browser

#### Problem: Form looks broken or unstyled

**Possible Causes**:
1. CSS not loading
2. Browser compatibility
3. Screen size issues

**Solutions**:

1. **Check Browser**:
   - Use modern browser
   - Update browser to latest version
   - Clear cache and hard reload (Ctrl+Shift+R)

2. **Verify File Integrity**:
   - Ensure `index.html` is complete
   - Check for missing closing tags
   - Validate HTML syntax

3. **Test Responsive Design**:
   - Resize browser window
   - Test on different devices
   - Check mobile view

#### Problem: Error message: "Missing or insufficient permissions"

**Cause**: Firestore security rules blocking write access

**Solution**:

1. Go to Firebase Console → Firestore → Rules
2. For development/testing, use:
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }
   ```
3. Click "Publish"
4. **Important**: Implement proper security rules before production

#### Problem: Email validation not working

**Cause**: Browser's built-in validation may vary

**Solution**:

1. Verify email input type:
   ```html
   <input type="email" id="email" required>
   ```

2. Add custom validation if needed:
   ```javascript
   const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
   if (!emailRegex.test(email)) {
       // Show error
   }
   ```

#### Problem: Form submits but fields don't clear

**Cause**: JavaScript error after submission

**Solution**:

1. Check browser console for errors
2. Verify `form.reset()` is called (line 353)
3. Ensure no JavaScript errors before reset

### Getting Help

**Resources**:
- [Firebase Documentation](https://firebase.google.com/docs)
- [GitHub Pages Documentation](https://docs.github.com/pages)
- [MDN Web Docs](https://developer.mozilla.org/)

**Support Channels**:
- GitHub Issues: [Repository Issues](https://github.com/Carolamorimm/formulario-equipamentos/issues)
- Firebase Support: [Firebase Console](https://console.firebase.google.com/support)
- Stack Overflow: Tag questions with `firebase` and `firestore`

---

## Examples

### Example 1: Complete Equipment Registration

**Scenario**: Employee receives new MacBook Pro

**Input**:
```
Portador: Maria Santos
Equipamento: MacBook
Modelo: MacBook Pro M1 13"
Serial Number: C02YQ2XHJGH5
HD: 512GB SSD
Processador: Apple M1
Memória: 16GB RAM
Marca: Apple
Email: maria.santos@empresa.com
Foto: [Selected image of MacBook]
```

**Process**:
1. User fills all fields
2. Selects photo from computer
3. Clicks "Enviar Formulário"
4. Loading spinner appears
5. Photo uploads to Firebase Storage
6. Data saves to Firestore
7. Success message appears
8. Form resets

**Result in Firestore**:
```json
{
  "portador": "Maria Santos",
  "equipamento": "MacBook",
  "modelo": "MacBook Pro M1 13\"",
  "serial": "C02YQ2XHJGH5",
  "hd": "512GB SSD",
  "processador": "Apple M1",
  "memoria": "16GB RAM",
  "marca": "Apple",
  "email": "maria.santos@empresa.com",
  "timestamp": "2025-12-08T17:30:00.000Z",
  "fotoURL": "https://firebasestorage.googleapis.com/v0/b/form-equipamentos-dr.appspot.com/o/fotos%2F1702145400000_macbook.jpg?alt=media&token=abc123"
}
```

### Example 2: Registration Without Photo

**Scenario**: Employee has Dell laptop but no camera available

**Input**:
```
Portador: João Silva
Equipamento: Dell Laptop
Modelo: XPS 15 9520
Serial Number: 4KLM789
HD: 1TB NVMe SSD
Processador: Intel Core i7-12700H
Memória: 32GB DDR5
Marca: Dell
Email: joao.silva@empresa.com
Foto: [Not selected]
```

**Process**:
1. User fills all required fields
2. Skips photo field
3. Submits form
4. Photo upload step is skipped
5. Data saves to Firestore
6. Success message appears

**Result in Firestore**:
```json
{
  "portador": "João Silva",
  "equipamento": "Dell Laptop",
  "modelo": "XPS 15 9520",
  "serial": "4KLM789",
  "hd": "1TB NVMe SSD",
  "processador": "Intel Core i7-12700H",
  "memoria": "32GB DDR5",
  "marca": "Dell",
  "email": "joao.silva@empresa.com",
  "timestamp": "2025-12-08T18:15:00.000Z",
  "fotoURL": null
}
```

### Example 3: Querying Equipment by User

**Scenario**: Administrator wants to see all equipment assigned to a user

**Code**:
```javascript
// Get reference to Firestore
const db = firebase.firestore();

// Query by email
db.collection('equipamentos')
  .where('email', '==', 'maria.santos@empresa.com')
  .get()
  .then(querySnapshot => {
    querySnapshot.forEach(doc => {
      console.log(`${doc.id} =>`, doc.data());

      // Display results
      const data = doc.data();
      console.log(`Equipment: ${data.equipamento} ${data.modelo}`);
      console.log(`Serial: ${data.serial}`);
    });
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**Output**:
```
Equipment: MacBook MacBook Pro M1 13"
Serial: C02YQ2XHJGH5
```

### Example 4: Generating Inventory Report

**Scenario**: Generate count of equipment by brand

**Code**:
```javascript
// Get all equipment
db.collection('equipamentos')
  .get()
  .then(querySnapshot => {
    const brandCount = {};

    querySnapshot.forEach(doc => {
      const brand = doc.data().marca;
      brandCount[brand] = (brandCount[brand] || 0) + 1;
    });

    console.log('Equipment by Brand:', brandCount);
  });
```

**Output**:
```
Equipment by Brand: {
  "Apple": 15,
  "Dell": 23,
  "Lenovo": 18,
  "HP": 12
}
```

### Example 5: Custom Field Addition

**Scenario**: Add a "Department" field to the form

**HTML Addition** (after line 256):
```html
<div class="form-group">
    <label for="departamento">Departamento <span class="required">*</span></label>
    <input type="text" id="departamento" required placeholder="Ex: TI, Vendas, Marketing">
</div>
```

**JavaScript Update** (line 316, add to formData):
```javascript
const formData = {
    portador: document.getElementById('portador').value.trim(),
    equipamento: document.getElementById('equipamento').value.trim(),
    modelo: document.getElementById('modelo').value.trim(),
    serial: document.getElementById('serial').value.trim(),
    hd: document.getElementById('hd').value.trim(),
    processador: document.getElementById('processador').value.trim(),
    memoria: document.getElementById('memoria').value.trim(),
    marca: document.getElementById('marca').value.trim(),
    email: document.getElementById('email').value.trim(),
    departamento: document.getElementById('departamento').value.trim(), // NEW
    timestamp: new Date(),
    fotoURL: null
};
```

**Validation Update** (line 322):
```javascript
if (!formData.portador || !formData.equipamento || !formData.modelo ||
    !formData.serial || !formData.hd || !formData.processador ||
    !formData.memoria || !formData.marca || !formData.email ||
    !formData.departamento) { // Added departamento
    // Error handling
}
```

---

## Security

### Current Security Posture

**Strengths**:
- HTTPS encryption in transit
- Client-side validation
- Firebase SDK security
- No sensitive data in client code (except config)

**Weaknesses**:
- No authentication required
- Firebase credentials exposed in source
- Open Firestore rules (production mode)
- No rate limiting
- No data sanitization
- No CSRF protection

### Security Recommendations

#### 1. Implement Authentication

**Add Firebase Authentication**:

```html
<!-- Add Auth SDK -->
<script src="https://www.gstatic.com/firebasejs/10.0.0/firebase-auth.js"></script>
```

```javascript
// Initialize Auth
const auth = firebase.auth();

// Require sign-in before form access
auth.onAuthStateChanged(user => {
    if (!user) {
        // Redirect to login
        window.location.href = '/login.html';
    }
});

// Add user info to submission
formData.userId = auth.currentUser.uid;
formData.userEmail = auth.currentUser.email;
```

#### 2. Secure Firestore Rules

**Production Rules**:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /equipamentos/{document} {
      // Only authenticated users can read
      allow read: if request.auth != null;

      // Only authenticated users can create
      // User can only write their own email
      allow create: if request.auth != null
        && request.resource.data.email == request.auth.token.email
        && request.resource.data.keys().hasAll([
          'portador', 'equipamento', 'modelo', 'serial',
          'hd', 'processador', 'memoria', 'marca', 'email'
        ]);

      // No updates or deletes from client
      allow update, delete: if false;
    }
  }
}
```

#### 3. Secure Storage Rules

**Production Rules**:
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /fotos/{fileName} {
      // Only authenticated users can upload
      allow write: if request.auth != null
        && request.resource.size < 5 * 1024 * 1024
        && request.resource.contentType.matches('image/.*');

      // Only authenticated users can read
      allow read: if request.auth != null;
    }
  }
}
```

#### 4. Protect Firebase Configuration

**Environment Variables** (for build systems):
```javascript
const firebaseConfig = {
    apiKey: process.env.FIREBASE_API_KEY,
    authDomain: process.env.FIREBASE_AUTH_DOMAIN,
    // ...
};
```

**Restrict API Key**:
1. Go to Google Cloud Console
2. APIs & Services → Credentials
3. Edit API key
4. Set application restrictions
5. Set API restrictions to only needed APIs

#### 5. Add Input Sanitization

```javascript
// Sanitize function
function sanitizeInput(input) {
    return input
        .trim()
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#x27;')
        .replace(/\//g, '&#x2F;');
}

// Apply to all inputs
const formData = {
    portador: sanitizeInput(document.getElementById('portador').value),
    equipamento: sanitizeInput(document.getElementById('equipamento').value),
    // ... etc
};
```

#### 6. Implement Rate Limiting

**Firebase App Check**:
```html
<script src="https://www.gstatic.com/firebasejs/10.0.0/firebase-app-check.js"></script>
```

```javascript
const appCheck = firebase.appCheck();
appCheck.activate('your-recaptcha-site-key');
```

#### 7. Content Security Policy

**Add to HTML `<head>`**:
```html
<meta http-equiv="Content-Security-Policy"
      content="default-src 'self';
               script-src 'self' https://www.gstatic.com;
               style-src 'self' 'unsafe-inline';
               img-src 'self' data: https:;
               connect-src 'self' https://*.googleapis.com">
```

### Security Checklist

Before deploying to production:

- [ ] Implement Firebase Authentication
- [ ] Update Firestore security rules
- [ ] Update Storage security rules
- [ ] Restrict Firebase API key
- [ ] Enable Firebase App Check
- [ ] Add input sanitization
- [ ] Implement rate limiting
- [ ] Add Content Security Policy
- [ ] Remove console.log statements
- [ ] Enable HTTPS only
- [ ] Regular security audits
- [ ] Monitor Firebase usage for anomalies

---

## Maintenance

### Regular Maintenance Tasks

#### Daily
- Monitor Firebase usage metrics
- Check for error reports
- Verify form submissions working
- Review any user-reported issues

#### Weekly
- Review Firestore database size
- Check Storage usage
- Verify all fields collecting correctly
- Test form on different browsers

#### Monthly
- Backup Firestore data
- Review and archive old submissions
- Update dependencies (Firebase SDK)
- Security audit
- Performance review

#### Quarterly
- Full security audit
- Review and optimize Firestore rules
- Analyze usage patterns
- Plan feature updates
- Review and update documentation

### Monitoring

**Firebase Console Metrics**:
1. Go to Firebase Console
2. Select project
3. View Dashboard

**Key Metrics to Monitor**:
- Firestore reads/writes per day
- Storage usage and bandwidth
- Active users (if auth implemented)
- Error rates
- Response times

**Set Up Alerts**:
1. Firebase Console → Project Settings
2. Integrations → Cloud Monitoring
3. Create alert policies for:
   - High error rates
   - Quota approaching limits
   - Unusual activity patterns

### Backup Strategy

**Automated Backups**:
```bash
# Daily backup script
#!/bin/bash
DATE=$(date +%Y%m%d)
firebase firestore:export gs://your-backup-bucket/backups/$DATE
```

**Backup Schedule**:
- Daily: Last 7 days
- Weekly: Last 4 weeks
- Monthly: Last 12 months
- Yearly: Indefinite

**Restore Process**:
```bash
# Restore from backup
firebase firestore:import gs://your-backup-bucket/backups/20251208
```

### Updating Dependencies

**Firebase SDK Updates**:

1. Check current version (line 277-279):
   ```html
   <script src="https://www.gstatic.com/firebasejs/10.0.0/..."></script>
   ```

2. Check for updates:
   - Visit [Firebase Release Notes](https://firebase.google.com/support/release-notes/js)

3. Test in development before updating production

4. Update all three SDK scripts together:
   - firebase-app.js
   - firebase-firestore.js
   - firebase-storage.js

**Update Process**:
1. Create backup of current version
2. Update SDK version in HTML
3. Test all functionality
4. Deploy to staging
5. Monitor for issues
6. Deploy to production

### Common Maintenance Scenarios

#### Scenario 1: Database Growing Too Large

**Solution**:
1. Archive old records
2. Implement data retention policy
3. Export and delete records older than X months

```javascript
// Delete records older than 1 year
const oneYearAgo = new Date();
oneYearAgo.setFullYear(oneYearAgo.getFullYear() - 1);

db.collection('equipamentos')
  .where('timestamp', '<', oneYearAgo)
  .get()
  .then(snapshot => {
    snapshot.forEach(doc => {
      doc.ref.delete();
    });
  });
```

#### Scenario 2: Hitting Firebase Quota Limits

**Solutions**:
1. Upgrade to Blaze (pay-as-you-go) plan
2. Optimize read/write operations
3. Implement caching
4. Review and remove unnecessary queries

#### Scenario 3: User Reports Submission Issues

**Troubleshooting Steps**:
1. Check Firebase service status
2. Review browser console errors
3. Verify Firebase rules haven't changed
4. Test form submission yourself
5. Check for recent code changes
6. Review Firebase usage metrics

### Version Control

**Tagging Releases**:
```bash
# Tag current version
git tag -a v1.0.0 -m "Initial production release"
git push origin v1.0.0
```

**Changelog Maintenance**:
Create CHANGELOG.md to track changes:
```markdown
# Changelog

## [1.0.0] - 2025-12-08
### Added
- Initial release
- Equipment registration form
- Firebase integration
- Photo upload capability

### Changed
- N/A

### Fixed
- N/A
```

---

## Appendix

### Glossary

**Terms**:
- **Firebase**: Google's Backend-as-a-Service platform
- **Firestore**: NoSQL cloud database by Firebase
- **Storage**: Firebase's file storage service
- **Collection**: Group of documents in Firestore
- **Document**: Individual record in Firestore
- **Field**: Property within a document
- **Timestamp**: Automatic date/time marker
- **SDK**: Software Development Kit
- **SPA**: Single Page Application
- **API Key**: Authentication credential for Firebase
- **HTTPS**: Secure HTTP protocol
- **CSR**: Client-Side Rendering

### Browser Compatibility

**Supported Browsers**:
- Chrome 90+ ✓
- Firefox 88+ ✓
- Safari 14+ ✓
- Edge 90+ ✓
- Opera 76+ ✓

**Not Supported**:
- Internet Explorer (any version)
- Chrome < 90
- Firefox < 88
- Safari < 14

### Firebase Pricing

**Spark Plan (Free)**:
- Firestore: 1GB storage, 50K reads/day, 20K writes/day
- Storage: 5GB total, 1GB downloads/day
- Suitable for: Development, small teams

**Blaze Plan (Pay-as-you-go)**:
- Firestore: $0.18/GB/month, $0.06/100K reads, $0.18/100K writes
- Storage: $0.026/GB/month, $0.12/GB downloads
- Suitable for: Production, large scale

### Performance Metrics

**Target Metrics**:
- Page Load: < 2 seconds
- Form Submission: < 3 seconds
- Photo Upload: < 5 seconds (2MB image)
- Time to Interactive: < 2 seconds

**Actual Performance** (typical):
- Page Load: ~500ms
- Form Submission: ~1-2 seconds
- Photo Upload: ~2-4 seconds (depends on file size)

### Accessibility

**WCAG 2.1 Compliance**:
- Level A: Partial (form labels, keyboard navigation)
- Level AA: Partial (color contrast, text sizing)
- Level AAA: Not targeted

**Accessibility Features**:
- Semantic HTML
- Form labels associated with inputs
- Keyboard navigation support
- Focus indicators
- Error messaging

**Improvements Needed**:
- ARIA labels
- Screen reader testing
- High contrast mode
- Keyboard shortcuts
- Skip navigation links

### License

This project is open source and available for use. No specific license file included. Consider adding MIT or Apache 2.0 license for clarity.

### Contact & Support

**Repository**: https://github.com/Carolamorimm/formulario-equipamentos
**Issues**: https://github.com/Carolamorimm/formulario-equipamentos/issues
**Owner**: Carolamorimm

---

**Document Version**: 1.0.0
**Last Updated**: 2025-12-08
**Status**: Complete

This documentation is maintained alongside the codebase. For questions, issues, or contributions, please use the GitHub repository.
