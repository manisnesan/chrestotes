# Overview of Personalization systems

## Tasks

- [x] Outline creation
- [] Discovery & Research
- [] Draft content
- [] Edit content
- [] Review content
- [] Publish content


## Outline

- What is personalization? (file:///home/msivanes/Downloads/Fan-Poole-2009-Whatispersonalization.pdf)
  - Personalization helps to solve the challenges of mass customization due to the high variety of complex products & services.
  - Helps to tailor contents to (individual user) customer wishes and needs.
- Why personalization is used in sites and what do they provide?
- What are the components of personalization technologies?
  - __User model__ used to extrapolate which items(products, services or units of information) that should be shown to the user.  Needs represented as say ratings or explicit requirements.
  - __Items__ are characterized by their ratings or a semantic description of item properties.
  - __collaborative filtering__ recommending items to the current user based on positive preferences of similar users to the current.Example: A is similar to B, C. B and C liked Die Hard. Recommend Die Hard to A.
  -  __content-based__ recommending based on items purchased/consumed by the user to recommend similar items in the future.
  - __knowledge-based recommenders__ identify and show relevant items on the basis of a set of filter constraints. Example - user specified keyword, selected product filter to show the content
  - __recommended items__ are ranked item list determined by recommender system
- Example Organizations using personalization to improve user experience, increase conversions.
- How should I get started with personalizing for my site?
- What are the gotchas involved?

### Basic architecture of configuration environment

- user model (needs represented as explicit requirements)
- configuration system (eg., constrain-based)
- configuration Knowledge base (component types, constraints, heuristics)
- configuration -> component instances and their connections

gv('''
user model -> configuration system
configuration knowledge base -> configuration system
configuration system -> configuration
''')

Sketch of the basic architecture of a configuration environment. User-related preferences (requirements) are stored in the user model. The set of potential configurations is defined implicitly by the configuration knowledge base. A configuration system (configurator) determines a configuration that takes into account the defined set of requirements and the constraints in the configuration knowledge base

### Basic architecture of recommendation environment

gv('''
user model -> recommender system
items -> recommender system
recommender system -> recommended items
''')

Sketch of the basic architecture of a recommendation environment. User-related preferences are stored in the user model. Items are characterized by their ratings and additional semantic information. A recommender system exploits the information of the user model and the item catalog to derive a ranked list of items which is presented as the recommendation to the user

### Definitions
(serendipity effect) - new relevant products and services a customer never thought of.

### References
- [An introduction to personalization and mass customization](https://link.springer.com/article/10.1007/s10844-017-0465-4)
- [Towards recommending configurable offerings](https://pdfs.semanticscholar.org/6a67/deda94ed42ea7665b357ccd2522012ef8ab1.pdf)
