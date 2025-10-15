# Technical Considerations

## Browser Support
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+
- Mobile browsers (iOS Safari, Chrome Mobile)

## Framework Recommendations
- **Frontend:** React with TypeScript
- **Styling:** Tailwind CSS with custom theme
- **State Management:** Redux Toolkit or Zustand
- **Animations:** Framer Motion
- **Unity Integration:** React Unity WebGL

## API Integration
- RESTful API design
- Real-time updates via WebSocket
- Optimistic UI updates
- Error handling and retry logic

## Unity WebGL Bridge & Embedding (Boss Fight 2D)

### Embedding Strategy
- Use React Unity WebGL’s hook-based API (useUnityContext) to instantiate the Unity WebGL build (loaderUrl, dataUrl, frameworkUrl, codeUrl).
- Mount the canvas within a full-screen container. Provide a skeleton/overlay during loading and a fallback message for unsupported browsers.
- Keep the Unity canvas isolated from React reflows; avoid frequent state changes that trigger React re-renders of the canvas container.
- Pass an ephemeral session token and runtime config via JS → Unity after the game signals it’s loaded.

### JavaScript ↔ Unity Bridge Contract
- Outbound (JS → Unity) messages:
  - StartSession: { sessionId, ephemeralToken, relayJoinCode, playerId, partyId?, ruleSet }
  - ReadyUp: { sessionId, playerId }
  - SubmitAnswer: { sessionId, playerId, questionId, optionId, clientTs }
  - SpendCharge: { sessionId, playerId, chargeType }
  - TriggerPowerPlay: { sessionId, playerId, powerPlayType }
  - CompleteSession: { sessionId, playerId }
  - CancelSession: { sessionId, reason }
  - ReportClientMetrics: { fps, memMB, pingMs }
- Inbound (Unity → JS) events:
  - GameLoaded
  - SessionReadyRequested
  - AnswerStationOpen: { questionId, options }
  - AnswerResult: { questionId, correct, serverDecisionTs }
  - BossHealthUpdate: { remainingQuestions }
  - PlayerResolveUpdate: { remainingHearts }
  - PowerPlayWindow: { windowMs }
  - NetworkStatus: { latencyMs, droppedPackets }
  - SessionCompleted: { score, total, accuracy }
  - SessionCancelled: { reason }

### Initialization Handshake
1) Frontend requests a co-op Boss Fight session from backend (POST /game/bossfight/sessions) and receives { sessionId, ephemeralToken, relayJoinCode, ruleSet }.
2) React mounts Unity and waits for Unity’s GameLoaded event.
3) JS sends StartSession with the ephemeralToken and relayJoinCode; Unity connects via NGO + Relay.
4) Unity emits SessionReadyRequested per player; JS responds with ReadyUp after local readiness checks (e.g., audio, input).
5) Normal gameplay flow begins (answer stations, charges, power plays, completion).

### Networking
- Unity Netcode for GameObjects (NGO) + Unity Relay handles transport; the frontend only brokers tokens and join codes and displays status.
- Display latency indicators from NetworkStatus and throttle UI updates to minimize React overhead.

### Security & Sandbox
- Treat all Unity → JS payloads as untrusted; validate shapes and types before acting.
- Consider COOP/COEP headers if using advanced threading/SharedArrayBuffer in modern WebGL builds.
- Use short-lived ephemeral tokens for Boss Fight sessions; never expose long-lived credentials to the Unity client.

### Error, Loading, Fallback
- Provide distinct states: initializing, loading, reconnecting, degraded mode, unsupported.
- Offer a “Return to Arena Lobby” and “Try Again” action when CancelSession or critical errors occur.
- Log bridge events and errors to a client telemetry endpoint for diagnostics.

### Example (React Unity WebGL)

```tsx
import { Unity, useUnityContext } from "react-unity-webgl";

export function BossFightCanvas({ session }) {
  const { unityProvider, sendMessage, addEventListener, removeEventListener, isLoaded } = useUnityContext({
    loaderUrl: "/Build/bossfight.loader.js",
    dataUrl: "/Build/bossfight.data.br",
    frameworkUrl: "/Build/bossfight.framework.js.br",
    codeUrl: "/Build/bossfight.wasm.br",
  });

  React.useEffect(() => {
    function onGameLoaded() {
      sendMessage("GameManager", "StartSession", JSON.stringify({
        sessionId: session.sessionId,
        ephemeralToken: session.ephemeralToken,
        relayJoinCode: session.relayJoinCode,
        playerId: session.playerId,
        ruleSet: session.ruleSet,
      }));
    }

    addEventListener("GameLoaded", onGameLoaded);
    return () => removeEventListener("GameLoaded", onGameLoaded);
  }, [sendMessage, addEventListener, removeEventListener, session]);

  return (
    <div className="unity-container">
      {!isLoaded && <div className="loading-overlay">Loading Boss Fight…</div>}
      <Unity unityProvider={unityProvider} style={{ width: "100%", height: "100%" }} />
    </div>
  );
}
```