## Firestone Instructions

<br />

### 1.Buy(Get) a Firestone
```
.\lava-cli.exe -rpcuser=test -rpcpassword=test buyfirestone "FirestoneAddress" "ReceiveChangeAddress"
```
When executed, the corresponding `Tx ID` will be returned.

Important Tips:

1) `FirestoneAddress` is the very address that carries the Firestone. After the purchase, only with the `PrivKey` to THIS address can you consume the Firestone (or free the Firestone if not consumed）.

2) In normal situations, `FirestoneAddress` should be your mining address. In this way, your Full Node will automatically consume a Firestone if a new block is mined and benefit this mining address with additional reward.

3) a `P2SH` format address CANNOT be the `FirestoneAddress` when buying a Firestone.

4)`ReceiveChangeAddress` is the address that will receive the change fund from the transaction.


<br />

### 2. Get Current Slot's Infomation
```
.\lava-cli.exe -rpcuser=test -rpcpassword=test getslotinfo
```
which returns results like:
```
{
  "index": 5,
  "price": 16716105000,
  "count": 10,
  "locktime": 767
}
```
Explanations:

`index` indicates which Slot you are currently at. index starts from #0, which means the first Slot is #0 Slot, and the second Slot is #1 Slot, and so on.

`ticketprice` indicates the current Price for a Firestone. The unit is `satoshi` (which is 10^-8 LV).

`ticketcount` indicates how many Firestones are created in this Slot (till the inquery time). 

`locktime` indicated the height where the Firestone bought will turn to be valid. In other words, it is also the end height for the current Slot.

<br />

### 3. Inquery for a Specific Address:
```
.\lava-cli.exe -rpcuser=test -rpcpassword=test getfirestone “Address”
```
which returns results like:
```
[
  {
    "outpoint": "615fb809eb973667ea8523ecf11bf93eafdf7924521bc172abb09329d141a3e8:1",
    "address": "n4CpQvwua2NXnoq7rJbp41JzbcvUGDL1f1",
    "lockheight": 511,
    "state": "IMMATURATE",
    "isSpent": false
  }
]
```
Explanations:

`outpoint` indicates the `Tx ID` for the very transaction that "buy the Firestone".

`lockheight` indicated the height where the Firestone will turn to be valid.

`state` indicates the status of the Firestone: `IMMATURATE` means not valid yet (need wait for the next Slot); `USABLE` means valid and consumable; `OVERDUE` means already expired.

`isSpent` indicates whether the Firestone is consumed or not. `FALSE` means not consumed. 

<br />

### 4. Free Expired Firestone

If a Firestone is unable to be consumed in `Nth Slot` (say, if the miner is not lucky enough to mine a block), the Firestone will be expired automatically in `N+1th Slot`, and whose fund staked needs to be freed manually via a special transaction.
```
.\lava-cli.exe -rpcuser=test -rpcpassword=test freefirestone "FirestoneAddress" "ReceiveRefundAddress"
```
When executed, a `Tx ID` will be returned.

`FirestoneAddress` and `ReceiveRefundAddress` can be the same address, which means you will get refund to your Firestone Address (usually your mining address).
