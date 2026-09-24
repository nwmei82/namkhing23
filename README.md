# 🎁 A Little Birthday Room

An interactive birthday gift website built with Vue 3 + TypeScript + Vite.

## Features

- 🚪 Interactive birthday door
- 🏠 Cozy birthday room
- 🎁 Sliding image puzzle
- 🎟️ Three reward coupons
- 🖼️ Video placeholder
- 🌷 Real flower reveal placeholder
- 💌 Birthday letter
- 📱 Mobile responsive
- ✨ Progress tracking during the current visit

## Run locally

```bash
npm install
npm run dev
```

Then open the local URL shown by Vite.

## Personalize it

### 1. Replace the video

Put your video in:

`public/media/birthday-video.mp4`

Then replace the video placeholder in `src/App.vue` with:

```html
<video controls playsinline class="birthday-video">
  <source src="/media/birthday-video.mp4" type="video/mp4" />
</video>
```

### 2. Replace the flower placeholder

Put the photo in:

`public/media/flowers.jpg`

Then replace the bouquet placeholder with:

```html
<img src="/media/flowers.jpg" alt="Birthday flowers" class="flower-photo" />
```

### 3. Replace the letter

Edit the text inside the `modal === 'letter'` section in `src/App.vue`.

### 4. Make the puzzle use a real photo

The current puzzle is a functional 3x3 sliding puzzle with numbered pieces.
A next step is to turn each piece into a background crop of your photo.

## Suggested next polish

- Add a real illustrated girl and room artwork
- Add subtle object idle animations
- Add a real image puzzle
- Add a music toggle
- Add a final confetti animation
- Deploy to Vercel / Netlify / GitHub Pages
