---
name: Sentinel Command
colors:
  surface: '#051424'
  surface-dim: '#051424'
  surface-bright: '#2c3a4c'
  surface-container-lowest: '#010f1f'
  surface-container-low: '#0d1c2d'
  surface-container: '#122131'
  surface-container-high: '#1c2b3c'
  surface-container-highest: '#273647'
  on-surface: '#d4e4fa'
  on-surface-variant: '#c6c6cd'
  inverse-surface: '#d4e4fa'
  inverse-on-surface: '#233143'
  outline: '#909097'
  outline-variant: '#45464d'
  surface-tint: '#bec6e0'
  primary: '#bec6e0'
  on-primary: '#283044'
  primary-container: '#0f172a'
  on-primary-container: '#798098'
  inverse-primary: '#565e74'
  secondary: '#4edea3'
  on-secondary: '#003824'
  secondary-container: '#00a572'
  on-secondary-container: '#00311f'
  tertiary: '#ffb95f'
  on-tertiary: '#472a00'
  tertiary-container: '#251400'
  on-tertiary-container: '#b47300'
  error: '#ffb4ab'
  on-error: '#690005'
  error-container: '#93000a'
  on-error-container: '#ffdad6'
  primary-fixed: '#dae2fd'
  primary-fixed-dim: '#bec6e0'
  on-primary-fixed: '#131b2e'
  on-primary-fixed-variant: '#3f465c'
  secondary-fixed: '#6ffbbe'
  secondary-fixed-dim: '#4edea3'
  on-secondary-fixed: '#002113'
  on-secondary-fixed-variant: '#005236'
  tertiary-fixed: '#ffddb8'
  tertiary-fixed-dim: '#ffb95f'
  on-tertiary-fixed: '#2a1700'
  on-tertiary-fixed-variant: '#653e00'
  background: '#051424'
  on-background: '#d4e4fa'
  surface-variant: '#273647'
typography:
  display-lg:
    fontFamily: Geist
    fontSize: 48px
    fontWeight: '700'
    lineHeight: 56px
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Geist
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
  body-md:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
  data-mono:
    fontFamily: JetBrains Mono
    fontSize: 14px
    fontWeight: '500'
    lineHeight: 20px
  label-sm:
    fontFamily: Inter
    fontSize: 12px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.05em
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  unit: 4px
  container-padding: 24px
  gutter: 16px
  stack-sm: 8px
  stack-md: 16px
---

## Brand & Style

The design system is engineered for high-stakes fintech environments where rapid data processing and threat mitigation are paramount. It adopts a **Cybersecurity Command Center** aesthetic—professional, sophisticated, and intentionally technical. The interface minimizes cognitive load by using a deep, low-light environment that allows critical data points and security alerts to "pop" with neon clarity.

The personality is authoritative and vigilant. It avoids decorative flourishes in favor of precision and functional density. The emotional response is one of absolute control and security, ensuring that fraud analysts feel equipped with a high-performance instrument rather than a standard corporate tool.

## Colors

The palette is anchored in a **Deep Midnight Navy** ecosystem to maintain a premium, focused atmosphere. 

- **Backgrounds:** Use a tiered system of dark shades. The base layer is a pure charcoal-navy (#020617), with surface containers using slightly lighter navy-slates (#0F172A).
- **Security Accents:** These are the primary semantic drivers. 
    - **Emerald Green (#10B981):** Represents "Safe" or "Verified" states.
    - **Amber (#F59E0B):** Used for "Warning," "Medium Risk," or "Pending Investigation."
    - **Vivid Crimson (#EF4444):** Reserved exclusively for "Fraud Detected" or "High Risk" alerts.
- **Data Visualization:** Use a secondary spectrum of icy cyans and deep purples to differentiate non-semantic data streams.

## Typography

This design system utilizes a tiered typographic approach to handle extreme data density while maintaining legibility.

- **Headlines:** Use **Geist** for its sharp, technical geometry and excellent kerning in dark environments.
- **Body & UI:** **Inter** provides a highly legible, neutral foundation for all interface labels and descriptive text.
- **Data & Monospace:** **JetBrains Mono** is utilized for transaction IDs, IP addresses, and numerical tables. This ensures that characters like '0' and 'O' or '1' and 'l' are never confused during critical investigations.
- **Sizing:** Keep body text primarily at 14px-16px for density. Use uppercase labels for secondary metadata to create a structured, "instrument panel" feel.

## Layout & Spacing

The layout follows a **structured, modular grid** designed for complex dashboards. 

- **Grid:** A 12-column system is used for desktop views. Components should snap to 4px increments to maintain mathematical rigor.
- **Density:** High. Margins between data-heavy widgets are kept tight (16px) to maximize the information visible above the fold. 
- **Sidebars:** Use a fixed left-hand navigation (64px collapsed, 240px expanded) to keep the workspace dedicated to monitoring.
- **Adaptive Behavior:** On smaller screens, data tables should transition to a horizontal scroll or card-based summary, though the primary use case remains desktop-first command centers.

## Elevation & Depth

In this dark-mode environment, depth is communicated through **Tonal Layering** and **Subtle Outlines** rather than heavy shadows.

- **Surfaces:** Use three primary levels. 
    1. Base: Deepest Navy (#020617)
    2. Surface: Midnight Navy (#0F172A)
    3. Popover/Modal: Slate Blue (#1E293B)
- **Outlines:** Instead of shadows, use 1px borders with low-opacity slate (#334155).
- **Glow Effects:** Critical alerts (Crimson) use a subtle outer glow (0px 0px 12px) to simulate a glowing LED indicator, signaling urgency.

## Shapes

The design system uses a **Soft (0.25rem)** roundedness level. This provides a modern feel while maintaining the "hard" industrial edge expected of a security product.

- **Small Components:** Checkboxes and small buttons use a 4px radius.
- **Cards & Widgets:** Use a 6px or 8px radius maximum.
- **Search Inputs:** Should remain rectangular with slight rounding to feel grounded and stable.
- **Avoid Pills:** Except for specific "Status Badges," avoid fully rounded pill shapes to maintain a professional, non-consumer aesthetic.

## Components

- **Risk Score Badges:** High-contrast, small containers using the semantic color palette. The score should be in monospace bold.
- **Action Buttons:** 
    - *Primary:* Solid Slate with white text. 
    - *Destructive:* Solid Crimson for "Block Account" or "Flag Fraud."
    - *Ghost:* Subtle outlines for "Export" or "Filter."
- **Data Tables:** Zebra-striping is discouraged. Use thin 1px horizontal dividers. The header row should be slightly darker than the body with uppercase labels.
- **Investigation Tools:** Timeline components with vertical connectors and "Status Nodes" (small glowing dots) to show the path of a transaction.
- **Input Fields:** Darker than the container background, using a bright 2px left-border or bottom-border accent when focused to indicate activity.
- **Interactive Charts:** High-contrast line graphs with "Area under the curve" opacity of 10%. Hover states should trigger crosshair guides for precision.