# tune.sol

## Vat

### mutators

#### administering a position

This is the core method that opens, manages, and closes a collateralised debt position. This method has the ability to issue or delete dai while increasing or decreasing the position's debt, and to deposit and withdraw "encumbered" collateral from the position. The caller specifies the Ilk `i` to interact with, and agent identifiers `u`, `v`, and `w`, corresponding to the sources of the debt, unencumbered collateral, and dai, respectively. The collateral and debt unit adjustments `dink` and `dart` are specified incrementally.

```
behaviour tune of Vat
interface tune(bytes32 i, bytes32 u, bytes32 v, bytes32 w, int256 dink, int256 dart)

types

    Can   : uint256
    Take  : uint256
    Rate  : uint256
    Ink_u : uint256
    Art_u : uint256
    Ink_i : uint256
    Art_i : uint256
    Gem_v : uint256
    Dai_w : uint256
    Debt  : uint256

storage

    #Vat.wards[CALLER_ID] |-> Can
    #Vat.ilks[i].take     |-> Take
    #Vat.ilks[i].rate     |-> Rate
    #Vat.urns[i][u].ink   |-> Ink_u  => Ink_u + dink
    #Vat.urns[i][u].art   |-> Art_u  => Art_u + dart
    #Vat.ilks[i].Ink      |-> Ink_i  => Ink_i + dink
    #Vat.ilks[i].Art      |-> Art_i  => Art_i + dart
    #Vat.gem[i][v]        |-> Gem_v  => Gem_v - (Take * dink)
    #Vat.dai[w]           |-> Dai_w  => Dai_w + (Rate * dart)
    #Vat.debt             |-> Debt   => Debt + (Rate * dart)

iff

    // doc: caller is `. ? : not` authorised
    Can == 1

iff in range uint256

    Ink_u + dink
    Art_u + dart
    Ink_i + dink
    Art_i + dart
    Gem_v - (Take * dink)
    Dai_w + (Rate * dart)
    Debt + (Rate * dart)

iff in range int256

    Take
    Take * dink
    Rate
    Rate * dart

if

    VGas > 300000
```
