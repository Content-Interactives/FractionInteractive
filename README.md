# Fraction Addition Interactive (`FractionInteractive`)

> **Status:** This repo is an **older interactive prototype** and is **not used** in current Content-Interactives curriculum delivery.  
> **Replacement:** Use the maintained project **[fraction_addition](https://github.com/Content-Interactives/fraction_addition)** — README: https://github.com/Content-Interactives/fraction_addition?tab=readme-ov-file

**Legacy live site (GitHub Pages):** https://content-interactives.github.io/FractionInteractive/

A React app that walks through **adding two proper fractions** with denominators from **1–10**: common denominator (LCM), equivalent numerators, sum, then **GCD simplification**. Each step can show **circle (pie) diagrams** built from Recharts so the fraction is visible as shaded wedges.

## Tech stack

| Layer | Choice |
|--------|--------|
| UI | React 18 |
| Build | Vite 6 with `@vitejs/plugin-react` |
| Charts | Recharts (`PieChart` / `Pie` / `Cell`) |
| Styling | `FractionAddition.css` (primary); Tailwind 3 is in the project but **`tailwind.config.js` has `content: []`**, so utility classes are not wired to source files |
| Deploy | `gh-pages` publishing `dist/`; `package.json` → `"homepage": "https://content-interactives.github.io/FractionInteractive"` |

Production assets use **`base: '/FractionInteractive/'`** in `vite.config.js`, matching the live URL above.

## Repository layout

```
src/
  main.jsx
  App.jsx
  App.css
  index.css              # @tailwind directives only
  components/
    FractionAddition.jsx # Main UI and math flow (mounted by App)
    FractionAddition.css
    FractionInteractive.jsx  # Empty stub; not imported
```

## User flow

1. **Input:** Two fractions; each numerator and denominator are integers **1–10** (`handleInputChange` clamps). **Start Steps** is enabled only when all four fields are filled.
2. **`startSteps`:** Computes:
   - `commonDen` = LCM(`den1`, `den2`) via `findLCM` / `findGCD`
   - Adjusted numerators `newNum1`, `newNum2`
   - `sumNum`, then simplified result with `gcd(sumNum, commonDen)`
   - Stores a `result` object with `steps` for the wizard and final `numerator` / `denominator`
3. **Step wizard (`currentStep` 0–3):** Animated transitions (`step-exit` / `step-enter` on `currentStep` change). **Continue** advances; step 3’s **Finish** sets `isComplete` and shows the completion screen.
4. **Reset:** Clears inputs, step index, flags, and result.

While `showSteps` is true, inputs are read-only (`handleInputChange` returns early).

## Pie chart model

`generatePieData(numerator, denominator)` builds `denominator` slices of equal weight; the first **`floor((numerator/denominator)*denominator)`** slices are marked `filled: true`. Recharts paints filled slices `#0088FE` and empty `#FFFFFF`, with a blue outline (`OUTLINE_COLOR`).

## Presentation details

- Inline **`Fraction`** component: stacked numerator / bar / denominator with typography aligned to the chart accent (`#0088FE`).
- **Accessibility:** Charts use `focusable={false}` on `Pie`; step content is heading + buttons without dedicated live regions for screen readers.

## Scripts

| Command | Purpose |
|---------|---------|
| `npm run dev` | Vite dev server |
| `npm run build` | Output to `dist/` |
| `npm run preview` | Serve `dist/` locally |
| `npm run deploy` | `predeploy` build then `gh-pages -d dist` |
| `npm run lint` | ESLint |

## Dead code and dependencies

- **`FractionInteractive.jsx`** exports an empty component and is **not** used; **`App.jsx`** renders **`FractionAddition`** only.
- **`package.json`** includes **`lucide-react`** and **`react-beautiful-dnd`** but there are **no imports** of these in `src/`; they may be removed in a cleanup pass if this repo is kept for archival reference only.

## Comparison with the replacement repo

For current fraction-addition interactives, prefer **[Content-Interactives/fraction_addition](https://github.com/Content-Interactives/fraction_addition)**. This repository remains as a **historical prototype** with the Pages/Capitalization path **`FractionInteractive`**.
