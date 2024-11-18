# AioBtcRpc

Modern Python3 async [aiohttp](https://github.com/aio-libs/aiohttp)-based JSONRPC client for [Bitcoin Core](https://github.com/bitcoin/bitcoin).  

This library allows the use of any [Bitcoin RPC method](https://developer.bitcoin.org/reference/rpc/) and now includes support for proxy configuration and `rpcwallet` commands.
---

## New Features

- **Proxy Support**: Interact with your Bitcoin node through a proxy server.
- **`rpcwallet` Command Support**: Seamlessly execute RPC wallet-specific commands.

---

## Install
```bash
pip install aiobtcrpc


# Usage
Pretty simple: 

`await BitcoinCoreClient.anybitcoinrpcmethodname(*args_in_order_like_in_the_docs)`
```python
import asyncio
from aiobtcrpc import BitcoinCoreClient, JSONRPCException

async def test():
    # Create client object by providing rpc-server URL to BitcoinCoreClient class
    cli = BitcoinCoreClient(rpc_url="https://USER:PASSWORD@HOST:PORT")

    # Optionally, set a proxy (if required)
    cli.set_proxy(proxy_url="socks5://127.0.0.1:9050")

    # Execute RPC commands
    balance = await cli.getbalance()

    # All float values return as Python `decimal.Decimal` objects
    print(balance)
    # >>> 0.02587228

    # Use wallet-specific RPC commands
    unlock = await cli.walletpassphrase("Your wallet.dat server bitcoin password", 10)

    # Send Bitcoin
    donate_tx = await cli.sendtoaddress("1EWGKaAdof35pdjBnQW9xh7dwRVJkA8vUR", 0.01)
    print(donate_tx)
    # >>> bd38d3e6c7ab8c25e183e818829e1f0e179af12ef418fa6f4f27c76ef77c924

    # Handle errors
    try:
        await cli.walletpassphrase("BadPassword")
    except JSONRPCException as ex:
        print(ex.code, ex.message)
        # >>> -14 Error: The wallet passphrase entered was incorrect.

asyncio.run(test())
```

This project is a fork of the original [AioBtcRpc](https://github.com/biobdeveloper/aiobtcrpc), with minor enhancements for proxy and rpcwallet support.
All credit for the foundational work goes to the original author.
