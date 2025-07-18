---
id: 7ge60f5z9a7qkmgh836qxdv
title: Proposal
desc: Project Proposal
updated: 1752787521481
created: 1743126355038
bibliography:
  - assets/refs.bib
---


# Proposal

## Research Aims and Questions

<!---
The core questions or hypotheses you're trying to answer.
Often phrased clearly and precisely, e.g.:
"To investigate whether..."
"How can X be improved by Y?"
-->

My project aims to answer the following research question:

*How effectively can a software framework reduce the complexity of implementing new Process Digital Twins?* 

*'Essential Complexity'* , or domain logic, is the "complexity inherent in solving the problem that you are trying to solve" [@farley2021modern]. *'Accidental Complexity'*, or application logic, refers to the "problems we are forced to solve as a side effect of doing something useful with computers". This includes infrastructure, scalibility, security, and data persistence. 

There is a lot of complexity in building a digital twin, including:

- Creating a model of the process 
- Adapting the models to work in an online context, where they can be continuously retrained
- Collecting, aggregating, visualising, and storing heterogeneous data in a cohesive fashion
- Leveraging the digital twin for operational decision support, supervisory control, and/or design-time optimisation.

My hypothesis is that much of this is accidental complexity; the domain logic is in the model itself. Thus, a standardised Digital Twin framework can implement the application logic, and eliminate the majority of accidental complexity. This will enable those building Process digital twins to focus on their model of the system, the essential complexity of the digital twin.

Machine learning techniques are frequently used to adapt a design-time behavioural model into an online context. There is less research into how they are systematically built and deployed, particularly with regard to dynamic systems. This can be standardised to reduce the accidental complexity in implementing machine learning methods.  

Process digital twins often build on existing frameworks for describing process structure (e.g DEXPI), describing process behaviour (e.g Modelica), and data collection (e.g OPC-UA). Linking these is a major source of accidental complexity. Building a DT framework that encompasses these standards will minimise the work required to create new Process Digital Twins.

Because digital twins include bi-directional communication with the physical sytem, they can be viewed as an advanced control system. Thus, supervisory control systems will provide a good template for how these various technologies can be linked and function together. 


## Methodology
<!--
How will you answer your research questions?
Details of methods, experiments, case studies, simulations, or theoretical frameworks.
Tools, data sources, analysis techniques.
-->

I first need to build a framework for developing Process Digital Twins. Then I can evaluate how much it can speed up the development of new Process Digital Twins.

I hypothesize that a framework that:

- makes it easy to link different data sources
- is built of existing control technologies, and
- uses machine learning techniques to create an adaptive behavioural model,

will best reduce the complexity of implementing a Process Digital Twin.

This of course leads to a number of sub-questions that I will need to answer as part of my research, such as:

- How much complexity is involved in linking different data sources? What standard tools could we apply to reduce the time spent on this?
- What level and types of control should a Digital Twin System implement to have maximum return on investment? What techniques are there to automate their setup?
- How can machine learning methods be embedded within DT frameworks to enhance adaptability without increasing the DT's complexity? 

To answer these questions and, each attribute of my framework will be 

- evaluated in terms of theoretical principles
- implemented in the Ahuora Digital Twin Platform, and 
- evaluated in case studies. 

This follows standard software engineering principles [@armstrong2003making]. It strikes a balance between logical reasoning, and concrete applications where real-world factors also affect the outcome. 

All three of these research activities will be conducted in parallel as much as is possible, enabling them to inform each other and improve designs. 

### Theoretical analysis of system properties

I have already identified key patterns and structures the hypothesis and through the literature review. As the software project evolves over time, new development patterns will also be identified. These patterns in the software architecture can provide a different lens into the problem space of designing a Digital Twin. We can then ask: *"If this property was to be taken to its logical extreme, and strictly enforced across the Digital Twin, what consequences - both positive and negative - would emerge?"* This enables a deeper understanding of the trade-offs in adopting an architectural principle.

For instance, if a technique for modularity emerges as a dominant pattern, one might ask: "What would a fully modular Digital Twin look like?" 
It could lead to benefits such as improved testability, parallel development, and plug-and-play component integration. However, it might also introduce challenges around coordination, interface design, and performance due to increased abstraction boundaries. These factors could then be evaluated in greater detail, or implemented in the platform to provide concrete evidence through case studies.

Ultimately, the theoretical analyis of a variety of system properties can be combined into a generalised framework for building Digital Twins.


### Development of the Ahuora Digital Twin Platform

The Ahuora Digital Twin Platform provides a means to study various properties of a Digital Twin Framework. To experiment with an aspect of a Digital Twin Framework, I will need to implement it in the Ahuora Digital Twin platform first. This gives me the flexibility to design a variety of experiments around it. I have the ability to modify both the functionality, and how it integrates with the wider system.

The major next stages of development include more complex methods of dynamic simulation, machine learning functionality, and integration with real-time processing systems. All of these are closely related to what I hypothesize are sources of accidental complexity in conventional Digital Twin systems.


### Implementation of the Ahuora Digital Twin in industrial case studies

