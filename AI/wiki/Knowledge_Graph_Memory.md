---
title: "Knowledge Graph Memory"
parent: "[[Agentic_Memory_System]]"
aliases: ["graph-memory", "ontology-memory", "schema-controlled-memory", "pydantic-memory"]
tags: ["memory", "knowledge-graph", "ontology", "vector-db", "retrieval"]
category: memory-systems
created: 2026-05-28
date: "2026-05-28"
stub: false
---

# Knowledge Graph Memory

**Core problem**: Vector-based memory stores facts as chunks and retrieves by semantic similarity. Multi-hop reasoning queries — connecting facts across chunks — require traversal, not matching. The solution is a schema-controlled knowledge graph where extraction is guided by a domain ontology.

> "Agent memory without schema discipline is a graph that behaves like a vector store."

[Source: raw/Pydantic fixed my Agent's Memory.md]

## The Multi-Hop Failure of Flat Retrieval

Three facts about a project:
1. Alice manages Project Atlas
2. Project Atlas runs on PostgreSQL  
3. The PostgreSQL cluster went down Tuesday

Query: "Was Alice's project affected by Tuesday's outage?"

Vector search returns facts 1 and 3 (both mention relevant terms). Fact 2 — the bridge — mentions neither "Alice" nor "Tuesday". **Similarity search misses the connecting edge.**

Knowledge graph stores Alice → manages → Project Atlas → runs on → PostgreSQL as traversable nodes/edges. This chain is invisible to flat vector retrieval but essential for multi-hop reasoning.

## Why Unguided Extraction Fails

Most frameworks have a black-box extraction step:
1. Pass in text
2. LLM decides entity types, relationship labels, attributes on its own
3. Results: generic ("Topic" nodes, "Object" nodes, "RELATES_TO" edges)

When the agent queries "Which enterprise customers have open sev-1 tickets?" against a graph where every ticket is a "Topic" and every customer is an "Object" — no structured filtering is possible.

**Root cause**: LLM extraction without domain vocabulary produces structurally valid but semantically useless graphs.

## Pydantic Ontology Pattern (Zep/Graphiti)

Define entity and edge types using Pydantic models. Docstrings and field descriptions teach the extractor domain vocabulary.

```python
class Project(EntityModel):
    """Represents a specific software project the user is building."""
    project_status: EntityText = Field(
        description="Current status: active, completed, paused, or archived."
    )
    project_type: EntityText = Field(
        description="Type: web app, mobile app, API, CLI tool, etc."
    )

class WorksOn(EdgeModel):
    """The user is currently working on or contributing to a project."""
    role: EntityText = Field(
        description="User's role: lead developer, contributor, maintainer, etc."
    )

# Wire into graph with source/target constraints
client.graph.set_ontology(
    entities={"Project": Project, "Technology": Technology},
    edges={
        "WORKS_ON": (WorksOn, [EntityEdgeSourceTarget(source="User", target="Project")]),
    }
)
```

**Schema as reasoning boundary**: if schema doesn't include an edge type for Project → Competitor, extraction cannot produce that relationship even if both are mentioned. The schema defines the *space of valid memories*.

## Extraction Pipeline (Zep/Graphiti 5 Steps)

1. **Entity extraction**: identify named entities in text
2. **Entity resolution**: merge duplicates ("Nexus" and "the Nexus project" → one node)
3. **Fact extraction**: identify relationships, output as typed edges
4. **Fact resolution**: detect contradictions, invalidate outdated facts (preserve history)
5. **Temporal extraction**: parse time references, map to validity windows on edges

Pydantic schema guides steps 1 and 3. Steps 2, 4, 5 are automatic.

## 10/10/10 Constraint

Zep enforces: max 10 custom entity types, 10 custom edge types, 10 fields per type.

**Rationale**: forces designers to identify what *matters* in a domain rather than modeling everything. Schema becomes a reasoning boundary — not just a data structure.

**Practical start**: 3–4 entity types + 3–4 edge types covering 80% of domain logic. Add complexity incrementally with evidence.

## Context Templates

Assembly layer: define which edge/entity types to include → formatted context block injected into agent prompt.

```python
client.context.create_context_template(
    template_id="dev-context",
    template="""
# PROJECTS
%{edges types=[WORKS_ON] limit=5}

# TECH STACK  
%{edges types=[USES_TECHNOLOGY] limit=10}
""")
```

Each entry is typed, temporally annotated, with defined attributes. Save once, reference by ID.

## When to Use Graph vs. Vector Memory

| Scenario | Use Case | Memory Type |
|----------|---------|------------|
| Multi-hop reasoning | "Was Alice's project affected?" | Knowledge Graph |
| Semantic fuzzy search | "Find notes about caching strategies" | Vector |
| Structured attribute queries | "Active projects using PostgreSQL" | Knowledge Graph |
| General fact retrieval | "What did we discuss last week?" | Vector |
| Domain-specific terminology | Custom entity types + relationships | Knowledge Graph |

## 关联页面

- [[Agentic_Memory_System]] — Four-layer memory architecture (in-context/external/episodic/parametric)
- [[Memory_MOC]] — Memory systems map of content
- [[Claude_Memory_Layers]] — Claude-specific memory layer operations
- [[PydanticAI]] — Pydantic as validation layer across agent stack
- [[Context_Engineering]] — Context that memory systems populate
