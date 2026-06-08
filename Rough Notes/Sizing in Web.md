---
aliases:
  - Responsive Design
tags:
  - Seed
references:
---
[[Websites]], [[Programming]], [[UX]], [[Apps]]
2026-06-05 16:59

### Relative Units
- **`rem` (Root EM):** Best for typography because it scales based on the user's default font size, so it ensures a readable, proportional experience
- **`em`:** Best for component-level sizing like padding because it scales relative to the font in its container
- **`vw/vh`**: Best for full-screen sections or responsive images because it's on a 100 scale relative to the screen size (same usage as `%`)
- **`ch`**: Relative to roughly the width of the digit "0" for the font family in use to ensure a readable width

### Absolute Units
- **`px`**: Best for fine details like borders and shadows; media queries to identify breakpoints for responsive design (identifying screen size); or fixed elements like logos or fixed spacing gaps