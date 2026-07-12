# ReaxPost

**ReaxPost** is an automated reaction network analysis and visualization tool for **ReaxFF molecular dynamics simulations**. It is designed to post-process ReaxFF bond-order trajectory data, identify molecular species and reaction events, construct reaction pathway networks, visualize complex reaction mechanisms, and extract quantitative kinetic information.

ReaxPost is especially useful for complex reactive systems where manual analysis is difficult, such as hydrocarbon oxidation, aromatic pyrolysis, polymer degradation, and macromolecular reaction networks.

## Overview

Reactive molecular dynamics simulations based on ReaxFF can capture bond formation and bond breaking in large-scale chemical systems. However, the resulting bond-order trajectories often contain a large number of intermediates, transient species, and competing reaction events. ReaxPost addresses this challenge by providing an automated workflow for species identification, reaction event detection, reaction pathway tracking, network visualization, and kinetic analysis.

The software uses bond order assignment and inter-frame topology comparison algorithms to automatically track atomic transfer pathways and identify reaction events. It further introduces a hierarchical reaction network visualization framework based on molecular nodes and reaction path nodes, enabling structured representation of complex reaction networks. ReaxPost also supports reaction-frequency filtering, species-based pathway tracing, delocalized bond order redistribution for aromatic systems, and polymer fragmentation tracking based on minimal snapshots. :contentReference[oaicite:0]{index=0}

## Key Features

- Automated parsing of ReaxFF bond-order trajectory files
- Molecular species identification from dynamic bond connectivity
- Reaction pathway extraction through frame-to-frame topology comparison
- Hierarchical two-dimensional reaction network visualization
- Molecular species nodes and reaction pathway nodes
- Reaction network filtering by frequency threshold
- Species-centered pathway tracing
- Delocalized bond order redistribution for aromatic systems
- Suppression of false reaction events caused by Kekulé resonance
- Polymer and macromolecular fragmentation tracking
- Pathway analysis for polymerization and degradation processes
- Quantitative statistics of reaction occurrence frequencies
- Reaction rate constant extraction
- Time evolution analysis of molecular species and bond counts
- Output of reaction networks, interactive pages, vector graphics, and logs

## Workflow

The ReaxPost workflow contains the following major steps:

1. **Topology construction**  
   ReaxPost reads ReaxFF bond-order data and converts continuous bond orders into chemically meaningful molecular connectivity.

2. **Bond order assignment**  
   Real-valued ReaxFF bond orders are assigned to integer-valued bond-order adjacency matrices according to bond-order thresholds, reference bond order, and atomic valence constraints.

3. **Delocalized bond order redistribution**  
   For aromatic and conjugated systems, ReaxPost redistributes delocalized bond orders to generate stable Kekulé-type molecular representations and avoid false bond breaking/forming events caused by bond-order oscillations.

4. **Frame comparison**  
   Adjacent frames are compared using atomic indices and molecular topology. When the molecular topology changes between two frames, the corresponding reaction event is recorded.

5. **Reaction network construction**  
   Chemical species are represented as molecular nodes, and reaction events are represented as pathway nodes. Directed edges connect reactants, reaction pathways, and products.

6. **Filtering and tracing**  
   Users can simplify large reaction networks by reaction frequency or extract local subnetworks related to a selected species.

7. **Kinetic analysis**  
   ReaxPost can calculate reaction occurrence counts and rate constants for selected elementary reactions.

The manuscript describes this workflow as an automated and traceable process initialized by user-defined parameters, including start frame, end frame, number of sampled frames, and valence information for each element. The software also supports multiple valence states for the same element and provides Save/Load functions for large-scale analyses. :contentReference[oaicite:1]{index=1}

## Core Algorithms

### Bond Order Assignment

ReaxPost converts continuous ReaxFF bond-order matrices into chemically plausible integer bond-order adjacency matrices. The algorithm considers the maximum bonding capacity of each atom, sorts neighboring atoms by bond-order magnitude, and iteratively assigns bond orders while satisfying valence constraints.

Default parameters described in the manuscript include:

- Bond order cutoff: `0.3`
- Reference bond order: `1.0`

