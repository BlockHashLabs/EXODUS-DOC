# Equations

## Staking

$$
deposit = withdrawal
$$

Swaps between EXO and sEXO during staking and unstaking are always honored 1:1. The amount of EXO deposited into the staking contract will always result in the same amount of sEXO. And the amount of sEXO withdrawn from the staking contract will always result in the same amount of EXO.

$$
rebase = 1 - ( exoDeposits / sEXOOutstanding )
$$

The treasury deposits EX into the distributor. The distributor then deposits EXO into the staking contract, creating an imbalance between EXO and sEXO. sEXO is rebased to correct this imbalance between EXO deposited and sEXO outstanding. The rebase brings sEXO outstanding back up to parity so that 1 sEXO equals 1 staked EXO.

## Bonding

$$
bond Price = 1 + Premium
$$

EXO has an intrinsic value of 1 DAI, which is roughly equivalent to $1. In order to make a profit from bonding, EXODIA charges a premium for each bond.

$$
Premium = debt Ratio * BCV
$$

The premium is derived from the debt ratio of the system and a scaling variable called [BCV](glossary.md#bcv). BCV allows us to control the rate at which bond prices increase.

The premium determines profit due to the protocol and in turn, stakers. This is because new EXO is minted from the profit and subsequently distributed among all stakers.

$$
debt Ratio = bondsOutstanding/exodSupply
$$

The debt ratio is the total of all EXO promised to bonders divided by the total supply of EXO. This allows us to measure the debt of the system.

$$
bondPayout_{reserveBond} = marketValue_{asset}\ /\ bondPrice
$$

Bond payout determines the number of EXO sold to a bonder. For reserve bonds, the market value of the assets supplied by the bonder is used to determine the bond payout. For example, if a user supplies 1000 DAI and the bond price is 250 DAI, the user will be entitled 4 EXO.

$$
bondPayout_{lpBond} = marketValue_{lpToken}\ /\ bondPrice
$$

For liquidity bonds, the market value of the LP tokens supplied by the bonder is used to determine the bond payout. For example, if a user supplies 0.001 EXO-DAI LP token which is valued at 1000 DAI at the time of bonding, and the bond price is 250 DAI, the user will be entitled 4 EXO.

## EXO Supply

$$
EXO_{supplyGrowth} = EXO_{stakers} + EXO_{bonders} + EXO_{DAO}
$$

EXO supply does not have a hard cap. Its supply increases when:

* EXO is minted and distributed to the stakers.
* EXO is minted for the bonder. This happens whenever someone purchases a bond.
* EXO is minted for the DAO. This happens whenever someone purchases a bond. The DAO gets the same number of EXO as the bonder.

$$
EXO_{stakers} = EXO_{totalSupply} * rewardRate
$$

At the end of each epoch, the treasury mints EXO at a set reward rate. These EXO will be distributed to all the stakers in the protocol.

$$
EXO_{bonders} = bondPayout
$$

Whenever someone purchases a bond, a set number of EXO is minted. These EXO will not be released to the bonder all at once - they are vested to the bonder linearly over time. The bond payout uses a different formula for different types of bonds. Check the bonding section above to see how it is calculated.

$$
EXO_{DAO} = EXO_{bonders}
$$

The DAO receives the same amount of EXO as the bonder. This represents the DAO profit.

## Backing per EXO

$$
EXO_{backing} = treasuryBalance_{stablecoin} + treasuryBalance_{otherAssets}
$$

Every EXO in circulation is backed by the EXODIA treasury. The assets in the treasury can be divided into two categories: stablecoin and non-stablecoin.

$$
treasuryBalance_{stablecoin} = RFV_{reserveBond} + RFV_{lpBond}
$$

The stablecoin balance in the treasury grows when bonds are sold. [RFV](glossary.md#rfv) is calculated differently for different bond types.

$$
RFV_{reserveBond} = assetSupplied
$$

For reserve bonds such as DAI bond, the RFV simply equals to the amount of the underlying asset supplied by the bonder.

$$
RFV_{lpBond} = 2sqrt(constantProduct) * (\%\ ownership\ of\ the\ pool)
$$

For LP bonds such as EXO-DAI bond, the RFV is calculated differently because the protocol needs to mark down its value. Why? The LP token pair consists of EXO, and each EXO in circulation will be backed by these LP tokens - there is a cyclical dependency. To safely guarantee all circulating EXO are backed, the protocol marks down the value of these LP tokens, hence the name _risk-free_ value (RFV).
