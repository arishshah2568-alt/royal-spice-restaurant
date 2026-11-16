# Royal Spice

Responsive restaurant ordering frontend with a separate Node.js API/admin backend.

## Hosting architecture

- **Frontend:** GitHub Pages publishes the `public/` directory.
- **Backend:** Deploy `server.js` as a Node.js web service on Render.
- **Database:** SQLite is configured through `DATABASE_FILE`; production must mount durable storage at `/var/data` (or migrate to a managed database) before accepting real orders.

## GitHub Pages

1. In the GitHub repository, open **Settings → Pages**.
2. Under **Build and deployment**, choose **GitHub Actions** as the source.
3. Push changes to `main`. The workflow in `.github/workflows/pages.yml` deploys `public/` automatically.
4. The expected site URL is `https://arishshah2568-alt.github.io/royal-spice-restaurant/`.

Before the site can use live menu/orders, edit `public/config.js` and replace the placeholder with the Render API base URL (no trailing slash), then commit the change.

## Render API configuration

Build command: `npm install`

Start command: `npm start`

Health check: `/health`

Set these environment variables in Render:

- `NODE_ENV=production`
- `ADMIN_USER_ID=arishshahroyal123`
- `ADMIN_PASSWORD` = a strong, unique password (set privately in Render)
- `JWT_SECRET` = a long random secret (Render can generate this)
- `FRONTEND_ORIGIN=https://arishshah2568-alt.github.io`
- `WHATSAPP_NUMBER=919669219030`
- `UPI_ID=9669219030@ybl`
- `RESTAURANT_NAME=Royal Spice`
- `DATABASE_FILE=/var/data/royal-spice.sqlite`

Attach a persistent disk mounted at `/var/data` before using this backend for real orders. An ephemeral filesystem can lose orders and payment records when the service restarts or redeploys. Do not deploy production orders without durable storage.

## Local development

```bash
cp .env.example .env
npm install
npm start
```

The frontend and API can run on the same origin locally. For split-origin development, set `FRONTEND_ORIGIN` to the exact frontend origin and set `window.ROYAL_SPICE_API_BASE` in `public/config.js`.

## Admin

Open `/admin.html` on the Pages site. Admin login uses a short-lived bearer token stored in the current browser tab's `sessionStorage`. Keep `JWT_SECRET` and `ADMIN_PASSWORD` out of GitHub and never paste them into public files.

## Payments

UPI payment is customer-initiated. A submitted UTR/reference is not proof of payment. Admin must match the exact transaction and amount against the merchant bank/UPI statement before marking it verified. This is manual reconciliation, not automatic bank verification.
