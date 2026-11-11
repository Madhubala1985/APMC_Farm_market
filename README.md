# APMC Farm Market

A farm-to-market web platform that lets farmers sell produce directly to buyers, inspired by India's APMC
(Agricultural Produce Market Committee) mandis. Farmers list crops and run auctions; buyers browse, compare,
bid and check out.

![react](https://img.shields.io/badge/React-18-61dafb) ![typescript](https://img.shields.io/badge/TypeScript-5-3178c6) ![supabase](https://img.shields.io/badge/Supabase-auth%20%7C%20db%20%7C%20storage-3ecf8e)

## Features

**For buyers**
- Marketplace with product grid, product detail pages and farmer reviews
- Live auction cards with bid history
- Side-by-side product comparison, wishlist, cart and checkout flow
- Farmer directory with public farmer profiles

**For farmers**
- Multi-step farmer registration and profile
- Dashboard with overview, listings manager, crop listing form with image upload, base-price settings, auctions and sales analytics (Recharts)

**Interface**
- Animated 3D farm scene on the landing page (Three.js), image slider, weather widget
- Responsive UI built with Tailwind CSS and shadcn/ui components

## Architecture

| Layer | Implementation |
|---|---|
| Frontend | React 18, TypeScript, Vite, React Router, TanStack Query, Tailwind CSS, shadcn/ui |
| Auth | Supabase Auth (email and password), user profiles with a farmer, consumer or admin role |
| Data | Supabase Postgres: `profiles` and `crops` tables, accessed through a typed service layer (`src/services/productService.ts`) |
| Files | Supabase Storage for crop images (`src/services/storageService.ts`) |
| Real time | Supabase Realtime subscription that pushes auction updates to open auction cards |
| Server | Supabase Edge Function (Deno) `increment-bid-count` |

## Project status
This is a working prototype. Sign-up, login, profiles, crop listings and image upload use the real Supabase
backend. Auction bidding, part of the farmer dashboard and the weather widget still run on sample data:
there is no auctions table yet.

## Quick start
```bash
npm install
npm run dev          # http://localhost:8080
```
The app connects to its Supabase project through `src/integrations/supabase/client.ts`. That file holds the
public anon key, which Supabase designs to be shipped in client code; data access is controlled by row-level
security on the database. To use your own backend, replace the URL and key there.

## Structure
```
src/
  pages/          Marketplace, ProductDetail, Auctions, Compare, Checkout, Farmers, FarmerDashboard, ...
  components/     product, auction, cart, wishlist and comparison components; farmer/ dashboard modules
  context/        authentication context
  services/       Supabase data and storage services
  integrations/   generated Supabase client and database types
supabase/
  functions/      edge functions
```

Initial scaffold generated with Lovable, then extended.
