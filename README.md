# Amazon Connect Hosted Widget JWT Setup NodeJS

NodeJS local testing setup for Amazon Connect Hosted Widget JWT Authentication ([documentation](https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html#confirm-and-copy-chat-widget-script))

## Specifications

- NodeJS: **v16+**
- JWT Algorithm: **HS256**

## Usage

### Step 1. Generate an Amazon Connect Hosted Widget

- 1a. Follow the documentation to create your widget: https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html
- 1b. Save the JWT Security key generated by Amazon Connect
- 1c. Update the `const MY_SECRET = '<jwtSecret_REPLACE_ME>';` in `app.js`

### Step 2. Update `public/index.html`

- 2a. Copy/paste your widget snippet code

```html
<script type="text/javascript">
  (function(w, d, x, id){
    s=d.createElement('script');
    s.src='https://asdfasdf.cloudfront.net/amazon-connect-chat-interface-client.js';
    s.async=1;
    s.id=id;
    d.getElementsByTagName('head')[0].appendChild(s);
    w[x] =  w[x] || function() { (w[x].ac = w[x].ac || []).push(arguments) };
  })(window, document, 'amazon_connect', '<widgetId_REPLACE_ME>');
  amazon_connect('styles', { iconType: 'CHAT_VOICE', openChat: { color: '#ffffff', backgroundColor: '#123456' }, closeChat: { color: '#ffffff', backgroundColor: '#123456'} });
  amazon_connect('snippetId', '<snippetId_REPLACE_ME>');
<script/>
```

- 2b. Add the `authenticate` callback, and add the widgetId from previous step

```js
amazon_connect('authenticate', function(callback) {
    const widgetId = '<replaceMe>';
    window.fetch(`/token?widgetId=${widgetId}`).then(res => {
      res.json().then(data => {
        console.log(data.data);
        callback(data.data);
      });
    });
});  
```

### Step 3. Run the local JWT server

```
git clone https://github.com/spenlep-amzn/amazon-connect-hosted-widget-jwt-testing-setup-nodejs.git
cd amazon-connect-hosted-widget-jwt-testing-setup-nodejs
npm install
npm run start
# App running on http://localhost:3000
```

### Step 4. Done! Test the Widget

Open http://localhost:3000 in your browser, and connect to an Agent!

---

![Visitors](https://api.visitorbadge.io/api/visitors?path=https%3A%2F%2Fgithub.com%2Fspenlep-amzn%2Famazon-connect-hosted-widget-jwt-testing-setup-nodejs&label=VIEWS&countColor=%23263759)
