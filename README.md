// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ERTCSmartContract {

    // Owner of the contract
    address public owner;

    // Employer structure
    struct Employer {
        uint256 employerId;
        string name;
        bool isEligible;
        uint256 totalCredit;
    }

    // Mapping of employer ID to Employer details
    mapping(uint256 => Employer) public employers;

    // Event logs for transparency
    event EmployerAdded(uint256 employerId, string name);
    event CreditCalculated(uint256 employerId, uint256 amount);
    event CreditDisbursed(uint256 employerId, uint256 amount);

    // Modifier to restrict access to owner only
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    // Constructor to set contract owner
    constructor() {
        owner = msg.sender;
    }

    // Add an employer
    function addEmployer(uint256 _employerId, string memory _name, bool _isEligible) public onlyOwner {
        require(employers[_employerId].employerId == 0, "Employer already exists");

        employers[_employerId] = Employer({
            employerId: _employerId,
            name: _name,
            isEligible: _isEligible,
            totalCredit: 0
        });

        emit EmployerAdded(_employerId, _name);
    }

    // Calculate and record ERTC for an employer
    function calculateCredit(uint256 _employerId, uint256 _creditAmount) public onlyOwner {
        Employer storage employer = employers[_employerId];
        require(employer.isEligible, "Employer is not eligible for ERTC");

        employer.totalCredit += _creditAmount;
        emit CreditCalculated(_employerId, _creditAmount);
    }

    // Disburse the credit to the employer (mock disbursement)
    function disburseCredit(uint256 _employerId) public onlyOwner {
        Employer storage employer = employers[_employerId];
        require(employer.isEligible, "Employer is not eligible for ERTC");
        require(employer.totalCredit > 0, "No credit available to disburse");

        uint256 creditToDisburse = employer.totalCredit;
        a.totalCredit = 0;

        // Logic to transfer credit (e.g., ERC20 tokens) can be added here

        emit CreditDisbursed(_employerId, creditToDisburse);
    }

    // View employer details
    function getEmployer(uint256 _employerId) public view returns (Employer memory) {
        return employers[_employerId];
    }
}
