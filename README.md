# Stackline Full Stack Take-Home

## Approach

First I played with the app as if I were an end user to identify any glaring issues in the core flow.
Then I prioritized which issues to address based on initial assumption of priorities (in a real-world env I would clarify priorities / values first with stakeholders), as follows;

### Priority (Descending)

1. **Core functionality** → At a basic level, does the app do its intended job?  
2. **Security** → Of both client and backend; ensure data safety and predictable behavior.  
3. **UX** → Is it usable at a basic level, followed by: is it enjoyable to use?  
   If latency or load spikes, how does it impact users, and can we optimize that experience?  

**Out of immediate scope:**  
- Telemetry  
- Caching, load balancing, pagination, and the like
- Extensibility: e.g. decoupling?

---

## Bugs and Fixes

### Bug 1: Subcategory dropdown displays all categories when category is selected
**Issue:** When a category is selected, the subcategory dropdown shows all the categories still, rather than the subcategories of that selected category.  
**Fix:** In `app/page.tsx`, updated `fetch` call to include selected category, and cleared subcategory state upon change of category.  
**Impact:** subcategory dropdown now works to display only the relevant options as intended, preventing invalid filters and cleaning up UX.

---

### Bug 2: Product detail page exposed JSON in the url
**Issue:** Upon clicking into a product, the page url contained the json’s entire product object in the URL, which exposed internal data to the user.  
**Fix:** Changed `app/page.tsx` and `app/product/page.tsx` to use the deterministic and public-eyes-safe product identifier `sku` for page detail urls rather than product JSON.  
**Impact:** Security (prevent data leakage), Deterministic behavior hardening (`sku` as an identifier changes less than other proxies for identity as dataset evolves).

---

### Bug 3: Trying to Load Images for Certain Products Causes Breaking Page
**Issue:** When trying to load images for certain products who had images hosted on `images-na.ssl-images-amazon.com`, the page would break.  
**Fix:** Added `images-na.ssl-images.amazon.com` as an allowed external host to Next.js config.  
**Impact:** Basic functionality, smoothness of UX.

---

## Future Steps

If I had additional time, I would expand from local bug-fixing into more systems level and AI/ML-powered improvements:
-  Introduce caching layers, rate limiting for production load handling, telemetry etc.
-  AI/ML for semantic search and ranking for cache optimization, etc.