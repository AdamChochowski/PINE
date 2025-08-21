# Node Reference – PINE (CatBorg Studio)

This document provides a comprehensive reference for all **Nodes** available in the **PINE** Unity asset.
It includes descriptions, fail conditions, notes, and legacy information for each Node.
All Node types are dynamically categorized using PINE's internal helpers to ensure consistency with the editor interface.

> Generated automatically from the current version of PINE.
> Last updated: 2025-08-21

## Angle_Check
**Type:** Fork Node

**Description:** Evaluates whether the angle between this GameObject’s forward direction and a Blackboard-provided target (Vector3, Transform, or GameObject) satisfies a comparison against the Required Angle value.

**Fail Condition:** Initially returns Failure if the declared Blackboard field is null or of an incompatible type. Otherwise, it computes the signed angle (ignoring height if specified), takes its absolute value, and applies the selected Comparison operator (>, <, ==, etc.) against the Required Angle.

This makes it useful for checks like “Is the target within my vision cone?” or “Am I facing the target within a tolerance angle?”.

**Notes:** Can ignore height for planar (2D) angle checks.
Supports Blackboard HardCoded (fields) and SoftCoded (string-keyed) access.

---

## Angle_Check_B_Legacy
**Type:** Branching Node

**Description:** A branching node that compares the signed angle between the agent's forward direction and a target, returning Success or Failure based on conditions.

**Fail Condition:** This node calculates the angle between the agent's forward vector and a given target position (Vector3, Transform, or GameObject). The result is then compared against a user-defined threshold using a selected comparison operator (>, <, =, etc.). The `IgnoreHeight` option allows comparison on the horizontal plane only. Supports both hardcoded and softcoded Blackboard references for flexible data access.

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to `Angle_Check` (ForkNode), which provides a more robust and streamlined implementation.

**Legacy:** Planned removal in 1.20.0 – superseded by Angle_Check (ForkNode).

---

## Animator_Check
**Type:** Fork Node

**Description:** Initially returns Failure if no Animator is assigned in the Blackboard. If valid, the node checks whether the Animator’s current state on the specified layer matches the RequiredName. The evaluation result is then passed through the ForkNode logic, either as a direct boolean result or routed through its True/False child outputs.

**Fail Condition:** Fails if Animator is missing or not yet enabled.

**Notes:** Can be used to branch behavior trees based on animation states without modifying the Animator directly.

---

## Animator_Check_Legacy
**Type:** Branching Node

**Description:** A branching node that checks if the Animator is currently in a specific state.

**Fail Condition:** This node verifies whether the Animator component on the Blackboard is currently playing an animation state with the given `RequiredName`. If the state matches, the node succeeds, otherwise it fails. Always checks against layer 0 of the Animator.

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to `Animator_Check` (ForkNode), which offers improved handling and more flexible checks.

**Legacy:** Planned removal in 1.20.0 – superseded by Animator_Check (ForkNode).

---

## Animator_Set
**Type:** Action Node

**Description:** Sets multiple Animator parameters at once on a specified Animator component.

**Fail Condition:** Allows you to set Float, Int, Bool, and Trigger parameters in one node. The node checks if all declared parameters exist in the Animator and logs warnings for missing parameters. Returns Success if parameters are successfully set, Failure if the Animator is null or required parameters are missing.

**Notes:** Parameters are configured in lists for editor convenience. Floats, Ints, Bools, and Triggers are converted to runtime arrays and hash codes for efficient setting during runtime. Useful for controlling complex Animator states from a single node.

---

## Animator_SetState
**Type:** Action Node

**Description:** Enables, disables, or toggles the state of an Animator component on the Blackboard.

**Fail Condition:** Controls the Animator component's enabled state based on the selected NewState (Enabled, Disabled, Change). Returns Success if the operation is performed, Failure if no Animator is assigned in the Blackboard.

**Notes:** Use this node to quickly enable or disable the Animator during runtime or toggle its current state.

---

## Animator_Sync
**Type:** Action Node

**Description:** Synchronizes Animator parameters with the GameObject's transform directions.

**Fail Condition:** Sets Animator float and int parameters based on the specified direction (Forward, Back, Right, Left, Up, Down) and axis (x, y, z). Returns Success if the values are applied, Failure if the Animator is missing or parameters are not declared.

**Notes:** Use this node to drive Animator parameters from the GameObject's current transform orientation, e.g., for movement or facing direction.

---

## BoxCollider_Set
**Type:** Action Node

**Description:** Dynamically modifies a BoxCollider's size and center over time or via constants/curves.

**Fail Condition:** Updates the BoxCollider's size and center based on the specified entry types: Constant, Random between constants, Constant over time, Curve over time, or Random between two curves. Can target a BoxCollider directly or via a field on the Blackboard. Evaluates curves or random ranges per update over a specified repetition length.

**Notes:** Useful for procedural animation, scaling, or dynamically adjusting colliders during runtime. Supports both size and center adjustments with curves and randomness.

---

## CapsuleCollider_Set
**Type:** Action Node

**Description:** Dynamically modifies a CapsuleCollider's radius, height, and center over time or via constants/curves.

**Fail Condition:** Updates the CapsuleCollider's radius, height, and center based on the specified entry types: Constant, Random between constants, Constant over time, Curve over time, or Random between two curves. Can target a CapsuleCollider directly or via a field on the Blackboard. Evaluates curves or random ranges per update over a specified repetition length.

**Notes:** Useful for procedural animation, scaling, or dynamically adjusting colliders during runtime. Supports radius, height, and center adjustments with curves and randomness. Fully configurable via entry types and AnimationCurves for more complex behavior.

---

## Collider_isTrigger
**Type:** Action Node

**Description:** Sets or toggles the 'isTrigger' property of a specified Collider.

**Fail Condition:** Changes the Collider's isTrigger property based on the selected NewState: Enabled (true), Disabled (false), or Change (toggle). Returns Success if the operation completes, Failure if the Collider is null.

**Notes:** Supports colliders declared in Blackboard or assigned in Node. Useful for dynamically switching physics behavior at runtime.

---

## Collider_OnCollision
**Type:** Fork Node

**Description:** Checks collisions on a specified collider and determines Success or Failure based on the configured criteria.

**Fail Condition:** Returns Success if a collision occurs with the specified TAGs or MonoBehaviour component according to the selected OnCollisionType (Enter, Exit, Stay). Returns Failure otherwise.

**Notes:** Supports colliders declared in Blackboard or assigned in Node. Can monitor multiple collision types and optionally override Rigidbody sleep thresholds. Uses ColliderCallback to receive collision events.

---

## Collider_SetState
**Type:** Action Node

**Description:** Changes the enabled state of a Collider dynamically at runtime.

**Fail Condition:** Targets a specified Collider and sets its state to Enabled, Disabled, or toggles its current state. Can reference the Collider directly or via a field on the Blackboard. Updates the Collider's enabled property immediately on execution.

**Notes:** Useful for enabling/disabling colliders for procedural gameplay, triggers, or runtime adjustments. Supports hardcoded references or Blackboard-based fields.

---

## Cooldown
**Type:** Decorator Node

