# Farmhouse Booking Admin

An offline-capable, browser-based booking administration dashboard for a farmhouse, event venue, or private rental property. The application is intentionally lightweight: it is a static Progressive Web App (PWA) built with plain HTML, CSS, and JavaScript, so it can be deployed without a backend or database.

## What the project provides

- **Dashboard:** View total, today's, upcoming, and current-month booking metrics.
- **Booking management:** Create, edit, search, preview, and delete reservations.
- **Booking confirmation slips:** Generate a branded confirmation slip with guest details, event details, amenities, payment summary, terms, and farmhouse contact information.
- **Print and sharing:** Print a slip or use the browser's print dialog to save it as a PDF. Share a booking through WhatsApp, email, or the device's native share sheet when supported.
- **Local persistence:** Store bookings and farmhouse settings in browser `localStorage`; no account or server is required.
- **Backup and restore:** Export bookings, settings, and automatic snapshots as JSON, then import the JSON backup later.
- **Farmhouse branding:** Configure the name, logo, phone numbers, address, footer message, and automatic backup preference.
- **Installable PWA:** Use the manifest and service worker to install the dashboard and load the shell while offline.
- **Responsive interface:** Works on phone, tablet, and desktop screen sizes with a dark theme.

## Technology

| Area | Choice |
| --- | --- |
| UI | Semantic HTML5 |
| Styling | CSS with responsive media queries |
| Application logic | Vanilla JavaScript |
| Storage | Browser `localStorage` |
| Offline support | Web App Manifest and Service Worker |
| Development server | Python `http.server` |
| Deployment | Any static host, including Vercel |

## Project structure

| File | Purpose |
| --- | --- |
| `index.html` | Application shell, navigation, forms, dashboard table, and slip dialog |
| `styles.css` | Theme, responsive layout, controls, booking slip, and print styles |
| `app.js` | State management, booking CRUD, validation, rendering, backup/import, sharing, and PWA registration |
| `manifest.json` | Installable PWA metadata, scope, theme, and icons |
| `sw.js` | Versioned app-shell cache and offline navigation fallback |
| `package.json` | Local development, build, start, and syntax-check scripts |
| `vercel.json` | Vercel clean-URL configuration |

## Requirements

- Node.js 18+ (only needed for the provided npm scripts)
- Python 3 (used by the development server)
- A modern browser with support for `localStorage`, `dialog`, and service workers

## Run locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). Keep the app on `localhost` or another secure origin when testing service-worker installation; service workers do not work from a normal `file://` URL.

To serve the project with the production-style script:

```bash
npm start
```

## Verify the project

Run the JavaScript syntax check:

```bash
npm run check
```

Run the static build check:

```bash
npm run build
```

The build is intentionally dependency-free because the project is served as-is.

## Booking workflow

1. Open **New Booking**.
2. Enter the client, contact, address, date, time, event, guest count, and payment details.
3. Save the booking. Remaining balance is calculated from total amount minus advance payment.
4. Use the dashboard search to find a booking by client, contact, booking ID, or date.
5. Use **Generate Slip** to preview, print, save as PDF, or share the confirmation.

The form prevents an advance payment greater than the total amount and requires the end time to be later than the start time.

## Data and backups

All data is stored locally in the current browser profile:

- `farmhouse_bookings_v1`: booking records
- `farmhouse_settings_v1`: farmhouse settings and logo data
- `farmhouse_backups_v1`: up to 20 automatic booking snapshots when automatic backup is enabled

Use **Export JSON Backup** regularly, especially before clearing browser data or moving to another device. Imported JSON may be either the exported object (`bookings` and `settings`) or a plain booking array.

There is no shared cloud database, authentication layer, or multi-user synchronization. Each browser/device has its own data.

## Deployment

The project can be deployed to any static hosting provider:

1. Upload or connect the repository.
2. Use `npm run build` as the build command, or leave the build command empty on hosts that support static files directly.
3. Set the output directory to the repository root (`.`).
4. Serve the site over HTTPS so the service worker can install in production.

For Vercel, the included `vercel.json` enables clean URLs. The app does not require environment variables.

## Privacy and limitations

- Booking data remains in the browser unless a user exports or shares it.
- Clearing site data removes locally stored bookings and settings.
- The “Print / Save PDF” action opens the browser print flow; it does not generate a binary PDF file itself.
- WhatsApp and email sharing open external apps or URLs and depend on the device/browser configuration.
- Logo images are stored as data URLs in `localStorage`, so very large images can exceed browser storage limits.

## License

No license file is currently included. Add a license before distributing the project outside its intended private or organizational use.
