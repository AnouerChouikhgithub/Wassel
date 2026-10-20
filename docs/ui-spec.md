# UI specification (source of truth for all screens)

INSTRUCTIONS FOR THE CODING AGENT
- Implement the screens below as real, working code in `frontend/`. Wording such as "generate" or "visual reference" means: build them in the codebase.
- Work in the batches the developer asks for. Never skip, merge or summarize screens inside a requested batch.
- Every screen must support light AND dark mode through the central theme tokens. No hard-coded colors in components.
- Use typed mock data in `src/mocks/` for now. No backend calls until the developer says so.
- Reuse components. If a pattern appears twice, extract a component.
- Before the first batch, build the foundation: theme tokens, theme provider with Light / Dark / System, base components, navigation skeleton with a placeholder for every screen.
- Ask before adding a dependency that is not already listed in AGENTS.md. If a listed library is incompatible with the installed framework version, tell the developer and propose the closest alternative.
NOTE FOR WASSEL: build ONE Expo app with two Expo Router route groups, `app/(customer)` and `app/(courier)`. The role is chosen at sign-in (mocked for now). Shared theme and components live outside the route groups.

---

## Part 1: Shared design foundation

You are a senior product designer and front-end engineer. Design a professional, polished, production-grade UI. It must look like a funded startup's app, not a template.