**Description:** A decorator node that enforces a cooldown period between executions of its child node.

**Fail Condition:** This node runs its child node only if the cooldown period has elapsed. The cooldown duration is defined by `CooldownTime`. If the child is attempted before the cooldown is finished, the node forces the child to OnStop, sets its state to Slipping for easier debugging, and returns Failure itself.The `RemainingTime` property shows how much time has passed since the last execution, allowing for tracking cooldown progress.

**Notes:** Useful for limiting how often a certain behavior can occur. Supports single or repeated cooldowns.

---

## Cooldown_Timer
**Type:** Decorator Node

**Description:** A decorator node that alternates between a timer phase and a cooldown phase, controlling when its child node can run.

**Fail Condition:** This node runs its child node only during the timer phase. During the cooldown phase, the node forces the child to OnStop, sets its state to Slipping for easier debugging,The node supports both constant and randomized durations for the timer and cooldown phases. RemainingCooldownTime and RemainingTimeTime track elapsed time, while NormalizedRemainingTime provides progress as a value between 0 and 1. The TimmerCycleOngoin property indicates whether the timer phase is currently active.

**Notes:** Useful for implementing repeated actions with timed delays and cooldowns. Ideal for AI behaviors or periodic events.

---

## DebulLog
**Type:** Action Node

**Description:** An action node that logs a message to the Unity console.

**Fail Condition:** This action node prints the specified message to the console every time it updates and then returns Success. Useful for debugging and tracking behavior tree execution.

**Notes:** Has one configurable property: `Message` (string). Always returns Success.

---

## Destroy
**Type:** Action Node

**Description:** Destroys the GameObject associated with the blackboard.

**Fail Condition:** Immediately destroys the GameObject stored in the blackboard. Returns Success if the GameObject exists and is destroyed, Failure if the blackboard or GameObject is null.

**Notes:** This node does not require any input parameters. Use with caution, as it will remove the GameObject from the scene.

---

## Distance_Check
**Type:** Fork Node

**Description:** Checks the horizontal distance (XZ plane) between two positions and compares it against a required distance. Supports dynamic input via EntryParameters, allowing GameObjects, Transforms, or Vector3 values for both 'From' and 'To'.

**Fail Condition:** Fails if any of the EntryParameters cannot be resolved, if a GameObject or Transform is null, or if an unsupported VariableType is used.

**Notes:** Distance is calculated ignoring the Y axis. ComparisonType allows you to specify how to compare the calculated distance squared against the required distance squared (>, <, ==, >=, <=, !=).

---

## Distance_Check_B_Legacy
**Type:** Branching Node

**Description:** Evaluates whether the distance between the object's transform and a specified target (Vector3, Transform, or GameObject) satisfies a defined comparison condition, with the option to ignore height for 2D plane checks. If the condition is met, it proceeds to execute the Left Output Children; otherwise, it runs the Right Output Children.

**Fail Condition:** Fails if the specified field in the Blackboard is null or the comparison condition is not met; otherwise, it returns the update result of the child nodes.

**Notes:** None

**Legacy:** Planned removal in 1.20.0 – superseded by Variable_Check (ForkNode).

---

## EventEmitter
**Type:** Action Node

**Description:** Emits a named event via the global EventManager, notifying all subscribed listeners.

**Fail Condition:** This node triggers a global event identified by a string name using the EventManager system. All listeners that have subscribed to the event via EventManager.Subscribe will be invoked when this node runs. The EventManager supports multiple subscribers per event and provides optional payload overloads, though this node currently emits events without a payload.

**Notes:** OnStart and OnStop are empty. OnUpdate immediately emits the event and returns Success. Ensure listeners have subscribed to the event before emission. If no listeners exist, Emit will have no effect. Event names should be unique and consistently referenced to avoid accidental overwrites or missing invocations.

---

## EventListener
**Type:** Decorator Node

**Description:** Waits for a specified event to occur and then executes its child node when the event is triggered.

**Fail Condition:** This decorator node subscribes to a global event via the EventManager using the specified EventName. While waiting for the event, it returns Running. Once the event is triggered, it sets an internal flag and updates its child node on the next tick. After executing the child node, it resets the flag and continues listening for the event. Handles automatic subscription on start and unsubscription on stop or destruction to prevent memory leaks.

**Notes:** The node maintains a boolean flag (m_eventTriggered) to track when the event occurs. OnStart subscribes to the event, OnUpdate checks the flag and updates the child node if triggered, and OnStop/OnDestroy unsubscribes to clean up. Returns Running until the event occurs, then passes control to the child node.

---

## Fail
**Type:** Action Node

**Description:** An action node that always returns Failure.

**Fail Condition:** This action node immediately returns a Failure state when updated. It does not perform any operations during OnStart or OnStop.

**Notes:** Useful for testing, debugging, or explicitly marking a branch in a behavior tree as failing. It has no configurable properties.

---

## Failer
**Type:** Decorator Node

**Description:** A decorator node that always returns Failure after running its child node.

**Fail Condition:** This node executes its child node normally by calling `Child.Update()`, but regardless of the child's result, it forces the outcome to be Failure. This can be useful for testing failure-handling in behavior trees or intentionally overriding a child's success.

**Notes:** Useful for debugging or enforcing failure paths in AI behaviors without stopping child execution.

---

## GameObject_SetLayer
**Type:** Action Node

**Description:** Sets the layer of a specified GameObject.

**Fail Condition:** Changes the layer of a target GameObject based on the selected source (Blackboard hardcoded, Blackboard softcoded, or Node reference). Returns Success if the GameObject exists and the layer is set, Failure if the GameObject is null.

**Notes:** You can choose the GameObject source via DeclaredTypeCurrent. Use FieldName for Blackboard hardcoded or softcoded references. LayerMask defines the new layer.

---

## GameObject_SetState
**Type:** Action Node

**Description:** Sets the active state of a GameObject.

**Fail Condition:** Changes the GameObject's active state based on NewStateCurrent (Enabled, Disabled, or Change). Supports Node reference, Blackboard hardcoded, or Blackboard softcoded GameObjects. Returns Success if the GameObject exists, Failure if null.

**Notes:** Use FieldName when referencing Blackboard values. NewStateCurrent determines the action on the GameObject.

---

## GameObject_SetTAG
**Type:** Action Node

**Description:** Sets the tag of a GameObject.

**Fail Condition:** Updates the GameObject's tag to the specified TAG value. Supports Node reference, Blackboard hardcoded, or Blackboard softcoded GameObjects. Returns Success if the GameObject exists, Failure if null.

**Notes:** Use FieldName when referencing Blackboard values. Ensure TAG is not empty or null.

---

## GameObject_Spawn
**Type:** Action Node

**Description:** Spawns a GameObject prefab with configurable position, rotation, scale, layer, and tag.

**Fail Condition:** Instantiates the assigned prefab based on the selected EntryTypes (Constant or RandomBetweenTwoConstants) for position, rotation, and scale. Optionally sets the GameObject's layer and tag. Supports retrieving the prefab from the Blackboard or a hardcoded reference.

