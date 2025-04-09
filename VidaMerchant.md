# VidaMerchant SDK

A JavaScript client library for integrating with Vida Merchant services, providing a streamlined experience for authentication and purchase processing.

## Table of Contents
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Core Methods](#core-methods)
- [Usage Examples](#usage-examples)
- [Purchase Object Structure](#purchase-object-structure)
- [Security Considerations](#security-considerations)
- [API Reference](#api-reference)
- [Troubleshooting](#troubleshooting)

## Installation

Include the VidaMerchant SDK in your HTML file:

```html
<!-- For sandbox environment -->
<script src="https://vida-dashboard-git-ft-agiletech-veendhq-engineering.vercel.app/vendor/vida-merchant.js"></script>

<!-- For production environment -->
<script src="https://app.mycreditprofile.me/vendor/vida-merchant.js"></script>
```

## Quick Start

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://vida-dashboard-git-ft-agiletech-veendhq-engineering.vercel.app/vendor/vida-merchant.js"></script>
</head>
<body>
  <button onclick="startPayment()">Pay Now</button>

  <script>
    function startPayment() {
      const vidaMerchant = new VidaMerchant({
        clientId: "YOUR_CLIENT_ID", 
        clientSecret: "YOUR_CLIENT_SECRET",
        redirectUri: "https://your-website.com/callback",
        environment: "sandbox"
      });
      
      vidaMerchant.createPurchase({
        reference: "ORDER-123",
        items: [{ name: "Product", unitPrice: 5000, quantity: 1 }],
        totalAmount: 5000
      });
    }
  </script>
</body>
</html>
```

## Configuration

The `VidaMerchant` constructor accepts a configuration object with the following properties:

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `clientId` | string | Yes | OAuth client ID provided by Vida |
| `clientSecret` | string | Yes | OAuth client Secret provided by Vida |
| `redirectUri` | string | No | Optional redirect URI for OAuth flow completion |
| `environment` | string | Yes | Either 'production', 'sandbox', or 'local' |
| `target` | string | No | Either 'iframe' (default) or 'tab' |

## Core Methods

### `createPurchase(purchaseData)`

Initiates a purchase transaction.

```javascript
vidaMerchant.createPurchase({
  reference: "ORDER-123",
  items: [
    { name: "Product 1", unitPrice: 5000, quantity: 2 }
  ],
  totalAmount: 10000
});
```

### `openVidaMerchatApplication()`

Opens the Vida Merchant application interface manually.

```javascript
vidaMerchant.openVidaMerchatApplication();
```

### `closeVidaMerchatApplication()`

Closes the Vida Merchant application interface manually.

```javascript
vidaMerchant.closeVidaMerchatApplication();
```

## Usage Examples

### Basic Integration

```javascript
const vidaMerchant = new VidaMerchant({
  clientId: "YOUR_CLIENT_ID", 
  clientSecret: "YOUR_CLIENT_SECRET",
  environment: "sandbox"
});

document.getElementById("payButton").addEventListener("click", function() {
  vidaMerchant.createPurchase({
    reference: "INV-" + Date.now(),
    items: [
      { name: "Premium Plan", unitPrice: 15000, quantity: 1 }
    ],
    totalAmount: 15000
  });
});
```

### Using Data Attributes

The SDK automatically binds to elements with the `data-vida-scope` attribute:

```html
<button data-vida-scope="merchant">Start Payment</button>

<script>
  // Initialize the SDK
  const vidaMerchant = new VidaMerchant({
    clientId: "YOUR_CLIENT_ID", 
    clientSecret: "YOUR_CLIENT_SECRET",
    environment: "sandbox"
  });
</script>
```

### Opening in a New Tab

```javascript
const vidaMerchant = new VidaMerchant({
  clientId: "YOUR_CLIENT_ID", 
  clientSecret: "YOUR_CLIENT_SECRET",
  environment: "sandbox",
  target: "tab"  // Opens in new tab instead of iframe
});
```

### Handling Purchase Completion

```javascript
window.addEventListener("message", event => {
  // Verify the message origin
  if (event.origin !== "https://app.mycreditprofile.me" && 
      event.origin !== "https://vida-dashboard-git-ft-agiletech-veendhq-engineering.vercel.app") {
    return;
  }
  
  if (event.data.type === "purchase" && event.data.status === "completed") {
    console.log("Purchase complete!");
    // Handle successful purchase
  }
});
```

## Purchase Object Structure

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `reference` | string | Yes | Your unique reference identifier |
| `items` | array | Yes | Array of items being purchased |
| `totalAmount` | number | Yes | Total purchase amount (in smallest currency unit) |

### Item Structure

Each item in the `items` array should have:

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `name` | string | Yes | Name of the item |
| `unitPrice` | number | Yes | Price per unit (in smallest currency unit) |
| `quantity` | number | Yes | Quantity of items |

## Security Considerations

1. **Client Secret Protection**: In production environments, consider implementing a server-side proxy to avoid exposing your client secret in client-side code.

2. **HTTPS**: Always use HTTPS in production environments to secure your authentication parameters.

3. **Origin Validation**: Implement additional validation when handling message events.

## API Reference

### Environment URLs

- **Production**: `https://app.mycreditprofile.me`
- **Sandbox**: `https://vida-dashboard-git-ft-agiletech-veendhq-engineering.vercel.app`
- **Local**: `http://localhost:3000`

### Authorization Endpoint

```
GET /authorization/merchants
```

Query Parameters:
- `client_id`: Your OAuth client ID
- `client_secret`: Your OAuth client secret
- `redirect_url`: (Optional) Redirect URL after completion

## Troubleshooting

### Popup Blocked
If using `target: "tab"` and the popup is blocked, ensure your code is triggered by a direct user action (like a click event).

### Cross-Origin Issues
If experiencing cross-origin message problems, verify that your SDK version matches your environment setting.

### Authentication Failures
Double-check your client ID and secret. For sandbox testing, ensure you're using sandbox credentials.

---

For additional support or questions about the VidaMerchant SDK, please contact the Vida support team.