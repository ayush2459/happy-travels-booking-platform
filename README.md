

 ✈️ Happy Travels

### Full-Stack Multi-Modal Travel Booking Platform

> A complete travel booking platform supporting flights, trains, buses, and cab bookings — with integrated Razorpay payment processing, digital ticket generation, and a full booking management system.

</div>

---

## 📌 Overview

Happy Travels is a production-ready full-stack travel booking application that consolidates flights, trains, buses, and cab bookings into a single unified platform. Users can search routes, compare options, complete secure payments via Razorpay, and receive auto-generated digital tickets — all from one seamless interface.

The backend is built on Django with a robust relational schema to handle complex booking states, seat inventory, and payment lifecycle management. The React frontend provides an intuitive, multi-step booking flow with real-time availability checks.

---

## ✨ Features

### Booking Capabilities
- Flights — Search by origin/destination/date, seat class selection, multi-city support
- Trains — PNR-style booking with coach and berth selection
- Buses — Seat map with visual seat picker
- Cabs — Point-to-point fare estimation with driver assignment

### Payment Workflow
- Razorpay Integration — Secure checkout with card, UPI, net banking, and wallet support
- Payment status tracking — Real-time webhook-based payment confirmation
- Refund management — Cancellation triggers automated refund initiation
- Order receipts — Auto-generated receipts with transaction IDs

### Ticket Generation
- Digital tickets — Auto-generated PDFs with QR codes post-payment
- Booking reference — Unique PNR/booking IDs for each ticket
- Email delivery — Tickets sent to registered email on booking confirmation

### User Management
- Authentication — JWT-based session management
- Booking history — Complete past and upcoming trip dashboard
- Profile management — Saved traveller profiles for faster checkout

---

## 🏗️ Architecture

┌─────────────────────────────────────────────────────────────┐
│                    React 18 + Vite Frontend                 │
│   Search  →  Results  →  Seat Select  →  Checkout  →  Ticket│
└───────────────────────────┬─────────────────────────────────┘
                            │ REST API
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    Django REST Backend                      │
│  BookingService  │  InventoryManager  │  TicketGenerator   │
└───────┬───────────────────┬────────────────────┬───────────┘
        │                   │                    │
        ▼                   ▼                    ▼
┌──────────────┐  ┌─────────────────┐  ┌──────────────────┐
│  PostgreSQL  │  │  Razorpay API   │  │  Email / PDF     │
│   Database   │  │  Payment Gateway│  │  Ticket Service  │
└──────────────┘  └─────────────────┘  └──────────────────┘

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Django, Django REST Framework |
| Frontend | React 18, Vite |
| Database | PostgreSQL |
| Payments | Razorpay SDK |
| Containerization | Docker, Dockerfile |
| Auth | JWT Tokens |
| Ticket Generation | ReportLab / WeasyPrint |

---

## 💳 Razorpay Payment Integration

1. User confirms booking details
2. Backend creates Razorpay Order → returns order_id
3. Frontend opens Razorpay checkout modal
4. User completes payment (card/UPI/wallet)
5. Razorpay sends webhook to backend
6. Backend verifies signature → marks booking CONFIRMED
7. Ticket PDF generated → emailed to user

Supported Payment Methods: Credit/Debit Cards, UPI, Net Banking, Wallets (Paytm, PhonePe)

---

## 🗄️ Database Schema

Users
  ├── id, email, name, phone, created_at

TravellerProfiles
  ├── user_id, name, age, id_type, id_number

Bookings
  ├── id, user_id, mode (flight/train/bus/cab)
  ├── origin, destination, departure_dt
  ├── status (PENDING / CONFIRMED / CANCELLED)
  └── payment_id, total_amount

Seats
  ├── booking_id, seat_number, class, passenger_id

Payments
  ├── razorpay_order_id, razorpay_payment_id
  ├── amount, currency, status, signature_verified

---

## 🔌 API Architecture

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/search/flights/ | Search available flights |
| GET | /api/search/trains/ | Search train routes |
| GET | /api/search/buses/ | Search bus routes |
| POST | /api/bookings/ | Create a new booking |
| POST | /api/payments/create-order/ | Initialize Razorpay order |
| POST | /api/payments/verify/ | Verify payment signature |
| GET | /api/bookings/{id}/ticket/ | Download booking ticket |
| GET | /api/users/bookings/ | User booking history |
| DELETE | /api/bookings/{id}/cancel/ | Cancel and initiate refund |

---

## ⚙️ Installation

### Prerequisites
- Python 3.11+
- Node.js 18+
- Docker (optional)
- Razorpay account (for payment keys)

### Backend Setup

cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver

### Frontend Setup

npm install
npm run dev

### Docker Setup

docker build -t happy-travels .
docker run -p 8000:8000 happy-travels

### Environment Variables

SECRET_KEY=your-django-secret-key
DATABASE_URL=postgresql://user:pass@localhost/happytravels
RAZORPAY_KEY_ID=rzp_test_xxxxxxxxxxxxx
RAZORPAY_KEY_SECRET=your-razorpay-secret
EMAIL_HOST=smtp.gmail.com
EMAIL_HOST_USER=your@email.com
EMAIL_HOST_PASSWORD=your-app-password

---

## 🎫 Booking Flow

Home → Search Form
  ↓
Search Results (sorted by price/duration)
  ↓
Seat / Class Selection
  ↓
Traveller Details Form
  ↓
Order Summary + Razorpay Checkout
  ↓
Payment Confirmation Page
  ↓
Ticket PDF (download + email)

---

## 🧩 Challenges Solved

Concurrent seat booking — Implemented database-level row locking to prevent double-booking when two users select the same seat simultaneously.

Payment state consistency — Used Razorpay webhooks with idempotency keys to ensure booking confirmation is atomic, even if the user closes the tab mid-payment.

Multi-modal fare logic — Each travel mode (flight/train/bus/cab) has different fare calculation rules. Designed a polymorphic FareCalculator base class with mode-specific implementations.

---

## 🗺️ Future Features

- Live seat availability via WebSocket
- Loyalty points / rewards system
- Group bookings with split payment
- Multi-city itinerary builder
- WhatsApp ticket delivery integration

---

## 👤 Author

Ayush Gupta — Backend & AI Systems Engineer

GitHub: https://github.com/ayush2459
LinkedIn: https://linkedin.com/in/ayush-gupta-933b5b287
