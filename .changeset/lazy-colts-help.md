---
'@backstage/cli': patch
'@backstage/core-app-api': patch
'@backstage/core-components': minor
'@backstage/core-plugin-api': patch
'@backstage/create-app': patch
'@backstage/dev-utils': patch
'@backstage/integration-react': patch
'@backstage/test-utils': patch
'@backstage/theme': patch
'@backstage/plugin-api-docs': patch
'@backstage/plugin-badges': patch
'@backstage/plugin-bitrise': patch
'@backstage/plugin-catalog': patch
'@backstage/plugin-catalog-import': patch
'@backstage/plugin-catalog-react': patch
'@backstage/plugin-circleci': patch
'@backstage/plugin-cloudbuild': patch
'@backstage/plugin-code-coverage': patch
'@backstage/plugin-config-schema': patch
'@backstage/plugin-cost-insights': patch
'@backstage/plugin-explore': patch
'@backstage/plugin-fossa': patch
'@backstage/plugin-gcp-projects': patch
'@backstage/plugin-git-release-manager': patch
'@backstage/plugin-github-actions': patch
'@backstage/plugin-github-deployments': patch
'@backstage/plugin-gitops-profiles': patch
'@backstage/plugin-graphiql': patch
'@backstage/plugin-ilert': patch
'@backstage/plugin-jenkins': patch
'@backstage/plugin-kafka': patch
'@backstage/plugin-kubernetes': patch
'@backstage/plugin-lighthouse': patch
'@backstage/plugin-newrelic': patch
'@backstage/plugin-org': patch
'@backstage/plugin-pagerduty': patch
'@backstage/plugin-register-component': patch
'@backstage/plugin-rollbar': patch
'@backstage/plugin-scaffolder': patch
'@backstage/plugin-search': patch
'@backstage/plugin-sentry': patch
'@backstage/plugin-shortcuts': patch
'@backstage/plugin-sonarqube': patch
'@backstage/plugin-splunk-on-call': patch
'@backstage/plugin-tech-radar': patch
'@backstage/plugin-techdocs': patch
'@backstage/plugin-todo': patch
'@backstage/plugin-user-settings': patch
'@backstage/plugin-welcome': patch
'@backstage/plugin-xcmetrics': patch
---

- Bumping `material-ui/core` version to at least `4.12.2` as they made some breaking changes in later versions which broke `Pagination` of the `Table`.
- Switching out `material-table` to `@material-table/core` for support for the later versions of `material-ui/core`
- This causes a minor API change to `@backstage/core-components` as the interface for `Table` re-exports the `prop` from the underlying `Table` components.
- `onChangeRowsPerPage` has been renamed to `onRowsPerPageChange`
- `onChangePage` has been renamed to `onPageChange`
- Migration guide is here: https://material-table-core.com/docs/breaking-changes
