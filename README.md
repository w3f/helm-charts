# Helm Charts

Registry for Helm charts published from other repositories. The `gh-pages` branch holds the
packaged charts and the index, served by GitHub Pages:

```
helm repo add w3f https://w3f.github.io/helm-charts/
```

## Publishing a chart

Service repositories publish through the reusable workflow in
[`.github/workflows/publish-chart.yml`](.github/workflows/publish-chart.yml). Add a job to the
repository's own workflow, typically after its image has been pushed:

```yaml
publish-chart:
  uses: w3f/helm-charts/.github/workflows/publish-chart.yml@master
  with:
    chart-path: deployment/chart
  secrets:
    HELM_CHARTS_TOKEN: ${{ secrets.HELM_CHARTS_TOKEN }}
```

`HELM_CHARTS_TOKEN` is a token with write access to this repository. The workflow lints and
packages the chart, merges it into `index.yaml` and pushes to `gh-pages`. A chart version that is
already published is skipped, so bump `version` in `Chart.yaml` to release.
