# CSS

In general, we follow the [Block Element Modifier (BEM)](http://getbem.com/) methodology when defining CSS or SASS styles.

## Block

Blocks encapsulate standalone entities in your HTML. When working with UI frameworks, this often means a component’s outermost element, but blocks can be nested in the markup too.

Names should be self-descriptive and consist of letters, digits and dashes.

Avoid tag names or ids and, instead, use class name selectors. Blocks should not depend on other blocks or elements on a page.

```
// do this
.block {
  color: (--white);
}

```

## Element

Elements are semantically tied to the parent blocks. They have no standalone meaning.

Element names should extend the block naming conventions. Their name should include the block name, followed by two underscores and then the element name.

```
// do this
.block__elem {
  color: var(--white);
}

```

## Modifier

Modifiers are flags on blocks or elements. They represent changes in behavior and appearance. Their name should match that of the block or element they modify followed by two dashes and then a description of the intended modification.

```
// do this
.block__elem--small {
  padding: var(--spacing--s);
}

```

## Selectors

Unless necessary, avoid targeting elements by their selector directly in your CSS. Instead, add BEM-style classes to the element and target them instead.

For example, given the component:

```
<nav class="NavBar">
  <a class="NavBar__link" href=#">Link</a>
</nav>
// don't do this
.NavBar {
  a {
    text-decoration: none;
  }
}

// do this instead
.NavBar {
  &__link {
    text-decoration: none;
  }
}

```

## State

Changes in element state should be represented by a state class in the format .{verb}-{state}, following the structure defined by [the SMACCS architecture](http://smacss.com/).

```
// do this
.BulletPoint {
  color: black;
  &.is-active {
    color: red;
  }
}

```

## Custom properties

Commonly known as &quot;CSS variables&quot;.

### Defining a custom property

Custom properties _must_ be within a selector and start with two dashes

### Using a custom property