**Notes:** Position, rotation, and scale can be set as constant values or randomized between two vectors. Layer assignment uses LayerMask. Prefab can be sourced from the node itself or the blackboard (hardcoded or soft-coded).

---

## Health_Change
**Type:** Action Node

**Description:** Modifies the Health value stored in the Blackboard using a constant value and a specified override method.

**Fail Condition:** Supports direct replacement, additive modification, or multiplicative modification of the Blackboard's Health. Returns Success after applying the change.

**Notes:** Uses the OverrideType enum to determine how the ModificationValue affects the current Health. Can override with NewValue, add/subtract with ChangeByValue, or scale with MultiplyByValue.

---

## Health_Check
**Type:** Fork Node

**Description:** Evaluates the Blackboard’s current Health value against a user-defined RequiredValue using the selected comparison operator (>, <, =, etc.). If the condition is satisfied, the node reports Success; otherwise, it reports Failure. The result is then routed through the ForkNode system, either returning a direct boolean or executing the corresponding True/False child branch.

**Fail Condition:** Fails only if the comparison condition is not met.

**Notes:** Useful for branching logic based on health thresholds (e.g., retreat if health is below 30, attack if above 70).

---

## Health_Check_Legacy
**Type:** Branching Node

**Description:** A branching node that compares the Blackboard's Health value against a required threshold.

**Fail Condition:** This node evaluates the current `Health` stored in the Blackboard and compares it to the `RequiredValue` using the specified `ComparisonType`. If the condition is satisfied, the node succeeds; otherwise, it fails.

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to `Health_Check` (ForkNode), which provides a more flexible and modernized implementation.

**Legacy:** Planned removal in 1.20.0 – superseded by Health_Check (ForkNode).

---

## Health_WasChanged
**Type:** Fork Node

**Description:** Checks whether the Blackboard’s Health value has changed compared to its previously stored value. On first activation, the current Health is cached as the baseline. On each update, the node evaluates whether Health has increased or decreased based on the selected SimpleComparisonType. If the condition is satisfied, it reports Success and updates the stored LastValue to the new Health.

**Fail Condition:** Fails if the chosen condition (increase or decrease) is not met.

**Notes:** Useful for detecting Health fluctuations, such as triggering defensive behavior when health decreases or rewarding recovery when health increases.

---

## Health_WasChanged_Legacy
**Type:** Branching Node

**Description:** A branching node that checks if the Blackboard's Health value has increased or decreased since the last update.

**Fail Condition:** This node compares the current `Health` value with the previously stored `LastValue`. Depending on the chosen `SimpleComparsionType`, it detects whether Health has increased or decreased. When a change is detected, it updates the `LastValue` to the new Health and continues execution.

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to `Health_WasChanged` (ForkNode), which offers improved handling and flexibility.

**Legacy:** Planned removal in 1.20.0 – superseded by Health_WasChanged (ForkNode).

---

## Inverter
**Type:** Decorator Node

**Description:** A decorator node that inverts the result of its child node.

**Fail Condition:** This node executes its child node normally, but inverts the result: Success becomes Failure, Failure becomes Success, and Running or Slipping are passed through unchanged. This is useful for behavior tree logic when you want to reverse the meaning of a condition or action.

**Notes:** Commonly used to negate conditions or enforce inverse logic in AI behavior trees.

---

## Limiter
**Type:** Decorator Node

**Description:** A decorator node that limits the number of times its child node can be executed.

**Fail Condition:** This node allows its child to run only a set number of times, defined by the `Repetition` property. Each time the child is run, `RemainingRepetition` decreases. Once the limit is reached, the node forces the child to OnStop, sets its state to Slipping for easier debugging, and returns Failure itself. This ensures the child cannot exceed its allowed number of executions.

**Notes:** Useful for restricting how often an action can occur, such as attacks or abilities in AI behavior trees.

---

## LineOfSight_Check
**Type:** Fork Node

**Description:** Performs a line-of-sight check from the Executor toward the Blackboard Target. Uses raycasts to determine whether the Target is visible, ignoring specified layers and optionally ignoring parent colliders. Returns Success if the Target is visible, otherwise Failure.

**Fail Condition:** Fails if no Target is assigned, if the raycast never reaches the Target, or if blocked by other colliders.

**Notes:** Supports ignoring parent colliders to prevent self-blocking. Iteratively steps through colliders up to a safety cap of 20 iterations to avoid infinite loops. Tags can be filtered to restrict valid hits.

---

## LineOfSight_Check_Legacy
**Type:** Branching Node

**Description:** A branching node that checks line of sight from the executor to the Blackboard.Target using raycasting.

**Fail Condition:** Casts rays toward the target, ignoring parent colliders if enabled, and only succeeds if the ray first hits the Blackboard.Target (with matching allowed tags).

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to the new `LineOfSight_Check` (ForkNode), which provides improved Blackboard integration and configurable parameters.

**Legacy:** Planned removal in 1.20.0 – superseded by LineOfSight_Check (ForkNode).

---

## Lumen_SetState
**Type:** Action Node

**Description:** Sets or toggles the Lumen Stealth Addon state on the blackboard's GameObject. Can Enable, Disable, or Change (toggle) the current state using the SensesIntegrator system.

**Fail Condition:** Fails if SensesAssetPack is not installed or if the target component cannot be found on the blackboard's GameObject.

**Notes:** Uses reflection to call the Lumen Stealth method. Caches component references and parameters for efficiency.

---

## Material_Change
**Type:** Action Node

**Description:** Changes the materials of all Renderers on the target GameObject or its children using hardcoded or node-specified materials.

**Fail Condition:** Supports multiple materials with DeclaredType selection (BlackboardHardCoded or Node). Applies new materials to all child Renderers. Returns Success if materials are applied, Failure if no Renderers found.

**Notes:** Uses reflection for BlackboardHardCoded materials. Logs a warning if the new materials array length does not match the target Renderer’s materials array length.

---

## Material_Flash
**Type:** Action Node

**Description:** Temporarily replaces the materials of all Renderers on the target GameObject or its children with a flash material for a specified duration.

**Fail Condition:** Supports BlackboardHardCoded or Node-specified flash material. Can use a constant or random flash duration. Automatically restores original materials after the flash time expires. Returns Running while flashing, Success once completed, Failure if no Renderers are found.

**Notes:** Uses reflection to retrieve BlackboardHardCoded materials. Random duration is picked between WaitTimeMin and WaitTimeMax if WaitType is set to random.

---

## Nav_Chase
**Type:** Action Node

**Description:** Moves a NavMeshAgent toward a specified target using NavMesh pathfinding.

**Fail Condition:** Supports moving toward a Transform or GameObject target. Reads the target from an EntryParameter. Returns Success when the agent reaches the stopping distance, Running while moving, and Failure if setup is invalid or target is missing.

**Notes:** Uses EntryParameter to define the target. Properly enables/disables the NavMeshAgent during OnStart/OnStop. Automatically calculates and sets paths using NavMesh. Requires a NavMeshAgent assigned in the Blackboard.

---

## Nav_Flee
**Type:** Action Node

**Description:** Moves a NavMeshAgent away from a specified target using NavMesh pathfinding.

