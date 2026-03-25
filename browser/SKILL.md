---
name: "browser"
description: "Browser automation and web page operations using Playwright and Selenium WebDriver"
version: "1.0.0"
author: "Anthropic"
license: "MIT"
tags: ["automation", "web", "browser", "playwright", "selenium"]
---

# Browser Skill

A comprehensive skill for browser automation and web page operations, providing tools for web scraping, testing, and interaction using Playwright and Selenium WebDriver.

## Features

- **Playwright Integration**: Modern browser automation with cross-browser support
- **Selenium WebDriver**: Legacy browser automation support
- **Screenshot Capture**: Take screenshots of web pages or specific elements
- **PDF Generation**: Generate PDFs from web pages
- **Element Interaction**: Click, type, select, and interact with web elements
- **Wait Conditions**: Smart waiting for elements and page states
- **Cookie Management**: Handle browser cookies and sessions
- **Network Monitoring**: Intercept and monitor network requests
- **JavaScript Execution**: Execute custom JavaScript in browser context
- **Form Handling**: Fill forms, submit data, handle file uploads
- **Navigation**: Navigate, go back/forward, refresh pages
- **Frame Handling**: Work with iframes and nested frames
- **Alert/Dialog Handling**: Handle browser alerts, confirms, and prompts

## Installation

### Prerequisites

- Node.js 14+ or Python 3.7+
- For Playwright: Run `npx playwright install` to install browser binaries
- For Selenium: Install WebDriver executables (ChromeDriver, GeckoDriver, etc.)

### Install Skill

```bash
# Clone the skills repository
git clone https://github.com/anthropics/skills.git
cd skills

# Copy browser skill to your skills directory
cp -r skills/browser ~/.agents/skills/
```

## Configuration

Create or update your skill configuration:

```json
{
  "browser": {
    "default_browser": "chromium",
    "headless": true,
    "timeout": 30000,
    "viewport": {
      "width": 1280,
      "height": 720
    },
    "downloads_path": "./downloads",
    "screenshots_path": "./screenshots"
  }
}
```

### Configuration Options

- `default_browser`: Default browser to use (`chromium`, `firefox`, `webkit`, `chrome`, `edge`)
- `headless`: Run browser in headless mode (default: true)
- `timeout`: Default timeout in milliseconds (default: 30000)
- `viewport`: Default viewport size
- `downloads_path`: Path for downloaded files
- `screenshots_path`: Path for screenshot files

## Usage

### Basic Navigation

```javascript
// Navigate to a URL
await browser.goto("https://example.com");

// Get page title
const title = await browser.getTitle();

// Get current URL
const url = await browser.getCurrentUrl();
```

### Element Interaction

```javascript
// Click an element
await browser.click("#submit-button");

// Type text into an input
await browser.type("#username", "myusername");
await browser.type("#password", "mypassword");

// Select from dropdown
await browser.select("#country", "United States");

// Check/uncheck checkbox
await browser.check("#agree-terms");
```

### Taking Screenshots

```javascript
// Full page screenshot
await browser.screenshot("fullpage.png");

// Element screenshot
await browser.screenshotElement("#main-content", "content.png");

// Screenshot with options
await browser.screenshot("custom.png", {
  fullPage: false,
  clip: { x: 0, y: 0, width: 800, height: 600 }
});
```

### PDF Generation

```javascript
// Generate PDF from current page
await browser.pdf("page.pdf");

// PDF with custom options
await browser.pdf("styled.pdf", {
  format: "A4",
  printBackground: true,
  margin: {
    top: "1cm",
    right: "1cm",
    bottom: "1cm",
    left: "1cm"
  }
});
```

### Waiting for Elements

```javascript
// Wait for element to be visible
await browser.waitForSelector("#loading-spinner", { state: "hidden" });

// Wait for text to appear
await browser.waitForFunction(() => {
  return document.querySelector("#status").textContent === "Ready";
});

// Custom wait condition
await browser.waitForTimeout(5000);
```

### JavaScript Execution

```javascript
// Execute JavaScript in page context
const result = await browser.evaluate(() => {
  return document.title;
});

// Execute with arguments
const sum = await browser.evaluate((a, b) => {
  return a + b;
}, 5, 10);
```

### Cookie Management

```javascript
// Get all cookies
const cookies = await browser.getCookies();

// Set a cookie
await browser.setCookie({
  name: "session_id",
  value: "abc123",
  domain: ".example.com",
  path: "/"
});

// Delete cookies
await browser.deleteCookies("session_id");
```

### Network Monitoring

```javascript
// Monitor network requests
await browser.route("**/*", (route) => {
  console.log(route.request().url());
  route.continue();
});

// Block specific requests
await browser.route("**/ads/**", (route) => {
  route.abort();
});

// Mock API responses
await browser.route("**/api/data", (route) => {
  route.fulfill({
    status: 200,
    contentType: "application/json",
    body: JSON.stringify({ data: "mocked" })
  });
});
```

### Form Handling