This step provides the molecular topology used for species identification, visualization, and reaction network construction.

### Delocalized Bond Order Redistribution

In high-temperature simulations of aromatic compounds, ReaxFF bond orders can oscillate between single-bond and double-bond character due to π-electron delocalization. Conventional threshold-based analysis may incorrectly identify these oscillations as repeated bond breaking and formation events.

ReaxPost addresses this problem using a delocalized bond order redistribution algorithm. Bonds are classified as delocalized when they satisfy bond-order range, atomic unsaturation, and atomic-type criteria. The algorithm then assigns alternating single and double bonds within the delocalized subgraph to generate a unified aromatic topology. This reduces noise in reaction networks and improves reaction pathway identification for aromatic systems. :contentReference[oaicite:2]{index=2}

### Frame Comparison

ReaxPost detects reactions by comparing molecular topology between adjacent frames. Since ReaxFF simulations usually preserve atomic indices during the trajectory, the software uses atom IDs to track molecular transformations.

For each atom, ReaxPost retrieves its parent molecule in the previous frame and the corresponding molecule in the next frame. If the atomic composition or SMILES representation changes, an iterative closure expansion is performed to identify all atoms involved in the reaction event. The final reaction is then recorded as a SMILES-based reaction equation.

This algorithm requires atomic indices to remain consistent throughout the trajectory. It is not suitable for trajectories where atoms are reordered, added, or removed during simulation.

### Reaction Network Construction

ReaxPost constructs a directed reaction network using two types of nodes:

- **Species nodes**: molecular species or intermediates
- **Pathway nodes**: specific reaction events or reaction equations

Edges point from reactant species to pathway nodes and then from pathway nodes to product species. The network is arranged using a hierarchical layout, dummy virtual layers, and intra-layer ordering optimization to reduce edge crossings and improve readability.

### Reaction Network Filtering

Large reaction networks often contain many low-frequency or transient events. ReaxPost provides two filtering strategies:

- **Frequency-based filtering**: retains reaction nodes above a user-defined occurrence threshold
- **Species-based tracing**: extracts local reaction networks associated with a selected molecule

These functions help identify dominant pathways and key intermediates in complex systems.

### Polymer Fragmentation Tracking

For polymer and macromolecular systems, ReaxPost introduces a fragmentation tracking algorithm based on the **minimum snapshot** mechanism. Users define polymer criteria, such as minimum numbers of C and H atoms. When a polymer fragment first no longer satisfies the criteria, the atom indices at that moment are captured as the minimum snapshot. Subsequent fragments are tracked by comparing atomic index sets.

This enables pathway evolution analysis during polymer degradation or polymerization.

## Applications

ReaxPost has been validated using three representative ReaxFF reactive molecular dynamics systems:

### Methane Oxidation

Methane oxidation was used as a benchmark small-molecule reaction system. ReaxPost successfully identified species evolution, reaction pathways, and rate constants. The software captured the dynamic formation and consumption of intermediates such as formaldehyde and methanol, and its calculated rate constants agreed well with ChemTraYzer results. :contentReference[oaicite:3]{index=3}

### Toluene Pyrolysis

Toluene pyrolysis was selected as an aromatic reaction system. The delocalized bond order redistribution algorithm was used to suppress false reaction events caused by resonance in the benzene ring. According to the manuscript, this algorithm reduced identified chemical species and reaction events by more than 68%, improving the physical realism of reaction pathway analysis. :contentReference[oaicite:4]{index=4}

### PDMS Polymer Pyrolysis

Polydimethylsiloxane pyrolysis was used to validate ReaxPost for polymer degradation. ReaxPost captured Si-C bond cleavage, methyl radical generation, methane formation, backbone rearrangement, cyclic product formation, and cross-linking processes. The calculated activation energy for methane formation was 53.15 kcal/mol, in excellent agreement with the reported literature value of 53 kcal/mol. :contentReference[oaicite:5]{index=5}

## Installation

Download all the three files: ReaxPost V2.9.zip.001, ReaxPost V2.9.zip.002, ReaxPost V2.9.zip.003. You can obtain ReaxPost 2.9 by extracting any one of the three compressed files. ReaxPost runs on the Windows platform.
