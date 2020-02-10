---
title: Terminal
layout: default
---

The `com.powsybl.iidm.network.Terminal` interface is used to model an equipment connection point in a [voltage level](voltageLevel.md)
topology. A terminal is created when the equipment it is connected to is created: it can not be created independently.

# Characteristics

| Attribute | Type | Required | Default value | Description |
| --------- | ---- | -------- | ------------- | ----------- |
| VoltageLevel | `VoltageLevel` | yes | - | The voltage level to which the terminal belongs |
| NodeBreakerView | `NodeBreakerView` | yes | - | The view to access node/breaker topology information at the terminal |
| BusBreakerView | `BusBreakerView` | yes | - | The view to access bus/breaker topology information at the terminal |
| BusView | `BusView` | yes | - | The view to access bus topology information at the terminal |
| Connectable | `Connectable` | yes | - | The equipment which is connected to the terminal |
| P | double | no | - | The active power injected at the terminal |
| Q | double | no | - | The reactive power injected at the terminal |
| I | double | no | - | The current at the terminal |
| Connected | boolean | yes | - | The connection status of the terminal (true if the terminal is connected, else false) |

## View
The available views for the terminal depends on the topology (node/breaker or bus/breaker) of the voltage level it
belongs to. More information about voltage level topologies can be found on the [voltage level](voltageLevel.md) page.

### NodeBreakerView
`NodeBreakerView` is available for node/breaker topologies. All elements are viewed as physical ones: busbar sections,
breakers and disconnectors.

**Characteristics**

| Attribute | Type | Required | Default value | Description |
| --------- | ---- | -------- | ------------- | ----------- |
| Node | int | yes | - | The connection node of the viewed terminal in a node/breaker topology |

### BusBreakerView
`BusBreakerView` is available for node/breaker and bus/breaker topologies. It presents an aggregated view of the
topology made of buses and switches.

**Characteristics**

| Attribute | Type | Required | Default value | Description |
| --------- | ---- | -------- | ------------- | ----------- |
| Bus | `Bus` | yes | - | The connection bus of the viewed terminal in a bus/breaker topology |
| ConnectableBus | `Bus` | yes | - | A bus that can be used to connect the viewed terminal in a bus/breaker topology |

### BusView
`BusView` is available for node/breaker, bus/breaker and bus-only topologies. It presents an aggregated view of
the topology made of buses.

**Characteristics**

| Attribute | Type | Required | Default value | Description |
| --------- | ---- | -------- | ------------- | ----------- |
| Bus | `Bus` | yes | - | The connection bus of the viewed terminal in a bus-only topology |
| ConnectableBus | `Bus` | yes | - | A bus that can be used to connect the viewed terminal in a bus-only topology |

# Flow sign convention
Flows at terminals (P and Q) follow the load sign convention.
- Flow out from a bus (bus and bus/breaker views) or a node (node breaker view) has a positive sign.
- Consumptions from the grid are positive.

# Examples
This example shows how to use the terminal of an [injection](injection.md):
```java
Terminal terminal = network.getLoad("LOAD").getTerminal();
if (Double.isNan(terminal.getP()) {
    terminal.setP(400);
}
```

This example shows how to use a terminal of a [branch](branch.md):
```java
Terminal terminal = network.getLine("LINE").getTerminal1().setP(300).setQ(100);
```

This example shows how to use a terminal of a [three windings power transformer](threeWindingsTransformer.md):
```java
Terminal terminal = network.getThreeWindingsTransformer("TWT3").getLeg1().getTerminal().setP(0.0);
```