Based on these features, the Ahuora Digital Twin Platform will be used to model a variety of industry case studies. These will be a combination of real-world case studies in collaboration with New Zealand industries, and simulated scenarios using benchmark processes [@DOWNS1993245]. 

I will be starting with the Tennessee Eastman Process Simulator. This is a benchmark of process control technologies, including handling disturbances [@DOWNS1993245].  It has been re-implemented with random variation to enable statistical methods in Matlab/Simulink [@capaci2019revised], and an interface has been created in python to enable interactivity in changing setpoints throughout the run [@reinartz2022pytep]. This will provide a reliable test case in absence of or addition to real world examples. Further benchmark simulations will then be identified as appropriate. Ahuora also has a number of other PhD students working on a variety of existing case studies and simulation problems, which could be used as well. 


Inevitably, feedback and requirements from these case studies will necessitate development of new features and refinement of workflows. These changes in requirements form the key data points of this research. They provide insight into how standards, frameworks, and technologies affect the development pace, quality, functionality, and reliability of a Digital Twin.

Through application of software engineering practices, the architecture of the platform will gradually adjust to better the problem domain. This principle of incremental change to "grow" an architecture is widely documented in the software industry [@foote1997big]. It is expected that through this process, the the amount of accidental complexity involved in building a digital twin will decrease. 

### Measuring Complexity

Ivan et al. would argue that software systems are not truly complex, as they are complicated finite approximations of real world systems and therefore decomposable into many simple problems [@ivan2013complexity].
In this context, complexity can be considered a measure of the entropy or complicatedness of the system; how many different interactions need to be considered [@harrison1992entropy]. *Structural Complexity* specifically focuses on the structure of the program itself.

It is hard to measure complexity in absolute terms, but it is easier to compare the relative structural complexity of two systems. For two codebases in a similar programming language, this can be done through comparing lines of code, or number of branch conditions, or compressed file size. 
Each of these can provide some insight into the complexity of the system, when they are carefully balanced against each other. For example, a program with many lines of code may not be particularly complex if there is little branching and the code is repetitive.

For example, here is a comparison of two HTTP web servers in python, one using Flask, and one using a the standard python HTTP library.

```python
### Flask Web Server
from flask import Flask

app = Flask(__name__)

@app.route('/hello')
def hello():
  return "Hello, World!"

app.run(port=5000)

### Pure Python http.server

from http.server import BaseHTTPRequestHandler, HTTPServer

class HelloHandler(BaseHTTPRequestHandler):
  def do_GET(self):
    if self.path == '/hello':
      self.send_response(200)
      self.send_header('Content-type','text/plain')
      self.end_headers()
      self.wfile.write(b'Hello, World!')
    else:
      self.send_response(404)
      self.end_headers()

server = HTTPServer('localhost',5000)
server.server_forever()
```

The Flask version is shorter, has less conditional logic, and doesn't have to worry about the underlying details of HTTP. For this use case, it can be argued that using Flask makes designing the system much less complex - even though Flask may have more complex logic inside it.

To test my hypothesis, I will compare the structural complexity of Process Digital Twin systems built using the Ahuora Digital Twin Platform with the equivalent Digital Twins built using conventional simulation and data processing techniques.
As the Ahuora Digital Twin Platform is a graphical application, comparisons will need to be made at a higher level, in terms of pseudocode or diagrams, accompanied by an analysis of each part. To ensure that the comparison is fair, comprehensive reasoning about each part of the system, and the appropriate level of detail to visualise it, will be required.

Both the Ahuora Platform and conventional Digital Twin Techniques will be compared from the perspective of building a Digital Twin. Thus, just as the Flask example did not discuss the complexity of the Flask library itself, the internal complexity of the Ahuora Platform will not be considered as the person building a Digital Twin doesn't interact with it. Instead, the complexity of the system *the user has to build* will be evaluated.

<!-- provide an example: flask vs pure python  -->

Less structurally complex systems will be easier to build [@complexityofsystems], so if a Digital Twin built with the Ahuora Platform is less complex, it is evidence that it is easier to build than using conventional simulation techniques.

<!---
In the appendix, include some potential characteristics, and some potential case studies
Potential characteristics avaliable in phd.potential_characteristics.md
-->

## Timeline

![Estimated PhD Timeline](assets/timeline.drawio.svg)



### Current Progress

I have developed the core steady-state functionality required for the Ahuora Digital Twin platform to become usable in industry. Collaborating with others at Ahuora, I implemented additional property packages in the platform to support milk and humid air, two critical components for modeling New Zealand industries.

In March 2025, I was heavily involved in releasing the Ahuora Digital Twin Platform to industry in a beta phase. This included a training day with 30 stakeholders from various sectors representing their companies. 
The training was well received, and I am now engaging in discussions on how to integrate the Ahuora Platform into their workflows. 
Throughout this process, I have identified additional requirements, such as better support for data processing, prebuilt models of common components, and use cases for design and operations.

Since then, I have expanded the platform with additional functionality for dynamic modelling, paving the way for the Ahuora Digital Twin Platform to be used for prediction and supervisory control. This is currently limited to storage tanks and PID control, but now the initial systems are there development will be simpler in the future.