THEMING (critical)
- Every screen must exist in BOTH light and dark mode. Default = follow system, plus a manual Light / Dark / System switch in Settings.
- Dark mode is NOT an inversion: use layered surfaces (background < surface < elevated), a slightly lighter and desaturated primary, subtle 1px borders instead of shadows, and softer white text (never pure #FFF on pure #000).
- All colors come from semantic tokens in ONE theme file (no hard-coded hex in components).
- Neutral tokens. Light: background #F8FAFC, surface #FFFFFF, border #E2E8F0, text #0F172A, text-secondary #475569. Dark: background #0B0F14, surface #121821, elevated #1A2230, border #263041, text #E6EAF0, text-secondary #9AA6B8.
- Status tokens. Light: success #16A34A, warning #D97706, danger #DC2626, info #2563EB. Dark: success #4ADE80, warning #FBBF24, danger #F87171, info #60A5FA.
- The project's primary color is given in the project prompt (light value and dark value). Use it sparingly: primary actions, active states, key highlights. Do not flood the UI with it.

DESIGN SYSTEM
- Font: Inter (system font fallback). Scale: display 28/34 semibold, title 20/28 semibold, body 16/24 regular, caption 13/18 medium. Tabular numbers for prices, times and metrics.
- 4pt spacing grid. Corner radius: 12 cards and inputs, 16 sheets and modals, full for chips and avatars.
- Minimum touch target 44pt (primary buttons 52pt tall). Respect safe areas.
- Icons: one consistent single-stroke set (Lucide), 1.75px stroke. No emoji as icons.
- Components to build once and reuse: buttons (primary, secondary, ghost, destructive, with loading state), text inputs (label above, helper text, error state, disabled), cards, list rows, chips and badges, avatars, segmented control, switch, bottom sheet or modal, toast, skeleton loaders.
- Every list and data screen needs designed states: loading (skeletons), empty (simple line icon + one-line explanation + primary CTA), error (message + Retry), and success feedback.
- Motion: 150-250ms ease-out transitions, subtle press feedback, haptics on key actions (mobile). Respect reduced-motion settings.
- Accessibility: WCAG AA contrast (4.5:1 text) in BOTH modes, visible focus states, labels for every control, dynamic text sizing must not break layouts.

CONTENT
- Use realistic sample data set in Tunisia: names like Amine Ben Salah, Ons Trabelsi, Yassine Jebali; cities Tunis, Sfax, Sousse, Bizerte, La Marsa; currency TND (e.g. 48,500 TND with 3 decimals where appropriate); French-style addresses. NO lorem ipsum, no gray placeholder boxes.
- Keep copy short, human, and action-oriented.

CODE RULES
- TypeScript, strict. Clean folder structure: theme/, components/, screens/, mocks/ (all sample data lives in mocks/). No backend calls yet: typed mock data only.
- Reusable components, no copy-pasted JSX. Keep the theme tokens centralized so the primary color can be changed in one place.

DO NOT: heavy gradients, drop shadows in dark mode, stock-photo clutter, more than one accent color, tiny text, crowded screens.

---

## Customer role spec

PROJECT: Wassel Customer app. Live delivery tracking: users order a courier to pick up and deliver a parcel and watch the courier move on a map in real time. "Wassel" means "deliver" in Arabic. Tone: fast, clear, trustworthy, map-first.

PRIMARY COLOR: light #2563EB (white text on it), dark #60A5FA (near-black text on it). Logo idea: a location pin whose tail stretches into two speed lines forming a stylized W.

SIGNATURE FEATURE: smooth live tracking. The courier marker moves continuously, the route line updates, and the ETA changes in real time with a subtle animated number.

NAVIGATION: bottom tabs: Home, Orders, Profile. Tracking opens full-screen over the tabs.

SCREENS (each in light AND dark):
1. Onboarding (3 slides, minimal) + phone number sign-in with OTP entry (6 boxes, resend timer).
2. Home: full-screen map, top greeting chip, bottom sheet with "Where to deliver?" search field, saved places (Home, Work), recent addresses, and an active-order mini card when a delivery is in progress.
3. New delivery: pickup and drop-off fields with swap button, package size selector (Envelope, Small, Medium, Large with icons), notes, delivery options (Standard, Express) with price and time estimate in TND, price breakdown, primary button "Request courier".
4. Finding courier: map with pulsing radar around the pickup point, status text, cancel button.
5. Live tracking (the hero screen): full-screen map with route and moving courier marker, draggable bottom sheet showing ETA large (e.g. "12 min"), courier card (photo, name, rating, vehicle and plate, call and chat buttons), delivery status timeline (Courier assigned, Picked up, On the way, Arriving), share-tracking-link button, order code for handover.
6. Delivered: success state, summary, photo proof, 1-5 star rating, tip chips (1 / 2 / 3 TND), written feedback, done button.
7. Orders: segmented control Active / History, order cards (route summary, date, status, price) with empty state.
8. Order detail / receipt: route map snapshot, timeline with times, price breakdown, payment method, "Get help" and "Reorder".
9. Profile: appearance (Light / Dark / System), payment methods (card, cash), saved addresses, language (French, Arabic with RTL support, English), notifications, support, log out.

Use react-native-maps with a custom light and dark map style. Mock the courier movement with a looping animation along a polyline.

DELIVERY: Generate ALL 9 screens listed above, each in light and dark mode, as ONE connected, navigable app (working tab bar, working navigation between screens, working theme switch). Do not skip, merge or summarize any screen. These screens are the visual reference for the implementation, so be complete and consistent.

---

## Courier role spec

PROJECT: Wassel Courier app. The courier-side companion of Wassel: couriers go online, receive delivery requests, navigate step by step, prove delivery and track earnings. Tone: efficient, glanceable, one-handed use on a scooter or in a car. Large text, big buttons.

PRIMARY COLOR: light #2563EB (white text on it), dark #60A5FA (near-black text on it). Same logo as the customer app, with a small "Courier" label.

NAVIGATION: bottom tabs: Drive, Earnings, History, Profile.

SCREENS (each in light AND dark):
1. Sign-in + verification status screen (documents: ID, licence, vehicle, each with Approved / Pending / Rejected badge).
2. Drive (home): full-screen map, large Go Online / Go Offline toggle button at the bottom, today's earnings and trips chip at the top, heat-zone overlay showing busy areas.
3. Incoming request: modal card over the map with a 15-second circular countdown, pickup and drop-off addresses, distance, estimated earnings in TND (large), Accept (primary) and Decline (ghost) buttons, with strong haptic feedback.
4. Active delivery: step-based flow with a sticky top step indicator (Go to pickup, Picked up, Deliver). Each step has a map with route, address, customer contact buttons (call, message), a big "Arrived" / "Picked up" slide-to-confirm control.
5. Proof of delivery: take photo, or enter the customer's 4-digit code, then confirm. Success animation and earnings added.
6. Earnings: week selector, large total, bar chart by day, payout status card, breakdown (base, distance, tips), "Request payout" button.
7. History: list of trips with date, route summary, earnings; detail view.
8. Profile and Settings: vehicle, documents, appearance (Light / Dark / System), navigation app preference, sound and haptics, language, log out.

Use react-native-maps with custom light and dark map styles.

DELIVERY: Generate ALL 8 screens listed above, each in light and dark mode, as ONE connected, navigable app (working tab bar, working navigation between screens, working theme switch). Do not skip, merge or summarize any screen. These screens are the visual reference for the implementation, so be complete and consistent.
