# League of Legends Gallery

A web development activity that showcases League of Legends champions through an interactive gallery with detailed champion information and lore.

## About

This project was created as part of a Web Development course to demonstrate modern web design principles, responsive layouts, and dynamic data fetching. The site features an interactive champion gallery where users can browse, search, and learn about all 170+ League of Legends champions.

## Features

- Responsive design that works across all devices
- Interactive champion gallery with 20 champions per page
- Pagination with previous/next navigation
- Search functionality to filter champions by name
- Desktop hover cards displaying champion details and full lore
- Mobile-friendly modal for viewing champion information
- Real-time lore fetching from the League of Legends API
- Smooth animations and LoL-themed styling
- Champion role icons and detailed champion descriptions

## Technologies Used

- HTML5
- CSS3
- Bootstrap 4.6.2
- JavaScript (ES6+)
- League of Legends Data Dragon API
- Google Fonts (Outfit)

## Installation

To run this project locally:

1. Clone the repository
2. Open `index.html` in your web browser
3. No build process or dependencies required (uses CDN for Bootstrap and fonts)

## Project Structure

```
Activity 7/
├── index.html
├── styles.css
├── fonts.css
├── Assets/
│   ├── class/
│   ├── favicon/
│   ├── fonts/
│   └── img/
└── README.md
```

## Features in Detail

### Champion Gallery
- Browse all 170+ champions with high-quality loading images
- View champion name, title, and role at a glance
- Lazy loading for optimized performance

### Search & Filter
- Real-time search as you type
- Filter champions by name
- Results counter showing matched champions

### Champion Details

**Desktop (Hover):**
- Hover over any champion card to see a popover
- Displays champion name, role with icon, and full lore
- Smart positioning to prevent overflow off-screen

**Mobile/Tablet (Click):**
- Click on a champion card to open a centered modal
- Same information displayed in an optimized layout
- Easy close functionality

### Pagination
- Browse champions 20 at a time
- Navigate between pages with previous/next buttons
- Buttons disable when at first or last page
- Current page indicator

## API Reference

This project uses the **League of Legends Data Dragon API**:

### Data Endpoints
- **Champion List**: `https://ddragon.leagueoflegends.com/cdn/{version}/data/en_US/champion.json`
  - Returns all champions with basic info (name, title, tags, blurb, etc.)
  - Used for: Gallery display and search functionality

- **Champion Details**: `https://ddragon.leagueoflegends.com/cdn/{version}/data/en_US/champion/{championId}.json`
  - Returns full champion data including complete lore
  - Used for: Populating hover cards and modals with extended lore text

### Image Endpoints
- **Champion Loading Screen**: `https://ddragon.leagueoflegends.com/cdn/img/champion/loading/{championId}_0.jpg`
  - High-quality champion loading screen image
  - Dimensions: 1215x717px
  - Used for: Main card display in gallery

- **Champion Splash Art**: `https://ddragon.leagueoflegends.com/cdn/img/champion/splash/{championId}_0.jpg`
  - Full resolution splash art
  - Used for: (Future enhancement)

- **Champion Tiles**: `https://ddragon.leagueoflegends.com/cdn/img/champion/{championId}.png`
  - Square thumbnail format
  - Used for: (Future enhancement)

- **Role Icons**: `Assets/class/icon-{role}.png`
  - Local SVG/PNG icons for champion roles
  - Roles: Assassin, Fighter, Mage, Marksman, Support, Tank

### Current Version
- **Data Dragon Version**: 15.24.1
- Updates automatically with each League patch

## Developer Documentation

### Key Functions

#### `getChampions()`
Fetches all champions from the Data Dragon API and initializes the gallery.
- **Called on**: Page load
- **Returns**: Populates `allChampions` array

#### `showPage(pageNumber)`
Renders champions for the specified page.
- **Parameters**: `pageNumber` (1-indexed)
- **Generates**: HTML cards with champion data attributes
- **Calls**: `attachHoverEvents()` after rendering

#### `attachHoverEvents()`
Attaches event listeners to champion cards for hover/click interactions.
- **Desktop**: Mouseenter → show hover card, Mousemove → track position, Mouseleave → hide
- **Mobile**: Click → show modal
- **Features**: Viewport boundary detection to prevent off-screen overflow

