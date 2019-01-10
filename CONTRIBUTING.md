# Contributing

## Running the project locally

[govuk-react](https://github.com/penx/govuk-react) is a [monorepo](https://github.com/babel/babel/blob/master/doc/design/monorepo.md) using [yarn workspaces](https://yarnpkg.com/blog/2017/08/02/introducing-workspaces/) and [lerna](https://github.com/lerna/lerna).

This is so that components can be published independently and applications can require different versions of a component if a breaking change is introduced in a version of a specific component. We are loosely following the [structure that Jest uses](https://github.com/facebook/jest).

As such, the build process for development is slightly more involved than an `npm install`:

1. Install yarn

2. Install dependencies, link packages, compile and start storybook:

```sh
yarn
yarn build
yarn start
```

## Creating a new component
To create a new component:
- `yarn create-component -- my-new-component` where _my-new-component_ is the name of your new component.

This creates a folder named _my-new-component_ in `src/components` with the component file (index.js), a basic render test (test.js), and a default story (stories.js). You will need to add this to `src/stories/index.js` to view it in storybook.


## Unit testing
Unit testing follows similar patterns as [Glamorous with Jest](https://github.com/paypal/glamorous/tree/master/examples/with-jest), utilising [Jest _snapshots_](https://facebook.github.io/jest/docs/en/snapshot-testing.html), and [Enzyme](https://github.com/airbnb/enzyme).

To run unit & eslint tests:
```sh
yarn test
```

To run & watch unit tests:
```sh
yarn test:unit
```

## Opening a pull request

- when adding a new component, include a screenshot of the component in the PR
- when adding new components, try to keep 1 component per PR, unless there are unavoidable interdependencies
- when fixing a visual issue, include a unit test or snapshot test that proves the issue, and include a screenshot of the component before and after in the PR
- when fixing a functional issue, include a unit test that proves the issue (fails before PR is merged and succeeds after)

## Releasing

In order to prepare a release:

- ensure you have lerna installed _globally_ (`npm i -g lerna`)
- ensure you have [hub](https://hub.github.com) installed (e.g. `brew install hub`)
- run `./scripts/release.sh`, you will be asked to choose a semver increment and, if this is the first time running hub, you will be asked for your GitHub credentials
- you will also be asked for a title and description for the draft release, though this can be left blank and filled in on GitHub later
- this will open a PR on GitHub and draft a release
- Edit the PR and write the release notes in the PR description
- get approval for the PR (reviewers should be reviewing the release notes in the PR description) then merge the PR
- once the PR is merged, open the [draft release corresponding to the new version number on GitHub](https://github.com/penx/govuk-react/releases), change the target branch to master, check the title and description and then click `Publish release`.

When the tag is created, the CI server will automatically release to npm using lerna exec, see:

- https://github.com/lerna/lerna/issues/1056#issuecomment-374192818
- https://github.com/lerna/lerna/issues/961

## Conventions

The following conventions are planned but not fully implemented. We should be consistent on these before releasing version 1. At the point where the library is consistent, the conventions should be moved to README.md rather than CONTRIBUTING.md.

### Fields vs. Inputs

For the purpose of this project:

- A field is the combination of a label, input(s) and validation/error messages. The Field components used by this project should follow the prop structure of Final Form and Redux Form fields, in that they accept an `input` prop and a `meta` prop.
- An input is just the form control that the user enters data in to, without a label or error/validation message. Inputs are normally associated with a Field and should be used on their own by importing the Field then using `<FieldName.Input />`. Inputs follow the prop structure of Final Form and Redux Form inputs.

More details in https://github.com/govuk-react/govuk-react/issues/164.

### Anchors

We want to be router agnostic. We want to support parent projects using React Router or Reach Router, without introducing either as a dependency of this project. As such we provide an `as` prop on components that we expect can be used as React Router Links or NavLinks. We recommend using the [`asNavLink` HOC](https://www.npmjs.com/package/as-nav-link). More details in https://github.com/govuk-react/govuk-react/issues/423.


### White Space

Components are built to have no white space around them, and are then wrapped with the withWhiteSpace HOC where we can provide some default values. This allows the parent application to override the defaults with e.g. an `mb` prop.

More details in https://github.com/govuk-react/govuk-react/issues/173