# AquaRide (MERN)

India's marine travel platform: React (Vite) client, Express API, MongoDB (Mongoose).

## Run it (one command)

Requires only Node 18+.

```bash
npm run go
```

This installs everything, starts a built-in MongoDB (saved in `server/data`), seeds the demo data
and launches the API (:5000) and the site (:5173). The first run downloads MongoDB once, so allow a minute or two.
Open http://localhost:5173. The terminal also prints a Network address to open on your phone (same Wi-Fi).

To use your own MongoDB or Atlas, put its connection string in `server/.env` as `MONGO_URI`.
In development the OTP is shown on the verify screen; set `NODE_ENV=production` to hide it
and plug an SMS provider into `server/src/routes/auth.js`.

## Structure

```
server/src
  index.js            Express app + Mongo connection
  seed.js             Demo data
  models/             User, Otp (5 min TTL), Ferry, Route, ActiveFerry, Booking
  routes/auth.js      POST /api/auth/send-otp, /api/auth/verify  (JWT)
  routes/ferries.js   GET /api/ferries?from=&to=, /:id, /:id/seats?date=
  routes/bookings.js  GET/POST /api/bookings, PATCH /api/bookings/:bookingId/cancel   (auth)
  routes/tracking.js  GET /api/tracking?q=
client/src
  pages/              Splash, Login (OTP), Home, Search, Seats, Checkout, Bookings, Track
  api.js, auth.jsx    Fetch helper and JWT auth context
```

## Notes

- Seat availability is per ferry and date; the server re-checks seats when booking and returns 409 on a clash.
- Price = fare × seats + 5% GST + ₹50 fee, calculated on the server.
- Not wired up yet: real payments (the button creates the booking directly), SMS delivery, Google sign-in, QR/ticket download, live GPS positions (tracking uses seeded data).
