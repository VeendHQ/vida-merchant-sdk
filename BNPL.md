---

# VeendBNPL JS SDK

A lightweight JavaScript SDK to integrate **Veend Buy Now, Pay Later (BNPL)** functionality into your merchant website or application.

---

## 🔧 Features

- Seamless integration with Veend BNPL services
- Support for both `iframe` and `new tab` rendering
- Works in `production`, or `sandbox` environments
- Handles purchase initiation and inter-window communication
- Configurable redirect URI support

---

## 📦 Installation

Include the script directly in your HTML file:

```html
<script src="https://veend-web-2-0-git-ft-fulgo-mobile-veendhq-engineering.vercel.app/vendor/veend-bnpl.min.js"></script>
```

Or load it dynamically from a CDN (if available):

```html
<script src="https://veendhq.com/vendor/veend-bnpl.min.js"></script>
```

> **Note:** This package assumes it runs in a browser environment.

---

## 🚀 Usage

### Initialize the SDK

```javascript
const bnplClient = new VeendBNPL({
  accessToken: 'your-veend-access-token',
  environment: 'sandbox', // or 'production'
  redirectUri: 'https://your-redirect-url.com/callback', // Optional
  target: 'iframe', // or 'tab' (Optional)
});
```

### Trigger a Purchase

```javascript
const purchaseData = {
  reference: "your_uniq_reference",
  items: [{ name: "item_name", unitPrice: 3000, quantity: 1 }],
  amount: 50000,
  deliveryFee: 0,
  otherCharges: 0,
  // ...any other purchase payload required
};

bnplClient.createPurchase(purchaseData);
```

> Depending on the `target`, this will either open a modal (`iframe`) or a new tab to begin the BNPL flow.

---

## 🔘 HTML Integration

Add this to any clickable HTML element to automatically set it up as a trigger:

```html
<button data-veend-scope="purchase-trigger">Buy Now, Pay Later</button>
```

> Custom click handling can be added via JavaScript if needed.

---

## 📄 Type Definitions

### `VeendBNPLConfig`

```ts
{
  accessToken: string;              // Required - Veend Accesstoken
  redirectUri?: string;             // Optional - where to redirect after flow
  environment: 'production' | 'sandbox';
  target?: 'iframe' | 'tab';        // Optional - default is 'iframe'
}
```

### `PurchaseResponse`

```ts
{
  status: 'success' | 'failed';
  data?: {
    reference: number;
  };
}
```

---

## 📥 Message Handling

The SDK uses `window.postMessage` to communicate between your site and the Veend iframe or tab. Only trusted origins (`veendhq.com`, development preview, or localhost) are allowed.

You can listen for iframe responses as needed:

```js
window.addEventListener("message", (event) => {
  if (event.origin.includes("veendhq.com")) {
    console.log("Received message:", event.data);
  }
});
```

---

## ❌ Closing the Iframe

The iframe includes a default close button (`X`). You can also programmatically close it:

```javascript
bnplClient.closeVeendMerchatApplication();
```

---

## 🧪 Environments

| Environment | URL Origin                                 |
|-------------|---------------------------------------------|
| `production` | `https://veendhq.com`                      |
| `sandbox`    | `https://veend-web-2-0-git-ft-fulgo-mobile-veendhq-engineering.vercel.app` |

---

## 📎 Notes

- Ensure the `accessToken` is valid and scoped correctly.
- Popup blockers may interfere with the `tab` mode. Consider fallback options or user prompts.
- All DOM manipulations wait for `DOMContentLoaded`.

---

## 🛠 License

MIT License – © VeendHQ

---
