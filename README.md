# ICO-SMART-CONTRACT
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";

contract Rubeluchukwuisi is ERC20, ERC20Burnable, Ownable {
    constructor() ERC20("Rubeluchukwuisi", "BUNN") {
        _mint(msg.sender, 1000000000000000000000 * 10 ** decimals());
    }

    // Contract ICO
    Contract ICO 
}
    using SafeMath for uint256;

    address public owner;
    uint256 public tokenPrice = 0.01 ether;
    uint256 public registrationStartTime; 
    uint256 public registrationEndTime;   
    bool public isICOEnded = false;      
    mapping(address => uint256) public registeredUsers;
    mapping(address => bool) public hasClaimed;


    // ERC-20 actual Token Contract 
    IERC20 public tokenContract;

    event Registered(address indexed user, uint256 amount);
    constructor(address _tokenContract, uint256 _registrationDuration) {
        owner = msg.sender;
        tokenContract = IERC20(_tokenContract);
        registrationStartTime = block.timestamp;
        registrationEndTime = registrationStartTime+_registrationDuration;
    }
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function.");
        _;
    }

    modifier ICOEnded() {
        require(block.timestamp >= ICOEndTime, "ICO has not ended yet.");
        _;
    }

    modifier hasNotClaimed() {
        require(!hasClaimed[msg.sender], "You have already claimed your tokens.");
        _;
    }


    modifier notICOEnded() {
        require(!isICOEnded, "ICO has ended");
        _;
    }
    uint256 public constant ICO_DURATION = 86400 seconds;

    function register() public payable notICOEnded {
        require(msg.value >= tokenPrice, "Insufficient ETH sent");
        require(registeredUsers[msg.sender] == 0, "You are already registered");

        uint256 tokensToBuy = msg.value.div(tokenPrice);

        tokenContract.transfer(msg.sender, tokensToBuy);

        
        registeredUsers[msg.sender] = msg.value;

        emit Registered(msg.sender, msg.value);
    }

    function endICO() public onlyOwner {
        isICOEnded = true;
    }

    
    function withdrawExcessEth() public onlyOwner {
        require(address(this).balance > 0, "No excess ETH to withdraw");
        owner.transfer(address(this).balance);
    }
{
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }
}


interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
}

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);

    uint256 amountToTransfer = 50 * (10**18); 
    token.transfer(recipient, amountToTransfer);
    }




interface IICO {
    event registered(address indexed _address, bool status);
    event claimed(address indexed _address, bool status);

    function register() external payable;
    function claim(address _address) external returns(bool);
}

  interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

}
contract Rubeluchukwuisi is IERC20 {
    string public name = "Rubeluchukwuisi";
    string public symbol = "BUNN";
    uint8 public decimals = 18;
    uint256 private _totalSupply;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
}    
{
    address public owner; // Add owner state variable
    constructor(uint256 initialSupply) {
        owner = msg.sender; // Set the contract deployer as the owner
        _totalSupply = initialSupply * 10 ** uint256(decimals);
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }
}

    function burn(uint256 amount) public {
        require(msg.sender == owner, "Only the owner can burn tokens");
        require(amount > 0, "Amount to burn must be greater than 0");
        require(_balances[msg.sender] >= amount, "Not enough balance to burn");

        _balances[msg.sender] -= amount;
        _totalSupply -= amount; // Deduct the amount from the total supply
        _balances[msg.sender] -= amount; // Deduct the amount from the owner's balance

        emit Transfer(msg.sender, address(0), amount);
    }


