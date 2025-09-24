Two flip-flops are used: q and q1.

On reset = 1, both outputs (q and q1) are cleared to 0.

After reset, q1 is forced to constant 1 at every clock cycle.

q copies the value of q1, so it becomes 1 one clock later.

The output stabilizes at q = 1 regardless of input conditions.

The extra flip-flop (q1) is redundant → synthesis tool will optimize it away.
