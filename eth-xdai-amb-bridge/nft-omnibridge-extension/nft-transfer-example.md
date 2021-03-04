---
description: Use the NFT OmniBridge extension to transfer NFTs between xDai and Ethereum
---

# NFT Transfer Example

The NFT extension is operational, and a UI to transfer NFTs \(ERC721s\) is currently in development. For now, users can access and write to contracts using BlockScout and Etherscan. In the following example, we will bridge an NFT from xDai to Ethereum and back using methods accessed through these block explorers.

{% hint style="warning" %}
The NFT Extension is in Beta and transfers are at your own risk. NFT transfers can be very expensive and are not reversible once you initiate a transfer. Keep this in mind when deciding whether or not to bridge NFTs between xDai and Ethereum.
{% endhint %}

## xDai to Ethereum

Minting NFTs on xDai is inexpensive. These NFTs can then be bridged to Ethereum with the NFT OmniBridge extension. The process consists of several steps.

1. Locate your NFT contract and tokenID.
2. Approve the mediator contract on xDai to transfer your NFT.
3. Initiate a request to transfer.
4. Finalize the transfer request on Ethereum.

{% hint style="info" %}
Token contracts must be verified in BlockScout to access write operations. If your token contract is not yet verified, [follow these steps for BlockScout](https://docs.blockscout.com/for-users/smart-contract-interaction/verifying-a-smart-contract).
{% endhint %}

## 1\) Locate your NFT 

You will need the contract and token ID for your NFT to start. If you have trouble locating, you can enter your wallet address in the BlockScout search bar and go to the **Tokens** tab. Here you will see your ERC-721s.  Click on the contract to find more information including:

1. **ERC721 Contract Address**
2. **TokenID** you wish to transfer.

![](../../.gitbook/assets/nftbridge1.png)

![](../../.gitbook/assets/nft2.png)

## 2\) Approve Contract to Transfer

ERC721`transferFrom` functionality allows the contract to transfer your token and must be approved. 

1. Go to the token contract page and click on the Write Contract \(or Write as Proxy if using a Proxy contract\). 
2. Press the **Connect to MetaMask** button to connect the address that holds the NFT
3. In the `approve` method, add the following:
   1. to \(address\): [`0x80199C8D04Af4c5cEB532adF4463b18BB4B59ffC`](https://blockscout.com/poa/xdai/address/0x80199C8D04Af4c5cEB532adF4463b18BB4B59ffC)  _This is the xDai mediator contract._
   2. tokenId \(uint256\): TokenID for your ERC721 _May vary in size depending on how ids are created for this NFT._ 
4.  Click the **Write** button and confirm in MetaMask.

![](../../.gitbook/assets/nft3.png)

## 3\) Initiate theTransfer

Open the mediator contract \([`0x80199C8D04Af4c5cEB532adF4463b18BB4B59ffC`](https://blockscout.com/poa/xdai/address/0x80199C8D04Af4c5cEB532adF4463b18BB4B59ffC) \)  in BlockScout and go to the "Write Proxy" tab. Metamask should still be connected.

![](../../.gitbook/assets/nft5.png)

1. Scroll to the `relayToken` method and enter the NFT details from step 1.
   1. token \(address\) field: the ERC721 token contract address
   2. tokenId \(uint256\) field: the id of the token to transfer
2. Click the **Write** button and confirm in MetaMask.

![](../../.gitbook/assets/nft6.png)

## 4\) Finalize the Transfer on Ethereum

The Arbitrary Message Bridge oracles will use the xDai chain to collect confirmations of the NFT token transfer. As soon as the confirmations are collected, they will be relayed to Ethereum for finalization.

1\) Find and copy the transaction hash id of the previous transaction \(`relayToken`\). You can find this in your MetaMask wallet in the Activity section. Click on the Contract Interaction and the Details icon to copy the tx hash.

![](../../.gitbook/assets/nft7.png)

2\) Go to the the ALM Monitoring tool here: [https://alm-xdai.herokuapp.com/](https://alm-xdai.herokuapp.com/) and paste in the tx hash.

![](../../.gitbook/assets/nftalm1.png)

You can follow the status on the monitor. Once 4 validators are successful you can execute the transaction.

![](../../.gitbook/assets/nftalm2.png)

3\) Switch to **Ethereum Mainnet in MetaMask** and press the **Execute** button**.** Confirm the transaction in MetaMask**.**This will send the transaction to the Arbitrary Message Bridge contract with collected confirmations of the AMB oracles.

![](../../.gitbook/assets/allmy.png)

{% hint style="info" %}
Gas fees to transfer may be prohibitively expensive. You can try return later when fees are lower and execute then. There is no time limit for the transfer.
{% endhint %}

Once the transaction is verified and included in a block, the token will be transferred to the same account on Ethereum that called the `relayTokens` method.

## Ethereum to xDai

_Coming soon. The process is similar to the above using Etherscan rather than BlockScout to write transactions. You will not need to manually execute on the xDai side, this process is automated when bridging NFTs from Ethereum to xDai._





