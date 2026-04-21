# Basic-Faucet
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleFaucet {
    uint256 public constant WITHDRAW_AMOUNT = 0.01 ether;
    uint256 public constant LOCK_TIME = 1 days;
    
    mapping(address => uint256) public lastWithdraw;

    function withdraw() public {
        require(block.timestamp >= lastWithdraw[msg.sender] + LOCK_TIME, "Wait 24 hours");
        require(address(this).balance >= WITHDRAW_AMOUNT, "Faucet is empty");

        lastWithdraw[msg.sender] = block.timestamp;
        payable(msg.sender).transfer(WITHDRAW_AMOUNT);
    }

    // Owner có thể nạp tiền vào faucet
    receive() external payable {}
}
