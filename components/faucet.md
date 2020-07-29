---
description: This page describes the Kaspa Faucet for dropping Kaspa.
---

# Faucet

Faucet is an open source Kaspa faucet that sends Kaspa to anyone requesting. It has a defined address, where Kaspa is mined into. Requests to the faucet are passed via https POST. If the faucet address has enough Kaspa, it will send it. Each request yields a fixed amount, and requests are limited to one request per IP address per 24 hours.

If you need to modify the faucet parameters for development purposes, you can get the code on [GitHub](https://github.com/kaspanet/faucet) and modify it.

## Requesting Kaspa

To request Kaspa, make an https POST request to the faucet address on port 8081 to /request\_money using [curl](https://curl.haxx.se/).

```bash
curl https://[faucet_address]/request_money
     -X POST
     -H "Content-Type: application/json"
     -d '{"address":"your_public_kaspa_address"}'
```

For example:

```bash
curl https://testnet-faucet.kas.pa/request_money
     -X POST
     -H "Content-Type: application/json"
     -d '{"address":"kaspatest:pr6m7j9njldwwzlg9v7v53unlr4jkmx6eyvwc0uz5t"}'
```

Reply:

```bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100    66  100    66     40     40  0:00:01  0:00:01 --:--:--    80
"55ab99a7badb5a4cc51c4b4022fc2c20f10ee094e01435630bb473c5fa7564c5"
```

If you request more than once in 24 hours from the same IP, you will get the following error:

```bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   172  100   106  100    66    319    198 --:--:-- --:--:-- --:--:--   519
{"errorCode":403,"errorMessage":"A user is allowed to to have one request from the faucet every 24 hours"}
```

You may request Kaspa from one of the community faucets, listed below.

## List of Kaspa Testnet Faucets

<table>
  <thead>
    <tr>
      <th style="text-align:left">Faucet URL</th>
      <th style="text-align:left">Limits</th>
      <th style="text-align:left">Address for Contributions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="https://testnet-faucet.kas.pa">https://testnet-faucet.kas.pa</a>
      </td>
      <td style="text-align:left">
        <p>KAS 0.0001</p>
        <p>per IP address</p>
        <p>per 24 hours</p>
      </td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

To list your faucet, drop a message on [Discord](https://discord.gg/WmGhhzk).

## Contribute

If you want to run a community faucet, contact us on [Discord](https://discord.gg/WmGhhzk).

If you want to contribute to the faucet codebase, use the [integrated GitHub issue tracker](https://github.com/kaspanet/faucet/issues).

