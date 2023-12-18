# React

:::note
This document is a work in progress. It is not complete and will be updated as we go.
:::

## Classes, components and constructor functions

Any function that will be utilized as a prototype/template to generate objects should be defined with the first letter of the name capitalized. In practice, for us this generally means React components, however, it could also mean JavaScript classes and, in case you are reading this in 2008, constructor functions.

```typescript
import React from "react";

// don't do this
export default function homePage() {
  return <div />;
}

// do this instead
export default function HomePage() {
  return <div />;
}
// do this
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```

## Always declare components as functions

In React, we can declare components either through an ES6 `class` or as functions. With the introduction of hooks, most use cases for class components fell short, and even React recommends [moving away from them](https://reactjs.org/docs/hooks-intro.html#classes-confuse-both-people-and-machines) whenever possible.

```xml
// don't do this
class Button extends React.PureComponet {
  render() {
    return <button>Click!</button>
  }
}

// do this
function Button() {
  return <button>Click!</button>
}

```

## Queries and mutations

We will always name the variable ending in query or mutation as appropriate:

```tsx
const usersQuery;
const createUserMutation;
```

When naming functions, we will follow the following structure: use | action | resource | type:

- use will be used to indicate a hook.
- action will correspond to the action we are performing on the resource (list, get, update, delete…).
- resource will refer to the resource we are working with.
- type will be according to whether it corresponds to a query or a mutation.

For more information on how it works and the advantages of using this library, see [React Query](https://react-query.tanstack.com/).

```tsx
useListUsersQuery();
CreateUserMutation();
```

**Why?**

This makes it easier to identify how the information is being treated and therefore easier to find.

## Project structure

This section is unfinished

**React applications** are generally structured as follows:

- `src/`
  - `api-client/`
    - `apiclient.service.ts`
  - `components/`
    - `Notifications/`
      - `Notifications.tsx`
      - `Notifications.spec.tsx`
      - `useNotifications.hook.ts`
      - `Notifications.module.scss`
  - `hooks/`
  - `pages`/
    - `home`
      - `components/`
        - …
      - `Home.page.tsx`
      - `Home.spec.tsx`
      - `Home.module.scss`
  - `services/`
  - `types/`
  - `utils/`

## Component structure

A component is usually made with different modules (the source code for the component, its styling, its tests, services, stories, etc…). We group all these files within a single folder named after the component:

```
MyComponent/
  MyComponent.tsx
  MyComponent.spec.tsx
  MyComponent.stories.tsx
  useMyComponent.ts

```

Each of the files should be named with the component name so that when searching them is easy to identify to which component they relate. Here are the naming conventions for each of the files:

- View → `{ComponentName}.tsx`
- Business logic → `use{ComponentName}.tsx`
- Styles → `{ComponentName}.module.scss`
- Tests → `{ComponentName}.spec.tsx`
- Storybook stories → `{ComponentName}.stories.tsx`

## Separation of concerns

When writing a new component, keep the view logic within the main component file and move the business logic to a hook:

```javascript
//❗Don't do this
// UserPage.tsx
export default function UserPage() {
  const session = useSession();
  const userQuery = useUserQuery();
  const navigate = useNavigate();
  // ...
  return (
    <View>
     <Button onClick={() => navigate(TRANSACTIONS_ROUTE)}>View transactions</View>
  // ...
}

// ✅ Do this
// UserPage.tsx
export default function UserPage() {
  const { companyName, onViewTransactions, userDetails } = useUserPage();
  // ...
  return (
    <View>
     <Button onClick={onViewTransactions}>View transactions</View>
  // ...
}
// useUserPage.ts
export default function useUserPage() {
  const session = useSession();
  const userQuery = useUserQuery();
  const navigate = useNavigate();
  // ...
  return {
    companyName: session.companyName,
    onViewTransactions: () => navigate(TRANSACTIONS_ROUTE),
    userDetails: userQuery.data?.details
  // ...
}

```

This allows us to refactor and share code across the project easier. It also sticks to the convention that component file is only in charge to render the view with the parameters injected by a hook which acts as a “controller”.

## Sub-components

Whenever a component has sub-components, place them in a `components` folder relative to the current component folder following a recursive architecture.

```
MyComponent/
  components/
    ComponentFoo/
      ComponentFoo.tsx
    ComponentBar/
      ComponentBar.tsx
      useComponentBar.ts
  MyComponent.tsx
  MyComponent.spec.tsx
  useMyComponent.ts

```