**Fail Condition:** Supports fleeing from a Transform or GameObject target. Reads the target from an EntryParameter. Flee distance can be constant or randomly chosen within a min/max range. Returns Success when the agent reaches the desired flee distance, Running while moving, and Failure if setup is invalid or target is missing.

**Notes:** Uses EntryParameter to define the target. Supports configurable flee distance and flee type (constant or random). Properly enables/disables the NavMeshAgent during OnStart/OnStop. Automatically calculates and validates paths using NavMesh. Requires a NavMeshAgent assigned in the Blackboard.

---

## Nav_MoveTowards
**Type:** Action Node

**Description:** Moves a NavMeshAgent towards a specified target or Vector3 destination using NavMesh pathfinding.

**Fail Condition:** Supports moving to a Transform, GameObject, or Vector3 as the destination. Reads the target/destination from an EntryParameter. Automatically calculates and sets paths using NavMesh. Returns Success when the agent reaches the destination, Running while moving, and Failure if setup is invalid or path cannot be calculated.

**Notes:** Uses EntryParameter to define the destination. Grabs the destination during OnStart and handles agent enable/disable automatically. Requires a NavMeshAgent assigned in the Blackboard.

---

## Nav_Patrol
**Type:** Action Node

**Description:** Moves a NavMeshAgent along a series of waypoints in a defined order, with options for endless looping or reversing patrol direction.

**Fail Condition:** Supports selecting start waypoint as either the nearest to the agent or the first in the array. Automatically moves to the next waypoint when the agent reaches the current target. Can run endlessly or stop after reaching the final waypoint. Optionally inverts patrol order on finish.

**Notes:** Requires a NavMeshAgent assigned in the Blackboard. Waypoints must be set in the Blackboard. Returns Running while moving and Success when patrol finishes (if not endless).

---

## Nav_ReachedDestination
**Type:** Fork Node

**Description:** Checks whether the NavMeshAgent assigned in the Blackboard has reached the specified destination. The destination and accuracy margin are provided via EntryParameters, which may reference a Transform, GameObject, or Vector3 for the position, and a Float or Int for the accuracy. The node uses NavMesh sampling to validate the destination point and then compares the agent’s current distance against its stopping distance with the given accuracy offset. Returns Success if the agent is considered to have arrived, otherwise Failure.

**Fail Condition:** Fails if the agent reference is missing, if the destination or accuracy parameter cannot be resolved, or if the agent has not reached the target within the given margin.

**Notes:** Useful for confirming arrival before triggering subsequent actions such as animations or state changes. The IgnoreHeight option allows for horizontal-only checks in 2D or flat terrain scenarios.

---

## Nav_ReachedDestination_Legacy
**Type:** Branching Node

**Description:** A branching node that checks whether the NavMeshAgent has reached its target destination within a given accuracy range.

**Fail Condition:** This node evaluates the current `NavMeshAgent` destination against a specified target (Transform, GameObject, or Vector3) and checks if the agent is within the required stopping distance plus an additional accuracy margin. If the agent has reached the destination, the node succeeds; otherwise, it fails.

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to `Nav_ReachedDestination` (ForkNode), which provides improved parameter handling and more reliable NavMesh integration.

**Legacy:** Planned removal in 1.20.0 – superseded by Nav_ReachedDestination (ForkNode).

---

## Nav_Stop
**Type:** Action Node

**Description:** Stops a NavMeshAgent attached to the executor GameObject or referenced in the Blackboard.

**Fail Condition:** This node attempts to stop the assigned NavMeshAgent by setting `isStopped = true`. If the `Agent` reference in the Blackboard is null, it will try to find a `NavMeshAgent` component on the executor (`Blackboard.This`). If no agent is found, it logs an error and returns Failure. Otherwise, it stops the agent and returns Success.

**Notes:** Useful for halting movement of AI agents in a behavior tree. Ensure the Blackboard.Agent is assigned or a NavMeshAgent is attached to the executor.

---

## Noise_Release
**Type:** Action Node

**Description:** Releases noise through Senses system with configurable strength and distribution over time. Supports constant, random, and curve-based noise evolution.

**Fail Condition:** Fails if SensesAssetPack is not installed or if the Noise component cannot be found on the blackboard's GameObject.

**Notes:** Uses reflection to call SensesIntegrator noise methods. Can accumulate values over time or follow curves. Optional use of NavMesh and distance distribution.

---

## RandomChance
**Type:** Decorator Node

**Description:** A decorator node that executes its child node based on a percentage chance.

**Fail Condition:** This node randomly determines whether to run its child node depending on the configured `Chance` value (0-100). If the random roll is below the chance, it runs the child node. Otherwise, it forces the child node to Failure (triggering OnStop), immediately sets the child state to Slipping for easier debugging, and returns Failure itself.

**Notes:** Has one configurable property: `Chance` (int). Can be used to introduce probabilistic behavior in a behavior tree.

---

## RandomDestination
**Type:** Action Node

**Description:** Generates a random destination within a defined range around a given origin and optionally ensures it lies on a NavMesh.

**Fail Condition:** Calculates a random point around the specified origin (Transform, GameObject, or Vector3) within the given Range. If HadToBeOnNavMesh is true, the point is validated against the NavMesh using NavMesh.SamplePosition. The resulting destination can be stored in the Blackboard as a Vector3 according to the configured SaveParameters.

**Notes:** Requires a valid origin provided via EntryParameter (Transform, GameObject, or Vector3). If HadToBeOnNavMesh is enabled, requires a valid NavMesh and a NavMeshAgent on the Blackboard. Returns Success when a destination is successfully determined and stored; Failure if the origin is invalid or no valid NavMesh point could be found.

---

## RandomInCircle
**Type:** Action Node

**Description:** Generates a random point within a circular area around a given origin, optionally constrained to the NavMesh.

**Fail Condition:** Calculates a random point inside a circle with radius between the specified min and max distances from the origin. The origin can be a Transform, GameObject, or Vector3. If `HadToBeOnNavMesh` is true, the node ensures the position is valid on the NavMesh using NavMesh.SamplePosition. The resulting position is stored according to the configured SaveParameters, usually as a Vector3 in the Blackboard.

**Notes:** IMPORTANT: Requires the executor's Transform in the Blackboard. If HadToBeOnNavMesh is enabled, requires a valid NavMesh and a NavMeshAgent on the Blackboard. Supports random distance ranges.

---

## Repeat
**Type:** Decorator Node

**Description:** A decorator node that repeatedly executes its child node indefinitely.

**Fail Condition:** This node runs its child node every time it is updated and always returns `Running`, ensuring that the child keeps executing without interruption. It does not track repetitions or enforce limits, so the child continues running as long as the parent tree is active.

**Notes:** Useful for continuous behaviors or looping actions in AI behavior trees.

---

## Retry
**Type:** Decorator Node

**Description:** A decorator node that retries its child node a specified number of times upon failure.

**Fail Condition:** This node executes its child node and, if the child returns `Failure`, it will retry the child up to the number of times defined by `Atempts`. `RemainingAtempts` tracks how many retries are left. If all attempts are exhausted forces the child to OnStop, sets its state to Slipping for easier debugging, 

