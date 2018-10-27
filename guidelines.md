# Guidelines

Please enforce these guidelines at all times. Small or large, call out what's incorrect.

Every line of code should appear to be written by a single person, no matter the number of contributors.

This set of rules generate some constraints and conventions. If you run into instances where a convention isn’t obvious or a solution could be handled in a few different ways, contact the Cosmos community, have a conversation about how to handle it and update this guidelines when needed.

## Separation of UI structure concerns

Comos is made out of isolated and modular structures that fall into 2 categories:

- Layouts
- Components

### Layouts

Layouts are the containers that allow for organizing and grouping its immediate children on the page.

- A layout never imposes padding or element styles on its children.

The classes are prefixed with `-l` (after the Cosmos prefix `cs-`), for example: `cs-l-split` or `cs-l-stack`.

**Note:** I need to learn if this naming convention is something that can be done or is standard in react. I just want to make a clear definition of what is a cosmos component and what is a user component

### Components

Components are modular and independent structures concerned with how a thing looks.

- A component always touches all four sides of its parent container.
- The component itself never has widths, floats or margins.
- Elements inside a component never use top margins. The first element touches the top of its component.
- Components should include semantic markup and necessary Aria tags to implement the [accessibility guidelines](accessibility-guide.md).

The parent container of an component weather is an atom or a molecule is always prefixed with `-c` (after the Cosmos prefix `cs-`), for example: `cs-c-alert` or `cs-c-button`.

Cosmos components follow the Atomic design principles, and they fall into 2 categories:

#### Atoms

Atoms are elements that stand by themselves, can't be reduce any further, and don't allow dependencies.

For example: a button

#### Molecules

Molecules are complex componets that are made out of other components or inluce a component with them.

For example: a card, or an alert

#### Theming a component

Each component can subscribe to a set of modifiers, or props. For example a button can take different backgrounds.

These modifications will only allow change of color, state or scale.  If the change generates a new entity, then create a new component.

Repetition is better than the wrong abstraction.

**Note:** I need to understand better how props works and how they relate to tokens

### Demos

Demos show how components and layouts can be put together to build more complex structures. 

- A demo never includes its own styles. If styling is necessary to implement a desired demo, then new components or layouts, or variants of the components or layouts used, should be created instead.
- A demo doesn't add any accessibility tags that aren't already in the components. All accessibility should be handled at the component level.


## Use of tokens

**Note:** These examples are using sass variables, and pure css, need to change them for tokens and styled elements

Comos follows a two-layer theming system where **global tokens** always inform **component token**. Each one of those layers follow a set of very specific rules.

### Global tokens

The main reason to have global tokens is to maintain consistency. They adhere to the following rules:

- They are prefixed with the word `global` and follow the formula `$cs-global--concept--PropertyCamelCase--modifier--state`.
  - a `concept` is something like a `spacer` or `main-title`;
  - a `PropertyCamelCase` is something like `BackgroundColor` or `FontSize`.
  - a `modifier` is something like  `sm`, or `lg`;
  - and a `state` is something like  `hover`, or `expanded`;
- They are concepts, never tied to an element or component. This is incorrect: `$cs-global--FontSize--h1`, this is correct: `$cs-global--FontSize--3xl`.

For example a global token setup would look like:

```scss

  /* $cs-global--concept--size */
  --cs-global--spacer--lg: .5rem;
  --cs-global--spacer--xl: 1rem;
  --cs-global--spacer--2xl: 2rem;

  /* $cs-global--PropertyCamelCase--modifier */
  --cs-global--FontSize--3xl: 2rem;
  --cs-global--FontSize--2xl: 1.8rem;
  --cs-global--FontSize--lg: 1rem;

  /* cs-global--PropertyCamelCase--state */
  --cs-global--BackgroundColor--hover: #ccc;

  /* $cs-global--PropertyCamelCase--modifier */
  --cs-global--Color--100: #000;

  /* $cs-global--concept--modifier */
  --cs-global--primary-color--100: blue;
```

### Component tokens

The second layer is scoped to themeable component properties and follow these rules:

- They follow this formula `$cs-c-block__element--modifier--state--PropertyCamelCase`.
  - The `cs-c-block__element--modifier` is the selector name is something like `cs-c-alert__actions`;
  - a `state` is something like `hover` or `active`;
- The token always has a default value.
- The value of component scoped tokens is **always** defined by a global token.
- There are tokens for all propierties that are not related to layout.

For example:

```scss
/* Component scoped tokens are always defined by global tokens */
  --cs-alert--Padding: var(--cs-global--spacer--xl);
  --cs-alert--hover--BackgroundColor: var(--cs-global--BackgroundColor--hover);
  --cs-alert__title--FontSize: var(--cs-global--FontSize--2xl);

/* --block--PropertyCamelCase */
.alert {
  padding: var(--cs-alert--Padding);
}

/* --block--state--PropertyCamelCase */
.alert {
  &:hover {
    background-color: var(--cs-alert--hover--BackgroundColor);
  }
}

/* --block__element--PropertyCamelCase */
.alert__title {
  font-size: var(--cs-alert__title--FontSize);
}
```

This setup allows for consistency across components, it generates a common language between designers and developers, and it gives granular control to authors.

### Comment all magic values

If a situation arises where a value needs entering into the style sheets that isn't already defined in the global tokens this should serve as a red flag to you.

In the case where a 'magic' value needs entering, ensure a comment is added on the line above to explain its relevance.

## Writting HTML

lintable rules for what the html ouput should be

## Writting CSS

lintable rules for what the CSS ouput should be

* other ideas *
## BEM

**Note:** I'd like to use BEM for the way we construct a component, I need to learn what is the norm in react about capitalization, I just think we need a rule for this so we all write things the same way.

for exmaple:

```
<Dialog>
  <Dialog__header>
  <Dialog__body>
  <Dialog__footer>
```


