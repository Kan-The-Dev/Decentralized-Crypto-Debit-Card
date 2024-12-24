// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract CryptoDebitCard {
    address public owner;
    mapping(address => bool) public approvedWallets;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not authorized");
        _;
    }

    event PaymentProcessed(address indexed user, uint256 amount, string merchant);

    constructor() {
        owner = msg.sender;
    }

    function approveWallet(address wallet) external onlyOwner {
        approvedWallets[wallet] = true;
    }

    function revokeWallet(address wallet) external onlyOwner {
        approvedWallets[wallet] = false;
    }

    function processPayment(address token, uint256 amount, string calldata merchant) external {
        require(approvedWallets[msg.sender], "Wallet not approved");
        IERC20(token).transferFrom(msg.sender, address(this), amount);
        emit PaymentProcessed(msg.sender, amount, merchant);
    }

    function withdrawFunds(address token, uint256 amount) external onlyOwner {
        IERC20(token).transferFrom(address(this), owner, amount);
    }
}
