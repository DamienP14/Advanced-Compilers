# Advanced-Compilers

This tool reads a Bril program, splits it into basic blocks (little straight pieces of code), and builds a map of where control can go next (the CFG). Then it runs a simple loop that keeps passing facts along those arrows until nothing changes.

It does two jobs. Reaching Definitions answers: “which assignments could have made the current value?” For each block we record what it creates (the last writes inside the block) and what it overwrites (other writes to the same variables). We move sets forward through the graph: what arrives from predecessors, minus what we overwrite, plus what we create. Keep repeating until the sets stop changing.

Available Expressions answers: “which computations (like a+b) have already been done and are still valid on every path?” We collect all two-argument expressions in the function, treat a+b the same as b+a, and mark any expression as killed if the block rewrites one of its variables. We move info forward, but at merges we keep only what all paths agree on. Again, repeat until stable.

Why it stops: there are only so many facts to add/remove, so the loop can’t run forever. Tests are run with Turnt: each .bril has a matching expected .out. From the project folder, run turnt to check everything.
