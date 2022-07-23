# Supplychain
pragma solidity >=0.4.21 <0.6.0;
struct Turmeric {
    uint turmericId;
    string notes;
    uint vintageYear;
    address farmOwner;
    turmericState state;
    Farm farm;
}
mapping (uint => Turmeric) turmeric;
enum BottleState {Bottled, ForDistributionSold, ShippedRetail, ForSale,  Sold, Shipped, Received, Consumed}
struct TurOilBottle {
    uint sku;
    Turmeric turmeric;
    uint price;
    BottleState bottleState;
    address buyer;
    address bottleOwner;
    string TurOilNotes;
}

event TurmericHarvested(uint turmericId);
event TurmericPressed(uint turmericId);
event TurmericFermented(uint turmericId);
event TurOilBottled(uint sku);
event TurOilBottleForDistributionSold(uint sku);
event TurOilBottleShippedRetail(uint sku);
event TurOilBottleForSale(uint sku);
event TurOilBottleSold(uint sku);
event TurOilBottleShipped(uint sku);
event TurOilBottleReceived(uint sku);

modifier TurOilBottleExists(uint toil) {
    require(bottles[toil].toil > 0, "TurOil Bottle doesn't exist!");
    _;
}
modifier verifyBottleState(uint toil, BottleState bottleState) {
    require(bottles[toil].bottleState == bottleState, "TurOil Bottle State cannot be verified!");
    _;
}

function getBottleInfo(uint _toil) public view 
			TurOilBottleExists(_toil)
			returns (uint toil, uint price, address owner, address buyer, string state, uint turmericId, string notes) {
				toil = _toil;
				price = bottles[_toil].price;
				owner = bottles[_toil].bottleOwner;
				buyer = bottles[_toil].buyer;
				
				if(uint(bottles[_toil].bottleState) == 0) {
					state = "Turmeric Bottled";
				}
				if(uint(bottles[_toil].bottleState) == 1) {
					state = "Turmeric Bottle sold for Distribution";
				}
				if(uint(bottles[_toil].bottleState) == 2) {
					state = "Turmeric Shipped to Retailer";
				}
				if(uint(bottles[_toil].bottleState) == 3) {
					state = "Turmeric Bottle for Sale with Retailer";
				}
				if(uint(bottles[_toil].bottleState) == 4) {
					state = "Turmeric Bottle Sold to Consumer";
				}
				if(uint(bottles[_toil].bottleState) == 5) {
					state = "Turmeric Bottle Shipped to Consumer";
				}
				if(uint(bottles[_toil].bottleState) == 6) {
					state = "Turmeric Bottle Received by Consumer";
				}
				
				turmericId = bottles[_toil].turmerics.turmericsId;
				notes = bottles[_toil].TurOilNotes;
				
			}