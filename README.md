# Neurology-Diagnostic-Reasoning-with-Multi-Agent-GraphRAG
A LangGraph system in which competing LLM agents debate differential diagnoses for neurology cases. The agents work over a provenance-tracked Neo4j knowledge graph built from 100 open-access neurology case reports, and independent critics review every argument.

<img width="1920" height="580" alt="image" src="https://github.com/user-attachments/assets/67ff7d35-d118-4799-91c4-84d7e986c16d" />


Structure:

Propose: an initial list of candidate diagnoses is generated from the case.

Advocates: each runs in parallel and defends one diagnosis. It chooses its own graph searches, argues from the retrieved evidence, and rebuts its rivals' previous arguments.

Critics: two critics review the arguments independently of each other. The evidence critic checks that each claim is supported by its citation. The clinical critic judges fit with the case and names any diagnosis being missed.

Moderator: applies the advocates' revisions, adds missed diagnoses, and drops diagnoses both critics scored low.

Judge: after N rounds, produces a ranked differential with confidence levels, the cited evidence, how the hypotheses evolved, and the tests that would best separate the top candidates.


Key features

Hypotheses evolve: advocates can refine a diagnosis (e.g. optic neuritis → MS-related optic neuritis) or withdraw it when the evidence strongly rules it out. Withdrawals are enforced in code and logged with a reason.

Provenance: every passage carries its chunk ID and source paper. Citations are checked in code against what was actually retrieved, and unverified ones are flagged to the critics and the judge.


Simple: a single file built from standard LangGraph (Send fan-out) and langchain-neo4j components.
