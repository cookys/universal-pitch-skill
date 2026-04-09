# Screenshot Capture Agent Instructions

This file is an agent prompt — the skill reads it and spawns a subagent to execute
the screenshot pipeline. It's not a bash script because the process requires
adaptive decisions (which pages to screenshot, how to hide nav, how to auth).

## When This Script Runs

Phase 3 (Marketing Pro) calls this after deciding which pages to capture.
It receives a `screenshot_plan.json` from Phase 3 with the list of pages and their jobs.

## Input Contract

The Marketing Pro produces a `screenshot_plan.json`:

```json
{
  "base_url": "http://localhost:3999",
  "viewport": { "width": 1440, "height": 900 },
  "output_dir": "doc/pitch/screenshots",
  "nav_hide_css": "nav { display: none !important; } main { margin-left: 0 !important; }",
  "auth": {
    "method": "cookie | token | none",
    "details": "session cookie from login, or API token, or none needed"
  },
  "anonymize": {
    "enabled": true,
    "map": {
      "Real Name": "Fake Name",
      "real@email.com": "fake@demo.com"
    }
  },
  "pages": [
    {
      "url": "/dashboard",
      "filename": "01-dashboard.png",
      "job": "category",
      "full_page": false,
      "wait": 2000,
      "notes": "Hero screenshot — show overall UI quality"
    },
    {
      "url": "/settings",
      "filename": "02-settings.png",
      "job": "value-proof",
      "full_page": true,
      "wait": 1500,
      "crop": { "description": "Only the AI config panel section" }
    }
  ]
}
```

## Execution Steps

### Step 1: Environment Detection

Before taking any screenshot, verify the environment:

```
1. Check if base_url is reachable:
   curl -s -o /dev/null -w "%{http_code}" <base_url>
   
2. If not reachable, try to start the dev server:
   - Check package.json for "dev" or "start" script
   - Check docker-compose.yml for frontend service
   - Start in background, wait for port to respond
   
3. If auth is required:
   - cookie: Navigate to login page, fill credentials, capture cookie
   - token: Set Authorization header via page.setExtraHTTPHeaders()
   - none: proceed directly
```

### Step 2: Navigation Detection (if pages not provided)

If the screenshot plan doesn't specify pages, auto-detect from the codebase:

```
1. Find router file:
   - Vue: src/router/index.ts or src/router.ts
   - React: src/App.tsx routes, src/routes/
   - Next.js: app/ or pages/ directory structure
   - Angular: app-routing.module.ts

2. Extract route paths and names

3. For each route, determine screenshot job:
   - "/" or "/dashboard" → "category" (overview, hero candidate)
   - "/settings", "/admin/*" → "value-proof" (specific feature)
   - Routes with :id params → skip (need specific data)
   
4. Generate screenshot_plan.json
```

### Step 3: Nav Element Detection

If nav_hide_css is not provided, auto-detect:

```
1. Take initial screenshot of any page
2. Identify navigation elements:
   - <nav> elements
   - [role="navigation"]
   - Elements with sidebar/sidenav/side-nav class
   - Fixed/sticky positioned elements on left/top
   
3. Generate CSS to hide them:
   nav, [role="navigation"], .sidebar, .side-nav,
   [class*="sidebar"], [class*="sidenav"] {
     display: none !important;
   }
   main, [role="main"], .main-content, [class*="main-content"] {
     margin-left: 0 !important;
     padding-left: 0 !important;
     width: 100% !important;
     max-width: 100% !important;
   }

4. Verify by taking a test screenshot — content should fill viewport
```

### Step 4: Anonymization Map Building

If anonymize.map is empty but anonymize.enabled is true:

```
1. Navigate to a data-heavy page (e.g., team members, user list)
2. Extract visible text nodes that look like:
   - Person names (2-3 word patterns, capitalized)
   - Email addresses (@domain patterns)
   - Company names (from page title, headers)
   - Repository/project names
   
3. Generate fake replacements:
   - Names: use locale-appropriate fake names
   - Emails: use @demo-corp.com domain
   - Companies: use generic industry names
   
4. Confirm map with user before proceeding
```

### Step 5: Screenshot Capture Loop

The core Playwright execution:

```javascript
async (page) => {
  const plan = /* loaded screenshot_plan.json */;
  
  // Set viewport
  await page.setViewportSize(plan.viewport);
  
  // Auth if needed
  if (plan.auth.method === 'cookie') {
    // Navigate to login, authenticate
  }
  
  // Build anonymization function from map
  const anonFn = () => {
    const MAP = plan.anonymize.map;
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
    // Input/textarea values
    document.querySelectorAll('input, textarea').forEach(el => {
      for (const k of keys) {
        if (el.value?.includes(k)) {
          el.value = el.value.split(k).join(MAP[k]);
        }
      }
    });
    return 'anonymized';
  };
  
  // Screenshot each page
  for (const p of plan.pages) {
    await page.goto(`${plan.base_url}${p.url}`, {
      waitUntil: 'networkidle'
    });
    await page.waitForTimeout(p.wait || 2000);
    
    // Hide navigation
    await page.addStyleTag({ content: plan.nav_hide_css });
    await page.waitForTimeout(300);
    
    // Anonymize
    if (plan.anonymize.enabled) {
      await page.evaluate(anonFn);
      await page.waitForTimeout(500);
    }
    
    // Screenshot
    await page.screenshot({
      path: `${plan.output_dir}/${p.filename}`,
      type: 'png',
      scale: 'css',
      fullPage: p.full_page || false,
    });
  }
}
```

### Step 6: Post-Capture Verification

After all screenshots are taken:

```
1. List all generated files, verify count matches plan
2. Open each screenshot and verify:
   - No real names visible (spot-check against anonymize map)
   - Navigation is hidden
   - Content fills viewport (no large empty left margin)
   - Data is loaded (no spinners, no "loading..." text)
   - Dark theme contrast is sufficient (no "melting" edges)
3. Report any issues found
```

## Output

```
<output_dir>/
├── 01-dashboard.png
├── 02-settings.png
├── ...
└── screenshot_plan.json    ← saved for future retakes
```

The `screenshot_plan.json` is saved alongside screenshots so future retakes can
reuse the same configuration without re-running Phase 1-3.
