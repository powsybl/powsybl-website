---
title: HVDC line
layout: default
---

The `com.powsybl.iidm.network.HvdcLine` interface is used to model an HVDC Line. An HVDC line is connected to the DC side of two [HVDC
converters stations](hvdcConverterStation.md).

# Characteristics

| Attribute | Type | Unit | Required | Default value | Description |
| --------- | ---- | ---- | -------- | ------------- | ----------- |
| R | double | $$\Omega\$$ | yes | - | The resistance of the line |
| ConvertersMode | `ConvertersMode`| - | yes | - | The converter's mode |
| NominalV | double | kV | yes | - | The nominal voltage |
| ActivePowerSetpoint | MW | double | yes | - | The active power setpoint |
| MaxP | double | MW | yes | - | The maximum active power |
| ConverterStationId1 | String | - | yes | - | The ID of the HVDC converter station connected on side 1 |
| ConverterStationId2 | String | - | yes | - | The ID of the HVDC converter station connected on side 2 |

## ConvertersMode
The `com.powsybl.iidm.network.HvdcLine.ConvertersMode` enum contains these two values:
- SIDE_1_RECTIFIER_SIDE_2_INVERTER,
- SIDE_1_INVERTER_SIDE_2_RECTIFIER

The active power setpoint and the maximum active power should always be positive values. The flow sign is given by the type of the converter station. Power always flows from rectifier converter station to inverter converter station. At a terminal on AC side, P and Q follow load sign convention. P is positive on rectifier side. P is negative at inverter side.

# Examples
This example shows how to create a new `HvdcLine` in the network:
```java
HvdcLine hvdcLine = network.newHvdcLine()
    .setId("HL")
    .setR(5.0)
    .setConvertersMode(HvdcLine.ConvertersMode.SIDE_1_INVERTER_SIDE_2_RECTIFIER)
    .setNominalV(440.0)
    .setMaxP(50.0)
    .setActivePowerSetpoint(20.0)
    .setConverterStationId1("C1")
    .setConverterStationId2("C2")
    .add();
```
