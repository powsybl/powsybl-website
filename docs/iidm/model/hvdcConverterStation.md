---
title: HVDC converter station
layout: default
---

The `com.powsybl.iidm.network.HvdcConverterStation` interface is used to model a HVDC Converter Station. This is the base
class for [VSC](vsc.md) and [LCC](lcc.md). As such, its sub interfaces are:
- [LCC Converter Station](lccConverterStation.md)
- [VSC Converter Station](vscConverterStation.md)

## Characteristics
| Attribute | Type | Required | Default value | Description |
| --------- | ---- | -------- | ------------- | ----------- |
| HvdcType | `HvdcType` | yes | - | The HVDC type |
| LossFactor | float | yes | - | The loss factor |

## HVDC Type
The `com.powsybl.iidm.network.HvdcConverterStation.HvdcType` enum contains these two values:
- LCC
- VSC

It determines if the Converter Station is a LCC Converter Station or a VSC Converter Station.
