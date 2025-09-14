# **Frontend Architecture**

### **Component Architecture**

```plaintext
roguelearn-web/
└── src/
    ├── app/
    ├── components/
    │   ├── ui/
    │   ├── icons/
    │   ├── layout/
    │   ├── features/
    │   │   ├── quests/
    │   │   ├── skill-tree/
    │   │   └── boss-fight/
    │   │       └── UnityGameEmbed.tsx
    │   └── auth/
    ├── lib/
    └── hooks/
```

#### **Component Template**
```typescript
// Example: src/components/features/quests/QuestItem.tsx
import * as React from 'react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Quest } from '@roguelearn/shared-types';

interface QuestItemProps {
  quest: Quest;
}

const QuestItem: React.FC<QuestItemProps> = ({ quest }) => {
  return (
    <Card>
      <CardHeader>
        <CardTitle>{quest.title}</CardTitle>
      </CardHeader>
      <CardContent>
        <p>{quest.description}</p>
      </CardContent>
    </Card>
  );
};

export default QuestItem;
```

### **State Management Architecture**

*   **Server State:** **React Query** (`@tanstack/react-query`) will be the default for all data fetching and caching.
*   **Client State:** **Zustand** will be used for any necessary global client-side state.

### **Routing Architecture**

*   **Library:** **Next.js App Router** will be used for all routing.
*   **Protected Routes:** A `src/middleware.ts` file will be used to protect routes, integrating with the **Supabase Auth Helpers SDK**.

### **Unity WebGL Integration**

*   **Strategy:** The Unity game builds will be hosted on Supabase Storage. The Next.js application will embed the game within a dedicated React component.
*   **Communication:** A JavaScript bridge will be used. The React component will send initialization data (like a JWT) to the Unity instance. The Unity game will use `Application.ExternalCall` to send messages back to the React component (e.g., to report the final score), which then calls the backend API.

```typescript
// src/components/features/boss-fight/UnityGameEmbed.tsx
import React, { useEffect } from 'react';
import { Unity, useUnityContext } from 'react-unity-webgl';

interface UnityGameEmbedProps {
  gameBuildUrl: string;
  onGameComplete: (score: number) => void;
}

const UnityGameEmbed: React.FC<UnityGameEmbedProps> = ({ gameBuildUrl, onGameComplete }) => {
  const { unityProvider, addEventListener, removeEventListener } = useUnityContext({
    loaderUrl: `${gameBuildUrl}/Build.loader.js`,
    dataUrl: `${gameBuildUrl}/Build.data`,
    frameworkUrl: `${gameBuildUrl}/Build.framework.js`,
    codeUrl: `${gameBuildUrl}/Build.wasm`,
  });

  useEffect(() => {
    const handleGameOver = (score: number) => {
      onGameComplete(score);
    };
    addEventListener('GameOver', handleGameOver);
    return () => {
      removeEventListener('GameOver', handleGameOver);
    };
  }, [addEventListener, removeEventListener, onGameComplete]);

  return <Unity unityProvider={unityProvider} style={{ width: '100%', height: '100%' }} />;
};

export default UnityGameEmbed;
```

### **Frontend Services Layer**

*   **API Client:** An `axios` instance will be configured with interceptors to automatically attach the JWT Bearer token from **Supabase** to all outgoing requests.

```typescript
// src/lib/api-client.ts
import axios from 'axios';
import { createClient } from '@/utils/supabase/client';

const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
});

apiClient.interceptors.request.use(async (config) => {
  const supabase = createClient();
  const { data: { session } } = await supabase.auth.getSession();
  
  if (session?.access_token) {
    config.headers.Authorization = `Bearer ${session.access_token}`;
  }
  return config;
});

export default apiClient;
```
