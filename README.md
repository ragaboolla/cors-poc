# 🔓 CORS Vulnerability Exploit POC

## 📋 Overview
This is a **Proof of Concept (POC)** demonstration that showcases critical Cross-Origin Resource Sharing (CORS) misconfiguration vulnerabilities in web applications. The exploit demonstrates how attackers can steal sensitive user data through improper CORS configuration.

## 🎯 Purpose
This POC is designed for:
- **Security Researchers** - To demonstrate real-world CORS exploitation
- **Bug Bounty Hunters** - As evidence for vulnerability reports
- **Security Teams** - For educational and testing purposes
- **Penetration Testers** - As a demonstration tool

## ⚠️ Legal Disclaimer
**IMPORTANT**: This tool is for **EDUCATIONAL AND AUTHORIZED TESTING PURPOSES ONLY**. 
- Only use on systems you own or have explicit permission to test
- Do not use for malicious activities
- The creators are not responsible for misuse
- Always comply with applicable laws and regulations
- Obtain proper authorization before testing任何 systems

## 🚀 Features
- **Real-time Exploitation** - Demonstrates live data exfiltration
- **Multiple Endpoints** - Tests various API endpoints for CORS misconfiguration
- **PII Detection** - Identifies and highlights Personally Identifiable Information exposure
- **Visual Feedback** - Clear success/failure indicators with hacker-themed UI
- **Console Logging** - Detailed logging for evidence collection
- **Custom Target Support** - Easily test different websites and endpoints

## 🔧 Technical Details

### Common Vulnerable Endpoints Tested:
1. **`/api/comments`** - User comments containing PII
2. **`/wp-json/wp/v2/comments`** - WordPress comment endpoints
3. **`/api/users`** - User data endpoints
4. **`/rest/api/2/user`** - Jira and similar API endpoints
5. **Custom endpoints** - Configurable for specific targets

### Exploitation Method:
```javascript
fetch('https://target-website.com/api/sensitive-data', {
    method: 'GET',
    credentials: 'include',  // Sends session cookies
    headers: {
        'Accept': 'application/json',
        'Origin': 'https://attacker.com'  // Arbitrary malicious origin
    }
})
```

### Security Impact:
- ✅ **Cross-Origin Data Access** - Any domain can read sensitive data
- ✅ **Authentication Bypass** - Uses victim's existing session
- ✅ **PII Exposure** - User names, emails, and personal data
- ✅ **Silent Exploitation** - No user interaction required
- ✅ **Credential Theft** - Session hijacking capabilities

## 📊 What the Exploit Demonstrates

### Data That Can Be Stolen:
- **User Comments** with real names and personal opinions
- **Personal Identifiable Information (PII)** 
- **User profiles** and account information
- **Financial data** and transaction history
- **Internal application data** and business logic

### Attack Scenario:
1. Victim visits malicious website while authenticated to target application
2. Malicious site automatically makes cross-origin requests to vulnerable APIs
3. Sensitive user data is exfiltrated without user knowledge
4. Attacker collects PII for social engineering or fraud

## 🛠️ Installation & Usage

### Quick Start:
1. **Host the HTML file** on any web server
2. **Open the page** in a web browser
3. **Configure target endpoints** in the code
4. **Click "Start Exploit"** to demonstrate the vulnerability
5. **View results** showing stolen data and security impact

### Basic Configuration:
```javascript
// Configure your target endpoints
const targetEndpoints = [
    {
        name: "User Comments (PII Exposure)",
        url: "https://target-site.com/api/comments",
        sensitive: true
    },
    {
        name: "User Profiles", 
        url: "https://target-site.com/api/users",
        sensitive: true
    }
];
```

### For Bug Bounty Reports:
1. Take **screenshots** of successful exploitation
2. Record **console logs** showing stolen data
3. Document the **impact assessment**
4. Include this POC as evidence in your report
5. Follow responsible disclosure practices

## 📈 Severity Assessment

### Risk Levels:

#### **CRITICAL (P1)**
- Mass PII exposure with financial data
- Authentication bypass leading to account takeover
- Regulatory compliance violations

#### **HIGH (P2)** 
- Significant PII exposure
- Session hijacking capabilities
- Business logic exposure

#### **MEDIUM (P3)**
- Limited data exposure
- Non-sensitive information leakage
- Configuration issues without direct data theft

### CVSS Scoring:
- **Base Score**: 6.5 - 8.2 (Medium to High)
- **Impact**: Confidentiality compromise
- **Exploitability**: Network-based, low complexity

### Business Impact:
- **Regulatory Violations**: GDPR, CCPA, LGPD non-compliance
- **Reputation Damage**: Loss of customer trust
- **Financial Risk**: Potential fines and legal liability
- **Competitive Disadvantage**: Data breach consequences

