# Disable Azure Function

![](https://badgen.net/badge/icon/gitguardian/green?icon=gitguardian&label)
[![Linux runner](https://github.com/JosiahSiegel/disable-azure-function/actions/workflows/test_linux_runner.yml/badge.svg)](https://github.com/JosiahSiegel/disable-azure-function/actions/workflows/test_linux_runner.yml)
[![Windows runner](https://github.com/JosiahSiegel/disable-azure-function/actions/workflows/test_windows_runner.yml/badge.svg)](https://github.com/JosiahSiegel/disable-azure-function/actions/workflows/test_windows_runner.yml)

## Synopsis

Disable or enable a function within an Azure Function App.

## Inputs

```yml
inputs:
  function-app-name:
    description: "Function app name"
    required: true
  resource-group-name:
    description: "Resource group name"
    required: true
  function-name:
    description: "Function name"
    required: true
  disable:
    description: "Disable function: true/false"
    required: true
```

## Quick start

`sample_workflow.yml`
```yml
jobs:
  set_functions:
    runs-on: ubuntu-latest
    outputs:
      functions: "[\"batch\",\"send\"]"
    steps:
      - name: Check out repository
        uses: actions/checkout@v2
  run_local_action:
    runs-on: ubuntu-latest
    needs:
      - set_functions
    strategy:
      matrix:
        function: ${{ fromJson(needs.set_functions.outputs.functions) }}
    steps:
      - name: Check out repository
        uses: actions/checkout@v2
      - name: Login to Azure
        uses: azure/login@v1
        with:
          creds: ${{ secrets.SERVICE_PRINCIPAL_CREDS }}
      - name: Disable function
        uses: JosiahSiegel/disable-azure-function@v1
        with:
          function-app-name: ${{ secrets.TEST_FUNCTION_APP }}
          resource-group-name: ${{ secrets.TEST_RG }}
          function-name: ${{ matrix.function }}
          disable: true
```
