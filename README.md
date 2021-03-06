# Continuous Reforestation

[<img src="logo.svg" align="right" width="250">](https://github.com/protontypes/continuous-reforestation)
A GitHub Action for planting trees within your development workflow using the Reforestation as a Service (RaaS) API developed by [DigitalHumani](https://digitalhumani.com/). Planting trees is an easy way to make a difference in the fight against climate change. Every tree helps to bind CO2 as long as it grows and creates living space for wildlife. Automating the process gives you total control of where, when and how much you want to contribute while saving you the fuss of doing the whole process manually. By using the RaaS API, you or your project can plant trees in a transparent way by exposing the API calls and related statistics. <br>  <br>
[![Actions Status](https://github.com/protontypes/continuous-reforestation/workflows/Lint/badge.svg)](https://github.com/jacobtomlinson/protontypes/continuous-reforestation/actions)
[![Actions Status](https://github.com/protontypes/continuous-reforestation/workflows/Integration%20Test/badge.svg)](https://github.com/protontypes/continuous-reforestation/actions)
[![](https://badgen.net/badge/icon/Community%20Chat/green?icon=gitter&label)](https://gitter.im/protontypes/community)

## Use cases
Plant trees on ...
* pull requests (and/or push, ...).
* failed or successful tests.
* the very first contribution to an open source project.
* a new release, a milestone, or a closed issue.
* a scheduled event (i.e. once per week).
* to compensate the carbon footprint when deploying digital product.
* See more possible trigger events [here](https://docs.github.com/en/actions/reference/events-that-trigger-workflows).

## Usage

1. 🏁 To get started, you need an account with DigitalHumani RaaS. Since they are currently in the early stages, you have to contact them to get an account. Send them an email [here](https://digitalhumani.com/#contact).

2. Copy the example worflow to `<your_git_repository>/.github/workflow/integration.yaml` and change the variables in the workflow to your data. Set the `production` variable to `false` to test your implementation within the sandboxed development API. Push your script to GitHub and check the GitHub Action tab of your project.

3. 📈 A dashboard is provided to ensure a high level of transparency. This is currently under development and will provide additional details. For this purpose visit:
``
https://digitalhumani.com/dashboard/<enterpriseid>
``

4. Verify the number of trees planted in the dashboard development statistics. After a successful run add the API key to the GitHub secrets of your project our organizations as `raaskey`

To see a list of all supported reforestation projects and more details on the RaaS API read the [documentation of DigitalHumani](https://digitalhumani.com/docs/#appendixlist-of-projects).

### Example workflow

```yaml
name: Integration Example
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Plant a Tree
        id: planttree
        uses: protontypes/continuous-reforestation@main
        with:
        # Enter your API variables below
            apikey: ${{ secrets.raaskey }}
            enterpriseid: "cd7cedcd"
            user: ${{ github.actor }}
            treecount: 10
            projectid: "92222222" # onetreeplanted.org/collections/latin-america/products/brazil-forests
            production: "true"

      - name: Response of digitalhumani.com RaaS API
        run: |
            echo "${{ steps.planttree.outputs.response }}"
```
---

### Inputs

| Input            | Description                           |
|------------------|---------------------------------------|
| `enterpriseid`   | ID of your enterprise.                |
| `user`           | End user by whom the trees were planted. Default is your GitHub user name. |
| `projectid`      | ID of the reforestation project for where you want the trees to be planted.    |
| `treeCount`      | Number of trees requested to plant per API call as integer. Every tree will create costs of 1$ per tree |
| `production`     | Set 'true' for the production API or false for the development API  |

### Outputs

| Output           | Description                           |
|------------------|---------------------------------------|
| `response`       | JSON response of the RaaS API |
