# ToiFlow: A Secure Flowise Chatbot Proxy

ToiFlow provides a secure proxy for hosting the [Flowise chatbot](https://github.com/FlowiseAI/FlowiseChatEmbed), allowing users to maintain all features of the native Flowise chatbot while enhancing security for the public embed chatbot.

https://github.com/user-attachments/assets/9f52cb53-6058-4ba3-b21b-797a1a66a188

## ✨ Features

- 🤖 **Full Flowise Functionality**: Retains all features of the native Flowise chatbot.
- 🔒 **Enhanced Security**: Protects sensitive information like CHATFLOW_ID and API_HOST.
- 🏠 **Self-Hosting**: Users can host their own web.js, maintaining control over their chatbot implementation.
- 🔑 **JWT Authentication**: Uses JSON Web Tokens (JWT) to securely encrypt and verify the CHATFLOW_ID.
- 🛡️ **Proxy Server**: All requests are routed through a secure proxy server, adding an extra layer of protection.
- 🌊 **Streamed Responses**: Supports streamed responses for real-time interactions.
- 🌐 **IP Address Integration**: Automatically fetches and includes the user's IP address in chatbot interactions. This is useful for custom tools that require the user's IP address. More about this [here](https://github.com/toi500/static/issues/1).
- 📱 **Progressive Web App (PWA) Support**: ToiFlow now functions as a PWA (Progressive Web App).

## 📚 Chatbot Library Options

This project supports two options for including the Flowise Chatbot library:

### 1. Using the Official CDN

You can use the official Flowise Chatbot library hosted on a CDN. This is the easiest option and ensures you're always using the latest version.

To use the CDN version, update the `<script>` tag in `index.html` as follows:

```html
<script type="module">
  import Chatbot from "https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js";
  // ... rest of the script
</script>
```

### 2. Self-Hosting web.js

For more control or in environments where external CDNs are not accessible, you can host the web.js file yourself:

1. Download the `web.js` file from the Flowise repository or npm package.
2. Place it in your project's `public/dist/` directory.
3. Update the `<script>` tag in `index.html` to point to your local file:

```html
<script type="module">
  import Chatbot from "./dist/web.js";
  // ... rest of the script
</script>
```

Choose the option that best fits your security requirements and deployment environment.

**Note:** When using a self-hosted web.js or the official CDN, you may need to adjust the `styles.css` file accordingly. The chatbot's default styles might differ between versions, so review and update your CSS to ensure proper styling and layout of the chatbot interface.

## 📋 Prerequisites

- An active [Flowise workflow](https://github.com/FlowiseAI/Flowise)
- Node.js (version 14 or higher)
- npm (Node Package Manager)

### Dependencies
- axios ^1.7.7 - HTTP client for making API requests
- cors ^2.8.5 - Cross-Origin Resource Sharing middleware
- dotenv ^16.4.5 - Environment variable management
- express ^4.21.1 - Web application framework
- form-data ^4.0.1 - Form data handling
- jsonwebtoken ^9.0.2 - JWT implementation
- multer ^1.4.5-lts.1 - File upload handling

## 🚀 Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/toi500/toiflow.git
   cd toiflow
   ```

2. Install the dependencies:

   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory and add your Flowise API credentials:
   
   ```bash
   CHATFLOW_ID=your_chatflow_id_here
   API_HOST=your_flowise_api_host_here
   JWT_SECRET=your_long_random_string_here  # At least 32 characters
   ```

   **Note**: The JWT_SECRET can be any string, but for security, use a long random string (32+ characters). You can generate one using:

   ```bash
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```

4. Start the server:

   ```bash
   npm start
   ```

5. Open a web browser and navigate to `http://localhost:3000` to interact with the chatbot.

## 🖥️ Usage

1. Access the chatbot interface at `http://localhost:3000` (or your deployed URL).
2. The chatbot will automatically initialize with secure configurations.
3. Interact with the chatbot as normal, with enhanced security.
4. To use the PWA features, visit the site on a supported browser and look for the "Add to Home Screen" option.

## 🔧 Technical Details

### Server (server.js)

- Uses Express.js for the server framework.
- Implements a token system to mask the CHATFLOW_ID.
- Provides a `/api/config` endpoint to securely fetch configuration.
- Acts as a proxy for all Flowise API endpoints, adding security layers.
- Supports streaming responses for real-time interactions.
- Handles various HTTP methods (GET, POST) for different API endpoints.

### Client (index.html)

- Utilizes the Flowise Chatbot library for the chat interface.
- Fetches configuration securely from the server.
- Automatically retrieves the user's IP address for enhanced tracking.
- Initializes the chatbot with secure parameters and custom themes (via Flowise script settings).

## 🔒 Security Benefits

- **JWT Encryption**: Your CHATFLOW_ID is encrypted and signed using JWT, making it impossible to tamper with.
- **Server-Side Proxy**: All API requests are routed through the server, preventing direct client access to the Flowise API.
- **24-Hour Token Expiration**: JWT tokens automatically expire after 24 hours for enhanced security.
- **Environment Variable Usage**: Sensitive data (CHATFLOW_ID, JWT_SECRET) is stored in environment variables, not in the code.

## 🔑 JWT Token Implementation

ToiFlow uses JSON Web Tokens (JWT) to secure the communication between the client and server:

### How it Works

1. **Initial Setup**
   - Add your `JWT_SECRET` to the `.env` file:

   ```bash
   JWT_SECRET=your_long_random_string_here # At least 32 characters
   ```

2. **Token Generation**
   - When the chatbot loads, the server creates a JWT token containing your encrypted `CHATFLOW_ID`
   - This token expires after 24 hours for security
   - Only the encrypted token is sent to the frontend, never your actual `CHATFLOW_ID`

3. **Token Usage**
   - The frontend uses this token for all API calls
   - Your server verifies the token and extracts the real `CHATFLOW_ID` for Flowise API calls
   - If the token expires, the chatbot will automatically refresh on page reload

### Benefits

- 🔒 Your `CHATFLOW_ID` stays secure on the server
- ⏰ Tokens expire automatically after 24 hours
- 🔄 Simple refresh mechanism (just reload the page)
- 🛡️ Cryptographically signed tokens prevent tampering

### Token Refresh

Users don't need to manage tokens manually. The system will:
- Generate a new token on page load
- Handle expired tokens gracefully
- Maintain security without user intervention

This implementation ensures your Flowise chatbot remains secure while providing a good user experience.

## 🎨 Customization

Users can customize the Flowise chatbot's appearance and behavior by modifying the [Flowise script settings](https://docs.flowiseai.com/using-flowise/embed#chatflow-config) in the index.html file and the style of the chatbot (web.js file) following [this guide](https://docs.flowiseai.com/using-flowise/embed#custom-modificaton).

![image](https://github.com/user-attachments/assets/e00e3f94-e5f4-4abe-a6f0-ee8355b613d8)

## 📱 Progressive Web App (PWA) Features

ToiFlow uses Progressive Web App technology to provide an enhanced user experience:

- **Home Screen Installation**: Users can add ToiFlow to their home screens or app drawers for quick access and a standalone app experience.
- **Cross-Platform Compatibility**: Works seamlessly on mobile (iOS and Android) and desktop (Windows, macOS, Linux) devices.
- **App-like Experience**: Runs in its own window without browser UI, offering a more immersive interaction.
- **Customizable Icons**: The app icon can be tailored for different platforms and sizes, enhancing brand recognition.
- **Screenshots**: The PWA includes screenshots for both desktop and mobile views, providing users with a preview of the app before installation.

![PWA Installation](https://github.com/user-attachments/assets/fb75a022-b76d-4f23-8fce-3d00ea831b94)

### Customizing PWA Appearance

To personalize the PWA for your brand:

1. Replace the icon files (`icon-192x192.png` and `icon-512x512.png`) in the `public` directory with your own branded icons.
2. Update the `manifest.json` file to reflect your app's name, colors, and other metadata.
3. Modify the PWA-related meta tags in `index.html` to match your branding.
4. Add or replace the screenshot images:
   - `screenshot-wide.png`: A desktop view of your app (1280x720 pixels recommended)
   - `screenshot-narrow.png`: A mobile view of your app (750x1334 pixels recommended)

### Installing the PWA

Users can install the ToiFlow PWA using the following methods:
- On desktop: Click the install button in the browser's address bar.
- On mobile: Use the "Add to Home Screen" option in the browser menu.

## 🔧 Other Features

- **Custom Variables**: The setup allows for passing custom variables (IP_ADDRESS) to the chatbot flow via custom tools. More about this [here](https://github.com/toi500/static/issues/1).

## 🛠️ Troubleshooting

- If the chatbot fails to initialize, check the browser console for error messages.
- Ensure that the `.env` file is properly configured with valid CHATFLOW_ID and API_HOST values.
- For PWA-related issues, make sure your server is configured to serve the necessary files (manifest.json, service-worker.js, and icons) correctly.

## 📄 License

ToiFlow is licensed under the ISC License. Use as you see fit.

**Note:** The `web.js` file is owned by Flowise and is made available under the MIT License. All other source code in this repository is licensed under the ISC License unless otherwise specified.

### ISC License

This project is licensed under the ISC License.

### MIT License (for web.js)

The `web.js` file is subject to the MIT License as specified by FlowiseAI.

## ⚠️ Disclaimer

Ensure that the `.env` file is not shared or committed to version control to keep sensitive data secure. Always use HTTPS in production environments to encrypt data in transit. ToiFlow is not officially associated with Flowise and is an independent implementation for enhanced security.

Users are encouraged to fork the project and adapt it to their specific needs and use cases. Please note that ongoing support or updates should not be expected.

**Made with ❤️ by @toi500**
