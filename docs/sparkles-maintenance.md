# Managed aiohttp compatibility patch

**Keywords:** aioresponses aiohttp stream_writer feed_data Python Ring security compatibility fork

## aioresponses source and scope

Base: upstream 0.7.9, `f95f26bdf226b0c8329e7d4e96b623a3dc33ab22`. The aiohttp 3.14 backport follows upstream PR 292, `1a48e1f898035e3bedc981f06520842c55977706`, with forwarding-constructor coverage. Poetry Core builds static metadata without executing the legacy PBR version plugin. Public version remains 0.7.9; consumers must pin the full fork Git SHA. Actions stays disabled; fleet hooks run locally.

## aioresponses qualification and retirement

Build a wheel from clean committed source with `uv build --wheel --out-dir <new-artifacts>/wheels`. From an empty directory, run `uvx --python 3.12 --with <wheel> --with ddt --with pytest-asyncio --with pytest-socket --from pytest pytest -q -W error --import-mode=importlib --disable-socket --allow-unix-socket --allow-hosts=127.0.0.1 -c <repo>/pyproject.toml <repo>/tests`. Clear inherited `PYTEST_ADDOPTS`. Keep source SHA, wheel hash, exact test package versions, and JUnit results. All tests must pass, including large streaming bodies, custom response constructors, and real loopback passthrough. No external service is needed.

When upstream publishes these fixes, qualify its installed wheel, then replace the Ring development dependency's fork pin. A failed run blocks replacement; revert the consumer pin to the previously qualified SHA. This fork serves tests and has no daemon deployment.