**Notes:** Useful for handling transient failures or adding resilience to AI behaviors.

---

## RootNode
**Type:** Root Node

**Description:** The entry point of a behavior tree. Executes its single child node and propagates its state.

**Fail Condition:** Serves as the top-level node in a behavior tree. The RootNode itself does not perform actions; it simply updates its child and returns the child's state.

**Notes:** Essential for all behavior trees as the starting node. Has exactly one child node assigned.

---

## Scent_Set
**Type:** Action Node

**Description:** Sets and updates the Scent strength and visualization parameters in the Senses system. Supports constant, random, or curve-based values over time.

**Fail Condition:** Fails if SensesAssetPack is not installed or if the Scent component cannot be found on the blackboard's GameObject.

**Notes:** Uses reflection to update multiple Scent properties: strength, visualization toggle, color gradient, offset, node separation, and layer. Supports advanced entry types including random and curve-based variations.

---

## Scent_SetState
**Type:** Action Node

**Description:** Controls the state of the Scent component in the Senses system. Can enable, disable, or toggle its current state.

**Fail Condition:** Fails if SensesAssetPack is not installed or if the Scent component cannot be found on the blackboard's GameObject.

**Notes:** Directly sets the enabled property of the Scent MonoBehaviour. Internal toggle state is handled within the node.

---

## ScentZoner_SetState
**Type:** Action Node

**Description:** Controls the state of the Scent Zoner addon in the Senses system. Can enable, disable, or toggle the current state.

**Fail Condition:** Fails if SensesAssetPack is not installed or if the Scent component cannot be found on the blackboard's GameObject.

**Notes:** Uses reflection to set the Zoner Scent state. Internal toggle state is tracked.

---

## Search
**Type:** Action Node

**Description:** Searches for nearby objects using physics overlap checks, either by tag or by checking for a specific component type.

**Fail Condition:** Performs a Physics.OverlapSphereNonAlloc centered on the executor within the given range and LayerMask. If searching by tag, the detected colliders’ tags are compared against the provided list. If searching by behaviour, the detected colliders are checked for the specified component type. When a match is found, the result can be stored in the Blackboard as Transform, GameObject, or Vector3 depending on SaveParameters.

**Notes:** IMPORTANT: This node will only detect GameObjects that have a Collider (regular or trigger). Objects without colliders will never be found. Requires the executor’s Transform in the Blackboard. Supports including/excluding trigger colliders and will expand the hit array automatically if many objects are found. Returns Success when a valid object is found and saved, Failure if none match or parameters are invalid. OnStop performs no additional cleanup.

---

## Search_TagFrenzy
**Type:** Action Node

**Description:** Searches for nearby objects using physics overlap checks, either by tag or by checking for a specific component type.

**Fail Condition:** Performs a Physics.OverlapSphereNonAlloc centered on the executor within the given range and LayerMask. If searching by tag, the detected colliders’ tags are compared against the provided list. If searching by behaviour, the detected colliders are checked for the specified component type. When a match is found, the result can be stored in the Blackboard as Transform, GameObject, or Vector3 depending on SaveParameters.

**Notes:** IMPORTANT: This node requires TagFrenzy to be installed and initialized. It will only detect GameObjects that have a Collider (regular or trigger). Objects without colliders will never be found. Requires the executor’s Transform in the Blackboard. Supports including/excluding trigger colliders and will expand the hit array automatically if many objects are found. Returns Success when a valid object is found and saved, Failure if none match or parameters are invalid. OnStop performs no additional cleanup.

---

## SeeBooster_Set
**Type:** Action Node

**Description:** Sets and updates a selected aspect of the See buff in the Senses system, affecting detection parameters such as deltaSeeAwareness, centralVisionRadius, and peripheralVisionRadius. Supports constant, random, or curve-based values over time.

**Fail Condition:** Fails if the SensesAssetPack is not installed or if the SeeBooster component cannot be found on the blackboard's GameObject.

**Notes:** Uses reflection to modify SeeBooster properties for a selected affected aspect. Supports advanced entry types: constant, random between two values, and curve-based variations over time. Buff values are clamped between 0 and 1 and can increment over multiple frames.

---

## SeeBooster_SetState
**Type:** Action Node

**Description:** Sets or toggles the active state of a SeeBooster (vision vs stealth buff) in the Senses system.

**Fail Condition:** Fails if the SensesAssetPack is not installed or if the SeeBooster component cannot be found on the blackboard's GameObject.

**Notes:** Uses reflection to enable, disable, or toggle the SeeBooster's active state. This node does not modify buff values, only the component's active/inactive state.

---

## Selector
**Type:** Selector Node

**Description:** Executes child nodes in order until one succeeds or all fail.

**Fail Condition:** This composite node iterates through its children sequentially. If a child returns Success, the Selector immediately sets all remaining children to Slipping and returns Success. If a child returns Running, the Selector sets all remaining children to Slipping and continues returning Running. If a child returns Failure, the Selector moves to the next child. If all children fail, the Selector returns Failure. This Slipping behavior helps with debugging the tree state by clearly marking inactive children.

**Notes:** Use for fallback logic where the first successful child determines the outcome. Properly updates remaining children to Slipping for easier debugging.

---

## Senses_WasAnyHeard
**Type:** Fork Node

**Description:** Checks if any sound-based event is detected by the Senses system based on awareness, selected factions, and 'LookFor' type.

**Fail Condition:** Returns Success if a sound meeting the conditions is detected, Failure otherwise. Stores relevant information (Target, HearAwareness, Sound Position) in the blackboard according to SaveParameters.

**Notes:** Uses reflection to call Senses_WasAnythingHeard. Supports multiple SaveParameter lists for capturing detected sound data. Can filter by RequiredAwareness, selected factions, and LookFor search type.

---

## Senses_WasAnySeen
**Type:** Fork Node

**Description:** Checks if any target is detected by the Senses system based on awareness, distance, angle, and faction filters.

**Fail Condition:** Returns Success if a target meeting the conditions is found, Failure otherwise. Stores relevant information (Target, Awareness, Distance, Angle) in the blackboard according to SaveParameters.

**Notes:** Uses reflection to call Senses_WasAnythingSeen. Supports multiple SaveParameter lists for capturing detected target data. Can filter by RequiredAwareness, central range, selected factions, and LookFor type.

---

## Senses_WasAnySmelled
**Type:** Fork Node

**Description:** Checks if any target is detected by the Senses system based on smell awareness, scent strength, last known position, and faction filters.

**Fail Condition:** Returns Success if a scent meeting the conditions is found, Failure otherwise. Stores relevant information (Target, CurrentAwareness, ScentLastPosition, ScentStrength) in the blackboard according to SaveParameters.

**Notes:** Uses reflection to call Senses_WasAnythingSmelled. Supports multiple SaveParameter lists for capturing detected scent data. Can filter by RequiredAwareness, RequiredScentStrength, selected factions, and LookFor type.

---

## Sequencer
**Type:** Composite Node

**Description:** Executes child nodes in order, stopping at the first failure or running child.

