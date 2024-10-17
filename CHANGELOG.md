# buildevents-orb changelog

## [0.12.0] - 2024-10-17

### Enhancements

- feat(w3c): partially implement w3c tracecontext (#110) | [Liz Fong-Jones](https://github.com/lizthegrey)
- feat(w3c): add BE trace_id and TRACEPARENT to env (#114) | [Liz Fong-Jones](https://github.com/lizthegrey)

### Maintenance

- maint(deps): upgrade buildevents binary (#111) | [Liz Fong-Jones](https://github.com/lizthegrey)
- docs: update vulnerability reporting process (#109) | [Robb Kidd](https://github.com/robbkidd)
- maint: update repo for pipeline team ownership (#108) | [Jamie Danielson](https://github.com/JamieDanielson)
- change codeowners to pipeline (#107) | [Brooke Sargent](https://github.com/brookesargent)

## [0.11.1] - 2024-03-19

### Fixed

- job names containing spaces should still work (#105) | [@ismith](https://github.com/ismith)

## [0.11.0] - 2024-03-01

### Fixed

- Don't expand $PATH when writing to BASH_ENV (#98) | [@ravron](https://github.com/ravron)

### Maintenance

- bump to buildevents v0.16.0 (#103) | [@cewkrupa](https://github.com/cewkrupa)
- add docs section about add_context pitfalls (#102) | [@dstrelau](https://github.com/dstrelau)
- add CI integration test (#101) | [@dstrelau](https://github.com/dstrelau)

## [0.10.2] - 2023-07-18

### Fixed

- update cache key to handle empty caches after a v0.10.0 run (#96) - [@jharley](https://github.com/jharley)

## [0.10.1] - 2023-07-18

### Fixed

- paths for arm binaries (#94) - [@ismith](https://github.com/ismith)

## [0.10.0] - 2023-07-17

### Enhancements

- Reduce final orb output size (#91) - [@RainofTerra](https://github.com/RainofTerra)

### Maintenance

- bump buildevents from 0.14.0 to 0.15.0 (#92) - [@jharley](https://github.com/jharley)
- bump buildevents from 0.13.0 to 0.14.0 (#90) - [@jharley](https://github.com/jharley)

## [0.9.0] - 2022-09-12

### Enhancements

- Support benames with spaces (#77) - [@abangser](https://github.com/abangser)

### Fixed

- Wrap additional env var usage with double quotes (#75) - [@abangser](https://github.com/abangser)

NOTE: The changes to replace single quotes with double quotes to correctly evaluate environment variables effectively reverts #53.
You may need to reset your $PATH afterwards if affected (see issue for more details).

## [0.8.1] - 2022-09-06

### Fixed

- Use double-quotes for BUILDEVENTS_SPAN_ID (#73) | [@lirossarvet](https://github.com/lirossarvet)

## [0.8.0] - 2022-08-26

### Maintenance

- bump buildevents from 0.12.1 to 0.13.0 (#70) | [@vreynolds](https://github.com/vreynolds)

## [0.7.1] - 2022-07-20

### Maintenance

- Bump buildevents from 0.12.0 to 0.12.1 (#68) | [@kentquirk](https://github.com/kentquirk)

## [0.7.0] - 2022-07-14

### Maintenance

- Bump buildevents from 0.9.2 to 0.12.0 (#65, #66) | [@vreynolds](https://github.com/vreynolds) & [@robbkidd](https://github.com/robbkidd)

## [0.6.0] - 2022-05-04

### Maintenance

- Bump buildevents from 0.8.0 to 0.9.2 (#61) | [@vreynolds](https://github.com/vreynolds)

## [0.5.0] - 2022-01-18

### Improvements

- lightweight command to create a build marker (#57) | [@nelzkiddom](https://github.com/nelzkiddom)

### Maintenance

- Add re-triage workflow (#56) | [@vreynolds](https://github.com/vreynolds)
- update build events to 0.7.2 (#59) | [@MikeGoldsmith](https://github.com/MikeGoldsmith)
- update build events to 0.8.0 (#60) | [@MikeGoldsmith](https://github.com/MikeGoldsmith)

## [0.4.1] - 2021-12-16

### Fixed

- Use single quoted bash strings (#53) | [@mbilokonsky](https://github.com/mbilokonsky)

### Maintenance

- Don't run publish-dev step for dependabot and PRs (#54) | [@MikeGoldsmith](https://github.com/MikeGoldsmith)

## [0.4.0] - 2021-11-19

### Added

- Track job status (#50) | [@ryanking](https://github.com/ryanking)

### Maintenance

- Bump buildevents from 0.7.0 to 0.7.1 (#51) | [@vreynolds](https://github.com/vreynolds)

## [0.3.2] - 2021-11-03

### Maintenance

- bump buildevents to v0.7.0 (#48)
- empower apply-labels action to apply labels (#47)
- Change maintenance badge to maintained (#45)
- Adds Stalebot (#46)

## [0.3.1] - 2021-09-24

### Added

- Allow busting the buildevents cache (#42) | [@nelzkiddom](https://github.com/nelzkiddom)

### Maintenance

- Add NOTICE (#41)
- Add issue and PR templates (#40)
- Add OSS lifecycle badge (#39)
- Add community health files (#38)
- Add pipeline parameter notes to README (#37)

## 0.3.0 2021-07-16

### Added

- update to buildevents to 0.6.0 (#30) | [@MikeGoldsmith](https://github.com/MikeGoldsmith)

### Maintenance

- add codeowners (#28) | [@MikeGoldsmith](https://github.com/MikeGoldsmith)
- Updates Github Action Workflows (#27) | [@bdarfler](https://github.com/bdarfler)
