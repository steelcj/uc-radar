## FastAPI

**Category:** Web framework
**Status:** Considering
**URL:** https://fastapi.tiangolo.com

### Why interesting

Natural fit for the SAT web API tier. Python-native, consistent with the
existing SAT toolchain. Auto-generates OpenAPI documentation from code,
which would give SAT a self-documenting API with minimal effort.
Async-capable for long-running transmogrification jobs.

### Concerns

Premature until the SAT engine and driver interface contract are stable.
Introducing it too early risks building the API around current limitations
rather than the intended architecture.

### Relationship to SAT

Would sit between the future web UI and the SAT engine:
```
web UI  →  SAT API (FastAPI)  →  SAT engine (core + drivers)
```

### Status notes

Park until the driver interface contract is defined and at least two
drivers are stable. Revisit when the engine is ready to be wrapped.