- Frontend:
  - Next.js
  - [react-stripe-js](https://github.com/stripe/react-stripe-js) for [Checkout](https://stripe.com/checkout) and [Elements](https://stripe.com/elements)
- Backend
  - Next.js [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers) and [Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/forms-and-mutations)
  - [stripe-node with TypeScript](https://github.com/stripe/stripe-node#usage-with-typescript)


## Included functionality

- Stripe Checkout
  - Custom Amount Donation with redirect to Stripe Checkout:
    - Server Component: [app/donate-with-checkout/page.tsx](app/donate-with-checkout/page.tsx)
    - Server Action: [app/actions/stripe.ts](app/actions/stripe.ts)
    - Checkout Session 'success' page fetches the Checkout Session object from Stripe: [donate-with-checkout/result/page.tsx](app/donate-with-checkout/result/page.tsx)
- Stripe Elements
  - Custom Amount Donation with Stripe Payment Element & PaymentIntents:
    - Server Component: [app/donate-with-elements/page.tsx](app/donate-with-elements/page.tsx)
    - Server Action: [app/actions/stripe.ts](app/actions/stripe.ts)
    - Payment Intent 'success' page (via `returl_url`) fetches the Payment Intent object from Stripe: [donate-with-elements/result/page.tsx](app/donate-with-elements/result/page.tsx)
- Webhook handling for [post-payment events](https://stripe.com/docs/payments/handling-payment-events)
  - Route Handler: [app/api/webhooks/route.ts](app/api/webhooks/route.ts)

## How to use

example:

```bash
npx create-next-app --example with-stripe-typescript with-stripe-typescript-app
```

```bash
yarn create next-app --example with-stripe-typescript with-stripe-typescript-app
```

```bash
pnpm create next-app --example with-stripe-typescript with-stripe-typescript-app
```

### Required configuration

Copy the `.env.local.example` file into a file named `.env.local` in the root directory of this project:

```bash
cp .env.local.example .env.local
```

You will need a Stripe account ([register](https://dashboard.stripe.com/register)) to run this sample. Go to the Stripe [developer dashboard](https://dashboard.stripe.com/apikeys) to find your API keys and replace them in the `.env.local` file.

```bash
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=<replace-with-your-publishable-key>
STRIPE_SECRET_KEY=<replace-with-your-secret-key>
```

Now install the dependencies and start the development server.

```bash
npm install
npm run dev
# or
yarn
yarn dev
```

### Forward webhooks to your local dev server

First [install the CLI](https://stripe.com/docs/stripe-cli) and [link your Stripe account](https://stripe.com/docs/stripe-cli#link-account).

Next, start the webhook forwarding:

```bash
stripe listen --forward-to localhost:3000/api/webhooks
```

The CLI will print a webhook secret key to the console. Set `STRIPE_WEBHOOK_SECRET` to this value in your `.env.local` file.

### Setting up a live webhook endpoint

After deploying, copy the deployment URL with the webhook path (`https://your-url.vercel.app/api/webhooks`) and create a live webhook endpoint [in your Stripe dashboard](https://stripe.com/docs/webhooks/setup#configure-webhook-settings).

Once created, you can click to reveal your webhook's signing secret. Copy the webhook secret (`whsec_***`) and add it as a new environment variable in your [Vercel Dashboard](https://vercel.com/dashboard):

- Select your newly created project.
- Navigate to the Settings tab.
- In the general settings scroll to the "Environment Variables" section.

After adding an environment variable you will need to rebuild your project for it to become within your code. Within your project Dashboard, navigate to the "Deployments" tab, select the most recent deployment, click the overflow menu button (next to the "Visit" button) and select "Redeploy".


