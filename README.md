# Token-with-cooldown-between-transfers
Token with cooldown between transfers
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract CooldownToken is ERC20 {
    mapping(address => uint256) public lastTransfer;

    constructor() ERC20("CooldownToken", "CDT") {
        _mint(msg.sender, 1000 ether);
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal override {
        if (from != address(0)) {
            require(block.timestamp - lastTransfer[from] >= 60, "Cooldown 60s");
            lastTransfer[from] = block.timestamp;
        }
        super._beforeTokenTransfer(from, to, amount);
    }
}
