# de_Couple
 decoupled components for rendering and creation

# Avatar Components

This repository contains two main components: `AvatarRenderer` and `AvatarCreator`. These components are segregated and should each work on their own to allow you to render the rpm avatars.

## AvatarRenderer

The `AvatarRenderer` component handles the displaying and animating of 3D avatars.

### Usage

1. **Import the component:**

```tsx
import React, { useState } from 'react';
import AvatarRenderer, { VisemeData } from './AvatarRenderer';

2. **use in app**

const App: React.FC = () => {
  const [avatarUrl, setAvatarUrl] = useState<string>('https://your-avatar-url.glb');
  const [animationUrl, setAnimationUrl] = useState<string>('/animations/dance/F_Dances_001.glb');
  const [playAnimation, setPlayAnimation] = useState<boolean>(false);
  const [visemeData, setVisemeData] = useState<VisemeData | null>(null);
  const [playViseme, setPlayViseme] = useState<boolean>(false);

  const handlePlayAnimation = () => {
    setPlayAnimation((prev) => !prev);
  };

  const handlePlayViseme = () => {
    fetch('/viseme/Viseme1.json')
      .then((response) => response.json())
      .then((data) => {
        setVisemeData(data);
        setPlayViseme(true);
      });
  };

  return (
    <div style={{ width: '800px', height: '600px' }}>
      <AvatarRenderer
        avatarUrl={avatarUrl}
        animationUrl={animationUrl}
        playAnimation={playAnimation}
        visemeData={visemeData || undefined}
        playViseme={playViseme}
      />
      <button onClick={handlePlayAnimation}>Play Animation</button>
      <button onClick={handlePlayViseme}>Play Viseme</button>
    </div>
  );
};

export default App;
```

Notes:

Replace 'https://your-avatar-url.glb' with avatar's URL or hand it over after saving from the Creator.
"Play Animation" toggles the animation.
"Play Viseme" starts lip sync using the fetched viseme data, make sure that both the audio and the handover are synced to avoid any desync issues.

### AvatarCreator
The AvatarCreator component just links up to the rpm api and displays in an iframe.
Usage

```
const App: React.FC = () => {
  const handleAvatarCreated = (avatarUrl: string) => {
    console.log('Avatar URL:', avatarUrl);
    // Hand over to renderer or just use however you see fit
  };

  return (
    <div style={{ width: '800px', height: '600px' }}>
      <AvatarCreator onAvatarCreated={handleAvatarCreated} />
    </div>
  );
};

export default App;
```
Notes:

The onAvatarCreated callback provides the URL of the new avatar.
Use the avatar URL with AvatarRenderer to display the avatar.