## type ForcedExit
[ForcedExit](../../../api-and-sdk/data-types/transaction/forced_exit.md) transaction type.

```dart
ForcedExit(
    int toChainId,
    int initiatorAccountId,
    int initiatorSubAccountId,
    int targetSubAccountId,
    String target,
    int l2SourceToken,
    int l1TargetToken,
    String exitAmount,
    int initiatorNonce,
    bool withdrawToL1,
    int? ts,
)
```

### func sign

```dart
void sign(ZkLinkSigner zkLinkSigner)
```

Sign transaction with given `ZkLinkSigner`

### func toJson

```dart
String toJson()
```

Get the json str of [ForcedExit](#type-ForcedExit)