#### `showChampionModal(card)`
Displays champion details in a Bootstrap modal (mobile/tablet).
- **Parameters**: Champion card element
- **Features**: Fetches full lore asynchronously, displays in centered modal

#### `getFullLore(id)`
Fetches complete champion lore from the individual champion data endpoint.
- **Caching**: Results stored in `loreCache` for performance
- **Returns**: Full lore text or empty string if unavailable

#### `setupSearch()`
Implements real-time search functionality.
- **Triggers**: On button click or keyup
- **Filters**: Champions by name (case-insensitive)
- **Updates**: Gallery with search results and reattaches hover events

#### `setupPaginationButtons()`
Handles previous/next page navigation.
- **Features**: Disables buttons at first/last page
- **UX**: Smooth scroll to search bar on page change

#### `updateButtonStates()`
Updates pagination button opacity and cursor state based on current page.

### Data Structures

#### `allChampions` Array
```javascript
[
  {
    id: "Akshan",
    name: "Akshan",
    title: "The Rogue Sentinel",
    tags: ["Marksman"],
    blurb: "Short description...",
    // ... other properties from API
  },
  // ... more champions
]
```

#### `loreCache` Object
```javascript
{
  "Akshan": "Full lore text...",
  "Ahri": "Full lore text...",
  // ... cached lore by champion ID
}
```

#### `roleIcons` Object
```javascript
{
  "Assassin": "Assets/class/icon-assassin.png",
  "Fighter": "Assets/class/icon-fighter.png",
  "Mage": "Assets/class/icon-mage.png",
  "Marksman": "Assets/class/icon-marksman.png",
  "Support": "Assets/class/icon-support.png",
  "Tank": "Assets/class/icon-tank.png"
}
```

### CSS Classes

#### Hover Card
- `.#hoverCard` - Main popover container
- `.hover-header` - Header with icon and name
- `.hover-role-icon` - Role icon image
- `.hover-champion-name` - Champion name styling
- `.hover-role-text` - Role text styling
- `.hover-champion-title` - Title (italic)
- `.hover-champion-blurb` - Lore text

#### Modal
- `.modal-content` - Modal container styling
- `.champion-modal-header` - Modal header flex layout
- `.champion-modal-icon` - Modal icon sizing
- `.champion-modal-name` - Modal champion name
- `.champion-modal-role` - Modal role text
- `.champion-modal-title` - Modal title styling
- `.champion-modal-lore-header` - "Champion Lore" heading
- `.champion-modal-lore` - Modal lore text

#### Cards
- `.client-card` - Champion card container
- `.champion-name` - Card champion name
- `.champion-title` - Card title text
- `.overlay` - Hover overlay effect

### Viewport Boundary Detection

Located in `attachHoverEvents()` mousemove listener (lines 328-360):

1. Gets hover card dimensions and viewport size
2. Calculates initial position relative to cursor (+15px offset)
3. **Right boundary check**: If card goes off-screen right, flip to left side
4. **Bottom boundary check**: If card goes off-screen bottom, flip to top
5. **Left boundary check**: Prevents going off left edge
6. **Top boundary check**: Prevents going off top edge
7. Applies adjusted position to maintain full visibility

### Responsive Breakpoints

- **Desktop** (>991px): Hover cards on mouseover
- **Mobile/Tablet** (≤991px): Modal on click

Breakpoint matches Bootstrap's `.lg` breakpoint for consistency.

### Performance Optimizations

- **Lazy Loading**: Images load on demand with `loading="lazy"`
- **Lore Caching**: Full lore fetched once and cached to prevent redundant API calls
- **Efficient Pagination**: Only renders 20 champions per page
- **Search Filtering**: Client-side filtering (no API calls needed)

### Future Enhancements

- Add champion skin gallery
- Implement champion abilities display
- Add ability videos
- Create champion comparison tool
- Add filters by role, tier, or playstyle
- Implement champion build recommendations

## Credits

Created by James Carlo for Web Development coursework.

Powered by [Riot Games](https://www.riotgames.com/) League of Legends Data Dragon API.

### Documentation & References

This project was developed following the official Riot Games Developer documentation:
- [Data Dragon Documentation](https://developer.riotgames.com/docs/lol#data-dragon)
- [Riot Games Developer Portal](https://developer.riotgames.com/)

---

*This website is a student project created for educational purposes only and is not affiliated with Riot Games.*
