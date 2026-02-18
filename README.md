# Data Analyst Portfolio Website

A modern, interactive portfolio website built with React, TypeScript, and Tailwind CSS.

## Tech Stack

- **Frontend Framework:** React 19 with TypeScript
- **Build Tool:** Vite
- **Styling:** Tailwind CSS with custom design tokens
- **Routing:** React Router v7
- **Data Visualization:** Chart.js + React-Chartjs-2
- **Form Handling:** React Hook Form + Zod
- **Testing:** Vitest + React Testing Library + fast-check

## Project Structure

```
src/
├── components/
│   ├── common/       # Reusable UI components
│   ├── home/         # Home page components
│   ├── about/        # About page components
│   ├── projects/     # Project showcase components
│   ├── skills/       # Skills display components
│   └── contact/      # Contact form components
├── pages/            # Page components
├── hooks/            # Custom React hooks
├── types/            # TypeScript type definitions
├── data/             # Static data files
├── utils/            # Utility functions
├── styles/           # Global styles and theme
└── test/             # Test setup and utilities
```

## Getting Started

### Install Dependencies

```bash
npm install
```

### Development Server

```bash
npm run dev
```

### Build for Production

```bash
npm run build
```

### Run Tests

```bash
npm test
```

### Preview Production Build

```bash
npm run preview
```

## Design Tokens

The project uses a custom design system defined in `src/styles/theme.ts`:

- **Colors:** Primary blue, secondary purple, accent cyan
- **Typography:** Inter (headings), Open Sans (body), Fira Code (code)
- **Spacing:** Consistent spacing scale from xs to 2xl
- **Transitions:** Fast (150ms), normal (300ms), slow (500ms)
- **Breakpoints:** Mobile-first responsive design

## Testing Strategy

- **Unit Tests:** Specific examples and edge cases
- **Property-Based Tests:** Universal correctness properties (100+ iterations)
- **Integration Tests:** Key user flows and interactions

## License

MIT
