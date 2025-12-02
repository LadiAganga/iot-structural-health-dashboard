# IoT Structural Health Monitoring Dashboard

A Next.js 14 dashboard for monitoring structural health sensor data in real-time.

## Features

- 📊 **Dashboard** - Overview with strain graphs and latest readings
- 📜 **History** - Table view of all stored sensor data with pagination
- 🔴 **Live Monitoring** - Real-time updating charts (polls every 3 seconds)
- 🎨 **Modern UI** - Built with Tailwind CSS and Shadcn/UI components
- 📈 **Charts** - Interactive charts using Chart.js
- ⚙️ **Configurable Backend** - Switch between local and production APIs
- 🔄 **Automatic Retry** - Built-in retry logic with exponential backoff
- ⚡ **Error Handling** - Comprehensive error handling with retry buttons
- ⏱️ **Request Timeouts** - Prevents hanging requests

## Tech Stack

- **Next.js 14** - App Router
- **TypeScript** - Type safety
- **Tailwind CSS** - Styling
- **Chart.js** - Data visualization
- **Supabase** - Database client (optional, can use backend API)

## Getting Started

### Prerequisites

- Node.js 18+ 
- Backend API running on `http://localhost:3000`

### Installation

1. Install dependencies:
```bash
npm install
```

2. Create `.env.local` file:
```bash
cp env.local.example .env.local
```

3. Update `.env.local` with your configuration:

**For Local Development:**
```env
NEXT_PUBLIC_API_MODE=local
NEXT_PUBLIC_API_URL=http://localhost:3000
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_key
```

**For Production:**
```env
NEXT_PUBLIC_API_MODE=production
NEXT_PUBLIC_PRODUCTION_API_URL=https://your-production-backend.com
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_key
```

4. Run the development server:
```bash
npm run dev
```

5. Open [http://localhost:3001](http://localhost:3001) in your browser.

## Project Structure

```
dashboard/
├── app/
│   ├── layout.tsx          # Root layout with navbar
│   ├── page.tsx            # Dashboard page
│   ├── history/
│   │   └── page.tsx        # History page
│   ├── live/
│   │   └── page.tsx        # Live monitoring page
│   └── globals.css         # Global styles
├── components/
│   ├── Navbar.tsx          # Navigation component
│   ├── ChartCard.tsx       # Chart container
│   ├── LineChart.tsx       # Chart.js line chart
│   └── SensorCard.tsx      # Sensor reading card
├── lib/
│   ├── config.ts           # Backend API configuration
│   ├── api.ts              # Backend API client with retry logic
│   ├── supabase.ts         # Supabase client
│   ├── types.ts            # TypeScript types
│   └── utils.ts            # Utility functions
└── package.json
```

## API Configuration

The dashboard can switch between local and production backends using the `NEXT_PUBLIC_API_MODE` environment variable.

### Configuration Options

- **Local Mode** (`NEXT_PUBLIC_API_MODE=local`): Uses `NEXT_PUBLIC_API_URL` (default: `http://localhost:3000`)
- **Production Mode** (`NEXT_PUBLIC_API_MODE=production`): Uses `NEXT_PUBLIC_PRODUCTION_API_URL`

The configuration is managed in `lib/config.ts` and automatically applied to all API calls.

### Retry Logic

All API calls include automatic retry logic:
- **Max Attempts**: 3 retries
- **Initial Delay**: 1 second
- **Max Delay**: 5 seconds
- **Backoff**: Exponential (2x multiplier)
- **Timeout**: 10 seconds per request

Retries are automatically triggered for:
- Network errors
- Timeout errors
- Server errors (5xx)
- Rate limiting (429)
- Request timeout (408)

Client errors (4xx) are not retried except for 408 and 429.

## API Integration

The dashboard fetches data from your backend API:

- `GET /api/sensor/data?limit=100&offset=0` - Fetch sensor data
- `POST /api/sensor/upload` - Upload sensor data

The backend should return data in this format:
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "strain": 123.45,
      "created_at": "2024-01-01T00:00:00Z"
    }
  ],
  "count": 100
}
```

## Pages

### Dashboard (`/`)
- Displays latest sensor readings
- Shows strain graph for recent data
- Summary cards with key metrics

### History (`/history`)
- Table view of all sensor data
- Pagination support
- Sortable columns

### Live (`/live`)
- Real-time updating chart
- Auto-refreshes every 3 seconds
- Pause/resume functionality

## Building for Production

```bash
npm run build
npm start
```

## Deployment

See [VERCEL_DEPLOYMENT.md](./VERCEL_DEPLOYMENT.md) for detailed deployment instructions to Vercel.

### Quick Deploy to Vercel

1. Push your code to GitHub/GitLab/Bitbucket
2. Import your repository in [Vercel](https://vercel.com)
3. Add environment variables in Vercel dashboard
4. Deploy!

## License

ISC
