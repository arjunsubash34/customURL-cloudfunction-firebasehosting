# Firebase Hosting - Custom URL for Cloud Function

Serve different HTML files based on specific URL paths using Firebase Cloud Functions, mapped to a custom domain with HTTPS support, using any DNS management. The goal is to ensure seamless delivery of HTML content and demonstrate the setup of custom routes.

## Prerequisites

- [Introduction](#introduction)
- [Configuration](#configuration)
  - [Firebase Project Setup](#firebase-project-setup)
  - [Custom Domain](#custom-domain)
  - [Cloud Function Setup](#cloud-function-setup)
  - [Firebase Hosting Configuration](#firebase-hosting-configuration)
- [Experiments and Tests](#experiments-and-tests)
  - [Step 1: Write the Cloud Function](#step-1-write-the-cloud-function)
  - [Step 2: Deploy the Cloud Function](#step-2-deploy-the-cloud-function)
  - [Step 3: Set Up a Custom Domain in Firebase Hosting](#step-3-set-up-a-custom-domain-in-firebase-hosting)
  - [Step 4: Deploy Firebase Hosting](#step-4-deploy-firebase-hosting)
  - [Step 5: Add Custom Domain](#step-5-add-custom-domain)
  - [Step 6: Access the Custom URL](#step-6-access-the-custom-url)
- [Budget](#budget)
- [Conclusion](#conclusion)

## Configuration

### Firebase Project Setup

- **Project ID**: Project ID of Google Project
- **Cloud Functions**: Deployed with Firebase's Functions to handle routing and serving of HTML files.
- **Hosting**: Configured to serve static content, rewrites for Cloud Functions, and handle custom domain integration.

### Custom Domain

- **Domain**: [https://domain.name](https://domain.name)
- **DNS Provider**: Any
  - Configured with DNS A record pointing to Firebase Hosting.
  - SSL settings ensure the domain is HTTPS-enabled.

### Cloud Function Setup

- **Function Name**: `helloWorld`
- **Routing**: Handled inside the Cloud Function to serve two HTML files for `/hi` and `/test` routes.

### Firebase Hosting Configuration (`firebase.json`)

```json
{
  "hosting": {
    "public": "public",
    "rewrites": [
      {
        "source": "/hi",
        "function": "helloWorld"
      },
      {
        "source": "/test",
        "function": "helloWorld"
      }
    ]
  }
}
```
## Experiment
***Step 1: Write the Cloud Function***

- Create a file called `index.js` with the following code:
```
exports.helloWorld = (req, res) => {
  if (req.path === '/hi') {
    res.send('Hello World');
  } else if (req.path === '/test') {
    res.send('This is for testing purpose');
  } else {
    res.status(404).send('Not Found');
  }
};
```
- Create a `package.json` file:
```
{
  "dependencies": {
    "@google-cloud/functions-framework": "^3.0.0"
  }
}
```

***Step 2: Deploy the Cloud Function***
Deploy the Cloud Function using Firebase CLI.



***Step 3: Set Up a Custom Domain in Firebase Hosting***
- Install Firebase Tools:
```
sudo npm install -g firebase-tools
```
- Login Into Firebase:
```
firebase login
```
- Initialize Firebase:
```
firebase init
```
- Ensure your directory structure looks like this:
  
```
/project-root
├── firebase.json
└── public/
    ├── index.html
    └── 404.html (optional)

```
- Modify your `firebase.json` to redirect requests from a custom URL to the Cloud Function.


***Step 4: Deploy Firebase Hosting***
After setting up `firebase.json`, deploy the Firebase Hosting configuration:

```
firebase deploy --only hosting
```

***Step 5: Add Custom Domain***

In the Firebase Console, go to Hosting > Add Custom Domain. Verify in Cloudflare by adding a CNAME Record in DNS. After adding the CNAME record, SSL certificates will be managed via Firebase, ensuring HTTPS support.

***Step 6: Access the Custom URL***

Access the following URLs to trigger the Cloud Function:

- [https://domain.name/hi](https://domain.name) - Returns "Hello World"
- [https://domain.name/hello](https://domain.name) - Returns "This is for testing purpose"

### Budget

***Estimate of Costs***
- Google Cloud Functions: Free for up to 2 million requests per month. Costs start at $0.40 per million requests beyond that.
- Firebase Hosting:
   - Storage: $0.026/GB. Estimated price for 10GB: $0.26
   - Data transfer: $0.15/GB. Estimated price for 10GB: $1.5
   - Custom domain & SSL: Free
   - Multiple sites per project: Free
- Total Estimated Budget: $3.15 for one month, assuming minimal usage.

### Additional resources
- [Firebase Hosting Documentation](https://firebase.google.com/docs/hosting)
- [Google Cloud Run Function](https://cloud.google.com/functions?hl=en)












