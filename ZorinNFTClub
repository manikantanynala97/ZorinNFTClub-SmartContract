// SPDX-License-Identifier: MIT
pragma solidity >=0.7.0 <0.9.0;

contract PersonAC {
    // Variables
    enum State {
        NOT_INITIATED,
        AWAITING_DEPOSIT,
        DEPOSITED,
        TRANSFERED
    }

    State public currentState;

 
    bool public isPersonAAgree;
    bool public isPersonCAgree;
    

    uint256 public amount;

    address public owner ;
    address public personA;
    address  public personC;

    // Modifiers
    modifier onlyPersonA() {
        require(msg.sender == personA, "Only person A can call this function");
        _;
    }

    modifier onlyPersonC() {
        require(msg.sender == personC, "Only person C can call this function");
        _;
    }

    // Functions
    constructor(address _personA, address  _personC) {
        owner = msg.sender; // or _personA anthing is the same since A is the owner 
        personA = _personA;
        personC = _personC;
    }

    function SetApproveDeposit(bool value) public onlyPersonA
    {
          isPersonAAgree = value;
          if(isPersonAAgree == true)
          {
              currentState = State.AWAITING_DEPOSIT;
          }
    }

    function GetApproveDeposit() public view returns(bool)
    {
          return isPersonAAgree;
    }

    function ApproveFund(address AddressOfC) public view returns(bool)
    {
        require(AddressOfC == personC , "Not the right user who is asking money");
        require(currentState == State.AWAITING_DEPOSIT, "Already deposit");
        return true;
    }

    function RequestedAmountByC(uint256 _amount) public pure returns(uint256) {
        return _amount ;
    }

     receive() external payable 
     {

     }

    function guessAmount(uint256 _amount) public  onlyPersonC {
        amount =_amount;
        isPersonCAgree = true;
        require(GetApproveDeposit() == true , "You dont have the permission to take money from A ");
        bool approved = ApproveFund(msg.sender);
        require(approved == true ,"You are not the person to get the amount or else U have already deposit the amount");
        RequestedAmountByC(_amount);
        require(address(this).balance >= amount , "Not required amount sent by user A");
        payable(msg.sender).transfer(_amount);
        currentState = State.TRANSFERED;
    }

    function withdraw() public  onlyPersonA {
        require(currentState == State.DEPOSITED, "Can't withdraw now");
        require(address(this).balance > 0  , "No amount is there in the account withdraw");
        payable(msg.sender).transfer(amount);
        currentState = State.AWAITING_DEPOSIT;
    }
}
