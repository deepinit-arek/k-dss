# Frob

## defaults

  * path = dss/out/Lad.abi

### vars

    Vat  : address
    Line : int256
    Live : int256

### storage

    #Lad.vat  |-> Vat
    #Lad.Line |-> Line
    #Lad.live |-> Live

## methods

### file(bytes32,bytes32,int256)

#### vars

    ABI_ilk  : bytes32
    ABI_what : bytes32
    ABI_risk : int256
    Spot_i   : int256
    Line_i   : int256

#### storage

    #Lad.ilks(ABI_ilk, "spot") |-> #unsigned(Spot_i) => #if (ABI_what ==Int 52214633679529120849900229181229190823836184335472955378023737308807130251264) #then #unsigned(ABI_risk) #else #unsigned(Spot_i) #fi
    #Lad.ilks(ABI_ilk, "line") |-> #unsigned(Line_i) => #if (ABI_what ==Int 49036068503847260643156492622631591831542628249327578363867825373603329736704) #then #unsigned(ABI_risk) #else #unsigned(Line_i) #fi

### frob

#### vars

    ABI_ilk                       : bytes32
    ABI_dink                      : int256
    ABI_dart                      : int256
    Spot_i                        : int256
    Line_i                        : int256
    Gem                           : int256
    Ink                           : int256
    Art                           : int256
    Art_i                         : int256
    Rate                          : int256
    Dai                           : int256
    Tab                           : int256
    Gem -Int ABI_dink             : int256
    Ink +Int ABI_dink             : int256
    Art +Int ABI_dart             : int256
    Art_i +Int ABI_dart           : int256
    Rate *Int ABI_dart            : int256
    Dai +Int (Rate *Int ABI_dart) : int256
    Tab +Int (Rate *Int ABI_dart) : int256

#### storage

    #Lad.ilks(ABI_ilk, "line") |-> #unsigned(Line_i)
    #Lad.ilks(ABI_ilk, "spot") |-> #unsigned(Spot_i)

#### requires

    Rate =/=Int 0
    ((((Art +Int ABI_dart) *Int Rate) <=Int #wad2rad(Spot_i)) andBool (((Tab +Int (Rate *Int ABI_dart))) <Int #wad2rad(Line))) orBool (ABI_dart <=Int 0)
    (((ABI_dart <=Int 0) andBool (ABI_dink >=Int 0)) orBool (((Ink +Int ABI_dink) *Int Spot_i) >=Int ((Art +Int ABI_dart) *Int Rate)))
    Live ==Int 1

#### foreign_stores

##### Vat

    #Vat.urns(ABI_ilk, ABI_lad, "gem") |-> #unsigned(Gem) => #unsigned(Gem -Int ABI_dink)
    #Vat.urns(ABI_ilk, ABI_lad, "ink") |-> #unsigned(Ink) => #unsigned(Ink +Int ABI_dink)
    #Vat.urns(ABI_ilk, ABI_lad, "art") |-> #unsigned(Art) => #unsigned(Art +Int ABI_dart)
    #Vat.ilks(ABI_ilk, "rate")         |-> #unsigned(Rate)
    #Vat.ilks(ABI_ilk, "Art")          |-> #unsigned(Art_i) => #unsigned(Art_i +Int ABI_dart)
    #Vat.dai(CALLER_ID)                |-> #unsigned(Dai) => #unsigned(Dai +Int (Rate *Int ABI_dart))
    #Vat.Tab                           |-> #unsigned(Tab) => #unsigned(Tab +Int (Rate *Int ABI_dart))


