# Portfolio Assignment — Student Information Sheet

### CSC4035 Web Programming and Technologies

---

## My Information

| Field          | Details                                                |
| -------------- | ------------------------------------------------------ |
| **Name**       | Gift Kanene                                            |
| **Student ID** | 2021432939                                             |
| **Course**     | CSC4035 – Web Programming and Technologies             |
| **Assignment** | Assignment 1: Responsive Portfolio Website             |
| **GitHub**     | [github.com/Giftkanene](https://github.com/Giftkanene) |
| **Email**      | kanenegift@gmail.com                                   |

---

## 🎨 My Design Theme

My portfolio follows a **dark navy and crimson editorial** aesthetic. I wanted the site to feel professional and modern something that would stand out while still being clean and readable.

The colour palette is built around deep navy blues (`#1a1a2e`, `#16213e`) as the primary background tones, with a bold crimson-red accent (`#e94560`) used for highlights, buttons, and interactive elements. I chose this combination because it gives the portfolio a strong visual identity without being overwhelming.

For typography, I paired **Playfair Display** (a serif display font from Google Fonts) for headings with **DM Sans** for body text. This contrast between a classical serif and a clean geometric sans-serif gives the site an editorial, magazine-like quality that I personally like.

The site also supports a **dark/light mode toggle**, so the full colour scheme adapts — the dark mode uses the navy palette while the light mode switches to a soft off-white (`#f8f8f4`) background, keeping everything readable and accessible.

---

## 🛠️ CSS Techniques Used

### ✅ CSS Custom Properties

I used CSS custom properties (variables) extensively throughout the project. All colours, spacing values, font families, border-radius, and shadow values are defined in `:root` and a `[data-theme="dark"]` block. This made implementing the dark/light mode toggle extremely straightforward — flipping the theme just swaps the variable values, and every element updates automatically without any JavaScript touching individual styles.

```css
:root {
  --primary: #1a1a2e;
  --accent: #e94560;
  --body-bg: #f8f8f4;
  --radius: 12px;
  --shadow: 0 4px 24px rgba(0, 0, 0, 0.09);
}
```

### ✅ Flexbox

I used Flexbox in several key areas: the sticky navigation bar (logo left, menu centre, controls right), the hero section (centring the content vertically and horizontally), the footer (copyright left, social links right), the contact details list (icon + text alignment), and the skill pills row (wrapping flex layout).

```css
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

### ✅ CSS Grid

I used CSS Grid for the main page layout (`grid-template-rows: auto 1fr auto` on `body`), the about section two-column layout (image + text), the skills grid, the projects card grid (`repeat(auto-fit, minmax(300px, 1fr))`), and the contact section (info + form side by side).

```css
.projects__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 28px;
}
```

### ✅ Media Queries

I implemented three responsive breakpoints to make the portfolio fully mobile-friendly:

- **860px** — About section and contact wrapper collapse to single column
- **768px** — Navigation switches to the CSS-only hamburger menu; project grid becomes single column
- **480px** — Font sizes reduce, footer stacks vertically, hero shrinks

```css
@media (max-width: 768px) {
  .nav__hamburger {
    display: flex;
  }
  .nav__menu {
    display: none;
    flex-direction: column;
  }
  #nav-toggle:checked ~ .header .nav__menu {
    display: flex;
  }
}
```

### ✅ Other Techniques Used

**CSS Animations & Keyframes** — I added six custom `@keyframes` animations: `fadeInUp` (hero content staggered reveal), `slideInLeft` / `slideInRight` (about section entrance), `popIn` with staggered delays (skill pills), `cardReveal` (project cards), `underlinePulse` (section heading underline loop), and `spinIn` (dark/light toggle icon). All animations use the `animation-fill-mode: both` shorthand and most include an `animation-delay` for a staggered effect.

**CSS-Only Hamburger Menu** — The mobile hamburger menu works entirely without JavaScript. It uses a hidden `<input type="checkbox" id="nav-toggle">` and a `<label for="nav-toggle">` as the clickable trigger. The `~` general sibling CSS selector detects the `:checked` state and shows/hides the menu. The three bars also animate into an X shape using CSS `transform`.

```css
#nav-toggle:checked ~ .header .nav__hamburger .hamburger-bar:nth-child(1) {
  transform: translateY(7px) rotate(45deg);
}
```

**Dark/Light Mode Toggle** — Implemented using `data-theme` attribute switching on the `<html>` element combined with CSS custom property overrides. The user's preference is saved to `localStorage` so it persists between visits.

**Print Stylesheet** — A `@media print` block strips all navigation, animations, and interactive elements, forces white backgrounds and black text, simplifies the hero section into a plain name block, and appends full URLs after external links for readability on paper.

**CSS Transitions** — Smooth transitions are applied to nearly all interactive elements: button hover lifts, project card hover elevations with box-shadow changes, skill pill colour changes, navigation link backgrounds, and the global background/colour swap when switching themes.

---

## 🚧 Challenges & Solutions

**Challenge 1 — CSS-only hamburger with sticky header**
The hamburger checkbox had to be a sibling of the header element to use the `~` CSS sibling selector, but it also had to come _before_ the header in the DOM. This was tricky because I needed the `#nav-toggle:checked ~ .header` selector to work. My solution was to place the `<input>` just before `<header>` as a direct sibling, which made the selector work correctly.