**Fail Condition:** This composite node iterates through its children sequentially. If a child returns Running, the Sequencer sets all remaining children to Slipping for easier debugging and returns Running. If a child returns Failure, it sets all remaining children to Slipping and returns Failure immediately. If all children succeed, the Sequencer returns Success. The node keeps track of the current child to resume execution efficiently.

**Notes:** Use for sequential execution of behaviors where all steps must succeed. Supports proper debugging via the Slipping state.

---

## Shoot
**Type:** Action Node

**Description:** Spawns a Rigidbody projectile with configurable position, rotation, and scale, applying force towards a target using the Blackboard.

**Fail Condition:** This node allows runtime instantiation of a Rigidbody prefab, setting its position, rotation, and scale based on constant values or randomized ranges. It supports optional tagging and layer assignment. The node calculates direction from 'This' to 'Target' in the Blackboard and applies force to propel the Rigidbody. Validates that required references (Prefab, Rigidbody component, Blackboard references) exist before performing the action, logging warnings and returning Failure if any checks fail.

**Notes:** OnStart and OnStop are unused. OnUpdate performs instantiation and force application. Returns Success if the projectile is spawned and force applied successfully, otherwise Failure. Ensure Blackboard.This and Blackboard.Target are properly assigned before use.

---

## SphereCollider_Set
**Type:** Action Node

**Description:** Dynamically adjusts a SphereCollider's radius and center over time.

**Fail Condition:** Allows runtime modification of a SphereCollider's radius and center. Supports constant values, random ranges, and curves over time. Can target a SphereCollider directly or via a field on the Blackboard. Useful for procedural effects, dynamic hitboxes, and animation-driven collider changes.

**Notes:** Radius and center adjustments can be applied instantly or gradually using curves. The node cycles over a specified CurveLength to evaluate curve-based changes. Supports both single and randomized value updates for procedural variation.

---

## Stealth_Set
**Type:** Action Node

**Description:** Dynamically sets and updates the Stealth buff in the Senses system, affecting stealth detection mechanics. Supports constant, random, incremental, and curve-based values over time.

**Fail Condition:** Fails if the SensesAssetPack is not installed or if the Stealth component cannot be found on the blackboard's GameObject.

**Notes:** Uses reflection to modify the StealthBuff property of the Stealth component. Supports advanced entry types: constant, random between two values, incremental over time, and curve-based variations. Buff value is clamped between 0 and 1 and can be updated over multiple frames.

---

## Stealth_SetState
**Type:** Action Node

**Description:** Sets the state of the Stealth component in the Senses system. Can enable, disable, or toggle the Stealth behavior on the target GameObject.

**Fail Condition:** Fails if the SensesAssetPack is not installed or if the Stealth component cannot be found on the blackboard's GameObject.

**Notes:** Controls the active state of the Stealth component using the NewState enum: Enabled, Disabled, or Change (toggle). Uses SensesIntegrator to detect and access the correct Stealth component.

---

## Succeeder
**Type:** Decorator Node

**Description:** A decorator node that forces its child node to always return Success.

**Fail Condition:** This node executes its child node normally, but regardless of the child's actual result, the node itself always returns `Succes`. Useful for ensuring a behavior is considered successful even if its internal operations fail.

**Notes:** Useful for AI behaviors where failure of a subtask should not interrupt the overall behavior tree.

---

## Success
**Type:** Action Node

**Description:** An action node that always returns Success.

**Fail Condition:** This action node immediately returns a Success state when updated. It does not perform any operations during OnStart or OnStop.

**Notes:** Useful for testing, debugging, or explicitly marking a branch in a behavior tree as successful. It has no configurable properties.

---

## Target_Check
**Type:** Fork Node

**Description:** Checks whether the Blackboard has a valid Target reference assigned. Returns Success if the Target is not null, otherwise returns Failure.

**Fail Condition:** Fails if the Blackboard Target is null.

**Notes:** Useful as a quick guard check to ensure a Target exists before running other nodes that depend on it.

---

## Target_Check_B_Legacy
**Type:** Branching Node

**Description:** A branching node that checks whether a target is assigned in the Blackboard.

**Fail Condition:** This node succeeds if `Blackboard.Target` is not null, otherwise it fails.

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to `Target_Check` (ForkNode), which provides improved Blackboard integration and parameter flexibility.

**Legacy:** Planned removal in 1.20.0 – superseded by Target_Check (ForkNode).

---

## Target_Forget
**Type:** Action Node

**Description:** Clears the current target stored in the Blackboard.

**Fail Condition:** Simply sets the Blackboard's Target field to null. Useful for resetting AI targeting logic when a target is lost or no longer relevant.

**Notes:** This node performs no checks or additional logic. OnStart and OnStop do not perform any operations. Returns Success immediately after clearing the target.

---

## Throw
**Type:** Action Node

**Description:** Instantiates a Rigidbody prefab and propels it towards a target with calculated projectile velocity, supporting randomized or constant rotation, position, and scale.

**Fail Condition:** This node allows spawning of a Rigidbody prefab at runtime, applying position, rotation, and scale offsets. The node supports both constant and randomized values for each transform property. The Rigidbody is assigned a velocity calculated to reach the Blackboard target, factoring in the specified Force and gravity. The node also allows setting of LayerMask and tag for the spawned object. It validates the existence of the prefab and Blackboard references before performing the spawn.

**Notes:** OnStart is empty. OnUpdate handles instantiation, transform application, and physics velocity calculation. Returns Success if the Rigidbody is successfully spawned and directed, or Failure if any required references are missing. The node does not handle multi-threading safety; it assumes execution in a valid Unity context.

---

## Timer
**Type:** Decorator Node

**Description:** A decorator node that runs its child node for a specified duration of time.

**Fail Condition:** This node executes its child node only while the specified `TimerTime` has not elapsed. During this period, the child is updated normally. Once the timer expires, the node forces the child to OnStop, sets its state to Slipping for easier debugging, and returns Failure itself. The `RemainingTime` property tracks how much time has passed since the timer started.

**Notes:** Useful for time-limited behaviors, like temporary actions or delayed effects in AI or gameplay logic.

---

## TPC_Chase
**Type:** Action Node

**Description:** Chases a target using PlayerInput-driven movement, optionally using NavMeshAgent for path calculation guidance.

**Fail Condition:** Supports chasing a Transform, GameObject, or Vector3 target. Movement is always applied via PlayerInput_MoveInput. Returns Success when target is reached, Running while moving, and Failure if setup is invalid.

**Notes:** Uses EntryParameterTarget to define the target and EntryParameterStoppingDistance for stopping. NavMeshAgent is only used for pathfinding; motion is always driven through the Starter Assets PlayerInput system. Properly resets movement when stopping.

---

## TPC_Flee
**Type:** Action Node

**Description:** Flees from a target using PlayerInput-driven movement, optionally using NavMeshAgent for path calculation guidance.

**Fail Condition:** Supports fleeing from a Transform, GameObject, or Vector3 target. Movement is always applied via PlayerInput_MoveInput. Returns Success when safe distance is reached, Running while moving, and Failure if setup is invalid.