```javascript
// Fill form fields
await browser.fill("#name", "John Doe");
await browser.fill("#email", "john@example.com");

// Upload file
await browser.setInputFiles("#file-upload", "path/to/file.pdf");

// Submit form
await browser.click("#submit-form");
```

### Frame Handling

```javascript
// Work with iframe
const frame = await browser.frame("#iframe-id");
await frame.click("#button-inside-iframe");

// Switch to frame by URL
const frameByUrl = await browser.frame({ url: /iframe-page/ });
await frameByUrl.fill("#input", "text");
```

### Alert Handling

```javascript
// Accept alert
await browser.on("dialog", (dialog) => {
  dialog.accept();
});

// Dismiss confirm dialog
await browser.on("dialog", (dialog) => {
  if (dialog.type() === "confirm") {
    dialog.dismiss();
  } else {
    dialog.accept();
  }
});
```

## Advanced Usage

### Browser Contexts

```javascript
// Create new browser context
const context = await browser.newContext();

// Create page in context
const page = await context.newPage();
await page.goto("https://example.com");

// Close context (closes all pages)
await context.close();
```

### Multiple Browsers

```javascript
// Launch multiple browsers
const browser1 = await browser.launch("chromium");
const browser2 = await browser.launch("firefox");

await browser1.newPage().goto("https://site1.com");
await browser2.newPage().goto("https://site2.com");
```

### Custom Browser Launch

```javascript
// Launch with custom options
const browser = await browser.launch("chromium", {
  headless: false,
  args: ["--disable-web-security", "--disable-features=VizDisplayCompositor"]
});
```

### Device Emulation

```javascript
// Emulate mobile device
await browser.emulateDevice("iPhone 12");

// Custom device
await browser.emulateDevice({
  name: "Custom Device",
  viewport: { width: 375, height: 667 },
  userAgent: "Custom UA",
  deviceScaleFactor: 2
});
```

## Error Handling

```javascript
try {
  await browser.goto("https://example.com");
  await browser.click("#nonexistent-element");
} catch (error) {
  if (error.name === "TimeoutError") {
    console.log("Element not found within timeout");
  } else if (error.name === "NotFoundError") {
    console.log("Element does not exist");
  } else {
    console.log("Unexpected error:", error);
  }
}
```

## Best Practices

1. **Use appropriate waits**: Prefer `waitForSelector` over `waitForTimeout`
2. **Handle dynamic content**: Use `waitForFunction` for complex conditions
3. **Clean up resources**: Always close browsers and contexts
4. **Use selectors wisely**: Prefer data attributes over CSS classes
5. **Handle network failures**: Implement retry logic for unstable sites
6. **Respect robots.txt**: Don't scrape sites that prohibit it
7. **Rate limiting**: Add delays between requests to avoid being blocked
8. **User agents**: Rotate user agents for better anonymity

## Troubleshooting

### Common Issues

**Element not found errors:**
- Check if element is in iframe
- Verify selector accuracy
- Ensure page is fully loaded

**Timeout errors:**
- Increase timeout values
- Check network connectivity
- Verify page loading issues

**Browser launch failures:**
- Install browser binaries with `npx playwright install`
- Check system dependencies
- Try different browser engines

**Network interception issues:**
- Ensure routes are set before navigation
- Check URL patterns for accuracy
- Verify interception is enabled

### Debug Mode

```javascript
// Enable debug logging
await browser.launch("chromium", {
  headless: false,
  slowMo: 1000, // Slow down operations by 1 second
  devtools: true // Open DevTools
});
```

## Examples

### Web Scraping

```javascript
const browser = await browser.launch("chromium");
const page = await browser.newPage();

await page.goto("https://news.example.com");

// Extract article titles
const titles = await page.evaluate(() => {
  const elements = document.querySelectorAll("h2.article-title");
  return Array.from(elements).map(el => el.textContent);
});

console.log(titles);
await browser.close();
```

### Automated Testing

```javascript
const browser = await browser.launch("chromium");
const page = await browser.newPage();

await page.goto("https://app.example.com/login");

// Fill login form
await page.fill("#username", "testuser");
await page.fill("#password", "testpass");
await page.click("#login-button");

// Wait for dashboard to load
await page.waitForSelector("#dashboard");

// Verify login success
const isLoggedIn = await page.isVisible("#user-menu");
console.log("Login successful:", isLoggedIn);

await browser.close();
```

### Screenshot Comparison

```javascript
const browser = await browser.launch("chromium");
const page = await browser.newPage();

await page.goto("https://example.com");

// Take baseline screenshot
await page.screenshot("baseline.png");

// Make changes and take new screenshot
await page.click("#update-button");
await page.waitForTimeout(2000);
await page.screenshot("updated.png");

// Compare screenshots (requires additional image comparison library)
const areEqual = await compareImages("baseline.png", "updated.png");
console.log("Screenshots match:", areEqual);

await browser.close();
```

## Contributing

Contributions are welcome! Please see the main skills repository for contribution guidelines.

## License

MIT License - see LICENSE file for details