## 🔒 Recommended Fixes

### Server-Side Configuration:
```apache
# Proper CORS configuration for Apache
Header always set Access-Control-Allow-Origin "https://trusted-domain.com"
Header always set Access-Control-Allow-Credentials "false"
Header always set Access-Control-Allow-Methods "GET, POST, OPTIONS"
Header always set Access-Control-Allow-Headers "Content-Type, Authorization"
```

### Application-Level Protections:
```javascript
// Express.js CORS configuration
const corsOptions = {
  origin: function (origin, callback) {
    const allowedOrigins = ['https://trusted-domain.com', 'https://app.domain.com'];
    if (!origin || allowedOrigins.indexOf(origin) !== -1) {
      callback(null, true);
    } else {
      callback(new Error('CORS policy violation'));
    }
  },
  credentials: false
};
app.use(cors(corsOptions));
```

### Security Headers:
```http
Access-Control-Allow-Origin: https://trusted-domain.com
Access-Control-Allow-Credentials: false
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'
```

## 📝 Evidence Collection

### For Security Reports:
- [ ] Screenshot of successful exploit
- [ ] Console output showing stolen data
- [ ] Network tab showing CORS headers
- [ ] Impact analysis with user count
- [ ] Request/response headers evidence
- [ ] Data sensitivity classification

### Sample Report Template:
```markdown
## Vulnerability: CORS Misconfiguration

### Proof of Concept
A live demonstration shows that any malicious website can:
1. Access authenticated user data from vulnerable APIs
2. Steal PII including user names and personal data
3. Perform silent data exfiltration without user interaction

**Technical Details**:
- Vulnerable Endpoint: `https://target.com/api/sensitive-data`
- CORS Headers: `Access-Control-Allow-Origin: https://attacker.com`
- Data Exposed: User profiles, comments, financial data

**Impact Assessment**:
- 100+ user records with real identities
- Financial and personal data exposure
- Regulatory compliance violations

**Recommended Fix**:
Implement origin validation and remove wildcard CORS policies.
```

## 🎯 Testing Methodology

### 1. Reconnaissance Phase
- Identify API endpoints
- Check for CORS headers in responses
- Map application functionality

### 2. Exploitation Phase
- Test with arbitrary origins
- Verify credential inclusion
- Validate data access

### 3. Impact Assessment
- Classify data sensitivity
- Estimate user impact
- Calculate business risk

### 4. Reporting Phase
- Document findings
- Provide evidence
- Suggest remediation

## 🔧 Customization Guide

### Adding New Endpoints:
```javascript
const customEndpoints = [
    {
        name: "Custom API Endpoint",
        url: "https://your-target.com/api/endpoint",
        sensitive: true, // Mark as PII-sensitive
        method: "GET",   // HTTP method
        headers: {       // Custom headers
            'Accept': 'application/json',
            'X-Custom-Header': 'value'
        }
    }
];
```

### Modifying UI Themes:
```css
/* Custom color scheme */
body {
    background: #1a1a2e;
    color: #e94560;
}
.container {
    border-color: #e94560;
}
button {
    background: #e94560;
}
```

## 🤝 Responsible Disclosure

### Best Practices:
1. **Obtain Permission** - Always get authorization before testing
2. **Use Test Environments** - Avoid production systems when possible
3. **Limit Data Collection** - Only collect minimum required evidence
4. **Secure Evidence** - Protect collected data appropriately
5. **Follow Program Rules** - Adhere to bug bounty program guidelines

### Disclosure Timeline:
1. **Discovery** - Identify and verify vulnerability
2. **Reporting** - Submit detailed report to vendor
3. **Validation** - Allow time for vendor verification
4. **Remediation** - Vendor implements fixes
5. **Disclosure** - Public disclosure after fixes deployed

## 📞 Support & Resources

### Learning Resources:
- OWASP CORS Security Guide
- MDN Web Docs - CORS
- PortSwigger CORS Tutorials
- Security Training Platforms

### Community:
- Security researcher forums
- Bug bounty platforms
- OWASP local chapters
- Security conferences

### Tools for Further Testing:
- Burp Suite CORS Scanner
- OWASP ZAP CORS Add-on
- Custom scripting frameworks
- Browser developer tools

---

## 🔐 Security Best Practices

### For Developers:
- Always validate origins server-side
- Never use wildcard origins with credentials
- Implement proper authentication checks
- Use security headers effectively
- Conduct regular security testing

### For Organizations:
- Implement security training programs
- Conduct regular penetration testing
- Establish bug bounty programs
- Maintain incident response plans
- Follow security compliance frameworks

**Remember**: With great power comes great responsibility. Use this knowledge to make the web safer for everyone. 🔐

*Last updated: October 2025*  
*Version: 2.0 - Generic CORS Testing Tool*
