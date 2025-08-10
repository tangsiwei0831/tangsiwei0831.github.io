---
title: 'Test Coverage'
date: 2025-08-09
permalink: /posts/testCoverage
tags:
  - Software
---
In my second rotation, I used to help to create test coverage workflow to accommodate both `Jest` and `Vitest` two frameworks.

As `nyc` package only aworks with Jest well. For `Vitest`, it needs some special configuration.In `vitest.config.ts`, add below section
For `Jest`, configure output directory to be `.nyc_output`.
```
    coverage: {
        provider: 'Istanbul',
        reporter: ['json', 'json-summary']
    }
```

So in the repo, you will either use `jest --coverage --coverageReporters=json` or `vitest --coverage` as your `npm run test` command in `package.json`.

Then in the behind scene github action workflow, how do we accmmodate these two frameworks?

For `Jest`, command will be run as below
```
    # below command all run in package.json located folder
    nyc --all jest --coverage --coverageReporters=json
    nyc merge .nyc_output raw-coverage.json
    nyc report --reporter=json-summary --report-dir=coverage
```

For Vitest, just run the command, then it is fine.


For airflow test coverage, we use [python coverage.py](https://coverage.readthedocs.io/en/7.8.2/) to accompolish the goal, several key things to note:
1. report generates in XML format, then we convert to json format by custom code if needed
2. Airflow test should not be running locally, instead it should run in cloud(GCP composer)