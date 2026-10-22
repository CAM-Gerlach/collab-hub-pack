# E2E image for the Cog-execution kind test. Runs the real package under test —
# collab_hub_execution is copied verbatim from its source. Serves as both the Cog
# worker runner (default CMD) and the Op driver (the Job overrides the command).
# Build context is the repo root.
FROM python:3.13.17-slim
WORKDIR /app
# The 3.13.17 pin is what guarantees a clean bootstrap pip: its ensurepip
# ships pip 26.2.2, whose vendored certifi (2026.10.19) excludes the
# compromised eMudhra roots. The pip floor below keeps a later rebase from
# regressing that, and the certifi floor does the same for httpx's bundle.
RUN python -m pip install --no-cache-dir --upgrade "pip>=26.2.2" \
 && python -m pip install --no-cache-dir "httpx>=0.28.0" "certifi>=2026.10.19"
# The real package under test.
COPY execution/src/collab_hub_execution /app/collab_hub_execution
COPY scripts/cog-execution-e2e/runner.py scripts/cog-execution-e2e/driver.py /app/e2e/
ENV PYTHONPATH=/app
CMD ["python", "-m", "e2e.runner"]
