---
id: qm15j5oxr0cppx1veuop79o
title: Stream_specification
desc: ''
updated: 1759273119863
created: 1759272265661
---

Right now, you always specify the stream conditions at the start of the stream, and then the downstream values are calculated. 

However, we're running into problems with e.g the desuperheater or makeup header stream, where you want to specify stream conditions downstream and then calculate the upstream values.

You could do this with DOF replacement sometimes, but it is a bit of a hassle. Perhaps we could have an alternative mechanism to specify when stream conditions are required. Almost similar to DOF replacement, but it replaces the entire stream that is specified (and you can move it anywhere dowstream from the source stream.) if a unit operation is placed downstream that sets the upstream conditions, it "enforces" the replacement so you can no longer set the inlet stream conditions.

However, this will raise other questions:

- if you have a mixer upstream from the header's makeup inlet, which inlet part gets specified?
- If you try and set something downstream instead of the inlet conditions, what if the stream goes through a mixer? in theory, you can set one of the streams conditions down from the mixer, but not both. That would be really confusing and unintuitive to a user though.