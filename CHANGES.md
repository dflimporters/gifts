# DFL 2026 Gifting Tracker — Notes

Base file: `DFL_2026_Gifting_Tracker.html`. Standalone, unguarded page (same posture as `field-intel.html`): own Supabase client with the anon publishable key, no auth-guard, not linked from nav or `routes.js`.

## Storage — done

Persists to Supabase (`gift_tracker_settings`, `gift_tracker_tiers`, `gift_tracker_items`, `gift_tracker_appreciation_settings`, `gift_tracker_appreciation_requests`), all RLS-enabled with permissive anon policies matching `field_intel`. Field edits debounce (~700ms) before saving; add/remove/toggle actions save immediately. The header pill shows Saving…/All changes saved/Save failed. Export/Import JSON kept as a manual backup/restore option — Import now also pushes the imported state back to Supabase.

## Costing — done

Landed cost (JMD) = China unit cost (USD) × 1.5 × exchange rate. Freight/duty/GCT/other-fee fields and per-item overrides are gone; only China unit cost, qty, and the exchange rate remain editable. The 1.5 markup is a JS constant (`MARKUP`), not stored or user-editable.

## Open / not done

- No role/nav integration — reachable only by direct URL. Promote it later by adding an auth-guard + `DFL_PAGE_ROLES` entry if it needs to move behind login.
- Multi-user editing is last-write-wins, no conflict handling — fine for the current single/small-team usage.
