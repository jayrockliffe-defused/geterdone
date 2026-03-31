# Character SVGs for Project Dashboard

This reference file contains pixel art character SVGs used in team member avatars throughout the Geterdone dashboard. Each character includes wave and bob animations for visual personality and team engagement.

## Marketing Professional (Girly)

```svg
<svg width="78" height="102" viewBox="0 0 26 34" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes bob {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-2px); }
      }
      @keyframes wave {
        0% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
        25% { transform: rotateZ(15deg); transform-origin: 70% 70%; }
        50% { transform: rotateZ(-10deg); transform-origin: 70% 70%; }
        75% { transform: rotateZ(10deg); transform-origin: 70% 70%; }
        100% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
      }
      .character-body { animation: bob 2s ease-in-out infinite; }
      .character-arm-right { animation: wave 1.5s ease-in-out infinite; }
    </style>
  </defs>
  <g class="character-body">
    <!-- Head -->
    <circle cx="13" cy="6" r="3" fill="#f4a261"/>
    <!-- Hair -->
    <path d="M 10 4 Q 13 2 16 4" stroke="#d84315" stroke-width="1.5" fill="none" stroke-linecap="round"/>
    <!-- Eyes -->
    <circle cx="12" cy="5" r="0.5" fill="#000"/>
    <circle cx="14" cy="5" r="0.5" fill="#000"/>
    <!-- Smile -->
    <path d="M 12 6 Q 13 6.5 14 6" stroke="#000" stroke-width="0.5" fill="none" stroke-linecap="round"/>
    <!-- Body -->
    <rect x="11" y="9" width="4" height="6" fill="#ff6b6b" rx="0.5"/>
    <!-- Arms -->
    <rect x="8" y="10" width="3" height="1.5" fill="#f4a261" rx="0.5"/>
    <g class="character-arm-right">
      <rect x="15" y="10" width="3" height="1.5" fill="#f4a261" rx="0.5"/>
    </g>
    <!-- Legs -->
    <rect x="11" y="15" width="1" height="3" fill="#333"/>
    <rect x="14" y="15" width="1" height="3" fill="#333"/>
    <!-- Shoes -->
    <rect x="11" y="18" width="1" height="1" fill="#000"/>
    <rect x="14" y="18" width="1" height="1" fill="#000"/>
  </g>
</svg>
```

## Businessman

```svg
<svg width="78" height="102" viewBox="0 0 26 34" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes bob {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-2px); }
      }
      @keyframes wave {
        0% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
        25% { transform: rotateZ(15deg); transform-origin: 70% 70%; }
        50% { transform: rotateZ(-10deg); transform-origin: 70% 70%; }
        75% { transform: rotateZ(10deg); transform-origin: 70% 70%; }
        100% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
      }
      .character-body { animation: bob 2s ease-in-out infinite; }
      .character-arm-right { animation: wave 1.5s ease-in-out infinite; }
    </style>
  </defs>
  <g class="character-body">
    <!-- Head -->
    <circle cx="13" cy="6" r="3" fill="#d4a574"/>
    <!-- Hair -->
    <path d="M 10 3 Q 13 1.5 16 3" stroke="#3d2817" stroke-width="1.5" fill="none" stroke-linecap="round"/>
    <!-- Eyes -->
    <circle cx="12" cy="5" r="0.5" fill="#000"/>
    <circle cx="14" cy="5" r="0.5" fill="#000"/>
    <!-- Mouth -->
    <line x1="12" y1="6.5" x2="14" y2="6.5" stroke="#000" stroke-width="0.5"/>
    <!-- Neck -->
    <line x1="13" y1="9" x2="13" y2="10" stroke="#d4a574" stroke-width="1"/>
    <!-- Suit/Body -->
    <rect x="10" y="10" width="6" height="7" fill="#1a1a1a" rx="1"/>
    <!-- Tie -->
    <rect x="12" y="10" width="0.8" height="4" fill="#e74c3c"/>
    <!-- Arms -->
    <rect x="7" y="11" width="3" height="1.5" fill="#d4a574" rx="0.5"/>
    <g class="character-arm-right">
      <rect x="16" y="11" width="3" height="1.5" fill="#d4a574" rx="0.5"/>
    </g>
    <!-- Legs -->
    <rect x="11" y="17" width="1" height="3" fill="#1a1a1a"/>
    <rect x="14" y="17" width="1" height="3" fill="#1a1a1a"/>
    <!-- Shoes -->
    <rect x="10.5" y="20" width="2" height="1" fill="#000"/>
    <rect x="13.5" y="20" width="2" height="1" fill="#000"/>
  </g>
</svg>
```

## Developer

