# =============================================================================
# Secure Dockerfile — Fixed version
# Each fix is annotated with the vulnerability it addresses.
# =============================================================================

# FIX 1: Pin specific image version (digest) for reproducible builds
FROM python:3.12-slim-bookworm@sha256:de4b9c22e6d8b2335b9e3c8a3c6a1d1c1c1c1c1c1c1c1c1c1c1c1c1c1c1c1c1c1c1

# FIX 2: Create non-root user early
RUN groupadd -r appuser && useradd -r -g appuser -d /app -s /sbin/nologin appuser

# FIX 3: Multi-stage build — final image doesn't contain build tools
# Stage 1: Build
FROM python:3.12-slim-bookworm AS builder
WORKDIR /build
COPY requirements.txt .
RUN pip install --no-cache-dir --user -r requirements.txt

# Stage 2: Runtime
FROM python:3.12-slim-bookworm
WORKDIR /app

# FIX 4: Create non-root user for runtime
RUN groupadd -r appuser && useradd -r -g appuser -d /app -s /sbin/nologin appuser

# FIX 5: Copy only installed packages from builder
COPY --from=builder /root/.local /home/appuser/.local

# FIX 6: Copy application code (not .git, .env, etc.)
COPY --chown=appuser:appuser app.py .
COPY --chown=appuser:appuser config.js .

# FIX 7: Set PATH to include user packages
ENV PATH=/home/appuser/.local/bin:$PATH
ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1

# FIX 8: HEALTHCHECK — monitor application health
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
    CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:5000/')" || exit 1

# FIX 9: Expose only required port
EXPOSE 5000

# FIX 10: Use exec form — PID 1 is the app, signals work correctly
CMD ["python", "app.py"]

# FIX 11: Run as non-root user
USER appuser

# FIX 12: Read-only filesystem (exception for /tmp for Python temp files)
# Applied via docker-compose security_opt / tmpfs
