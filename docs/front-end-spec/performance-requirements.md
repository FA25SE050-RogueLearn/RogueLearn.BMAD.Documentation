# Performance Requirements

## Loading Performance
- Initial page load under 3 seconds
- Subsequent navigation under 1 second
- Progressive loading for large datasets (skill trees, learning progress)
- Optimized images and assets

## Unity WebGL Optimization
- Lazy loading for boss fight components (stream core, defer effects)
- Target 60 FPS; graceful degradation to 30 FPS with reduced visuals
- Memory budget: ≤ 512 MB desktop, ≤ 384 MB mobile; monitor and shed non-critical assets
- Build footprint: core loader ≤ 15 MB compressed; addressable assets streamed on demand
- Texture compression: ASTC on mobile, ETC2/DXT on desktop where supported
- Canvas scaling: respect device pixel ratio; allow dynamic resolution on mobile for battery savings
- Input latency: target ≤ 120 ms end-to-end during co-op; warn at ≥ 200 ms
- Network join: Relay connect and ready-check within 5 seconds under normal conditions
- Fallback experiences for low-performance devices
- Graceful degradation for unsupported browsers

## Mobile Performance
- Touch response under 100ms
- Smooth scrolling and animations
- Efficient battery usage
- Offline capability for core features