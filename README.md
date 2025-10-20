# Playwright BDD Test Automation

**pure Playwright-BDD** test automation framework using TypeScript and ES modules. This framework follows a **Page Object Model** with clean separation of concerns for automated testing of web applications.

## 🏗️ Architecture

- **Features/Steps**: Cucumber `.feature` files with `playwright-bdd` step definitions
- **Page Objects**: Located in `pages/` with corresponding locators in `locators/`
- **Native Playwright**: Pure Playwright Test with native browser management and HTML reporting
- **Cross-Platform**: Supports desktop browsers (Chrome, Edge, Firefox, Safari) and mobile devices (Android, iPhone)
- **Responsive Design**: Mobile-aware locators and page interactions
- **Configuration**: YAML-based environment configs in `config/` with env variable substitution

## 📋 Prerequisites

- **Node.js** (version 16 or higher)
- **npm** (comes with Node.js)

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone <repository-url>
cd "Playwright BDD"
```

### 2. Install Dependencies
```bash
npm install
```

This command will install all required dependencies including:
- Playwright Test framework
- playwright-bdd for Cucumber integration
- TypeScript and related tooling
- ESLint and Prettier for code quality

### 3. Install Playwright Browsers
```bash
npx playwright install
```

## 🎯 Running Tests

### Desktop Browser Tests
```bash
# Chrome/Chromium
npm run test:chrome

# Microsoft Edge  
npm run test:edge

# Firefox
npm run test:firefox

# Safari
npm run test:safari
```

### Mobile Device Tests
```bash
# Android (Pixel 5)
npm run test:android

# iPhone (iPhone 12)
npm run test:iphone
```

### Cross-Platform Tests
```bash
# Run on all desktop browsers
npm run test:desktop

# Run on all mobile devices
npm run test:mobile

# Run on all platforms (desktop + mobile)
npm run test:all

# Run specific combinations
npx playwright test --project=Chrome --project=Android
```

### View Test Reports
```bash
# Open HTML test report
npm run test:report
```

## 🛠️ Development Commands

### Code Quality
```bash
# Format code with Prettier
npm run format

# Check code formatting
npm run format:check

# Run ESLint
npm run lint

# Fix ESLint issues automatically
npm run lint:fix
```

### CI Pipeline
```bash
# Run full CI pipeline (lint + format check + tests)
npm run ci
```

## 📁 Project Structure

```
Playwright BDD/
├── config/                 # Environment configurations
│   └── prod.yml           # Production config
├── features/              # Cucumber feature files
│   ├── login.feature      # Feature definitions
│   └── step_definitions/  # Step implementations
│       └── loginSteps.ts
├── locators/              # Page element locators
│   ├── HomePageLocators.ts
│   ├── LoginPageLocators.ts
│   └── AccountPageLocators.ts
├── pages/                 # Page Object Model classes
│   ├── HomePage.ts
│   ├── LoginPage.ts
│   └── AccountPage.ts
├── utils/                 # Helper utilities
│   ├── configLoader.ts    # Configuration management
│   ├── helper.ts          # Custom Playwright helpers
│   └── pageFactory.ts     # Page object factory
├── playwright.config.ts   # Playwright configuration
├── package.json          # Dependencies and scripts
└── tsconfig.json         # TypeScript configuration
```

## 🔧 Configuration

### Environment Variables
The framework supports environment-specific configurations using YAML files in the `config/` directory. Variables can be substituted using the format `${VAR_NAME:-default_value}`.

### Cross-Platform Configuration
Tests are configured to run on multiple platforms via Playwright projects:

**Desktop Browsers:**
- Chrome/Chromium
- Microsoft Edge  
- Firefox
- Safari

**Mobile Devices:**
- Android (Pixel 5 simulation)
- iPhone (iPhone 12 simulation)

See `playwright.config.ts` for detailed configuration including viewport sizes, user agents, and device-specific settings.

## 🎨 Key Patterns

### Page Factory Pattern
```typescript
const pages = new PageFactory(page);
await pages.homePage.verifyHomePage();
await pages.loginPage.login(config.username, config.password);
```

### Custom Helper Functions
The framework includes custom helper functions with built-in waits and error handling:
- `customClick()` - Enhanced click with wait conditions
- `customFill()` - Enhanced fill with wait conditions
- `isMobile()` - Mobile device detection for responsive testing

### Responsive Locator Pattern
Locators are functions returning Playwright `Locator` objects with mobile/desktop awareness:
```typescript
// Basic locator
usernameInput: (page: Page): Locator => page.locator('input[name="j_username"]')

// Responsive locator with fallbacks
signinButton: (page: Page): Locator => 
  page.getByRole('link', { name: 'Sign in' }).or(
    page.locator('[data-testid="signin-link"]')
  )
```

### Mobile-Aware Page Objects
Page objects automatically adapt to mobile vs desktop interactions:
```typescript
async navigateToLogin() {
  // On mobile, open menu first if needed
  if (isMobile(this.page)) {
    const menuButton = HomePageLocators.mobileMenuButton(this.page);
    if (await menuButton.isVisible()) {
      await customClick(menuButton, 'Mobile menu button');
    }
  }
  await customClick(HomePageLocators.signinButton(this.page), 'Sign in button');
}
```

## 📊 Reports

Test results are generated as HTML reports using Playwright's native reporting. Reports include:
- Test execution details
- Screenshots on failure
- Video recordings on failure
- Execution traces for debugging

## 🤝 Contributing

1. Follow the existing code patterns and conventions
2. Run `npm run lint:fix` and `npm run format` before committing
3. Ensure all tests pass with `npm run ci`
4. Use meaningful commit messages

## 📝 Notes

- This framework uses ES modules with `.js` imports (even for `.ts` files)
- Tests run with native Playwright Test framework enhanced with BDD capabilities
- Configuration supports multiple environments and credential management
- Reports are generated in `playwright-report/index.html`