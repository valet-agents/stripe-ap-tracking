# Stripe AP Tracking

Vendor invoices, due dates, and payment confirmations all post to one channel — past-due amounts, weird totals, and duplicates get called out.

## Prerequisites
- A [Stripe](https://stripe.com) account with a Restricted Key that has read access to Invoices and Customers (a live secret key works too, but the restricted key is strongly recommended)
- A Slack workspace where you can install the agent's bot and invite it to one or more channels

<table>
  <tr>
    <td><strong>CHANNELS</strong></td>
    <td><code>slack</code> · <code>cron</code> — 8am PT weekdays</td>
  </tr>
  <tr>
    <td><strong>CONNECTORS</strong></td>
    <td><code>stripe-mcp</code></td>
  </tr>
  <tr>
    <td colspan="2" align="center">
      <br />
      <a href="https://valet.dev/deploy?from=github.com/valet-agents/stripe-ap-tracking">
        <img src="https://raw.githubusercontent.com/valet-agents/stripe-ap-tracking/main/.github/deploy-button.svg" alt="Deploy Agent →" height="40" />
      </a>
      <br /><br />
    </td>
  </tr>
</table>
