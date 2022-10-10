# Designing Redux State

There are two categories of things we store in Redux State

## Types of Data

### Reference Data

Reference data is data we need to have available for the user to support business transactions (side-effects). Product lists, shipping schedules, etc.

### Application State

This can be anything from UI configuration, data specific to the user (profile information, etc.)

## Shape of Data

Data in the store is either scalar values or lists of entities. When storing lists of entities, use `@ngrx/entity` with the adapter.

## Loading Data

**Eager** loading means to load it _before_ you anticipate the user will need the data.

Best if:

- Data is fairly static
- Not concurrent (multiple people editing/deleting/adding)
- Comes from external APIs (outside your control)
- Datum can be identified with a version.
  - e.g. a price list could have a version identifier (July 2022)

Issues:

- Data can become very stale - may need to be reloaded.
- For very static data, should be cached.

**Lazy** loading means you load the data _as_ the user needs it.

Best if:

- Data is less static / more concurrent
- Comes from your API (BFF)
- Can be retrieved _quickly_

## The URL As Primary State

The displayed URL in the user's browser is the most significant indicator of state in the application.

```
http://localhost:4200/employees?dept=DEV

http://localhost:4200/employees/93893
```

**Access to the state by loading the application through a URL visit must be ensured**

### Pattern: Loading State Conditionally on Router Events

Consider assuming state will be lazy-loaded based on the route.

Selector functions in combination with router actions indicate whether the data should be loaded.

```ts
export const selectEmployeesNeedLoaded = createSelector(() => true); // HC at start

export const selectEmployeeNeedsLoaded(id:string) => createSelector( /*imp */)
```

## Action Design

Three categories of actions:

### Events

No explicit cause and effect (loose coupling).

Events are indications that something has happened. Expressed as a past-tense term.

```ts
export const EmployeeEvents = createActionGroup({
  source: "Employee Events",
  events: {
    selected: props<{ payload: { id: string } }>(),
    fired: props<{ payload: { id: string } }>(),
    // etc.
  },
});
```

Components are the primary source of events. Be skeptical of any actions from components that aren't events.

### Commands

Cause and effect are more explicit. Commands assume something is going to happen as a result.

Names are verbs.

> An Event may turn into several commands. This is a "Fan out"

Examples:

```ts
export const EmployeeCommands = createActionGroup({
  source: "Employees Commands",
  events: {
    load: emptyProps(),
    fire: props<{ payload: { id: string } }>(),
    create: props<{ payload: EmployeeCreateRequest }>(),
    // etc.
  },
});
```

### Documents

These are the things that reducer actually needs. They are _often_ the "effect" of the command _cause_.

Names are nouns.

```ts
export const EmployeeDocuments = createActionGroup({
  source: "Employees Documents",
  events: {
    employee: props<{ payload: EmployeeEntity }>(),
    employees: props<{ payload: EmployeeEntity[] }>(),
    // etc.
  },
});
```
