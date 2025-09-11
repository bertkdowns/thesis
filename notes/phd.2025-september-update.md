---
id: 7g0ti4b43vwkcmmexys9msj
title: 2025 September Update
desc: ''
updated: 1757570119800
created: 1757549858076
---


# Next Steps:


# Relaxations for IDAES Unit Models

Make initialisation better by rewriting the IDAES Unit models initialisation to have relaxations. 

Think about when does initialisation fail? E.g when one of the streams in the Heat Exchanger has insufficient flow, and the later step will calcualate the correct amount of flow required. So we could get rid of the constraint on energy transfer, and just minimise the difference between the two. This will get us as close as we can to a good initialisation state. Look into andrew's [paper on complementary distillation columns with dry trays](https://www.osti.gov/servlets/purl/1924675) as an example.

Then do the same with all the other unit operations and submit a PR to idaes. What else makes initialisation flaky?

Switch to use the new initialision methods rather than the old ones, and maybe update the unit operations to use the new methods too.

# Write a paper

See [[phd.paper-ideas]]

# Read Larry's book

Learn how IPOPT works properly

# 1d Unit Operations

We need to be able to model 1d unit operations better in the platform. How do we do that?


[[ahuora.1d]]

[[ahuora.1d-proposal]]