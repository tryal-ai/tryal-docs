# Getting Started

## Signing up and Registering as a Developer

Before we begin, you'll need to:

- [register](https://app.tryal.ai/signup) an account with our online platform.

- activate your developer account by going to [accounts](https://app.tryal.ai/account).

<aside class="notice">
  If you already have an active user subscription, you cannot activate developer mode. Please
  contact us if you have this issue.
</aside>

## Subscribing and Billing

- Subscribe for the subscription tier that best suits your needs
    - **Basic** - 160,000 Marks per Month, +0.07p per mark after that - *£49*
    - **Standard** - 400,000 Marks per Month, +0.06p per mark after that - *£99*
    - **Advanced** - 800,000 Marks per Month, +0.05p per mark after that - *£199*

All plans are monthly, and allow you to cancel at anytime. When generating material, your usage
is typically equivalent to the number of marks you are generating. As standard GCSE papers 
are between 80 and 100 marks, the basic tier includes between 1,600 to 2,000 papers per month.

<aside class="notice">
  When generating individual questions a minimum billing fee of 5 marks applies, more details
  are included on the <a href="#post-questions-create"><span class="post">post</span> /questions/create</a> documentation section
</aside>

## Setting up an Application

- Create a new application on the [Develop](https://app.tryal.ai/develop) page and give
  it an appropriate name
- Make a note of your application ID. We'll refer to this as `<tryal-app-id>` throughout, where
you need to use it.
- Generate a new application token and save it [**somewhere secure**](https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa), we'll refer to this
as `<tryal-app-token>`.

<aside class="warning">
  This token is a secret, and can only be generated once. Sharing an application token allows someone to make API calls on your behalf that are billed to your account
</aside>

Your application ID and Application token serve as a way to identify your application with our
server without the need for you to use your password. 

For your security, application ID and tokens are scoped. This means that all 
data you can access regarding usage, existing material etc. are only for that application, 
and not for the developer account as a whole. 

You can still access your overall usage through our [usage dashboard](https://app.tryal.ai/usage).

