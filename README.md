# BLOCK-CREATION-IN-SOLIDITY
BLOCKCHAIN,IOT TRAINER

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleBlockchain {
    // ek block ka structure
    struct Block {
        uint index;            // block number
        uint timestamp;        // banne ka time
        string data;           // transaction/data
        bytes32 previousHash;  // pehle block ka hash
        bytes32 currentHash;   // is block ka hash
    }

    Block[] public blockchain;  // poora blockchain (array of blocks)

    // constructor ke andar genesis block banayenge
    constructor() {
        // genesis block ka hash hum khud bana rahe hain
        blockchain.push(Block(0, block.timestamp, "Genesis Block", 0x0, keccak256(abi.encodePacked("Genesis Block"))));
    }

    // naye block add karne ke liye function
    function addBlock(string memory _data) public {
        Block memory prevBlock = blockchain[blockchain.length - 1];
        uint newIndex = prevBlock.index + 1;

        // naye block ka hash calculate karte hain
        bytes32 newHash = keccak256(abi.encodePacked(newIndex, block.timestamp, _data, prevBlock.currentHash));

        // naye block ko blockchain me push karte hain
        blockchain.push(Block(newIndex, block.timestamp, _data, prevBlock.currentHash, newHash));
    }

    // total blockchain length
    function getBlockCount() public view returns (uint) {
        return blockchain.length;
    }

    // kisi specific block ka data dekhna
    function getBlock(uint _index) public view returns (uint, uint, string memory, bytes32, bytes32) {
        require(_index < blockchain.length, "Block does not exist");
        Block memory b = blockchain[_index];
        return (b.index, b.timestamp, b.data, b.previousHash, b.currentHash);
    }
}






🔹 Step by Step Explanation

struct Block

Ek block ke andar 5 fields rakhe:

index: block number

timestamp: banne ka time

data: koi bhi transaction ya message

previousHash: pehle block ka hash

currentHash: is block ka hash

Genesis Block (constructor me)

Pehla block manually banate hain "Genesis Block" data ke saath.

iska previousHash = 0x0 hota hai.

addBlock() function

Naya block add karta hai.

Pehle last block uthata hai.

Fir new block ka hash calculate karta hai keccak256() se.

Aur blockchain array me push kar deta hai.

getBlockCount() & getBlock()

Blockchain ki length aur specific block ke details dekhne ke liye helper functions.

🔹 Example Run (Remix IDE me)

Remix IDE (https://remix.ethereum.org
) kholo.

Naya file banao → SimpleBlockchain.sol

Code paste karo aur compile karo.

Deploy karo.

Pehle getBlockCount() run karo → 1 milega (Genesis block).

Ab addBlock("Alice pays Bob 10 ETH") call karo.

getBlockCount() run karo → 2.

Ab getBlock(1) call karke dekho → usme aapko Alice pays Bob 10 ETH ke data ke saath hash aur previousHash milega.
