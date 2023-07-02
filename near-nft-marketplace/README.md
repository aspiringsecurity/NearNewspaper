# NFT marketplace on Near blockchain for Near Newspaper

Near's user content NFT registration module is to enable users to register their citizen IDs as well as their contact address details in a decentralized manner. An individual can be identified by his/her SSN and a contact address by Contact Registry Number both of which for now are integers between 0 and 65535 (16 bit integers). Every SSN or individual is associated to an address of an individual Near account. We are developing our own NFT smart contract from the ground up following Near's NEP-171 standard and the key tutorial shared at the Near github website. We are also logging the identities of the newspaper content owners, their newspaper content assets using exchange of unique identifiers powered by Near.


## Prerequisites
```=bash
git clone repo
cd repo
git switch 6.royalty
yarn build
```

Now that you've cloned and built the contract we can try a few things. 

## Mint An NFT

Once you've created your near wallet go ahead and login to your wallet with your cli and follow the on-screen prompts

```=bash
near login
```

Once your logged in you have to deploy the contract. Make a subaccount with the name of your choosing 

```=bash 
near create-account nft-example.your-account.testnet --masterAccount your-account.testnet --initialBalance 10
```

After you've created your sub account deploy the contract to that sub account, set this variable to your sub account name

```=bash
NFT_CONTRACT_ID=nft-example.your-account.testnet

MAIN_ACCOUNT=your-account.testnet
```

Verify your new variable has the correct value
```=bash
echo $NFT_CONTRACT_ID

echo $MAIN_ACCOUNT
```


### Deploy Your Contract
```=bash
near deploy --accountId $NFT_CONTRACT_ID --wasmFile out/main.wasm
```

### Initialize Your Contract 

```=bash
near call $NFT_CONTRACT_ID new_default_meta '{"owner_id": "'$NFT_CONTRACT_ID'"}' --accountId $NFT_CONTRACT_ID
```

### View Contracts Meta Data

```=bash
near view $NFT_CONTRACT_ID nft_metadata
```
### Minting Token

```bash=
near call $NFT_CONTRACT_ID nft_mint '{"token_id": "token-1", "metadata": {"title": "My Non Fungible Team Token", "description": "The Team Most Certainly Goes :)", "media": "https://bafybeiftczwrtyr3k7a2k4vutd3amkwsmaqyhrdzlhvpt33dyjivufqusq.ipfs.dweb.link/goteam-gif.gif"}, "receiver_id": "'$MAIN_ACCOUNT'"}' --accountId $MAIN_ACCOUNT --amount 0.1
```

After you've minted the token go to wallet.testnet.near.org to `your-account.testnet` and look in the collections tab and check out your new sample NFT! 



## View NFT Information

After you've minted your NFT you can make a view call to get a response containing the `token_id` `owner_id` and the `metadata`

```bash=
near view $NFT_CONTRACT_ID nft_token '{"token_id": "token-1"}'
```

## Transfering NFTs

To transfer an NFT go ahead and make another [testnet wallet account](https://wallet.testnet.near.org).

Then run the following
```bash=
MAIN_ACCOUNT_2=your-second-wallet-account.testnet
```

Verify the correct variable names with this

```=bash
echo $NFT_CONTRACT_ID

echo $MAIN_ACCOUNT

echo $MAIN_ACCOUNT_2
```

To initiate the transfer..

```bash=
near call $NFT_CONTRACT_ID nft_transfer '{"receiver_id": "$MAIN_ACCOUNT_2", "token_id": "token-1", "memo": "Go Team :)"}' --accountId $MAIN_ACCOUNT --depositYocto 1
```

In this call you are depositing 1 yoctoNEAR for security and so that the user will be redirected to the NEAR wallet.

## Errata

Large Changes:
* **2022-06-21**: updated the rust SDK to version 4.0.0. PR found [here](https://github.com/near-examples/nft-tutorial/pull/32)

* **2022-02-12**: updated the enumeration methods `nft_tokens` and `nft_tokens_for_owner` to no longer use any `to_vector` operations to save GAS. In addition, the default limit was changed from 0 to 50. PR found [here](https://github.com/near-examples/nft-tutorial/pull/17). 

Small Changes:
* **2022-02-22**: changed `token_id` parameter type in nft_payout from `String` to `TokenId` for consistency as per pythonicode's suggestion
