# Implementing CSS Media Queries
This article suggests a pattern for implementing CSS breakpoints in your responsive frontend. It assumes that you have already [created and named](creating-and-naming-css-breakpoints.md) your breakpoints and that you're using a CSS pre-processor (LESS in this case).

## Issue
Media queries should be implemented as intuitively as possible, both for your semantic frontend as well as future/co-developers who may be on the project. Their values should be preserved for edge-case sub-breakpoints and they should be correctly ordered for simple overriding.

## Setting up breakpoints
Once you've established what you're breakpoints will be they should be intuitively broken up as part of your frontend CSS. They should fit in with other `layout` stylesheets.

#### 1. Establishing Units
In your stylesheet should be the preserved breakpoint values as you've determined them and should follow the naming convention you've established.

```less

/* Base Variables */
/* -----------------------------------------------*/

@wideUnit: 1600px;
@fullUnit: 1140px; // this is default grid width
@narrowUnit: 1024px;
@compactUnit: 700px;
@smallestUnit: 400px;

```

#### 2. Establishing media queries
Now that the breakpoint values are preserved for future use, wrap them in the desired media query for simple usage.

```less

/* Media Query Wrappers */
/* -----------------------------------------------*/

@wide: ~'(min-width:@{wideUnit})';
@full: ~'(max-width:@{wideUnit})';
@narrow: ~'(max-width:@{narrowUnit})';
@compact: ~'(max-width:@{compactUnit})';
@smallest: ~'(max-width:@{smallestUnit})';

```

#### 2.5. Establishing modifier media queries, if neccessary
It might be desired to create modified media queries that only occur in certain breakpoints and not others. Below are modifiers that only occur at certain breakpoints and larger, essentially removing them from certain implications of the cascade. Below is a modifier that occurs only within the scope of `narrow`.  These should follow the normal modifier pattern and used sparingly.

```less

@narrow--only: '(min-width:@{narrowUnit}) and (max-width:@{fullUnit})';

```

#### 3. Using media queries in context
Media queries should be treated as first class citizens in your stylesheets. This means using them in the context of the CSS you are modifying as a result of a viewport change. It also means establishing an easily overridable cascade of selectors. Modifications should be made from largest to smallest

```less

.person-info
{
  float: left;
  width: 70%;
  margin-left: 1rem;
  @media screen
  {
  	@media @narrow { width: 85%; }
  	@media @compact { width: 100%; }
  }
}

```

#### 4. Edge case media queries
Edge cases happen, handle them uniformly and frame the change in the context of established values. Here we have an element that needs to be a little wider between `@narrow` and `@compact`. Notice we're using LESS's Math functions to modify our breakpoint's unit value.

```less

.person-info
{
  @media screen
  {
    @media (max-width: @narrowUnit - 100px) { width: 90%; }
  }
}

```

Be sure to place them in the correct order of your other media queries to make overriding simple.

```less

.person-info
{
  @media screen
  {
  	@media @narrow { width: 85%; }
  	@media (max-width: @narrowUnit - 100px) { width: 90%; }
  	@media @compact { width: 100%; }
  }
}

```

*If you find yourself hardcoding the same modified media queries again and again, you might consider adding it to the full roster of media queries or at least a named modifier*
