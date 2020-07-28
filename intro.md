Composition
-----------

<small>
  <span style="color: darkblue;">&lt;</span><span style="color: goldenrod;">/&gt;</span>
  <a href="https://github.com/psse-cpu/se-1223-composition-slides">
    https://github.com/psse-cpu/se-1223-composition-slides
  </a>
</small>

<h4 style="margin-top: 192px; font-size: 0.85em;">
  <span class="course-code">SE 1223</span>
  <span class="course-title">Software Development II</span>
</h4>

<div style="font-size: 0.75em; margin-top: 16px;">
  <b>Richard Michael Coo</b> |

  <a href="https://github.com/myknbani">
    <img style="vertical-align: middle" src="images/github-32px.png" alt="github logo">
  </a>
  <a href="https://twitter.com/myknbani">
    <img style="vertical-align: middle" src="images/twitter-32px.png" alt="twitterlogo">
  </a>
  <span style="vertical-align: middle">@myknbani</span>
</div>



### Outline

* one-to-one
* one-to-many
* composition getter gotchas
  - built-in immutable objects
  - the reference problem
* collection operations
  - imperative techniques
    + linear search
    + map and reduce
    + existential and universal statements
  + overview of functional techniques



### _has-(a)_ Relationships

![petshop](images/petshop.png)

- A petshop **has-a** business permit, **has-a** veterinarian
- A petshop **has** many shelves, **has** many pets, **has** many customers, **has** many employees



### Composition

* In computer science, **object composition** is a way to combine objects into more complex ones. 
* A petshop is _composed_ of ...
  - shelves, which is further _composed_ of
    + products (either pets or pet supplies)
  - people
    + customers
    + employees
    + owner
    + veterinarian
* one of the main tasks in OOP



### How to solve big problems? (1/2)

![org chart](images/ms-org-chart.png)

* Break it down into smaller problems <!-- .element class="fragment" -->



### How to solve big problems? (2/2)

![breakdown](images/breakdown.png)

* Break it down into smaller problems <!-- .element class="fragment" -->



### UML Notations: composition

![composition](images/composition.png)

* whole-part relationship, non-shareable parts
  - the _"whole"_ side gets the **solid** diamond
  - composition is _**"strict"**_ has-a
    - when the _"whole"_ gets destroyed, the part gets destroyed
    - when the Person dies, his/her unique personality disappears along with him/her



### UML Notations: aggregation (1/2)

![aggregation](images/aggregation.png)

* whole-part relationship, shareable parts
  - the _"whole"_ side gets the **hollow** diamond
  - aggregation is _**"weak"**_ has-a
    - when the _"whole"_ gets destroyed, the part does not
    - when a Terrorist dies, his/her Gun can be picked up by teammates/enemies



### UML Notations: aggregation (2/2)

![aggregation](images/aggregation2.png)

* a person has a residence
  - you and your parents share the same residence
  - when you die, the house does not get destroyed
* a residence has a balcony
  - if you destroy the residence, the balcony is destroyed
  - you can't sell the residence without selling the balcony too



### UML Notations: cardinality

<div style="display: flex">
  <img src="images/multiplicity.png" alt="multiplicity">
  <ul>
    <li>Has-one? Has-many?
    <ul>
      <li>
        A college has a home building, but 2 colleges can share 1 building (e.g. medicine, CNAHS)
      </li>
      <li>
        A college needs at least 1 department (strict has-a)
        <ul><li>When College of Eng'g "closes", the SE department "closes" too</li></ul>
      </li>
      <li>If all faculty members resigns, a department can have 0 teachers</li>
      <li>A faculty teaches many courses, sometimes have no teaching load</li>
    </ul>
    </li>
  </ul>
</div>



### Confused between ⬥ and ⬦?

<div style="display: flex">
  <img src="images/not-sure.png" alt="confused">
  <ul>
    <li>Just use association
    <ul>
      <li>It is also <em>"has-a"</em> and can mean either composition or aggregation</li>
      <li>Use it if unsure between ⬥ and ⬦</li>
      <li>Handy for quick whiteboard sketches in a team meeting</li>
      <li>This ambiguity is a double-edged 𐃉</li>
      <li>Note that the arrowhead is on the <em>part-side</em>, not the whole/owner side
        <ul>
          <li><em>"Rich <strong>owners</strong> have the diamonds"</em></li>
          <li><em>"I shoot my subordinates with arrows"</em></li>
        </ul>
      </li>
    </ul>
    </li>
  </ul>
</div>



### So our topic is association?

* Nope it's named composition
  - Simply think of association, composition, and aggregation as UML terms (kinda)
  - The whole-part (has-a) relationship is referred to as composition in general
    + [aggregation is a special type of composition](https://en.wikipedia.org/wiki/Object_composition#Aggregation)
  - There's a principle you'll learn in SE 2122 (Software Component Design)
    - it's the continuation of this course (not SE 2123- Softdev III)
    - That principle states: _"Favor composition over inheritance"_
    - That means _has-a_ (whether ⬥ or ⬦) is **sometimes** better than _is-a_