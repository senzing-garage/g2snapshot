# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
[markdownlint](https://dlaa.me/markdownlint/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.2.5] - 2024-06-26

### Added in 2.2.5

- fixed bug where cross source summary did not tie out

## [2.2.4] - 2024-02-05

### Added in 2.2.4

_note: version jumped to align with G2Explorer accompanying G2Explorer release_

- data and crossSourceSummary drill downs now compute stats by matchKey
- new principlesUsed report that computes stats by matchKey globally
- new multiSource report that computes stats for entities with multiple sources

## [2.1.4] - 2023-04-06

### Added in 2.1.4

- Computes entities with relations stats for data and cross source summaries
- Removed support for versions prior to 3.0

## [2.1.3] - 2023-01-12

### Added in 2.1.3

- Corrected cross data source record counts

## [2.1.2] - 2022-09-02

### Added in 2.1.2

- Timing issue fix where process queue could be falsely empty

## [2.1.1] - 2022-08-11

### Added in 2.1.1

- Improved support for `SENZING_ENGINE_CONFIGURATION_JSON`

## [2.1.0] - 2022-08-04

### Added in 2.1.0

- Support for `SENZING_ENGINE_CONFIGURATION_JSON`

### Changed in 2.1.0

- Improved backward compatibility

## [2.0.2] - 2022-06-08

### Added to 2.0.2

- Alter the G2IniParams import, after it was removed from the SDK.

## [2.0.1] - 2022-05-20

### Added to 2.0.1

- Add support for SenzingAPI 2.x

## [2.0.0] - 2022-05-04

### Added to 2.0.0

- Support SenzingAPI 3.0.0

## [1.3.0] - 2021-08-05

### Added to 1.3.0

- Shipped with SenzingAPI 2.8.1

## [1.2.0] - 2021-06-14

### Added to 1.2.0

- Shipped with SenzingAPI 2.7.0

## [1.1.0] - 2021-04-15

### Added to 1.1.0

- Shipped with SenzingAPI 2.5.0

## [1.0.0] - 2020-07-16

### Added to 1.0.0

- Shipped with SenzingAPI 2.0.0