**Notes:** Uses EntryParameterTarget to define the threat/source to flee from and EntryParameterStoppingDistance for stopping. NavMeshAgent is only used for pathfinding; motion is always driven through the Starter Assets PlayerInput system. Flee distance can be constant or random between two values. Properly resets movement when stopping.

---

## TPC_MoveTowards
**Type:** Action Node

**Description:** Moves the character toward a specified target using PlayerInput-driven movement, with optional NavMeshAgent pathfinding support.

**Fail Condition:** Supports moving toward a Transform, GameObject, or Vector3 target. Movement is always applied via PlayerInput_MoveInput. Returns Success when the stopping distance is reached, Running while moving, and Failure if setup is invalid.

**Notes:** Uses EntryParameterTarget to define the destination and EntryParameterStoppingDistance to determine how close to stop. NavMeshAgent is only used for pathfinding; motion is always applied through the Starter Assets PlayerInput system. Properly resets movement when stopping.

---

## TPC_Patrol
**Type:** Action Node

**Description:** Moves the character along a set of waypoints using PlayerInput-driven movement, with optional NavMeshAgent support for pathfinding.

**Fail Condition:** Supports endless or finite patrols, inversion of patrol order, and starting at the nearest waypoint or the first in the array. Movement is always applied via PlayerInput_MoveInput. Returns Running while moving, Success when patrol ends (if RunEndless is false), and Failure if setup is invalid.

**Notes:** Uses Blackboard.Waypoints for patrol positions and EntryParameterStoppingDistance to determine how close to stop. NavMeshAgent is used only for pathfinding; motion is always applied through Starter Assets PlayerInput system. Properly resets movement when stopping and disables player input during patrol.

---

## TPC_SetSprint
**Type:** Action Node

**Description:** Controls the character's sprint state using PlayerInput-driven input.

**Fail Condition:** Reads a Boolean value from an EntryParameter to enable or disable sprinting. Integrates with Starter Assets ThirdPerson system. Returns Success if the value was applied successfully, and Failure if setup is invalid or dependencies are missing.

**Notes:** Uses EntryParameterBool to determine whether sprinting is active. Properly disables player input for other movement actions while applying sprint state. Requires Starter Assets ThirdPerson to be installed.

---

## Tra_ChargeTowards
**Type:** Action Node

**Description:** Moves the executor forward in a straight line towards a target at a specified speed until reaching a stopping distance.

**Fail Condition:** Supports target selection via Transform, GameObject, or Vector3. Speed and stopping distance are configurable through entry parameters. Charges are constrained to the initial distance calculated at the start to avoid overshooting.

**Notes:** Requires the executor's Transform in the Blackboard. Returns Running while charging, Success when reaching the stopping distance, and Failure if the target is too far or entry parameters are invalid.

---

## Tra_Chase
**Type:** Action Node

**Description:** Moves the executor towards a target at a specified speed, optionally rotating smoothly, until reaching a stopping distance.

**Fail Condition:** Supports target selection via Transform, GameObject, or Vector3. Speed, rotation speed, and stopping distance are configurable through entry parameters. Rotation can be immediate or smooth based on the SlerpRotation flag.

**Notes:** Requires the executor's Transform in the Blackboard. Returns Running while chasing, Success when within stopping distance, and Failure if entry parameters are invalid or the target is null.

---

## Tra_Flee
**Type:** Action Node

**Description:** Moves the executor away from a target until a chosen flee distance is reached, applying movement via PlayerInput-style translation.

**Fail Condition:** Supports fleeing from a Transform, GameObject, or Vector3. Speed, rotation speed, and stopping distance are configurable through entry parameters. Flee distance can be constant or randomized between two values, ensuring dynamic behavior.

**Notes:** Requires the executor's Transform in the Blackboard. Uses Slerp or direct rotation towards the computed flee direction. Returns Running while retreating, Success when reaching the flee distance, and Failure if entry parameters are invalid. Resets flee distance checks after each successful escape.

---

## Tra_LookAt
**Type:** Action Node

**Description:** Rotates the executor to face a specified target with optional height ignoring and smooth slerp rotation.

**Fail Condition:** Supports looking at a Transform, GameObject, or Vector3 target. Rotation speed is configurable through entry parameters. Can instantly snap or smoothly interpolate towards the target direction depending on settings.

**Notes:** Requires the executor's Transform in the Blackboard. If IgnoreHeight is enabled, only the horizontal plane is considered when facing the target. Returns Running while rotating with slerp, Success when alignment is achieved or when snapping, and Failure if entry parameters are invalid.

---

## Tra_MoveTowards
**Type:** Action Node

**Description:** Moves the executor toward a target position while rotating to face it, with optional smooth slerp rotation.

**Fail Condition:** Supports moving toward a Transform, GameObject, or Vector3 target. Speed, rotation speed, and stopping distance are configurable through entry parameters. Movement is applied directly via Transform translation.

**Notes:** Requires the executor's Transform in the Blackboard. If SlerpRotation is enabled, the executor smoothly interpolates toward the desired rotation; otherwise, it snaps immediately. Returns Running while moving, Success when within stopping distance of the target, and Failure if entry parameters are invalid.

---

## Tra_Patrol
**Type:** Action Node

**Description:** Moves the executor along a series of waypoints in a patrol pattern using Transform-based movement and rotation.

**Fail Condition:** Supports configurable speed, rotation speed, and stopping distance via entry parameters. Patrol can start at the nearest waypoint or the first in the array, loop endlessly, or invert direction when reaching the ends.

**Notes:** Requires the executor's Transform and a Waypoints array in the Blackboard. If SlerpRotation is enabled, the executor smoothly interpolates toward each waypoint; otherwise, rotation is snapped. Returns Running while patrolling, Success if all waypoints are completed (when RunEndless is false), and Failure if setup is invalid (e.g., no waypoints or bad entry parameters).

---

## Tra_ReachedDestination
**Type:** Fork Node

**Description:** Checks whether the GameObject assigned in the Blackboard has reached the specified destination using simple transform-based distance calculation. The destination and accuracy margin are provided via EntryParameters, which may reference a Transform, GameObject, or Vector3 for the position, and a Float or Int for the accuracy. The node compares the distance between the GameObject’s position and the destination, optionally ignoring height if enabled. Returns Success if the object is considered within range of the target, otherwise Failure.

**Fail Condition:** Fails if the Blackboard reference is missing, if the destination or accuracy parameter cannot be resolved, or if the object has not reached the target within the given margin.

**Notes:** Useful for non-NavMesh movement systems, 2D projects, or cases where distance checks are sufficient. The IgnoreHeight option allows horizontal-only comparisons for flat terrains or top-down views.

---

## Tra_ReachedDestination_Legacy
**Type:** Branching Node

**Description:** A branching node that checks whether the Transform has reached a given destination within a specified accuracy margin.

**Fail Condition:** This node evaluates the distance between the Executor’s Transform and a provided destination (Transform, GameObject, or Vector3). If the distance is within the required accuracy plus Blackboard’s stopping distance, the node succeeds; otherwise, it fails.

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to `Tra_ReachedDestination` (ForkNode), which provides improved parameter handling and consistency with other destination-checking nodes.

