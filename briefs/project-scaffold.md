# Brief: Project Scaffold

## Objective

Create a starter project generator that uses this use-case pack as its default example domain.

## What To Consume

- shared schemas in `contracts/`
- all fixture sets under `fixtures/`
- flow definitions in `docs/use-case-definition.md`

## What The Example Should Demonstrate

- local config bootstrapping
- fixture loading
- mock tool registration
- one entrypoint per sub-scenario
- trace capture using the shared `RunTrace` shape
- a path for running a shared eval row locally

## Minimum Done Definition

- generated project can mount the local fixtures
- generated project exposes a way to run the flagship doctor sample-order flow
- generated project keeps integrations mocked and local-only
- generated project clearly points adopters back to this repo for scenario truth
