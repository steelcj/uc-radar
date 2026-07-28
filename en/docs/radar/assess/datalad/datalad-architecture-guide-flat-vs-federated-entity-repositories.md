# DataLad Architecture Guide: Flat vs Federated Entity Repositories

## Overview

This guide compares two canonical DataLad repository architectures for semantic container systems: the flat model and the federated (subdataset) model.

---

## 1. Flat Repository Model

```text
repository/
  entity-a/
    index.md
    meta/
    assets/

  entity-b/
    index.md
    meta/
    assets/
```

### Description

A single Git repository containing all entities as folders.

### Characteristics

- One DataLad dataset
- One Git repository
- Entities are simple directories

### Advantages

- Simple to set up
- Easy to understand
- Low operational overhead
- Good for small-to-medium projects

### Limitations

- No independent versioning per entity
- Limited reuse of individual entities
- Less suitable for distributed systems

### Best use cases

- Small archives
- Single-team projects
- Non-distributed workflows

### Flat Repository Model (DataLad) — Command Examples

## 1. Create the repository

```bash
datalad create repository
cd repository
```

## 2. Create entity folders (semantic containers)

```bash
mkdir -p entity-a/assets entity-a/meta
mkdir -p entity-b/assets entity-b/meta
```

------

## 3. Add content

```bash
echo "# Entity A" > entity-a/index.md
echo "# Entity B" > entity-b/index.md
```

------

## 4. Initialize DataLad tracking (if not already initialized)

```
datalad save -m "Add initial flat entity structure"
```

------

## 5. Check status

```
datalad status
```

------

## 6. Update an entity

```
echo "update" >> entity-a/index.md
datalad save -m "Update entity-a content"
```

------

## 7. Optional: view history

```
git log --oneline
```

------

## Summary

- One DataLad dataset
- Entities are just folders
- No subdatasets involved

------

## 2. Federated Entity Model (Subdatasets)

```text
repository/        (DataLad dataset)
  entity-a/        (subdataset)
    index.md
    meta/
    assets/

  entity-b/        (subdataset)
    index.md
    meta/
    assets/
```

### Description

Each entity is its own Git repository managed as a DataLad subdataset.

### Characteristics

- Multiple Git repositories
- Each entity is independently versioned
- Repository acts as a federation layer

### Advantages

- Full entity independence
- Strong reuse across projects
- Supports distributed systems
- Scales across institutions
- Aligns with DataLad design philosophy

### Limitations

- Higher complexity
- Requires disciplined synchronization
- More setup overhead

### Best use cases

- Large-scale archives
- Research or institutional systems
- Multilingual or modular knowledge systems
- Federated or distributed environments

### Federated Entity Model (DataLad Subdatasets) — Command Examples

1. Create root repository (federation hub)

```bash
datalad create repository
cd repository
```

2. Add first entity as a subdataset

```
datalad create -d . entity-a
```

This creates:

- `entity-a/` as its own Git repository (subdataset)

3. Initialize structure inside entity-a

```
mkdir -p entity-a/assets entity-a/meta
echo "# Entity A" > entity-a/index.md
```

4. Save entity-a dataset state

```
cd entity-a
datalad save -m "Initialize entity-a"
cd ..
```

5. Add second entity as subdataset

```
datalad create -d . entity-b
```

6. Initialize entity-b content

```
mkdir -p entity-b/assets entity-b/meta
echo "# Entity B" > entity-b/index.md
```

7. Save entity-b

```
cd entity-b
datalad save -m "Initialize entity-b"
cd ..
```

8. Register subdatasets in parent repository

```
datalad save -m "Add federated entities (subdatasets)"
```

9. Clone entire federated structure (example)

```
datalad clone <repository-url>
```

10. Update a single entity independently

```
cd entity-a
echo "update" >> index.md
datalad save -m "Update entity-a independently"
```

11. Pull updates from remote (federation sync)

```
datalad update --merge
```

------

## Summary

- Each entity is a **subdataset (own Git repo)**
- Root repository acts as **federation layer**
- Entities can be independently updated, cloned, and synced
- Strong support for distributed and multilingual architectures

------

## 3. Core Comparison

| Feature          | Flat Model | Federated Model |
| ---------------- | ---------- | --------------- |
| Git repositories | 1          | many            |
| Complexity       | low        | medium/high     |
| Scalability      | medium     | high            |
| Entity reuse     | limited    | strong          |
| Independence     | low        | high            |

------

## 4. Conceptual Summary

- Flat model = centralized folder-based archive
- Federated model = network of independent datasets

------

## 5. Architectural Guidance

- Use flat model for simplicity and early-stage design
- Use federated model for scalability, reuse, and distributed systems
- Hybrid transitions are possible (start flat, evolve to federated)

------

## Keywords (CAP)

DATA LAD
SEMANTIC CONTAINERS
FEDERATED ARCHITECTURE
FLAT REPOSITORY MODEL
SUBDATASETS
VERSIONED DATA SYSTEMS
ARCHIVAL DESIGN
DISTRIBUTED KNOWLEDGE SYSTEMS

