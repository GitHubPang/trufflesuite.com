---
title: Using the Truffle Dashboard
layout: docs.hbs
---
# Using the Truffle Dashboard

When deploying your smart contracts you need to specify an Ethereum account that has enough funds to cover the transaction fees of the deployment. A popular method of doing this is copy-pasting your mnemmonic phrase to a gitignored `.env` file so that it can be used for e.g. the `@truffle/hdwallet-provider`. But in general it is a bad practice to copy-paste your keys, especially since we have wallets like Metamask that can send transactions for us.

This is why we developed the Truffle Dashboard, an easy way to use your existing Metamask wallet for your deployments and for other transactions that you need to send from a command line context. Because the Truffle Dashboard connects directly to Metamask it is also possible to use it in combination with hardware wallets like Ledger or Trezor.

## Starting a dashboard

To start a Truffle Dashboard, you need to run the `truffle dashboard` command in a separate terminal window.

```
> truffle dashboard [--port <number>] [--host <string>] [--verbose]

Truffle Dashboard running at http://localhost:24012
                             http://192.168.178.21:24012
DashboardProvider RPC endpoint running at http://localhost:24012/rpc
                                          http://192.168.178.21:24012/rpc
```

By default, this starts a dashboard at `http://localhost:24012` and opens the dashboard in a new tab in your default browser. The Dashboard then prompts you to connect your wallet and confirm that you're connected to the right network.

![Connect your wallet to the Truffle Dashboard](/img/docs/truffle/using-the-truffle-dashboard/truffle-dashboard-connect.png)

![Confirm your connected network](/img/docs/truffle/using-the-truffle-dashboard/truffle-dashboard-confirm.png)

The port and host can be configured through command line options, or by configuring them inside your `truffle-config.js`.

```js
module.exports = {
  // ... rest of truffle config

  dashboard: {
    port: 25012,
  }
}
```


## Connecting to the dashboard

To connect to this dashboard window you need to add a network to your `truffle-config.js` that specifies the dashboard's RPC URL.

```js
module.exports = {
  // ... rest of truffle config

  networks: {
    // ... rest of network config
    dashboard: {
      url: "http://localhost:24012/rpc",
      network_id: "*"
    }
  }
};
```

From there you can use this network configuration with your deployments, scripts or console.

```
truffle migrate --network dashboard
truffle console --network dashboard
```

From there every Ethereum RPC request will be forwarded from Truffle to the Truffle Dashboard, where the user can inspect the RPC request and process them with Metamask.

![Truffle Dashboard Transaction](/img/docs/truffle/using-the-truffle-dashboard/truffle-dashboard-transaction.png)

## Usage with non-Truffle tooling

We know that not everyone uses Truffle for their smart contract development, but we believe that the Truffle Dashboard should be accessible to everyone. This is why we developed the Truffle Dashboard to be agnostic of the tools you're using, so it can also be used with other tools such as Hardhat.

When using the Truffle Dashboard with Hardhat, you need to create a network configuration inside your `hardhat.config.js` file that specifies the Truffle Dashboard's RPC URL.

```js
module.exports = {
  // ... rest of hardhat config

  networks: {
    // ... rest of network config

    dashboard: {
      url: "http://localhost:24012/rpc"
    }
  },
};
```

From there it can be used with any Hardhat tasks as well as tools like hardhat-deploy.

```
hardhat deploy --network dashboard
```

