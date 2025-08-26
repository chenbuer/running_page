# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Frontend (React + Vite + TypeScript)
- `pnpm dev` - Start development server (http://localhost:5173)
- `pnpm build` - Build for production
- `pnpm lint` - Run ESLint
- `pnpm check` - Check formatting with Prettier
- `pnpm format` - Format code with Prettier
- `pnpm ci` - Run full CI checks (format + lint + build)

### Backend (Python)
- `python run_page/garmin_sync.py` - Sync Garmin data
- `python run_page/gen_svg.py --from-db --type github --output assets/github.svg` - Generate GitHub-style SVG
- `python run_page/db_updater.py` - Update database schema
- `black .` - Format Python code

### Data Management
- `pnpm data:clean` - Clean all data files
- `pnpm data:download:garmin` - Download Garmin data
- `pnpm data:analysis` - Generate data analysis SVGs

## Architecture

### Frontend Structure
- **Framework**: React 18 with TypeScript
- **Build Tool**: Vite
- **Styling**: Tailwind CSS
- **Maps**: Mapbox GL with react-map-gl
- **Routing**: React Router DOM
- **State**: React Hooks (no external state management)
- **SVG Handling**: vite-plugin-svgr

Key files:
- `src/utils/const.ts` - Configuration constants (Mapbox token, styling options)
- `src/static/site-metadata.ts` - Site metadata and navigation
- `src/hooks/useActivities.ts` - Main data fetching hook
- `src/components/RunMap` - Map visualization component
- `src/components/RunTable` - Activities table component

### Backend Structure
- **Language**: Python 3.12+
- **Data Storage**: SQLite database (`run_page/data.db`)
- **Data Sync**: Multiple sync scripts for different platforms (Garmin, Strava, Nike, etc.)
- **SVG Generation**: GPXTrackPoster-based visualization

Key directories:
- `run_page/` - Python backend code
  - `garmin_sync.py` - Garmin data synchronization
  - `strava_sync.py` - Strava data synchronization  
  - `gen_svg.py` - SVG visualization generation
  - `gpxtrackposter/` - SVG generation library
- `GPX_OUT/`, `TCX_OUT/`, `FIT_OUT/` - Activity file output directories
- `activities/` - Processed activity data

### Data Flow
1. Sync scripts fetch data from platforms (Garmin, Strava, etc.)
2. Data stored in SQLite database and output directories
3. Frontend reads from `src/static/activities.json` (generated from DB)
4. SVG visualizations generated from database
5. React app displays activities and visualizations

### Deployment
- **Primary**: Vercel (configured in `vercel.json`)
- **Alternative**: GitHub Pages (via Actions)
- **Docker**: Container support available

### Configuration
- Mapbox token: Set in `src/utils/const.ts`
- Privacy settings: Environment variables for data filtering
- Styling: Constants in `src/utils/const.ts`
- Site metadata: `src/static/site-metadata.ts`