**Challenge 2 — Dark mode with the hero section**
The hero uses a hardcoded gradient background because CSS gradient values cannot reference custom properties in all browsers without extra handling. I had to keep the hero gradient hardcoded and use the overlay (`::before` pseudo-element with a radial gradient) as the part that shifts with the theme.

**Challenge 3 — CSS animation retrigger on the theme icon**
When toggling between dark and light mode, the `spinIn` animation on the Font Awesome icon would not retrigger because the browser considers the animation already completed. I solved this using a double `requestAnimationFrame` call — this waits for the browser to complete its current paint cycle before resetting and reapplying the animation class.

```javascript
themeIcon.style.animation = "none";
requestAnimationFrame(() => {
  requestAnimationFrame(() => {
    themeIcon.style.animation = "spinIn 0.4s ease both";
  });
});
```

**Challenge 4 — Responsive layout collapsing gracefully**
Getting the about section (image + text grid) and the contact section (info + form grid) to collapse to single-column on mobile without breaking spacing took several iterations with media queries. I resolved this by keeping the column widths in CSS variables and overriding just the `grid-template-columns` property inside each breakpoint.

---

## 📦 Credits & Resources

### Fonts

| Resource         | Source                                                             | Usage             |
| ---------------- | ------------------------------------------------------------------ | ----------------- |
| Playfair Display | [Google Fonts](https://fonts.google.com/specimen/Playfair+Display) | Headings and logo |
| DM Sans          | [Google Fonts](https://fonts.google.com/specimen/DM+Sans)          | Body text and UI  |

### Icons

| Resource       | Source                                                                                            | Usage                                                       |
| -------------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| Font Awesome 6 | [cdnjs.cloudflare.com](https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css) | Navigation icons, buttons, contact list icons, social links |

### Images

| Resource        | Source                                                                                                        | Usage                   |
| --------------- | ------------------------------------------------------------------------------------------------------------- | ----------------------- |
| Project 1 cover | [Unsplash – @benitoalberto](https://unsplash.com/photos/round-silver-colored-analog-watch-tCl4b9b_O08)        | ShopEase project card   |
| Project 2 cover | [Unsplash – @glenncarstenspeters](https://unsplash.com/photos/white-printer-paper-on-white-table-RLw-UC03Gwc) | TaskFlow project card   |
| Project 3 cover | [Unsplash – @andrewtneel](https://unsplash.com/photos/white-clouds-and-blue-sky-during-daytime-Qn_G-ZGfcKE)   | WeatherNow project card |

> **Note:** Project cover images are temporary placeholders. They are intended to be replaced with Pinterest product-style photos of your choice. To swap, replace the `src` attribute on each `<img>` inside `.project-card__image` with your chosen Pinterest image URL.

### Colour Palette Reference

| Name             | Hex       | Used For                                        |
| ---------------- | --------- | ----------------------------------------------- |
| Navy Primary     | `#1a1a2e` | Header, footer, skill pills                     |
| Navy Secondary   | `#16213e` | Hero gradient, secondary elements               |
| Deep Blue        | `#0f3460` | Hero gradient end                               |
| Crimson Accent   | `#e94560` | Buttons, borders, badges, highlights            |
| Dark Crimson     | `#c73652` | Hover state for buttons                         |
| Off-White        | `#f8f8f4` | Light mode page background                      |
| Light Blue-Grey  | `#eef3fb` | About and contact section backgrounds           |
| Dark Mode Accent | `#ff6b84` | Crimson accent shifted for dark mode legibility |

### External Libraries & CDNs

| Library      | Version | CDN                  |
| ------------ | ------- | -------------------- |
| Font Awesome | 6.5.1   | cdnjs.cloudflare.com |
| Google Fonts | —       | fonts.googleapis.com |

---

## 📁 File Structure

```
project/
├── index.html          ← Main HTML file (semantic, accessible markup)
├── styles.css          ← All CSS (variables, layout, animations, responsive, print)
└── images/
    └── profile.jpg     ← Add your own profile photo here
```

---

## ✅ Assignment Checklist

- [x] Semantic HTML5 elements (`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`)
- [x] All images have `alt` attributes
- [x] All form inputs have associated `<label>` elements
- [x] HTML5 form validation attributes (`required`, `minlength`, `type="email"`)
- [x] Responsive at mobile (480px), tablet (768px), and desktop breakpoints
- [x] CSS Custom Properties
- [x] Flexbox layout
- [x] CSS Grid layout
- [x] Media Queries (3 breakpoints)
- [x] Dark/Light mode toggle with CSS (+3%)
- [x] CSS animations and transitions (+3%)
- [x] CSS-only hamburger menu (+2%)
- [x] Print stylesheet (+2%)
- [x] Minimum 3 project cards
- [x] Contact form with Name, Email, Message fields
- [x] Navigation links to each section
- [x] Font Awesome icons integrated
- [x] External stylesheet (`styles.css`) separate from HTML
