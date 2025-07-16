# @masanto/hotku-mailhub

![npm version](https://img.shields.io/npm/v/@masanto/hotku-mailhub.svg)
![license](https://img.shields.io/npm/l/@masanto/hotku-mailhub.svg)
![downloads](https://img.shields.io/npm/dm/@masanto/hotku-mailhub.svg)

## 🚀 Professional Email Verification Library

HOTKU MailHub adalah library Node.js yang powerful untuk verifikasi email massal dengan arsitektur modular dan performa tinggi. Dirancang khusus untuk pengecekan kredensial Microsoft (Outlook, Hotmail, Live).

## ✨ Features

- ✅ **Mass Email Verification** - Verifikasi ribuan email sekaligus
- 📊 **Real-time Statistics** - Monitor progress secara langsung  
- 💾 **Auto Save Results** - Simpan otomatis hasil ke file
- 🎨 **Beautiful CLI Interface** - UI yang menarik dengan color coding
- 🔄 **Retry Mechanism** - Otomatis retry untuk koneksi yang gagal
- 📧 **Multiple Categories** - Kategorisasi hasil (Valid, Invalid, Custom, NFA, Retry)
- 🏗️ **Modular Architecture** - Kode terorganisir dengan library terpisah
- 📦 **Easy Integration** - Mudah diintegrasikan ke project lain

## 🛠️ Installation

### NPM Package

```bash
npm install @masanto/hotku-mailhub
```

### Git Repository

```bash
git clone https://github.com/axyid/hotku-mailhub.git
cd hotku-mailhub
npm install
```

## 📖 Usage

### 1. Command Line Interface (CLI)

```bash
# Install globally
npm install -g @masanto/hotku-mailhub

# Run CLI
hotku-mailhub

# Or run locally
npx @masanto/hotku-mailhub
```

### 2. As a Library in Your Project

```javascript
const { MailHubApp, MassChecker, FileHandler, Display } = require('@masanto/hotku-mailhub');

// Basic usage
const app = new MailHubApp();
await app.run();

// Advanced usage - Custom implementation
const checker = new MassChecker();

// Load combos from file
const combos = FileHandler.parseComboFile('./combo.txt');

// Process mass checking
await checker.processMass('./combo.txt', {
    outputDir: './my-results'
});

// Get results
const stats = checker.getStats();
const results = checker.getResults();
```

### 3. Programmatic Usage

```javascript
const { MicrosoftAuth } = require('@masanto/hotku-mailhub/lib/microsoft-auth');

// Single email verification
const auth = new MicrosoftAuth();
const [status, canary] = await auth.verify('email@outlook.com', 'password123');

console.log('Status:', status); // ok, fail, nfa, custom, retry
console.log('Canary:', canary);
```

## 📁 Project Structure

```
@masanto/hotku-mailhub/
├── index.js                 # Main entry point
├── package.json             # Package configuration
├── lib/                     # Core libraries
│   └── microsoft-auth.js    # Microsoft authentication
├── src/                     # Main source code
│   └── mass-checker.js      # Mass checking engine
├── utils/                   # Utility functions
│   ├── file-handler.js      # File operations
│   └── display.js           # Console formatting
└── config/                  # Configuration
    └── app-config.js        # App settings
```

## 🎯 API Documentation

### MassChecker

```javascript
const { MassChecker } = require('@masanto/hotku-mailhub');

const checker = new MassChecker();

// Process mass checking
await checker.processMass(comboPath, options);

// Get statistics
const stats = checker.getStats();
// Returns: { valid: 0, invalid: 0, custom: 0, nfa: 0, retry: 0 }

// Get all results
const results = checker.getResults();
// Returns: Array of result objects

// Reset checker state
checker.reset();
```

### MicrosoftAuth

```javascript
const MicrosoftAuth = require('@masanto/hotku-mailhub/lib/microsoft-auth');

const auth = new MicrosoftAuth();

// Verify credentials
const [status, canary] = await auth.verify(email, password);
// status: 'ok' | 'fail' | 'nfa' | 'custom' | 'retry'
```

### FileHandler

```javascript
const { FileHandler } = require('@masanto/hotku-mailhub');

// Parse combo file
const combos = FileHandler.parseComboFile('./combo.txt');
// Returns: [{ email: 'test@email.com', password: 'pass123' }]

// Save results
FileHandler.saveResults(results, 'output.txt', 'ok');

// Save categorized results
FileHandler.saveCategorizedResults(results, './results');
```

### Display

```javascript
const { Display } = require('@masanto/hotku-mailhub');

// Show banner
Display.showBanner();

// Show progress
Display.showProgress(current, total, stats);

// Show messages
Display.showSuccess('Operation completed!');
Display.showError('Something went wrong!');
Display.showInfo('Information message');
Display.showWarning('Warning message');
```

## 📝 File Formats

### Combo File Format

```
email1@outlook.com:password123
user@hotmail.com:mypassword
test@live.com:testpass456
```

### Result Categories

| Status | Description | Saved to File |
|--------|-------------|---------------|
| `ok` | Valid credentials | ✓ |
| `fail` | Invalid credentials | ✗ |
| `custom` | Custom verification needed | ✓ |
| `nfa` | Need Further Action | ✓ |
| `retry` | Network error/retry needed | ✗ |

## 🔧 Configuration

```javascript
const config = require('@masanto/hotku-mailhub/config/app-config');

// Modify settings
config.defaults.timeout = 120000;  // 2 minutes
config.defaults.delay = 2000;      // 2 seconds between requests
config.defaults.outputDir = './my-results';
```

## 🚀 Examples

### Basic CLI Usage

```bash
# Create combo file
echo "user1@outlook.com:pass123" > combo.txt
echo "user2@hotmail.com:pass456" >> combo.txt

# Run checker
npx @masanto/hotku-mailhub
# Enter path: combo.txt
# Results saved to valid.txt and ./results/
```

### Advanced Integration

```javascript
const { MassChecker, FileHandler, Display } = require('@masanto/hotku-mailhub');

class MyEmailChecker {
    constructor() {
        this.checker = new MassChecker();
    }

    async checkEmails(combos) {
        Display.showBanner();
        
        // Create temporary combo file
        const comboFile = './temp-combo.txt';
        const comboLines = combos.map(c => `${c.email}:${c.password}`).join('\n');
        require('fs').writeFileSync(comboFile, comboLines);

        // Process
        await this.checker.processMass(comboFile);

        // Get results
        const results = this.checker.getResults();
        const validResults = results.filter(r => r.status === 'ok');

        // Cleanup
        require('fs').unlinkSync(comboFile);

        return validResults;
    }
}

// Usage
const checker = new MyEmailChecker();
const validAccounts = await checker.checkEmails([
    { email: 'test@outlook.com', password: 'pass123' },
    { email: 'user@hotmail.com', password: 'pass456' }
]);

console.log('Valid accounts:', validAccounts.length);
```

## 🔒 Security & Ethics

- ⚠️ **Legal Use Only**: Tool ini hanya untuk testing kredensial yang sah
- 🚫 **No Illegal Activities**: Jangan gunakan untuk aktivitas ilegal
- 📋 **Respect ToS**: Ikuti terms of service dari provider email
- ⏱️ **Rate Limiting**: Gunakan delay yang wajar untuk menghindari blocking
- 🛡️ **Responsible Use**: Gunakan dengan etika dan tanggung jawab

## 🤝 Contributing

1. Fork repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details

## 👨‍💻 Authors & Contributors

- **masanto** - *Main Author* - [@masanto](https://github.com/axyid)
- **Not-ISellStuff** - *Original Developer* - HOTKU Brand

## 🔗 Links

- [NPM Package](https://www.npmjs.com/package/@masanto/hotku-mailhub)
- [GitHub Repository](https://github.com/axyid/hotku-mailhub)
- [Issue Tracker](https://github.com/axyid/hotku-mailhub/issues)
- [Documentation](https://github.com/axyid/hotku-mailhub#readme)

## 📈 Changelog

### v1.0.0
- Initial release
- Microsoft authentication support
- Mass checking functionality
- CLI interface
- Modular architecture
- NPM package

---

**🔥 HOTKU MailHub** - Professional email verification solution by masanto
