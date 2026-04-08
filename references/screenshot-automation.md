# Screenshot Automation Reference

## Playwright Workflow Template

This template works for any web application. Adapt selectors and URLs to your product.

### Basic Setup

```javascript
async (page) => {
  // 1. Set viewport
  await page.setViewportSize({ width: 1440, height: 900 });

  // 2. Login if needed (adapt to your auth flow)
  // await page.goto('https://your-app.com/login');
  // await page.fill('#email', 'demo@example.com');
  // ...

  // 3. Define pages to screenshot
  const PAGES = [
    { url: '/', name: 'dashboard', fullPage: false },
    { url: '/settings', name: 'settings', fullPage: true },
    // Add your pages here
  ];

  // 4. Define navigation hide CSS (adapt selector to your app)
  const HIDE_NAV_CSS = `
    /* Adapt these selectors to your product */
    nav, [role="navigation"], .sidebar, .side-nav {
      display: none !important;
    }
    /* Adjust main content to fill space */
    main, .main-content, [role="main"] {
      margin-left: 0 !important;
      width: 100% !important;
    }
  `;

  // 5. Optional: anonymization function
  const anonymize = () => {
    const MAP = {
      // 'Real Name': 'Fake Name',
      // 'real@email.com': 'fake@demo.com',
    };
    const keys = Object.keys(MAP).sort((a, b) => b.length - a.length);
    const walker = document.createTreeWalker(
      document.body, NodeFilter.SHOW_TEXT
    );
    let node;
    while (node = walker.nextNode()) {
      let t = node.textContent, changed = false;
      for (const k of keys) {
        if (t.includes(k)) {
          t = t.split(k).join(MAP[k]);
          changed = true;
        }
      }
      if (changed) node.textContent = t;
    }
    // Also handle input values
    document.querySelectorAll('input, textarea').forEach(el => {
      for (const k of keys) {
        if (el.value?.includes(k)) {
          el.value = el.value.split(k).join(MAP[k]);
        }
      }
    });
    return 'done';
  };

  // 6. Screenshot loop
  for (const p of PAGES) {
    await page.goto(`https://your-app.com${p.url}`, {
      waitUntil: 'networkidle'
    });
    await page.waitForTimeout(2000);

    // Hide navigation
    await page.addStyleTag({ content: HIDE_NAV_CSS });
    await page.waitForTimeout(300);

    // Anonymize (if needed)
    await page.evaluate(anonymize);
    await page.waitForTimeout(500);

    // Screenshot
    await page.screenshot({
      path: `screenshots/${p.name}.png`,
      type: 'png',
      scale: 'css',
      fullPage: p.fullPage || false,
    });
  }
};
```

### Key Gotchas

| Issue | Cause | Fix |
|-------|-------|-----|
| `page.evaluate(str)` does nothing | String creates function but doesn't invoke | Pass as function: `page.evaluate(() => { ... })` |
| Nav reappears after navigation | `goto()` reloads page, losing DOM changes | Use `addStyleTag()` CSS injection instead of DOM manipulation |
| Screenshots look identical to old | Build pipeline serves cached version | If using Docker: `docker compose build --no-cache` |
| File copy doesn't overwrite | `cp -r` with existing target dirs | `rm -rf target && cp -r source target` |
| Sub-tab not showing right content | Click-based tab switching is flaky in automation | Use URL query params: `/page?tab=name` |
| Empty data on some pages | Default time range has no data | Pass explicit time range: `?range=quarter` |

### Sync & Deploy Checklist

After retaking screenshots:

```bash
# 1. Sync source → public directory
rm -rf <public-dir>/screenshots
cp -r <source-dir>/screenshots <public-dir>/screenshots
cp <source-dir>/*.html <public-dir>/

# 2. Rebuild if using build pipeline
# For Docker:
docker compose build --no-cache frontend
docker compose up -d frontend

# 3. Verify deployed content matches source
md5sum <source-dir>/screenshots/example.png
# Compare with:
# docker exec <container> md5sum /path/to/example.png
# Or: curl <deployed-url>/screenshots/example.png | md5sum

# 4. Visual spot-check
# Open deployed URL in browser, scroll through, verify all images load
```

### Anonymization Best Practices

**Replacement map design:**
- Sort keys by length (longest first) to prevent partial replacements
- Include email addresses, not just display names
- Include company/org names
- Include repo/project names that might appear in URLs or text

**What to anonymize:**
- Text nodes in DOM (names, emails, companies)
- Input/textarea values (search boxes, form fields)
- Title and aria-label attributes
- URL-like text in the page

**What NOT to change:**
- Number magnitudes (keep 87 commits as ~87, not 3)
- Score distributions (keep realistic variance)
- Timestamps (keep realistic patterns)
- Technical terms and product names