**Legacy:** Planned removal in 1.20.0 – superseded by Tra_ReachedDestination (ForkNode).

---

## Transform_SetPosition
**Type:** Action Node

**Description:** Sets the local position of a target Transform using various input types such as constant values, random ranges, or animation curves over time.

**Fail Condition:** Supports BlackboardHardCoded, BlackboardSoftCoded, or Node-assigned Transforms. Can apply position instantly, over time, or randomly between two constants or curves. Returns Success after applying the position, Failure if the target Transform is null.

**Notes:** Uses animation curves for smooth or custom movement over time. Automatically loops through the curve using CurveLength. Supports offsets, min/max ranges, and random evaluation between curves.

---

## Transform_SetRotation
**Type:** Action Node

**Description:** Sets the local rotation of a target Transform using various input types including constants, random ranges, or animation curves over time.

**Fail Condition:** Supports BlackboardHardCoded, BlackboardSoftCoded, or Node-assigned Transforms. Can apply rotation instantly, over time, or randomly between two constants or curves. Returns Success after applying the rotation, Failure if the target Transform is null.

**Notes:** Uses animation curves for smooth or custom rotational movement over time. Automatically loops through the curve using CurveLength. Supports offsets, min/max ranges, and random evaluation between curves.

---

## Transform_SetScale
**Type:** Action Node

**Description:** Sets the local scale of a target Transform using various input types including constants, random ranges, or animation curves over time.

**Fail Condition:** Supports BlackboardHardCoded, BlackboardSoftCoded, or Node-assigned Transforms. Can apply scale instantly, over time, or randomly between two constants or curves. Returns Success after applying the scale, Failure if the target Transform is null.

**Notes:** Uses animation curves for smooth or custom scaling over time. Automatically loops through the curve using CurveLength. Supports offsets, min/max ranges, and random evaluation between curves.

---

## Variable_Check
**Type:** Fork Node

**Description:** Checks a Blackboard variable against a required value. Supports both hardcoded fields (via reflection) and softcoded (dynamic) keys. Works with float, int, and bool variables depending on configuration.

**Fail Condition:** Fails if the field name is invalid, if the type does not match the expected type, or if the Blackboard does not contain the specified key.

**Notes:** Comparison operators (>, <, ==, >=, <=, !=) are available for numeric values. Bool variables are compared directly against the required bool. Use DeclaredType to choose between hardcoded Blackboard fields and softcoded key-value collections.

---

## Variable_Check_B_Legacy
**Type:** Branching Node

**Description:** A branching node that checks a variable on the Blackboard against a required value.

**Fail Condition:** Supports hard-coded fields via reflection or soft-coded Blackboard dictionaries. Can compare float and int values using a specified ComparisonType, or check bool values for equality.

**Notes:** Deprecated and will be removed in version 1.20.0. Please migrate to the new `Variable_Check` (ForkNode), which provides improved performance, type safety, and Blackboard integration.

**Legacy:** Planned removal in 1.20.0 – superseded by Variable_Check (ForkNode).

---

## Variable_Component_Check
**Type:** Action Node

**Description:** Checks a Component’s field or property against a specified value.

**Fail Condition:** Validates that the requested field or property exists on the target Component. Once found, the node retrieves its value and compares it against a configured reference value, supporting GreaterThan, LessThan, EqualTo, GreaterThanOrEqualTo, LessThanOrEqualTo, and NotEqualTo checks.

**Notes:** This node does not modify values — it only reads them and performs the requested comparison. On failure (invalid field/property, type mismatch, or failed comparison), warnings are logged in the console where applicable. OnStart and OnStop perform no operations. Returns Success when the field/property is valid and the comparison passes, or Failure otherwise.

---

## Variable_Component_Get
**Type:** Action Node

**Description:** Retrieves a value from a Component’s field or property on the target GameObject.

**Fail Condition:** This node dynamically retrieves the value of a specified field or property from a Component attached to a GameObject and can save it in Blackboard Hardcoded or SoftCoded. It supports multiple types (Float, Int, Bool, Vector3, Transform, GameObject). Optionally, overrides can be applied using NewValue, ChangeByValue, or MultiplyByValue rules.

**Notes:** The node does not modify the underlying field or property. It simply reads the value and optionally applies an override. Logs warnings when the field/property cannot be found or type mismatches occur.

---

## Variable_Component_Set
**Type:** Action Node

**Description:** Dynamically sets a Component’s field or property to a specified value, supporting overrides and Blackboard integration.

**Fail Condition:** This node allows runtime modification of selected fields or properties on a Component attached to a GameObject. It supports multiple types (Float, Int, Bool, Vector3, Transform, GameObject) and can source values from NewValue, Blackboard Hardcoded, or Blackboard Softcoded inputs. Optional override rules—NewValue, ChangeByValue, MultiplyByValue—can modify the original value before assignment. The node validates type and existence of each field/property before attempting to set it, logging warnings for missing fields or type mismatches.

**Notes:** OnStart initializes component and field/property references. OnUpdate performs the actual value assignment. Returns Success if all variables were successfully set, or Failure if any checks fail. Does not perform multi-threaded safety checks; ensure context is appropriate for runtime assignment.

---

## Variable_Set
**Type:** Action Node

**Description:** Reads a value from an Entry Parameter and writes it to one or more Save Parameters, supporting multiple types and Blackboard integration.

**Fail Condition:** This node allows runtime transfer of variables from an input source (Entry Parameter) to one or more targets (Save Parameters) in the Hardcoded or Softcoded Blackboard. It supports multiple types (Transform, GameObject, Vector3, Float, Int, Bool) and automatically handles type conversions where applicable. The node ensures that all references are populated on start and validates read/write success on each update, logging warnings for failed operations.

**Notes:** OnStart initializes entry and save parameter references. OnUpdate reads the source value and writes it to all configured targets, returning Success if all writes succeed, or Failure if any read/write operation fails. Useful for dynamically updating Blackboard variables or propagating runtime data between components.

---

## Wait
**Type:** Action Node

**Description:** Pauses execution for a specified amount of time, either a constant duration or a random duration within a range.

**Fail Condition:** Starts a timer when the node is activated. If WaitType is constant, waits exactly WaitTime seconds. If WaitType is random, waits a random duration between WaitTimeMin and WaitTimeMax. Returns Running while waiting and Success when the wait completes.

**Notes:** Important: This node only affects execution timing and does not perform any other actions. For random waits, WaitTimeMin and WaitTimeMax must be set, otherwise the behavior is undefined. OnStop resets the timer so that the next activation will re-evaluate the wait time if using random wait.

---

## Zoner_SetState
**Type:** Action Node

**Description:** Sets, disables, or toggles the Zoner stealth addon state for the Stealth component in the Senses system.

**Fail Condition:** Fails if the SensesAssetPack is not installed or if the Stealth component cannot be found on the blackboard's GameObject.

**Notes:** Uses reflection to enable, disable, or toggle the Zoner stealth addon. Does not modify stealth values, only the usage state of the addon.

---

