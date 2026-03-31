# Device Preview 

A reusable Vue 3 component library for displaying images and videos in mobile and desktop mockups with smooth scrolling animations.

## Features

- 📱 **Mobile & Desktop Mockups** - iPhone and laptop device frames
- 🎬 **Video Support** - Autoplay, muted, looping videos
- 📜 **Smooth Scrolling** - Animated image scrolling with easing
- 🎯 **DRY Architecture** - Single `MediaContainer` with swappable styling wrappers
- ⚡ **Built with Vue 3 + Vite** - Fast development and production builds
- 🎨 **Tailwind CSS** - Utility-first styling

## Installation

### Prerequisites
- Node.js 16+
- pnpm

### Setup

```bash
# Install dependencies
pnpm install

# Start dev server
pnpm dev

# Build for production
pnpm build

# Preview production build
pnpm preview
```

## Project Structure

```
src/
├── components/
│   ├── MediaContainer.vue          # Core component (logic & animation)
│   ├── wrappers/
│   │   ├── MobileWrapper.vue       # iPhone styling
│   │   └── DesktopWrapper.vue      # Laptop styling
│   └── MediaGallery.vue            # Example parent component
└── main.js
```
