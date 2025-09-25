# Browser Extension Integration & Content Capture Flow

```mermaid
sequenceDiagram
    participant U as Student User
    participant Browser as Web Browser
    participant Extension as Browser Extension
    participant APIGateway as API Gateway (Azure)
    participant QuestsService as Quests Service
    participant AIProxyService as AI Proxy Service
    participant Database as Database (Supabase)

    alt Content Capture Flow
        U->>Browser: Finds an interesting academic article
        U->>Extension: Clicks "Save to Arsenal" button
        Extension->>Browser: Extracts page content (URL, text, title)
        Extension->>APIGateway: POST /arsenal/capture (content)
        APIGateway->>QuestsService: Forwards capture request
        
        %% Asynchronous processing starts here %%
        QuestsService-->>APIGateway: 202 Accepted (acknowledges the request)
        APIGateway-->>Extension: 202 Accepted
        Extension-->>U: Shows "Processing content..." message
        
        activate QuestsService
        Note right of QuestsService: AI analysis happens in the background.
        QuestsService->>AIProxyService: RequestContentAnalysis(content)
        AIProxyService-->>QuestsService: Returns structured data (summary, tags, key concepts)
        
        QuestsService->>Database: CREATE Note record in user's Arsenal
        QuestsService->>Database: Link Note to relevant Skill nodes
        deactivate QuestsService
    end

    alt Contextual Assistance Flow
        U->>Browser: Highlights text while reading on any website
        Extension->>Browser: Captures highlighted text
        Extension->>APIGateway: GET /arsenal/context-search?q={highlighted_text}
        APIGateway->>QuestsService: Forwards search request
        
        activate QuestsService
        QuestsService->>Database: Search user's Notes for relevant content
        Database-->>QuestsService: Returns matching notes
        QuestsService-->>APIGateway: Returns list of relevant notes
        deactivate QuestsService
        
        APIGateway-->>Extension: Returns search results
        Extension->>U: Displays a popup with links to relevant notes in the Arsenal
    end
    
    Extension->>U: Notify of new organized content
    
    Note over U,DB: Seamless academic content<br/>capture and organization system
```