```svg
<svg width="78" height="102" viewBox="0 0 26 34" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes bob {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-2px); }
      }
      @keyframes wave {
        0% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
        25% { transform: rotateZ(15deg); transform-origin: 70% 70%; }
        50% { transform: rotateZ(-10deg); transform-origin: 70% 70%; }
        75% { transform: rotateZ(10deg); transform-origin: 70% 70%; }
        100% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
      }
      .character-body { animation: bob 2s ease-in-out infinite; }
      .character-arm-right { animation: wave 1.5s ease-in-out infinite; }
    </style>
  </defs>
  <g class="character-body">
    <!-- Head -->
    <circle cx="13" cy="6" r="3" fill="#e8d5c4"/>
    <!-- Hair -->
    <path d="M 10 2 Q 13 0.5 16 2 Q 13 1 13 1 Q 13 1 10 2" stroke="#5d4037" stroke-width="1" fill="#5d4037"/>
    <!-- Eyes (with glasses) -->
    <rect x="11" y="4.5" width="1.2" height="1" fill="none" stroke="#333" stroke-width="0.5" rx="0.3"/>
    <rect x="13.8" y="4.5" width="1.2" height="1" fill="none" stroke="#333" stroke-width="0.5" rx="0.3"/>
    <line x1="12.2" y1="5" x2="13.8" y2="5" stroke="#333" stroke-width="0.3"/>
    <!-- Smile -->
    <path d="M 12 6 Q 13 6.5 14 6" stroke="#000" stroke-width="0.5" fill="none" stroke-linecap="round"/>
    <!-- Body -->
    <rect x="11" y="9" width="4" height="6" fill="#424242" rx="0.5"/>
    <!-- Arms -->
    <rect x="8" y="10" width="3" height="1.5" fill="#e8d5c4" rx="0.5"/>
    <g class="character-arm-right">
      <rect x="15" y="10" width="3" height="1.5" fill="#e8d5c4" rx="0.5"/>
    </g>
    <!-- Legs -->
    <rect x="11" y="15" width="1" height="3" fill="#2c3e50"/>
    <rect x="14" y="15" width="1" height="3" fill="#2c3e50"/>
    <!-- Shoes -->
    <rect x="11" y="18" width="1" height="1" fill="#000"/>
    <rect x="14" y="18" width="1" height="1" fill="#000"/>
  </g>
</svg>
```

## Scholar

```svg
<svg width="78" height="102" viewBox="0 0 26 34" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes bob {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-2px); }
      }
      @keyframes wave {
        0% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
        25% { transform: rotateZ(15deg); transform-origin: 70% 70%; }
        50% { transform: rotateZ(-10deg); transform-origin: 70% 70%; }
        75% { transform: rotateZ(10deg); transform-origin: 70% 70%; }
        100% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
      }
      .character-body { animation: bob 2s ease-in-out infinite; }
      .character-arm-right { animation: wave 1.5s ease-in-out infinite; }
    </style>
  </defs>
  <g class="character-body">
    <!-- Mortarboard (hat) -->
    <rect x="9" y="1" width="8" height="0.8" fill="#1a1a1a"/>
    <polygon points="13,1.8 9.5,3 16.5,3" fill="#1a1a1a"/>
    <!-- Head -->
    <circle cx="13" cy="6" r="3" fill="#f0c674"/>
    <!-- Hair -->
    <path d="M 10.5 4 Q 13 2.5 15.5 4" stroke="#8b4513" stroke-width="1.5" fill="none" stroke-linecap="round"/>
    <!-- Eyes -->
    <circle cx="12" cy="5" r="0.5" fill="#000"/>
    <circle cx="14" cy="5" r="0.5" fill="#000"/>
    <!-- Smile -->
    <path d="M 12 6 Q 13 6.5 14 6" stroke="#000" stroke-width="0.5" fill="none" stroke-linecap="round"/>
    <!-- Robe/Body -->
    <rect x="10" y="9" width="6" height="7" fill="#8b0000" rx="1"/>
    <!-- Arms -->
    <rect x="7" y="11" width="3" height="1.5" fill="#f0c674" rx="0.5"/>
    <g class="character-arm-right">
      <rect x="16" y="11" width="3" height="1.5" fill="#f0c674" rx="0.5"/>
    </g>
    <!-- Legs -->
    <rect x="11" y="16" width="1" height="4" fill="#333"/>
    <rect x="14" y="16" width="1" height="4" fill="#333"/>
    <!-- Shoes -->
    <rect x="11" y="20" width="1" height="0.8" fill="#000"/>
    <rect x="14" y="20" width="1" height="0.8" fill="#000"/>
  </g>
</svg>
```

## Athlete

