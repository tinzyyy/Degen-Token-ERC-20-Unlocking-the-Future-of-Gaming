# Degen-Token-ERC-20-Unlocking-the-Future-of-Gaming

This Solidity smart contract, named DegenToken, is developed to create a unique token that can reward players and take their game to the next level. It enables players to earn tokens in the game and exchange them for rewards in the in-game store, thereby enhancing player loyalty and retention. The token is deployed on the Ethereum blockchain via Remix IDE, providing fast and low-fee transactions, and facilitating seamless trading and utilization within the gaming ecosystem.

### Requirements 

1. Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
2. Setting items: The owner should be able to set items in the in-game store along with their prices.
3. Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
4. Burning tokens: Players should be able to burn tokens they own, which are no longer needed.

### Components and Functionality
`Minting new tokens:` The contract allows the owner to mint new tokens and distribute them to players as rewards. The mint function is accessible only by the contract owner.

`Setting items:` The owner can set items in the in-game store along with their prices using the setItem function.

`Redeeming tokens:` Players can redeem their tokens for items in the in-game store using the redeem function. The contract checks if the player has enough tokens to redeem the item.

`Burning tokens:` Players can burn tokens they own, which are no longer needed, using the burn function.

`Transferring tokens:` Players can transfer their tokens to other addresses using the standard ERC-20 transfer function.

`Checking token balance:` Players can check their token balance at any time using any Ethereum-compatible wallet or by querying the contract directly.

## Modifier
`onlyOwner:` This modifier restricts certain functions to be callable only by the contract owner.

### Challenges
As I delved into developing the DegenToken ERC-20 contract, I faced several challenges worth nothing. Here are some common hurdles I encountered and how I navigated through them:

- `Understanding the ERC-20 Standard:` Initially, I spent time acquainting myself with the ERC-20 standard to ensure that my contract aligns with the requirements for compatibility and functionality across various platforms. Resources like the official Ethereum documentation were instrumental in this process.

-`Testing on Avalanche Testnets:` While I intended to test the contract on Avalanche testnets like Fuji or Avalanche, I encountered difficulties connecting Remix IDE to these networks. As a workaround, I opted to focus on using Remix IDE for contract compilation and deployment.

Note:
As I embarked on this journey, I encountered difficulties connecting Remix IDE to the Avalanche testnet and acquiring AVAX funds for testing purposes in MetaMask. Consequently, I decided to focus on using Remix IDE for compiling and deploying my contract.


### Getting Started

To interact with this contract, you can deploy it on Remix Ethereum IDE or any Ethereum-compatible blockchain.
1. Open Remix Ethereum IDE.
2. Create a new file and paste the provided Solidity code.
3. Compile the code.
4. Deploy the contract.
5. Interact with the deployed contract by calling its functions.

Solidity
  ``` Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenToken is ERC20, Ownable {

    event TokensMinted(address indexed to, uint256 amount);
    event TokensRedeemed(address indexed from, uint256 amount, string itemName);
    event TokensBurned(address indexed from, uint256 amount);
    event ItemSet(string itemName, uint256 price);

    struct Item {
        uint256 price;
        bool exists;
    }

    mapping(string => Item) public items;

    constructor(address initialOwner) ERC20("DegenToken", "DGN") Ownable(initialOwner) {
       
    }

    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
        emit TokensMinted(to, amount);
    }

    function setItem(string memory itemName, uint256 price) external onlyOwner {
        require(price > 0, "Price must be greater than zero");
        items[itemName] = Item(price, true);
        emit ItemSet(itemName, price);
    }

    function redeem(string memory itemName) external {
        Item memory item = items[itemName];
        require(item.exists, "Item does not exist");
        require(balanceOf(msg.sender) >= item.price, "Insufficient balance");
        
        _burn(msg.sender, item.price);
        emit TokensRedeemed(msg.sender, item.price, itemName);
    }

    function burn(uint256 amount) external {
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");
        _burn(msg.sender, amount);
        emit TokensBurned(msg.sender, amount);
    }

    function transfer(address to, uint256 token) public override returns (bool) {
        _transfer(msg.sender, to, token);
        return true;
    }
}

```

This is the whole functionality within the code itself and the generalization of how the code will run.

### Authors

Baquiran, Kristine Mae P. 
8215029@ntc.edu.ph



