# Affensus

Extract structured coupons with **CouponBrain**, Affensus's specialized language model for promotion text.

- Website: https://affensus.com/
- Docs: https://affensus.com/docs

## How it works

1. Create an account and generate an API key (shown once).
2. Send promotion text to `POST https://affensus.com/api/extract` with `Authorization: Bearer aff_…`.
3. CouponBrain returns grounded coupon JSON: codes, discounts/tiers, scope flags, campaign events/themes, and an end date.
4. One successful extract uses one credit. Unused credits expire 12 months after purchase.

### Request fields

| Field | Required | Description |
| --- | --- | --- |
| `text` | yes | Promotion source text (max 20,000 characters) |
| `as_of` | no | `YYYY-MM-DD` temporal context for countdowns (not used as coupon evidence) |
| `model` | no | `latest` or `couponbrain-mini-1.10` (empty = latest) |
| `format` | no | `json` or `toon` (empty = json) |

### Example

```bash
curl https://affensus.com/api/extract \
  -H "Authorization: Bearer aff_…" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Use code SAVE20 for 20% off your order. New customers only. Ends in 3 days.",
    "as_of": "2026-09-27",
    "model": "latest",
    "format": "json"
  }'
```

### What you get back

A successful response includes `result.coupons` (each with `code`, `type`, `currency`, `scope`, `tiers`), plus `end_date`, `campaign_events`, `campaign_themes`, `credits_used`, and `credits_remaining`.

Coupon types include `percentage`, `fixed`, `free_shipping`, `gift`, `other`, and `unknown`. Scope flags appear only when true (for example `sitewide`, `new_customers_only`, `app_only`).

## Links

- [affensus.com](https://affensus.com/)
- [Documentation](https://affensus.com/docs)
