# DFL 2026 Gifting Tracker — Notes

Base file: `index.html` (served via GitHub Pages). Standalone, unguarded page (same posture as `field-intel.html`): own Supabase client with the anon publishable key, no auth-guard, not linked from nav or `routes.js`.

## Storage — done

Persists to Supabase (`gift_tracker_settings`, `gift_tracker_tiers`, `gift_tracker_items`, `gift_tracker_areas`, `gift_tracker_recipients`, `gift_tracker_appreciation_settings`, `gift_tracker_appreciation_requests`), all RLS-enabled with permissive anon policies matching `field_intel`.

- Typing never re-renders inputs — only computed cells/totals refresh — so focus and cursor stay put.
- Field edits debounce (~800ms) and flush immediately when the field loses focus; add/remove/toggle save immediately. Row saves are upserts. The header pill shows Saving…/All changes saved/Save failed; leaving the page mid-save prompts.
- Number fields are plain text inputs (no spinner arrows, no scroll-wheel changes); commas/`$` are ignored when parsing.
- "Download backup" exports all data as JSON; "Restore backup" replaces ALL shared data with a file (confirm prompt). Backups without areas/customers leave the current ones untouched.

## Costing — done

Landed cost (JMD) = China unit cost (USD) × 1.5 × exchange rate. The 1.5 markup is a JS constant (`MARKUP`). Cost per gift for a tier = sum of the landed unit cost of each of its items (one of each per recipient).

Tier header stats: Target Units, Assigned to Customers (sum from the area customer lists), Left to Assign (against issue qty when the tier has one, e.g. Gold 500 + 100 extra). For tiers with issue/extra qty, target is auto = issue + extra.

## Customers by Area — done

Mirrors the area tabs of `Customer Gifts December 2025 V2.xlsx` (South West/Trebor, South East/Travis, Hospitality/Phobea, South Central/Marilyn, North West/Rejene, North East/Ronald, Other). Each customer row: name, sales rep (person responsible for Other), Total YTD, qty per tier (columns follow the tiers table), notes. `tier_qty` is jsonb keyed by tier id. Dashboard matrix = area × tier counts + appreciation bags + est. cost, vs each tier's issue qty.

Customer Appreciation requests now have a Sales Area dropdown (`sales_area_id`); the old free-text "Area" column is relabelled Location.

## Open / not done

- No per-area caps (the 2025 Dashboard's 120 appreciation bags per manager / Bronze Allocation sheet).
- No role/nav integration — reachable only by direct URL.
- Multi-user editing is last-write-wins, no conflict handling or live refresh.
