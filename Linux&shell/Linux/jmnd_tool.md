```
jmnd_tool -D 0000:52:00.0 help
jmnd-tools(JaguarMicro DPU/NIC management utility) provides a set of commands that enable users to
efficiently interact with network cards, ensuring optimal network performance and reliability.
 
The following list is a cheat sheet of user commands in shortcut.
 
Guide usage commands:
    help                                                         Show cheat-sheet(this page) to view all available commands.
    search <keyword>                                             Find a set of commands with the keyword.
 
System commands:
    show board                                                   Show board information.
    show board ddr                                               Show board DDR information.
    show board resetreason                                       Show board reset reasons.
    show macbase                                                 Show base MAC address value.
    show chip temperature                                        Show chip temperature sensors.
    show chip efuse                                              Read chip efuse (OTP) data.
    show opt-module <port>                                       Show optical module status (e.g., SFP+).
    show opt-port                                                Show optical port status.
    set opt-port fec <port> <mode>                               Set optical port fec mode.
 
Pci device commands:
    show device info                                             List all emulation devices(on host).
    show topo {<host-id>}                                        List pci topology.
    set soc pf qsize <sfi> <qsize> <qnum>                        Set pf(on SOC) qsize and qnum.
    show soc pf qsize <sfi>                                      Show pf(on SOC) qsize.
 
Phy link commands:
    set uplink pfc port <port>                                   Set priority Flow Control (PFC) settings for uplink port.
    show uplink pfc port <port>                                  Show priority Flow Control (PFC) settings for uplink port.
    set uplink fc port <port>                                    Set flow Control (FC) settings for uplink port.
    show uplink fc port <port>                                   Show flow Control (FC) settings for uplink port.
    set uplink net-performance port <port>                       Set flow Control thresholds for uplink port.
    show uplink net-performance port <port>                      Show flow Control thresholds for uplink port.
    show uplink pfc rdma                                         Show backpressure from uplink by pfc of rdma.
    show downlink pfc rdma [n2|host]                             Show backpressure from downlink by pfc of rdma.
 
Forward commands:
    show forward flow-table tid <table-id> index <flow-id> count <cnt>
                                                                 List HW flow table entries.
    show qp hash                                                 Show network traffic shunting mode(enable:QP disbale:5-Tuple).
    set qp hash [enable|disable]                                 Spec network traffic shunting mode(enable:QP disbale:5-Tuple).
 
Stats commands:
    set stats vport <sfi0> {<sfi1>}                              Spec statistics for vport(s)
    show uplink statistic <port>                                 Show statistics of phy port
    show stats datapath vport                                    Show datapath statistics for a specified vport.
    show stats datapath global                                   Show global datapath statistics across all ports and flows.
    show stats dcqcn                                             Show statistics of dcqcn.
    show stats rdma pkt-drop [n2|host]                           Show drop packet statistics.
    show stats rdma pkt-rnr [n2|host]                            Show rnr packet statistics.
    show stats rdma pkt                                          Show statistics of received and sent packets.
    clear stats rdma pkt                                         Clear statistics of received and sent packets.
    show backpressure <module>                                   Show backpressure status and metrics for the NIC ports.
    metric <module> {<item>} {<instance>}                        Show metric stats
 
Diagnostic commands:
    show version                                                 Show software/firmware versions.
    show lastword                                                Retrieve last crash log.
    show lastword history                                        Retrieve last crash log stored in flash.
    show health {<item>}                                         Show device hw health.
    show log [all|scp|mcp|imu0|imu1|atf|bl32|uefi]               Dump firmware logs.
    snapshot <dirname> {[csub|dpe|psub|pcie|rpe]}                Dump diagnostic snapshot.
```