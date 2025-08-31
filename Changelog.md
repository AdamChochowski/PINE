# Changelog – PINE (CatBorg Studio)

All notable changes to the **PINE** Unity asset will be documented in this file.  
This project adheres to [Semantic Versioning](https://semver.org/) and follows the [Keep a Changelog](https://keepachangelog.com/) format.

---

## [1.16.0] – 2025-08-31
**Summary:** Added TPC_StaminaSprint decorator node, introduced node selection and inspection persistency, improved inspection workflow, and enhanced editor performance.

### Added
- TPC_StaminaSprint  – flexible decorator node decicated for TPC nodes family
  - Controls the Third Person Controller sprint state based on stamina value
  - Supports Stamina Regeneration Rate per second when the character is not sprinting
  - Supports Stamina Drain Rate while sprinting
  - Allows cooperation between multiple active TPC_StaminaSprint nodes in the Behaviour Tree, maintaining a shared character-level stamina value
  - Useful for controlling sprint state without interrupting child nodes, enabling smooth transitions between running and walking without extra logic
- Node Selection and Inspection Persistency – selection and inspection state is now stored in the Behaviour Tree and restored when switching between Play Mode ↔ Edit Mode or changing the inspected tree
- Inspected Node Indicator – inspected nodes are now visually highlighted in the Behaviour Tree View

### Fixed
- Behaviour Tree Editor – stability and minor bug fixes

### Changed
- BehaviourTreeView – performance and memory handling improvements

---

## [1.15.0] – 2025-08-17
**Summary:** Introduced Fork Node, migrated legacy nodes, fixed multiple OnStop issues, and improved editor performance.

### Added
- Fork Node – versatile new node type
  - Acts as an Action Node when no child is attached
  - Provides two output ports: True Output → attach Child_True, False Output → attach Child_False
  - Execution behavior: executes corresponding child or returns True/False if no child
  - Useful for controlling a child’s Running state without altering the child node itself
- Fork Node versions for legacy nodes (Animator_Check_Legacy, Angle_Check_B_Legacy, etc.)

### Fixed
- Behaviour Tree Editor – stability and minor bug fixes
- TagFrenzy_Integrator – stability and minor bug fixes
- Tra_LookAt Node – rotation comparison tolerance fixed
- Undo Group Bug – fixed node removal during undo
- TPC Nodes – prevent random movement during OnStop
- Nav Nodes – prevent random movement during OnStop
- Cooldown_Timer – fixed time bar preview
- EventListener – minor bug fix
- Wait Node – minor bug fix

### Changed
- Legacy nodes marked as Legacy; Fork Node replacements added
- Sequencer & Selector Nodes – OnStop now marks not-reached child nodes as Slipping
- SetState Node – proper OnStop calls for Success/Failure
- Retry Node – frame-safe retries, reset attempts on child success
- BehaviourTreeView – performance improvements
- Node descriptions – migrated to NodeInfoAttribute-based system

---

## [1.14.0] – Starter Assets ThirdPerson Integration

### Added
- TPC_Chase, TPC_MoveTowards, TPC_Flee, TPC_Patrol, TPC_SetSprint

### Fixed
- Minor bugs

---

## [1.12.0] – Senses 2 Integration

### Added
- Senses_WasAnythingSeen, Senses_WasAnythingHeard, Senses_WasAnythingSmelled

### Removed
- Senses_GetTarget

---

## [1.11.0] – Minor Update

### Fixed
- Minor bugs

---

## [1.10.0] – Minor Update

### Changed
- Reduced garbage collection (GC) for performance optimization

---

## [1.09.0] – Minor Update

### Added
- Copy, Paste, Cut functionality for behavior tree nodes

---

## [1.08.0] – Minor Update

### Added
- Node: RandomInCircle

### Changed
- Updated Nodes: Component_Variable_Set, Variable_Set, Distance_Check_A, Distance_Check_B, RandomDestination, Search, Senses_GetTarget, Search_TagFrenzy
- Optimized node group load times and garbage collection
- Saved last view position in behavior trees; allow moving view to selected node

### Fixed
- Minor bugs

---

## [1.07.0] – Minor Update

### Added
- Node: Search_TagFrenzy

### Changed
- Updated Nodes: Search, Senses_GetTarget

### Fixed
- Minor bugs

---

## [1.06.0] – Minor Update

### Changed
- Updated Nodes: Nav_MoveTowards, Nav_Chase, Nav_Flee, Nav_Patrol, Nav_ReachedDestination, Tra_MoveTowards, Tra_ChargeTowards, Tra_Chase, Tra_Flee, Tra_LookAt, Tra_Patrol, Tra_ReachedDestination

### Fixed
- Minor bugs

---

## [1.05.0] – Minor Update

### Changed
- Updated Nodes: Variable_Component_Set, Variable_Component_Get, Variable_Component_Check, Search (now stores results in Blackboard)
- Improved Blackboard data flow

---

## [1.04.0] – Minor Update

### Added
- Nodes: Variable_Component_Set, Variable_Component_Get, Variable_Component_Check

---

## [1.03.0] – Minor Update

### Fixed
- Minor bugs

---

## [1.02.0] – Minor Update

### Fixed
- Minor bugs

---

## [1.01.0] – Minor Update

### Fixed
- Minor bugs

---

## [1.00.0] – First Release