```svg
<svg width="78" height="102" viewBox="0 0 26 34" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes bob {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-2px); }
      }
      @keyframes wave {
        0% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
        25% { transform: rotateZ(15deg); transform-origin: 70% 70%; }
        50% { transform: rotateZ(-10deg); transform-origin: 70% 70%; }
        75% { transform: rotateZ(10deg); transform-origin: 70% 70%; }
        100% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
      }
      .character-body { animation: bob 2s ease-in-out infinite; }
      .character-arm-right { animation: wave 1.5s ease-in-out infinite; }
    </style>
  </defs>
  <g class="character-body">
    <!-- Headband/Hair -->
    <ellipse cx="13" cy="4" rx="3.5" ry="2.5" fill="#2c3e50"/>
    <!-- Head -->
    <circle cx="13" cy="7" r="3" fill="#d4a373"/>
    <!-- Eyes -->
    <circle cx="12" cy="6" r="0.5" fill="#000"/>
    <circle cx="14" cy="6" r="0.5" fill="#000"/>
    <!-- Determined smile -->
    <line x1="12" y1="7.5" x2="14" y2="7.5" stroke="#000" stroke-width="0.5"/>
    <!-- Athletic Body -->
    <rect x="10.5" y="10" width="5" height="6.5" fill="#ff6b6b" rx="0.5"/>
    <!-- Arms (muscular) -->
    <rect x="7" y="11" width="3.5" height="1.8" fill="#d4a373" rx="0.7"/>
    <g class="character-arm-right">
      <rect x="15.5" y="11" width="3.5" height="1.8" fill="#d4a373" rx="0.7"/>
    </g>
    <!-- Legs -->
    <rect x="11" y="16.5" width="1" height="3.5" fill="#333"/>
    <rect x="14" y="16.5" width="1" height="3.5" fill="#333"/>
    <!-- Shoes/Boots -->
    <ellipse cx="11.5" cy="20.2" rx="0.7" ry="0.5" fill="#000"/>
    <ellipse cx="14.5" cy="20.2" rx="0.7" ry="0.5" fill="#000"/>
  </g>
</svg>
```

## Friendly Person

```svg
<svg width="78" height="102" viewBox="0 0 26 34" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes bob {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-2px); }
      }
      @keyframes wave {
        0% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
        25% { transform: rotateZ(15deg); transform-origin: 70% 70%; }
        50% { transform: rotateZ(-10deg); transform-origin: 70% 70%; }
        75% { transform: rotateZ(10deg); transform-origin: 70% 70%; }
        100% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
      }
      .character-body { animation: bob 2s ease-in-out infinite; }
      .character-arm-right { animation: wave 1.5s ease-in-out infinite; }
    </style>
  </defs>
  <g class="character-body">
    <!-- Head -->
    <circle cx="13" cy="6" r="3.2" fill="#e8b4a8"/>
    <!-- Hair -->
    <path d="M 9.5 4 Q 13 1 16.5 4" stroke="#3d2817" stroke-width="2" fill="none" stroke-linecap="round"/>
    <!-- Rosy cheeks -->
    <circle cx="10" cy="6" r="0.8" fill="#ffb3ba" opacity="0.6"/>
    <circle cx="16" cy="6" r="0.8" fill="#ffb3ba" opacity="0.6"/>
    <!-- Eyes -->
    <circle cx="12" cy="5.5" r="0.5" fill="#000"/>
    <circle cx="14" cy="5.5" r="0.5" fill="#000"/>
    <!-- Big smile -->
    <path d="M 11.5 6.5 Q 13 7.2 14.5 6.5" stroke="#000" stroke-width="0.6" fill="none" stroke-linecap="round"/>
    <!-- Body -->
    <rect x="10.5" y="9.5" width="5" height="6" fill="#66bb6a" rx="0.5"/>
    <!-- Arms -->
    <rect x="7.5" y="11" width="3" height="1.8" fill="#e8b4a8" rx="0.6"/>
    <g class="character-arm-right">
      <rect x="15.5" y="11" width="3" height="1.8" fill="#e8b4a8" rx="0.6"/>
    </g>
    <!-- Legs -->
    <rect x="11.5" y="15.5" width="0.8" height="3.5" fill="#333"/>
    <rect x="13.7" y="15.5" width="0.8" height="3.5" fill="#333"/>
    <!-- Shoes -->
    <rect x="11.3" y="19" width="1.2" height="1" fill="#333" rx="0.2"/>
    <rect x="13.5" y="19" width="1.2" height="1" fill="#333" rx="0.2"/>
  </g>
</svg>
```

## Animation Specifications

### Bob Animation
Creates a gentle up-and-down motion for all characters, making them feel alive and present on the dashboard. The animation repeats every 2 seconds with easing applied for natural motion.

```css
@keyframes bob {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-2px); }
}

.character-body {
  animation: bob 2s ease-in-out infinite;
}
```

### Wave Animation
Applied to the right arm to create a friendly wave gesture. The waving motion occurs when users hover over team member avatars, creating interactive feedback and personality.

```css
@keyframes wave {
  0% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
  25% { transform: rotateZ(15deg); transform-origin: 70% 70%; }
  50% { transform: rotateZ(-10deg); transform-origin: 70% 70%; }
  75% { transform: rotateZ(10deg); transform-origin: 70% 70%; }
  100% { transform: rotateZ(0deg); transform-origin: 70% 70%; }
}

.character-arm-right {
  animation: wave 1.5s ease-in-out infinite;
}
```

## Usage Guidelines

- Each SVG has dimensions of width="78" height="102"
- ViewBox is "0 0 26 34" for consistent proportions
- All characters include both bob and wave animations
- Characters are used in circular avatar containers with background colors
- Select character type based on team member role or preference
- Animations run continuously for engaging visual presence
