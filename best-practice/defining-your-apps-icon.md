# Define Your App's Icon

Provide a favicon so wallets and browsers can identify your site. Put an icon link in the document's `<head>` and use a publicly accessible image URL:

```html
<head>
  <link rel="icon" href="https://example.com/icon.png" type="image/png" />
  <link rel="shortcut icon" href="https://example.com/icon.png" />
</head>
```

Wallet versions may retrieve or cache site metadata differently. Check the icon in the actual connection prompt after deployment; do not assume that changing the favicon immediately updates an existing wallet session.
