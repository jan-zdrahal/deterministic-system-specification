# Deterministic System Specification

Version 0.1  
Status: Stable  
Date: 2026-02-15  
Author: Jan Zdráhal  

Copyright (c) 2026 Jan Zdráhal  
Licensed under the MIT License.  

## 1. Scope

This specification defines the abstract transition semantics of deterministic state-transition systems.

It formally specifies:

- The abstract deterministic system model  
- The structural axioms governing state transitions  
- The determinism invariant  
- The system instance model  
- The execution semantics  
- The conformance conditions  

This specification is architecture-neutral and implementation-independent.

### Non-Goals

This specification does not define:

- Any data encoding format  
- Any cryptographic mechanism  
- Any storage or persistence model  
- Any execution environment  
- Any concurrency or parallel execution model  
- Any notion of time  
- Any deployment architecture  

All such concerns are outside the scope of this specification.


## 2. Terminology

The following terms are used normatively in this specification.

### 2.1 Deterministic System (DS)

An abstract relational state-transition structure defined as:

DS = (S, I, R, Valid)

where S is a set of states, I is a set of admissible inputs,  
R ⊆ S × I × S is the step relation, and Valid ⊆ S is the invariant domain.

### 2.2 State

An element s ∈ S representing the complete logical configuration of the system.

### 2.3 Input

An element i ∈ I representing an admissible transition input.

### 2.4 Valid

The subset Valid ⊆ S defining the invariant domain of the system.  
All transitions are defined only over states in Valid.

### 2.5 Step Relation

The relation R ⊆ S × I × S defining admissible state transitions.

A triple (s, i, s′) ∈ R denotes that state s transitions to state s′ under input i.

### 2.6 System Instance

A pair Inst = (DS, s₀) where s₀ ∈ Valid.

Execution semantics are defined only relative to a system instance.

### 2.7 Execution

A finite sequence of inputs σ ∈ I*.

Execution of a system instance produces a corresponding sequence of states defined by the step relation.

### 2.8 Determinism

The structural property that the step relation R is total and single-valued over Valid × I.

## 3. Abstract Deterministic System

### 3.1 Formal Definition

A deterministic system is defined as a quadruple:

DS = (S, I, R, Valid)

where:

- S is a set of states,
- I is a set of admissible inputs,
- R ⊆ S × I × S is the step relation,
- Valid ⊆ S is the invariant domain.

Valid is non-empty.

### 3.2 Model Semantics

The state space S represents the complete set of possible logical configurations of the system.

The input set I represents all admissible transition inputs.

The step relation R defines admissible state transitions.  
Each element (s, i, s′) ∈ R represents a transition from state s to state s′ under input i.

The subset Valid defines the invariant domain of the system.  
All transitions are defined only over states in Valid.

The model is purely relational and does not prescribe any operational or algorithmic interpretation.

## 4. Axioms and Determinism

A deterministic system DS = (S, I, R, Valid) satisfies the following axioms.

### A1 — Domain Restriction

R ⊆ Valid × I × Valid

All transitions are defined only over states within the invariant domain.

### A2 — Totality

For all s ∈ Valid and for all i ∈ I,  
there exists s′ ∈ Valid such that:

(s, i, s′) ∈ R

The step relation is total over Valid × I.

### A3 — Uniqueness

For all s ∈ Valid and for all i ∈ I,  
if (s, i, s₁) ∈ R and (s, i, s₂) ∈ R,  
then s₁ = s₂.

The step relation is single-valued over Valid × I.

### Determinism

Determinism is the invariant that the step relation R is total and single-valued over Valid × I.

Global execution determinism follows directly from Axioms A2 and A3.

## 5. System Instance

An abstract deterministic system defines a transition structure independently of any particular execution.

A concrete execution is defined relative to a system instance.

A system instance is defined as:

Inst = (DS, s₀)

where:

- DS is a deterministic system,
- s₀ ∈ Valid is the initial state.

The initial state s₀ is explicitly defined for any implementation claiming conformance.

The system instance introduces no additional structure beyond the selection of the initial state.

Execution semantics are defined only relative to Inst.

## 6. Execution Semantics

Execution is defined only relative to a system instance Inst = (DS, s₀).

### 6.1 Input Sequences

Let I* denote the set of all finite sequences over I.

An execution input is a sequence:

σ = (i₀, i₁, ..., iₙ) ∈ I*

### 6.2 Relational Execution

Given Inst = (DS, s₀) and σ ∈ I*,

there exists a unique sequence of states:

(s₀, s₁, ..., sₙ₊₁)

such that for all k in [0, n]:

(sₖ, iₖ, sₖ₊₁) ∈ R

All states in the execution sequence belong to Valid.

Existence follows from Axiom A2.  
Uniqueness follows from Axiom A3.

### 6.3 Derived Execution Function

Because the step relation R is total and single-valued over Valid × I, execution defines a total function:

T* : Valid × I* → Valid

such that:

T*(s₀, σ) = sₙ₊₁

I* includes the empty sequence.

The function T* is derived from the step relation R and does not replace the relational definition of the system.

## 7. Conformance

An implementation conforms to this specification if and only if all of the following conditions hold:

1. It defines a deterministic system DS = (S, I, R, Valid) satisfying axioms A1–A3.

2. It defines a system instance Inst = (DS, s₀) with s₀ ∈ Valid.

3. For every finite input sequence σ ∈ I*, execution relative to Inst is defined according to Section 6 and produces a uniquely determined resulting state.

4. All state transitions performed by the implementation are elements of R.

## 8. Versioning

Versioning of this specification is governed by the Deterministic System Specification — Versioning and Stability Policy document.

Normative changes to axioms, execution semantics, or conformance conditions are reflected according to that policy.