I have implemented new IDAES unit models for Direct Steam Injection with a Milk Property Package, supporting key industry case studies in New Zealand's milk processing plants ^[[This is accessible on GitHub. https://github.com/bertkdowns/direct_steam_injection](https://github.com/bertkdowns/direct_steam_injection)]. I anticipate this will make more New Zealand focused case studies accessible to me.

I have also conducted a literature review to survey current technologies, which forms the next chapter of this document. This review highlighted the need for a standardized workflow to construct digital twins and the tools in process engineering that a Digital Twin platform can utilize.

I have directed some initial prototyping into creating a library for NODE-RED, an event-driven data management platform, to work with the Ahuora Platform. I have tested using NODE-RED to communicate across a variety of protocols, including HTTP, TCP, and OPC-UA, and it is a promising method to enable implementation of diverse factory data systems with the Ahuora Platform. This work will continue and be directly applied to my upcoming case studies. 

In parallel, I have been researching radial basis operator networks [@kurz2024radial] and implemented these techniques in the Algebraic Modeling language Pyomo. This work may enable the creation of surrogate models of dynamical systems and their inclusion in a Digital Twin.

### Future Work

Through discussions from the Ahuora Platform day and Project Ahuora's other industry connections, I anticipate starting a case study by late 2025 on modeling a continuous process. This case study will provide feedback to improve the scope and architecture of the Ahuora Digital Twin Platform. I will also use additional case studies from other industrial partners, scenarios that existing PhD students are already studying as part of Project Ahuora, and simulated factory environments that serve as benchmarks [@DOWNS1993245].

Development of the ADTP and implementation in case studies will be conducted simultaneously, following the principles of iterative design [@beck2001manifesto]. The case studies will naturally influence the requirements and design of the system, and these changes will be recorded and documented. 

These notes and discussions on requirements changes will become a primary data source for future studies. They will be used to qualitatively discuss:

- The benefits and drawbacks of the Ahuora Digital Twin Platform from a systems design perspective
- How the Ahuora Digital Twin Platform provides structure for a Process Digital Twin
- How the Ahuora Digital Twin Platform can be used to build a Process Digital Twin

These topics are expected to be the major component of my thesis.

In order for a framework such as the Ahuora Digital Twin Platform to be truly useful in industry, a business model or long-term support plan is required. Towards the end of my PhD, I anticipate that more time will be taken up creating a business model to ensure that my research is able to provide the most value to the industry. As an open source product, this is also novel in the field of Process Digital Twins. 

## Expected Contributions

<!--
What will your research contribute to knowledge, practice, or theory?
How will it make a difference?
-->

![Anticipated PhD Outcomes](assets/outcomes.drawio.svg)

It is expected that this PhD will contribute both to academic theory on how to build Digital Twins, and the way Digital Twins are used in New Zealand Industries. 

This PhD will include substantial work on the Ahuora Digital Twin Platform, which it is intended will be avaliable as a partially commercial, partially open source product for the forseeable future. This will provide a medium to transfrom many techniques currently only used in academia into general industry practices.

Existing literature already outlines a number of different frameworks to view a Digital Twin. However, through participation in multiple case studies, these existing models can be reviewed and updated, and specialised to the domain of Chemical and Process Engineering. 

Through the case studies, a generalised methodology to build a DT of a chemical process can be constructed. This methodology will be shown in the context of the Ahuora Digital Twin Platform. It will be both a novel piece of research into DT application, and a guide to industry on how to create a DT system.

Finally, I anticipate that this process will also involve creation of new techniques for Machine Learning and/or Mathematical Modelling. As these techniques form the core of the DT, it is likely that tangential research into those fields will be required to enable the use of emerging techniques in Digital Twins. 
An example of this outcome is my work on operator networks, as it could be applied outside the field of Digital Twins.



## Challenges and Risk Management

<!---
What are the ## Current Progress
<!---
What you've done: experiments, models, code, papers, reviews, etc.
Can include early results or pilot studies.
-->

I am in discussions with multiple New Zealand businesses around their plans to retrofit with renewables and increase process efficiency. It is expected that I will be able to work on those case studies in the coming months. These will provide the foundational case studies, and will open the door for future industry collaboration. Because there are multiple case studies avaliable, my project is not reliant on access to a single data source, mitigating the risk of data access being a barrier to progress. 
The literature also includes a number of simulated processes that I can use, such as the Tennessee Eastman Process. These also will provide a fallback if industrial data is not accessible.

There could be technological concerns with the use of the Ahuora Digital Twin platform, if development of the required features is taking longer than expected. This can be mitigated through incremental development and managing scope. In this case, external tools such as the IDAES Process Modelling framework [@lee2021idaes] can be used in place of the Ahuora Digital Twin Platform. There is ample flexibility in the Ahuora Digital Twin platform to negotiate scope and prioritise features as required for our case studies, so it is unlikely that this will be an issue.

Depending on the case study, confidentiality may be required with regard to company data. This will be handled in accordance with the University policy's procedures and guidelines. Likewise, any ethical issues will be dealt with in accordance with University policy. 




\newpage
