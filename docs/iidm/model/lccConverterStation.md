---
title: LCC converter station
layout: default
---

The `com.powsybl.iidm.network.LccConverterStation` interface is used to model a LCC Converter Station. In IIDM, this is
a sub interface of [HvdcConverterStation](hvdcConverterStation.md).

## Characteristics

| Attribute | Type | Unit | Required | Default value | Description |
| --------- | ---- | ---- | -------- | ------------- | ----------- |
| PowerFactor | float | - | yes | - | The power factor |

The PowerFactor is equal to
$$
\frac{P}{\sqrt{P^{2} + Q^{2}}}
$$
and should be between -1 and 1. Note that at terminal on AC side, Q is always positive: the converter station always consumes reactive power.

## Examples
This example shows how to create a new `LccConverterStation` in a network:
```java
LccConverterStation lcs = voltageLevel.newLccConverterStation()
    .setId("LCS")
    .setConnectableBus("B1")
    .setBus("B1")
    .setLossFactor(0.011f)
    .setPowerFactor(0.5f)
    .add();
```
