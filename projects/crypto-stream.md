# CryptoStream — Real-Time Cryptocurrency Price Streaming Platform

## 1. Project Motivation
Real-time market data streaming is non-trivial because public APIs often add latency, apply rate limits, or abstract away the live price transitions visible in trading UIs. For applications that need immediate price changes, those abstractions can be too coarse or delayed. This project explores browser-driven real-time data by reading live prices directly from TradingView’s BINANCE exchange pages, which exposes the same tick updates a human sees.

## 2. High-Level Architecture
The system separates responsibilities between a Node.js + TypeScript backend and a Next.js + TypeScript frontend. The backend owns data acquisition and stream fan-out; the frontend owns ticker selection and rendering. Updates flow through ConnectRPC as a push-based, event-driven stream rather than periodic polling. The result is a clear split: the backend emits authoritative price events, and the frontend subscribes and renders them.

## 3. Data Streaming Design
Price changes are detected by observing TradingView’s live price elements in a headed Playwright browser session. DOM observation is preferred over interval-based polling to avoid missing rapid updates and to reduce unnecessary work when prices are stable. A single browser instance and context are reused, but each active ticker gets its own Playwright page keyed by symbol. When a ticker is added, the backend creates (or reuses) the page, navigates to the TradingView BINANCE view, attaches observers to the relevant DOM nodes, and starts emitting updates. Multiple clients share the same page and stream for that ticker. When the last subscriber disconnects, observers are detached and the page is explicitly closed to avoid orphaned pages and memory leaks.

## 4. Backend Responsibilities
- Playwright lifecycle management with one shared browser instance, a single reused context, and one page per active ticker
- TradingView navigation and DOM extraction for BINANCE pages, including selector stability checks before streaming
- Price normalization into a consistent numeric representation before streaming
- Streaming updates to clients via ConnectRPC push channels
- Logging and observability for stream starts, stops, and extraction errors

## 5. Frontend Responsibilities
- Ticker management (add/remove) with validation feedback to users
- Alphabetical sorting for a stable, readable list
- Real-time rendering with minimal re-renders by updating only affected tickers
- Stream lifecycle visibility so users can see which tickers are active or disconnected

## 6. Error Handling & Edge Cases
- Invalid tickers are detected early and surfaced without starting a browser session
- Page load failures trigger controlled retries and emit a stream error state
- Network disconnects are handled by reconnecting the client stream and resubscribing tickers
- Safe cleanup ensures browser pages and observers are released when no subscribers remain
- DOM or layout changes can break selectors, so extraction is guarded and failures are surfaced rather than silently drifting

## 7. Scalability Considerations
- Browser reuse is prioritized to avoid per-client duplication and to cap resource usage
- One-page-per-ticker favors isolation and failure containment over a shared multi-ticker page, at the cost of higher page count under load
- Playwright in headed mode trades ease of debugging for higher CPU/memory cost, so concurrency limits are enforced
- Push-based streaming keeps server load proportional to actual updates, not polling frequency
- The ticker abstraction is designed so additional exchanges or data sources can be added behind the same interface
- Clean boundaries between acquisition, streaming, and UI make future architectural changes feasible

## 8. Key Learnings
- Real-time streaming requires explicit ownership of end-to-end latency and event ordering
- Browser automation provides access to live data but adds operational complexity and resource constraints
- DOM observation is more reliable than polling for UI-driven updates when selectors are stable
- Push vs polling materially changes system behavior under load and during reconnects
- Consistent shared state across clients reduces UI drift and makes the system easier to reason about

## 9. Future Improvements
- Historical price snapshots alongside live streams for short-term context
- Authentication and per-user subscriptions to control access and personalization
- Multi-exchange support using the same streaming and fan-out pipeline
- Resilience improvements with retries and health checks around browser sessions
- A migration path to API-based feeds when reliable low-latency sources are available