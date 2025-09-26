# Request Management System

A comprehensive web-based request management system built with Node.js, Express, SQLite, and modern frontend technologies.

## Features

- 🔐 **User Authentication** - Login system with admin/admin credentials
- 📊 **Dashboard** - Real-time statistics and request overview
- 📝 **Request Management** - Create and view requests (read-only interface)
- 👥 **User Profiles** - Manage user information and team details
- 🔄 **Status Tracking** - Track requests through submitted → processing → completed (updated via Spark script)
- 💾 **SQLite Database** - Persistent data storage with audit trail
- 📱 **Responsive Design** - Works on desktop and mobile devices
- 🎨 **Clean UI** - Minimal, professional interface

## System Requirements

- **Node.js** (v14 or higher)
- **npm** (v6 or higher)
- Modern web browser (Chrome, Firefox, Safari, Edge)

## Installation & Setup

### 1. Install Dependencies

```bash
npm install
```

### 2. Initialize Database

```bash
npm run init-db
```

This will:
- Create the SQLite database file at `database/requests.db`
- Set up all required tables (users, requests, request_status_history)
- Insert sample data for testing

### 3. Start the Server

```bash
npm start
```

For development with auto-restart:
```bash
npm run dev
```

The server will start on `http://localhost:3000`

## Usage

### 1. Login
- Open your browser and navigate to `http://localhost:3000`
- Use the credentials:
  - **Username**: `admin`
  - **Password**: `admin`

### 2. Dashboard
After login, you'll see the main dashboard with:
- **Statistics Cards**: Total, submitted, processing, and completed requests
- **Requests Table**: All requests with status, dates, and actions
- **Navigation Tabs**: Dashboard, New Request, and User Info

### 3. Create New Request

Click on the "New Request" tab and fill out the form:

#### Requester Details
- **Requestor Name** (required)
- **Requestor Email** (required)
- **CC Email** (optional)
- **Team Name** (required)

#### Request Details
- **Category Name** (required): Select from dropdown (val1, val2)
- **Date** (required): Multi-select calendar for multiple dates
- **Account Number** (required)
- **Name** (required)
- **Currency** (required): Select from 20+ currency codes (USD, EUR, GBP, etc.)
- **Amount** (required): Numeric amount
- **Description** (optional): Additional details

### 4. User Information
- Set up your user profile in the "User Info" tab
- Information is saved to the database and used to auto-populate request forms

### 5. Request Management
- View all requests in the dashboard table (read-only)
- Click any row to see detailed request information
- Status changes are handled by Spark script and updated directly in the database
- All status changes are tracked with timestamps and user information

## Database Schema

### Users Table
- `id` - Primary key
- `username` - Unique username
- `team` - User's team
- `test_box` - Additional test information
- `created_at`, `updated_at` - Timestamps

### Requests Table
- `id` - Primary key
- `request_id` - Unique request identifier (REQ + timestamp + UUID)
- Requester information (name, email, cc_email, team_name)
- Request details (category, dates, account, name, currency, amount, description)
- Status tracking (status, request_datetime, status_update_datetime)
- Timestamps for audit

### Request Status History Table
- Audit trail for all status changes
- Tracks old status → new status transitions
- Records who made the change and when
- Optional notes for status changes

## API Endpoints

### Requests
- `GET /api/requests` - Get all requests
- `GET /api/requests/:requestId` - Get specific request
- `POST /api/requests` - Create new request
- `GET /api/requests/:requestId/history` - Get status change history
- **Note**: Status updates are handled by external Spark script

### Users
- `GET /api/users/:username` - Get user information
- `POST /api/users` - Create/update user information

### System
- `GET /api/statistics` - Get dashboard statistics
- `GET /api/health` - Health check endpoint

## File Structure

```
c3_app/
├── index.html              # Login page
├── dashboard.html          # Main application
├── server.js              # Express server
├── package.json           # Dependencies and scripts
├── README.md              # This file
├── css/
│   ├── main.css           # Login page styles
│   ├── util.css           # Utility styles
│   └── dashboard.css      # Dashboard styles
├── js/
│   ├── main.js            # Login functionality
│   └── dashboard.js       # Dashboard functionality
├── database/
│   └── requests.db        # SQLite database (created after init)
└── scripts/
    └── init-database.js   # Database initialization
```

## Unique Request ID Generation

Request IDs are generated using the format: `REQ{timestamp}{UUID}`
- **REQ** - Fixed prefix
- **timestamp** - Current timestamp in milliseconds
- **UUID** - First 8 characters of a UUID v4

This ensures uniqueness even with multiple concurrent users.

## Development

### Available Scripts
- `npm start` - Start production server
- `npm run dev` - Start development server with auto-restart
- `npm run init-db` - Initialize/reset database

### Adding New Features
1. Update database schema in `scripts/init-database.js`
2. Add API endpoints in `server.js`
3. Update frontend in `dashboard.html` and `dashboard.js`
4. Test with sample data

## Troubleshooting

### Database Issues
- Delete `database/requests.db` and run `npm run init-db` again
- Check file permissions in the database directory

### Server Won't Start
- Ensure Node.js and npm are installed
- Check if port 3000 is available
- Review console output for specific errors

### Login Issues
- Use exact credentials: `admin` / `admin`
- Clear browser cache and localStorage
- Check browser console for JavaScript errors

## Security Notes

⚠️ **Important**: This is a development/demo application with basic authentication.

For production use, consider:
- Implementing proper user authentication and authorization
- Using environment variables for sensitive configuration
- Adding input validation and sanitization
- Implementing proper error handling and logging
- Using HTTPS and security headers
- Adding rate limiting and request validation

## Browser Compatibility

Tested and supported on:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## License

MIT License - feel free to modify and use for your projects.

## Support

For issues or questions, please check:
1. This README file
2. Console output for error messages
3. Browser developer tools for frontend issues