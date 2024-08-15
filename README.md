## How to Solve reCAPTCHA with Node.js | Guide in 2024

![](https://assets.capsolver.com/prod/images/post/2024-08-13/30ba7441-2e36-4383-9926-077f0e1931fc.png)
Ever found yourself stuck trying to prove to a website that you’re not a robot? We’ve all been there. reCAPTCHA, designed to differentiate humans from bots, is a common hurdle for automation enthusiasts. But fear not! With Node.js and CapSolver, you can solve reCAPTCHA challenges efficiently. Let’s dive into this 2024 guide on how to automate reCAPTCHA solving with Node.js.



## What is reCAPTCHA


Before we dive into the code, it’s important to understand what reCAPTCHA is and how it works. reCAPTCHA is a free service designed to protect websites from spam and abuse by presenting challenges that are easy for humans but difficult for bots. There are different types of reCAPTCHA:

1. **reCAPTCHA v2**  

This version requires users to interact, such as clicking on images to verify their identity. There is also an invisible version of reCAPTCHA v2 that does not require user interaction.  
 ![](https://assets.capsolver.com/prod/images/post/2024-08-13/1239b7cb-cdc1-43c2-b034-b3bfe3ded39a.gif)


2. **reCAPTCHA v3**  

This version is entirely invisible. It typically displays a reCAPTCHA icon at the bottom of the page and assigns a score based on user behavior. A higher score indicates a higher likelihood of being a human.  
![](https://assets.capsolver.com/prod/images/post/2024-08-13/26b7e9b8-0fe9-454f-9035-e1f695572a88.png)


To accurately distinguish between these versions, you may need to check specific parameters. You can experience the different versions through the following demos:

- **reCAPTCHA v2**: [Demo](https://www.google.com/recaptcha/api2/demo)
- **reCAPTCHA v2 Invisible**: [Demo](https://www.google.com/recaptcha/api2/demo?invisible=true)
- **reCAPTCHA v3**: [Demo](https://recaptcha-demo.appspot.com/recaptcha-v3-request-scores.php)

> Struggling with the repeated failure to completely solve the irritating captcha?
>
> Discover seamless automatic captcha solving with **Capsolver** AI-powered Auto Web Unblock technology!
>
> Claim Your   <u>**Bonus Code**</u> for top captcha solutions; [CapSolver](https://www.capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=recaptchanodejs): **WEBS**. After redeeming it, you will get an extra 5% bonus after each recharge, Unlimited
> 
> ![](https://assets.capsolver.com/prod/images/post/2024-03-29/fbc29472-886c-45b2-9eb2-2b307f6d9700.png)



## Why Use Node.js?
Before diving into the technicalities of solving reCAPTCHA, it's important to understand why Node.js is an excellent choice for this task:
1. **Asynchronous Nature**: Node.js's non-blocking, event-driven architecture makes it ideal for handling I/O-heavy operations like web scraping and API requests. This means you can perform multiple tasks simultaneously without waiting for each task to complete sequentially.
2. **Rich Ecosystem**: Node.js has a vast ecosystem of libraries and modules available through npm (Node Package Manager). These libraries simplify various aspects of web scraping and automation, such as handling HTTP requests, browser automation, and CAPTCHA solving.
3. **JavaScript Everywhere**: Using Node.js allows you to use JavaScript on both the client and server sides. This unification can simplify your codebase and make it easier to share logic and data between different parts of your application.
4. **Performance**: Node.js is built on the V8 JavaScript engine, known for its high performance and efficient handling of asynchronous operations. This ensures that your scraping tasks are performed quickly and efficiently.


## Solving reCAPTCHA with CapSolver in Node.js

1. **Find the site_key**  

For reCAPTCHA v2, after clicking `I'm not a robot`,  a request similar to `https://www.google.com/recaptcha/api2/reload` is sent, where the value of `k` is the site_key.  
![](https://assets.capsolver.com/prod/images/post/2024-08-13/1fa8977f-d8d2-40cb-a7ee-e184ee8ed59b.png)


2. **Use CapSolver**

Replace the site_key from the first step and the api_key you received after [registering on the CapSolver](https://dashboard.capsolver.com/dashboard/overview/?utm_source=official&utm_medium=blog&utm_campaign=recaptchanodejs) platform into the code below. You will get a token within a few seconds:

```javascript
// npm install axios
const axios = require('axios');

const api_key = "YOUR_API_KEY";
const site_key = "6Le-wvkSAAAAAPBMRTvw0Q4Muexq9bi0DJwx_mJ-";
const site_url = "https://www.google.com/recaptcha/api2/demo";

async function capsolver() {
  const payload = {
    clientKey: api_key,
    task: {
      type: 'ReCaptchaV2TaskProxyLess',
      websiteKey: site_key,
      websiteURL: site_url
    }
  };

  try {
    const res = await axios.post("https://api.capsolver.com/createTask", payload);
    const task_id = res.data.taskId;
    if (!task_id) {
      console.log("Failed to create task:", res.data);
      return;
    }
    console.log("Got taskId:", task_id);

    while (true) {
      await new Promise(resolve => setTimeout(resolve, 1000)); // Delay for 1 second

      const getResultPayload = {clientKey: api_key, taskId: task_id};
      const resp = await axios.post("https://api.capsolver.com/getTaskResult", getResultPayload);
      const status = resp.data.status;

      if (status === "ready") {
        return resp.data.solution.gRecaptchaResponse;
      }
      if (status === "failed" || resp.data.errorId) {
        console.log("Solve failed! response:", resp.data);
        return;
      }
    }
  } catch (error) {
    console.error("Error:", error);
  }
}

capsolver().then(token => {
  console.log(token);
});
```

CapSolver supports solving both [reCAPTCHA v2](https://docs.capsolver.com/guide/captcha/ReCaptchaV2.html?utm_source=official&utm_medium=blog&utm_campaign=recaptchanodejs) and [reCAPTCHA v3](https://docs.capsolver.com/guide/captcha/ReCaptchaV3.html?utm_source=official&utm_medium=blog&utm_campaign=recaptchanodejs). The official documentation provides detailed code examples, making it easy to obtain a token within seconds with minimal steps!


## Conclusion
Dealing with reCAPTCHA doesn’t have to be a hassle. With Node.js and CapSolver, you can automate and simplify this process, saving time and boosting efficiency. Give it a shot and watch your productivity soar. Here’s to fewer CAPTCHA headaches and more time focusing on what truly matters!

## Note on Compliance

> **Important:** When engaging in web scraping, it's crucial to adhere to legal and ethical guidelines. Always ensure that you have permission to scrape the target website, and respect the site's `robots.txt` file and terms of service. **CapSolver firmly opposes the misuse of our services for any non-compliant activities**. Misuse of automated tools to bypass CAPTCHAs without proper authorization can lead to legal consequences. Make sure your scraping activities are compliant with all applicable laws and regulations to avoid potential issues.
