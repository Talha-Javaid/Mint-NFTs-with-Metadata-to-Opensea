# Mint-NFTs-with-Metadata-to-Opensea

## You are going to learn:

```
How to write and modify the smart contract using OpenZeppelin and Remix
Get free Testnet/Rinkeby ETH using Alchemy faucets
Deploy it on the Ethereum Rinkeby testnet blockchain to save on gas fees
Host the NFT tokens metadata on IPFS using Pinata.
Mint an NFT and visualize it on OpenSea
```

## First we are going to write our contract and select the following integrations:

```
Mintable: will create a mint function only callable by privileged accounts
Autoincrement: IDs will automatically assign incremental IDs to your NFTs
Enumerable: will give you access to on-chain Tokens enumeration and functions such as “totalSupply”, not present in the default ERC721 integration URI storage, to associate metadata and images to each of your NFTs
URI Storage: to be able to associate URIs to our NFTs
```

### Now that you've selected the features you want, the Smart Contract should look as follow:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract Alchemy is ERC721, ERC721Enumerable, ERC721URIStorage, Ownable {
    constructor() ERC721("Alchemy", "ALC") {}

    function safeMint(address to, uint256 tokenId, string memory uri)
        public
        onlyOwner
    {
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }

    // The following functions are overrides required by Solidity.

    function _beforeTokenTransfer(address from, address to, uint256 tokenId)
        internal
        override(ERC721, ERC721Enumerable)
    {
        super._beforeTokenTransfer(from, to, tokenId);
    }

    function _burn(uint256 tokenId) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }

    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721Enumerable)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}

```

## Using Remix to Modify the NFT Smart Contract

Starting from the top of the contract, there's the “SPDX-License-Identifier” that specifies the type of license your code will be published under - it’s good practice in web3 applications to keep the code open source as it will ensure trustworthiness.

```
// SPDX-License-Identifier: MIT
```
Then there's the pragma - the version of the compiler you'll want to use to compile the smart contract code. The little “^” symbol, tells the compiler that every version between 0.8.0 to 0.8.9 is suitable to compile our code.

```
pragma solidity ^0.8.4;
```

Then we're importing a bunch of libraries and initializing the smart contract.

```
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
```

We're then initializing the contract, inheriting from all the standards we're importing from the OpenZeppelin repository:

```
contract YourContractName is ERC721, ERC721Enumerable, ERC721URIStorage, Ownable {...}
```

Now that everyone will be able to mint our NFTs, you'll need to avoid people from minting more NFTs than the max number of NFTs in our collection. To do so let's specify the max number of mintable NFTs.

Let’s say want the users to be able to mint up to a total of 10,000 NFTs. To do so, let’s create a new uint256 variable, call it MAX_SUPPLY, and assign it 1000.

```
uint256 MAX_SUPPLY = 1000;
```

Next, let’s move into the safeMint function and add a require statement:

```
require(_tokenIdCounter.current() <= MAX_SUPPLY, "I'm sorry we reached the cap");
```

## Create a Free Alchemy Account

Give your application app and team a name, choose the Rinkeby network and click on create App

Once you'll have completed the onboarding process, we'll be redirected to the dashboard. Click on the application with the name you decided, in this case, “test”, click on the “VIEW KEY” button on the top right corner, and copy the HTTP URL:

## Get Free Rinkeby Test ETH in Your Metamask
Getting Rinkeby Test ETH is super simple, just navigate to Alchemy Faucets, copy the wallet address into the text bar, and click on “Send Me ETH”:

## Compile and Deploy the NFT Smart Contract on the Rinkeby Testnet
Back to Remix, let’s click on the compiler menu on the left-hand side of the page and click on The blue “Compile” button.
Then click on the “Deploy and Run Transactions menu, click on the Environment dropdown menu and select “injected Web3”:
Make sure the Metamask wallet is on the Alchemy Rinkeby network, select the NFT Smart contract from the Contract dropdown menu, and click on Deploy.
A Metamask pop-up window will appear, click on "sign", and proceed to pay the gas fees.
If everything worked as expected, after 10 seconds you should see the contract listed under Deployed Contracts.

Now that the Smart contract is deployed on the Rinkeby testnet, it’s time to mint our NFTs, but first, you'll need to create and upload the metadata on IPFS, let’s understand what we mean by the term “metadata”.

## How to Format Your NFT Metadata
According to the OpenSea documentation, the NFT Metadata should be stored in a .json file and structured as follow:

```
{ 
  "description": "YOUR DESCRIPTION",
  "external_url": "YOUR URL",
  "image": "IMAGE URL",
  "name": "TITLE", 
  "attributes": [
    {
      "trait_type": "Base", 
      "value": "Starfish"
    }, 
    {
      "trait_type": "Eyes", 
      "value": "Big"
    }, 
    {
      "trait_type": "Mouth", 
      "value": "Surprised"
    }, 
    {
      "trait_type": "Level", 
      "value": 5
    }, 
    {
      "trait_type": "Stamina", 
      "value": 1.4
    }, 
    {
      "trait_type": "Personality", 
      "value": "Sad"
    }, 
    {
      "display_type": "boost_number", 
      "trait_type": "Aqua Power", 
      "value": 40
    }, 
    {
      "display_type": "boost_percentage", 
      "trait_type": "Stamina Increase", 
      "value": 10
    }, 
    {
      "display_type": "number", 
      "trait_type": "Generation", 
      "value": 2
    }
  }
  
```

## Creating and Uploading the Metadata on IPFS
First of all, navigate to Pinata and create a new account.
Once logged in, click on the Upload button  on the side menu and upload the image you want to use for your NFT, 
Once uploaded click on it and copy the IPFS Gateway URL
My image can be viewed below:

```
https://gateway.pinata.cloud/ipfs/QmRnV91itVQYckDgvgsdaNF94bjAp2gVojgw5KVgMcNiSE
```

### Using any text editor, upload the JSON code too on pinata
My metadata can be viewed below:

```
https://gateway.pinata.cloud/ipfs/Qma6eprPaqfEMBjXjutKhwsdHoQUhZMfbfhiKB6zt6ZmfF
```

## Mint Your NFT to Opensea
Go back to Remix and in the Deploy & Run Transactions menu, go under “deployed contracts” - and click on the contract we just deployed, it will open up a list of all the methods contained in your Smart contact:

Click on the safeMint method dropdown icon and paste your address and the following string into the uri field:

```
ipfs://\<your\_metadata\_cid>
```

Clicking on transact will create a Metamask popup prompting you to pay the gas fees.

Click on "sign" and go on minting your first NFT!

Wait a couple of seconds and, to make sure the mint went through successfully, copy and paste your address in the balanceOf method input, and run it - it should show you have 1 NFT.

Do the same with the tokenUri method, inserting “0” as the id argument - it should display your tokenURI.

Great! You just minted your first NFT! 🎉


## Visualize Your NFT on OpenSea

Navigate to 
```
testnets.opensea.io
```
and log in with your Metamask wallet. Then click on your profile picture, you should see your newly minted NFT there. If the image is not yet visible, click on it, and click on the “refresh metadata” button.

## Have a Look at my minted NFT at Opensea

```
https://testnets.opensea.io/assets/rinkeby/0x46d3c5d8d0271fa28065882c52b4cf4cdaea4237/2
```

Congratulations, you have successfully created, modified, and deployed your first smart contract. Minted your first NFT, and published your image on IPFS! 🔥

