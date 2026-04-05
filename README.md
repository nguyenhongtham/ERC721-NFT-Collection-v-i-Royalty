# ERC721-NFT-Collection-v-i-Royalty
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/common/ERC2981.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract BaseNFT is ERC721, ERC2981, Ownable {
    uint256 public nextTokenId;
    string public baseURI;

    constructor(string memory name, string memory symbol) 
        ERC721(name, symbol) {
        _setDefaultRoyalty(msg.sender, 500); // 5%
    }

    function mint(address to) public onlyOwner {
        _safeMint(to, nextTokenId);
        nextTokenId++;
    }

    function setBaseURI(string memory _uri) external onlyOwner {
        baseURI = _uri;
    }

    function _baseURI() internal view override returns (string memory) {
        return baseURI;
    }

    function supportsInterface(bytes4 interfaceId) 
        public view override(ERC721, ERC2981) returns (bool) {
        return super.supportsInterface(interfaceId);
    }
}
