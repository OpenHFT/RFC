### Subscription Test [0]
this is how to create a subscription

sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
tid: 1434659194321
--- !!data
subscribe: !type net.openhft.chronicle.engine.api.map.MapEvent

```
sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
--- !!data
put: {
  key: testA,
  value: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: xyz,
    firm: !!null ""
}
}
```
sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
tid: 1434659194385
--- !!data
size: {
}
```

receives:

```yaml
--- !!meta-data
tid: 1434659194321
```



receives:

```yaml
--- !!not-ready-data!
reply: !InsertedEvent {
  assetName: /test,
  key: testA,
  value: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: xyz,
    firm: !!null ""
}
}
```



receives:

```yaml
--- !!meta-data
tid: 1434659194385
```



receives:

```yaml
--- !!data
reply: 1
```


sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
tid: 1434659194429
--- !!data
get: testA
```

receives:

```yaml
--- !!meta-data
tid: 1434659194429
```



receives:

```yaml
--- !!data
reply: !net.openhft.chronicle.engine.Factor {
  openPDFlag: 0,
  openUCFlag: 0,
  openActiveEMFlag: 0,
  openPastDueEMFlag: 0,
  accountCloseFlag: 0,
  missingPaperFlag: 0,
  rMLAgreementCodeFlag: 0,
  nMEAccountFlag: 0,
  accountClassificationTypeValue: 0,
  accountNumber: xyz,
  firm: !!null ""
}
```


sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
--- !!data
put: {
  key: testB,
  value: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: abc,
    firm: !!null ""
}
}
```
sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
tid: 1434659194435
--- !!data
get: testB
```

receives:

```yaml
--- !!meta-data
tid: 1434659194321
```



receives:

```yaml
--- !!not-ready-data!
reply: !InsertedEvent {
  assetName: /test,
  key: testB,
  value: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: abc,
    firm: !!null ""
}
}
```



receives:

```yaml
--- !!meta-data
tid: 1434659194435
```



receives:

```yaml
--- !!data
reply: !net.openhft.chronicle.engine.Factor {
  openPDFlag: 0,
  openUCFlag: 0,
  openActiveEMFlag: 0,
  openPastDueEMFlag: 0,
  accountCloseFlag: 0,
  missingPaperFlag: 0,
  rMLAgreementCodeFlag: 0,
  nMEAccountFlag: 0,
  accountClassificationTypeValue: 0,
  accountNumber: abc,
  firm: !!null ""
}
```


sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
--- !!data
put: {
  key: testA,
  value: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: ddd,
    firm: !!null ""
}
}
```
sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
tid: 1434659194458
--- !!data
get: testA
```

receives:

```yaml
--- !!meta-data
tid: 1434659194321
```



receives:

```yaml
--- !!not-ready-data!
reply: !UpdatedEvent {
  assetName: /test,
  key: testA,
  oldValue: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: xyz,
    firm: !!null ""
},
  value: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: ddd,
    firm: !!null ""
}
}
```



receives:

```yaml
--- !!meta-data
tid: 1434659194458
```



receives:

```yaml
--- !!data
reply: !net.openhft.chronicle.engine.Factor {
  openPDFlag: 0,
  openUCFlag: 0,
  openActiveEMFlag: 0,
  openPastDueEMFlag: 0,
  accountCloseFlag: 0,
  missingPaperFlag: 0,
  rMLAgreementCodeFlag: 0,
  nMEAccountFlag: 0,
  accountClassificationTypeValue: 0,
  accountNumber: ddd,
  firm: !!null ""
}
```


sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
--- !!data
remove: testA
```
sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
--- !!data
remove: testB
```

receives:

```yaml
--- !!meta-data
tid: 1434659194321
```



receives:

```yaml
--- !!not-ready-data!
reply: !RemovedEvent {
  assetName: /test,
  key: testA,
  oldValue: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: ddd,
    firm: !!null ""
}
}
```



receives:

```yaml
--- !!meta-data
tid: 1434659194321
```



receives:

```yaml
--- !!not-ready-data!
reply: !RemovedEvent {
  assetName: /test,
  key: testB,
  oldValue: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: abc,
    firm: !!null ""
}
}
```


sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
tid: 1434659194321
--- !!data
unSubscribe: ""
```

receives:

```yaml
--- !!meta-data
tid: 1434659194321
```



receives:

```yaml
--- !!data
reply: !!null ""
```


sends:

```yaml
--- !!meta-data
csp: /test?view=map&keyType=java.lang.String&valueType=net.openhft.chronicle.engine.Factor
--- !!data
put: {
  key: testC,
  value: !net.openhft.chronicle.engine.Factor {
    openPDFlag: 0,
    openUCFlag: 0,
    openActiveEMFlag: 0,
    openPastDueEMFlag: 0,
    accountCloseFlag: 0,
    missingPaperFlag: 0,
    rMLAgreementCodeFlag: 0,
    nMEAccountFlag: 0,
    accountClassificationTypeValue: 0,
    accountNumber: xyz,
    firm: !!null ""
}
}
```
