A framing of the problems of GossipHub protocol and how it impacts latency and bandwidth

- Message size ignorance: GossipSub does not consider message size in forwarding and IWANT message decisions
- Prolonged transmission times: Large messages significantly increase transmission times (e.g., from 100.8ms for 10KB to 180ms for 1MB), but the protocol doesn't adapt to this
- Lack of receiver awareness: Peers are unaware of ongoing message receptions during prolonged transfer times, leading to poor decision-making

### Bandwidth and Duplicate Message Problems
- Excessive duplicate transmissions: Higher transmission times increase the likelihood of simultaneous redundant transmissions to the same peer from different senders
- IDONTWANT limitations: IDONTWANT messages can only be sent after receiving the entire message, making them less effective for large messages where duplicates may start during the lengthy reception period
- Bandwidth utilization spikes: Very high bandwidth utilization occurs due to numerous duplicate transmissions

### IWANT Request Issues
- Multiple redundant IWANT requests: Peers typically make multiple IWANT requests to different hosts for the same message since responding to IWANT requests is optional in GossipSub v1.1
- Unaware ongoing receptions: Peers remain unaware of messages they're currently receiving and may initiate multiple IWANT requests for these messages during the reception period
- Workload concentration: Most IWANT requests are serviced by early message receivers or peers on optimal forwarding paths, creating bottlenecks

### Timing and Latency Problems
- Store-and-forward delay accumulation: Transmission delays accumulate at each hop, significantly increasing network-wide dissemination time
- Heartbeat interval conflicts: Longer transmission times increase the likelihood that heartbeat intervals will elapse before message transmission completes
- Message queuing delays: Busy peers with large outgoing message loads will enqueue new messages, introducing significant initial delays
- Lack of prioritization: No standardization in outgoing message prioritization leads to inconsistent message dissemination times

### Scalability Issues
- Performance degradation with size: A tenfold increase in message size results in an eightyfold rise in mesh transmission time for a mesh with D=8 peers
- Cold connection problems: Smaller congestion windows at less frequently used links can noticeably increase message dissemination time
- Traffic volume sensitivity: Performance improvement strategies like floodpublish and IWANT replies become ineffective when forwarding large messages