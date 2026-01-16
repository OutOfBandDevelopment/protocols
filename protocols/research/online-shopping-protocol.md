# Online Shopping Documentation Protocol

Standard procedure for tracking experiences with online retailers and sellers.

---

## Overview

This protocol ensures consistent documentation of online shopping experiences, particularly for electronics, test equipment, and components. The goal is to maintain a reference of reliable (and unreliable) sellers.

---

## Information to Capture

### For Each Purchase

| Field | Description |
|-------|-------------|
| Seller/Retailer | Name as shown on platform |
| Platform | eBay, Amazon, AliExpress, direct, etc. |
| Store URL | Direct link to seller's store |
| Item Purchased | What was bought |
| Date | When purchased |
| Price | Total including shipping |
| Condition Listed | How item was described (new, used, refurbished) |
| Condition Received | Actual condition on arrival |

### Seller Evaluation

| Criteria | Rating Scale |
|----------|--------------|
| Photo Accuracy | Accurate / Misleading / Deceptive |
| Item Description | Accurate / Incomplete / Misleading |
| Packaging Quality | Excellent / Good / Fair / Poor / Unacceptable |
| Shipping Speed | As expected / Delayed |
| Communication | Responsive / Slow / None needed |
| Would Buy Again | Yes / With caution / No |

### Red Flags to Document

- Deceptive photos (hiding damage, wrong angles)
- Stickers/labels added to hide defects
- Inadequate packaging materials
- Improvised shipping containers
- Item not as described
- Missing accessories
- Signs of prior repair or modification

---

## Default Assumptions

- **Would Buy Again:** Assume "Yes" unless the user explicitly states otherwise
- If seller is added to Blacklist, "Would Buy Again" is automatically "No"

---

## Conversation Triggers

When the user mentions any of the following, update `ShoppingOnline.md`:

- "I bought from..." or "I ordered from..."
- "The seller..." or "The package..."
- Feedback about shipping, packaging, or seller behavior
- Store links or seller names
- Pricing information for purchases
- Recommendations for or against sellers

---

## File Structure

Main file: `ShoppingOnline.md`

```markdown
# Online Shopping - Seller Notes

## eBay Sellers
[Quick reference table]

## Seller Details
[Expanded entries per seller]

## Online Retailers
[Non-eBay sources]

## Blacklist
[Sellers to avoid - with reasons]

## Notes
[General shopping tips]
```

---

## Adding a New Seller Entry

### Quick Table Entry

```markdown
| Seller Name | Rating | Packaging | Notes |
```

Ratings: Excellent / Good / Fair / Poor / Avoid

### Detailed Entry Template

```markdown
### [Seller Name]

- **Platform:** [eBay/Amazon/etc.]
- **Store URL:** [link]
- **Purchases:**
  - [Item] ([Date]) - $[price]
- **Photo Accuracy:** [Accurate/Misleading/Deceptive]
- **Packaging Quality:** [rating] - [details]
- **Item Accuracy:** [Accurate/Misleading] - [details]
- **Red Flags:** [list any issues]
- **Would Buy Again:** [Yes/With caution/No]
- **Notes:** [additional comments]
```

---

## Updating Existing Entries

When new information is received about a known seller:

1. Add new purchases to their purchase history
2. Update ratings if experience differs significantly
3. Add new red flags or positive notes
4. Update "Would Buy Again" recommendation if changed

---

## Blacklist Criteria

Move seller to Blacklist section if:

- Deceptive photos or descriptions
- Intentionally hiding damage
- Repeated poor packaging
- Failure to resolve issues
- Suspected counterfeits

---

*Protocol version: 1.0*
*Last updated: December 2025*
