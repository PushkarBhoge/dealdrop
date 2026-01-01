# 🛒 DealDrop - Smart Price Tracking Platform

**Never Miss a Price Drop Again!** Track prices from any e-commerce site and get instant alerts when prices drop.

![DealDrop Logo](./public/deal-drop-logo.png)

## ✨ Features

### 🚀 Core Functionality
- **Universal Price Tracking**: Track products from Amazon, Walmart, and other major e-commerce sites
- **Real-time Price Monitoring**: Automated price checking with intelligent web scraping
- **Smart Alerts**: Instant email notifications when prices drop below previous levels
- **Price History**: Visual charts showing price trends over time
- **User Authentication**: Secure login with Supabase Auth

### 🔧 Technical Features
- **Lightning Fast Scraping**: Powered by Firecrawl for handling JavaScript and dynamic content
- **Anti-bot Protection**: Built-in mechanisms to work reliably across all sites
- **Automated Monitoring**: Cron jobs for continuous price checking
- **Responsive Design**: Beautiful UI that works on all devices

## 🖥️ Live Demo
https://dealdrop-teal.vercel.app

### Pages
<img width="1917" height="872" alt="image" src="https://github.com/user-attachments/assets/b8ca8ee3-0983-4328-861c-d0a6ec25855b" />
<img width="1918" height="871" alt="image" src="https://github.com/user-attachments/assets/78592b1a-cf46-4b01-b488-b52d8fcebaa1" />
<img width="1918" height="868" alt="image" src="https://github.com/user-attachments/assets/dd15b9f4-d22b-446a-8d9b-9a1a4f8a5d7d" />

### Email
<img width="1918" height="867" alt="image" src="https://github.com/user-attachments/assets/db2044b9-6bb1-4dbc-8b16-e3dc93f702cf" />

## 🚀 Getting Started

### Prerequisites
- Node.js 18+ 
- npm/yarn/pnpm
- Supabase account
- Firecrawl API key
- Resend account (for emails)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/dealdrop.git
cd dealdrop
```

2. **Install dependencies**
```bash
npm install
# or
yarn install
# or
pnpm install
```

3. **Environment Setup**
Create a `.env` file in the root directory:

```env
# Firecrawl API for web scraping
FIRECRAWL_API_KEY=your_firecrawl_api_key

# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

# Email Notifications
RESEND_API_KEY=your_resend_api_key
RESEND_FROM_EMAIL=your_verified_email

# Cron Job Security
CRON_SECRET=your_secure_random_string
```

4. **Database Setup**
Set up your Supabase database with the following tables:

```sql
-- Products table
CREATE TABLE products (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  url TEXT NOT NULL,
  name TEXT NOT NULL,
  current_price DECIMAL(10,2) NOT NULL,
  currency TEXT DEFAULT 'USD',
  image_url TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(user_id, url)
);

-- Price history table
CREATE TABLE price_history (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  product_id UUID REFERENCES products(id) ON DELETE CASCADE,
  price DECIMAL(10,2) NOT NULL,
  currency TEXT DEFAULT 'USD',
  checked_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

5. **Run the development server**
```bash
npm run dev
# or
yarn dev
# or
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) to see the application.

## 📱 Usage Guide

### Adding Products
1. **Sign in** using the authentication button
2. **Paste product URL** from supported e-commerce sites
3. **Click "Track Price"** to start monitoring
4. **View your dashboard** with all tracked products

### Monitoring Prices
- Products are automatically checked for price changes
- Email alerts sent when prices drop
- View price history charts for trends
- Delete products you no longer want to track

### Supported Sites
- Amazon
- Walmart
- Target
- Best Buy
- And many more e-commerce platforms

## 🛠️ Tech Stack

### Frontend
- **Next.js 16** - React framework with App Router
- **React 19** - Latest React features
- **Tailwind CSS** - Utility-first styling
- **Lucide React** - Beautiful icons
- **Recharts** - Interactive price charts
- **Sonner** - Toast notifications

### Backend
- **Next.js API Routes** - Serverless functions
- **Supabase** - Database and authentication
- **Firecrawl** - Web scraping service
- **Resend** - Email delivery service

### DevOps
- **Vercel** - Deployment platform
- **Cron Jobs** - Automated price checking
- **Environment Variables** - Secure configuration

## 🔄 Automated Price Monitoring

### Cron Job Setup
The application includes automated price checking via `/api/cron/check-prices`:

```javascript
// Endpoint: POST /api/cron/check-prices
// Headers: Authorization: Bearer YOUR_CRON_SECRET
```

**Process:**
1. Fetches all tracked products
2. Scrapes current prices using Firecrawl
3. Compares with stored prices
4. Updates database with new prices
5. Sends email alerts for price drops
6. Logs price history

### Email Notifications
Beautiful HTML email templates with:
- Product images and details
- Price comparison (old vs new)
- Savings calculation
- Direct purchase links
- Responsive design

## 📊 Database Schema

### Products Table
```sql
- id: UUID (Primary Key)
- user_id: UUID (Foreign Key to auth.users)
- url: TEXT (Product URL)
- name: TEXT (Product Name)
- current_price: DECIMAL (Current Price)
- currency: TEXT (Currency Code)
- image_url: TEXT (Product Image)
- created_at: TIMESTAMP
- updated_at: TIMESTAMP
```

### Price History Table
```sql
- id: UUID (Primary Key)
- product_id: UUID (Foreign Key to products)
- price: DECIMAL (Historical Price)
- currency: TEXT (Currency Code)
- checked_at: TIMESTAMP (When Price Was Recorded)
```

## 🚀 Deployment

### Vercel Deployment
1. **Connect your repository** to Vercel
2. **Add environment variables** in Vercel dashboard
3. **Deploy** - Vercel will automatically build and deploy

### Cron Job Setup
Set up automated price checking using Vercel Cron Jobs or external services like:
- GitHub Actions
- Uptime Robot
- Cron-job.org

Example cron configuration:
```bash
# Check prices every hour
0 * * * * curl -X POST https://your-app.vercel.app/api/cron/check-prices \
  -H "Authorization: Bearer YOUR_CRON_SECRET"
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Pushkar Bhoge**
- GitHub: [@pushkarbhoge](https://github.com/pushkarbhoge)
- LinkedIn: [Pushkar Bhoge](https://linkedin.com/in/pushkarbhoge)

## 🙏 Acknowledgments

- [Next.js](https://nextjs.org/) for the amazing React framework
- [Supabase](https://supabase.com/) for backend infrastructure
- [Firecrawl](https://firecrawl.dev/) for reliable web scraping
- [Resend](https://resend.com/) for email delivery
- [Vercel](https://vercel.com/) for seamless deployment

---

**Built with ❤️ by Pushkar Bhoge**

*Never miss a deal again with DealDrop!* 🛒✨
