# PhD plan - next steps

See also: [Phd Cohesiveness](014_phd_cohesiveness.md)

From the [Proposal](https://raw.githubusercontent.com/bertkdowns/honours-dissertation/refs/heads/main/phdproposal/main.pdf), Four research questions are identified. Some next steps for each:

## How does the level of abstraction impact the effort required to design and implement a Digital Twin for an industrial chemical process? To what extent does it enable wider applicability of Digital Twin tools and techniques?

The general idea of this is to apply the software engineering principle of information hiding to the wider simulation community, allowing multi-scale modelling.

The generalisation of idaes property packages - to support mass and energy streams - can be a good example of this.

Data structures from before and after generalisation could be compared.

The same principles could be applied to the module design, and maybe other stuff as we support more types of modelling.



## How effective are Operator Networks at surrogate modelling dynamic chemical processes in an online setting?

Start a literature review looking at:
- Current research in operator networks (FNO, DeepONet, RBF ONet (Jason Kurz))
- Alternative techniques:
    - Transfer Component Analysis, and PCA (Suggestion from D. Allan)
    - LSTM and other time-series data processing techniques
    - Transformer architecture?
    - Any other techniques used for surrogate modelling
- Physics informed neural network techniques
    - Soft constraints
    - Hard constraints -baked into model structure
    - applications of these in RBFs
    - applications of these in operator networks or LSTMs or CNN or Transformer
- OMLT and other methods of putting ML models in idaes
- Global optimisation techniques for RBFs
- ODEs and Neural ODEs

Continue testing:
- Building and running an RBON in pyomo
- Using a RBON to predict a convolution operator
- Making the RBON predict/work with LTI systems (no bottom triangle stuff)
- Global optimisation of RBON using IPOPT 



## What are the real-world performance benefits associated with implementing Digital Twin Techniques?

This is a bit of a long-term one, and probably involves more discussion with my supervisors. It'll probably involve getting some industry case studies - either implementing it in industry or simulating a plant and the theoretical improvement it could have if changes were made to the design.

It'd probably be helpful to do some literature review on papers related to:
- Digital Twin Performance/Efficiency
- case study digital twin simulation

Otherwise, this involves networking and finding
- another PhD student who could do their industrial case study in the ahuora platform 
- an industry person who is keen to let me model or convert their model into the ahuora platform

The best thing to do with either of these is keep talking to people and learn about a diverse range of challenges and opportunities.


## What software engineering principles, practices, and frameworks help build more maintainable Digital Twin systems?

Maybe this could be reworded to "how does test-driven-development affect the maintenance of Digital Twin systems?" "How does documentation support or impede development of the system?" "How does typing affect development pace?" "how can typing and code generation be used effectively across multiple languages/projects/services?" "which software design practices (observer, prototype, Factory, builder, adapter, decorator, command, etc) are most helpful for modelling and interacting with a Digital Twin system?"

These can be combined to give a good framework or architecture for how a digital twin system can be built. the Ahuora Platform can be used as a case study for this to an extent.

I don't have much experience writing about or reading papers on this, and I don't want to research it by doing rigorous participant studies of developers trying to use different tools, so I will try read some papers on these topics and see their general format. 


# Other more general stuff

I first need to read papers to figure out how I can measure improvements, how I can tell something is benificial, what indicators there are of better reliability or a better development workflow. This might be partly by reading other phd dissertations.

I need to think about how to make this more cohesive - particularly the operator network stuff is a side tangent. I'm imaginging it'll have to be about 30% of the way into the dissertation, and probably take up no more than 30% of the report. Or it could be a bit later, and be used as an extended case study into development techniques.  

# See also

Other PhD dissertations on software engineering:
https://www.sigsoft.org/dissertations.html
https://erlang.org/download/armstrong_thesis_2003.pdf


# Things to Lit review

## Building Background Knowledge

> General principles from field's I'm not so familiar with.

- Control Theory
- Other Software PhD theses
- Software Engineering Design Theory
- Relevant chemical processes I might design
- Process engineering Design theory
- Process engineering Operation Theory
- General ML techniques

## "On the edge of understanding"

> Latest research that I want to continue on from to find novelty.

- RBON, operator networks, etc (see RQ above)
- Digital twin applications & case studies
- Existing simulation tools and techniques
- Existing DT development practices
- All the new IDAES papers
- Surrogate modelling
- Multi-fidelity modelling
- Live data processing techniques

