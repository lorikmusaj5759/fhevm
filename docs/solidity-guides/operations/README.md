// Optimized example usage of FHE encrypted types and operations

euint8 age = FHE.asEuint8(25);
euint8 percentage = FHE.asEuint8(75);

function mint(externalEuint32 encryptedAmount, bytes calldata inputProof) public {
  euint32 mintedAmount = FHE.asEuint32(encryptedAmount, inputProof);
  euint32 tempTotalSupply = FHE.add(totalSupply, mintedAmount);
  ebool isOverflow = FHE.lt(tempTotalSupply, totalSupply);
  totalSupply = FHE.select(isOverflow, totalSupply, tempTotalSupply);

  euint32 tempBalanceOf = FHE.add(balances[msg.sender], mintedAmount);
  balances[msg.sender] = FHE.select(isOverflow, balances[msg.sender], tempBalanceOf);

  FHE.allowThis(balances[msg.sender]);
  FHE.allow(balances[msg.sender], msg.sender);
}
