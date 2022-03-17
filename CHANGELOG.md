# Releases

## v0.6.5 - Custom Rigid Body Manipulation

* Removed restrictions for custom rigid bodies.
   * `RigidBody:Rotate()`
   * `RigidBody:SetPosition()`
* Optimized rigid bodies with CanRotate set to false (Avoided 2 loops in `RigidBody:Update()` and `RigidBody:Anchor()`).
* Added `RigidBody:SetScale()` method as an alternative to `RigidBody:SetSize()` but only for custom rigid bodies.
   * `RigidBody:SetScale(scale: number)` - The scale of the default size is 1. Passing in 2 as the scale will double the size of the custom rigid body. Similar to how UDim's scale property works.
* `RigidBody:SetPosition()` now works with custom rigid bodies.
* `RigidBody:Rotate()` now works with custom rigid bodies.
* Fixed bug in `RigidBody:SetSize()`. Earlier `RigidBody:SetSize()` changed only the size of the GuiObject and not the point-constraint structure.

## v0.6.4 - CanRotate Property, UniversalMass and changes to Friction

* Change default friction and airfriction to 0.1 and 0.02.
* Add CanRotate property as a valid property.
* Set CanRotate to true by default.
* Add new methods to RigidBodies
    * `RigidBody:CanRotate(canRotate: boolean)` - Determines if a rigid body can rotate after collisions or when forces are applied, extremely useful for creating platformer games, top-down games, character controllers etc.
* Restrict RigidBody:CanRotate() for custom rigidbodies
* Update RigidBody:Update() and Engine:Create() to adhere to CanRotate
* Add UniversalMass as a valid physical property of the Engine. By default set to 1
    * `Engine:SetPhysicalProperty("UniversalMass", 5)`

## v0.6.3 - Implemented Collision and Constraint Iterations

Iterations provide accurate calculations for more rigid and smoother physics. Constraint iterations are applied to `Constraint:Constrain()` method. Constraint iterations are extremely useful of rod constraints and rope constraints. Constraint iterations do not work on spring constraints.

Collision iterations are used to provide accurate and rigid collision detection and resolution. By default both of these iterations are set to 1. Iterations can be in the range of 1-10 only. Collision iterations can be set only if quadtrees are being used in collision detection.

Keep in mind that the higher the number of iterations the more accurate results. But, having more iterations means you'll have to sacrifice performance. The lesser the number of iterations, the better performance but we'll have to sacrifice on accuracy. So be careful where you use them!

* Added constraint and collision iterations and their functionality.
* Added new methods to Engine
    * `Engine:SetConstraintIterations(iterations: number)`
    * `Engine:SetCollisionIterations(iterations: number)`
* Updated RigidBody:Update() to use constraint iterations.
* Fixed RigidBody.Touched and RigidBody.TouchEnded after adding iterations.
* Fixed MouseConstraint plugin bug - Stop looping through the rigid bodies if we have already found one to attach to the mouse.

## v.0.6.1 - Bug fixes, Improvements to Physics Objects and Error Messages

* "CanTouch" is not longer a valid property for rigid bodies.
* Fixed bug where `Point:KeepInCanvas()` won't calculate collisions accurately.
* RigidBody.CanvasEdgeTouched event now fires only the moment a rigid body collides with the canvas' edge and not every rendered frame.
* Improved exception handling code.
* Improved error messages for `Engine:Create()`
   * If an invalid property is specified, the error message will now contain the name of the invalid property you specified for debugging.
   * If properties for completely different physics objects are specified, the error message will now contain the name of the invalid property you specified.
   * If a must-have property is not specified, the error message will now contain the name of that must-have property.
* RigidBody.Touched event now returns the rigid body's id as well as the collision information (Collision axis, depth, vertex and edge). This can be beneficial for creating visual effects for visualizing collisions.

## v0.6.0 - Clone() for Custom Rigid Bodies

* RigidBody:Clone() now works for custom rigid bodies.
* Added Structure parameter to RigidBody.new() to cache the rigid body's structure for the future.

## v0.5.7 - Optimizations

* Optimizations to how Rigid bodies are updated. There was a flaw earlier, I was using 2 for loops for the same task but in different locations. This has been cut down to just 1 loop.
* Extra work for non-collidable rigid bodies is not longer performed. Thanks to @boatbomber for the PR.
* Events like .Touched and .TouchEnded are fired only if the events are connected using :Connect() somewhere.
* Prevent memory leaks by:
   * Disconnecting all Engine events when Engine:Stop() is called.
   * Warning and returning from Engine:Start() if its already running.
   * Disconnecting and destroying RigidBody.TouchEnded event when RigidBody:Destoy() is called.


## v0.5.6 - Engine:GetDebugInfo(), Engine.Updated and bug fixes

* Added `Engine:GetDebugInfo()`
* Removed `Engine:GetCurrentCanvas()`
* Fixed rope constraints and KeepInCanvas property of RigidBodies.
* Added new event `Engine.Updated`

## v0.5.5 - A new look for Spring Constraints

* Fixed bug where setting Visible to true when creating a custom constraint won't rendcmder the constraint on screen.
* Spring Constraints now use a Coil ImageLabel instead of a straight line when rendered for better visual distinction between springs, ropes and rods.<br/>
![NUEeFzjp9j__online-video-cutter_com__SparkVideo](https://user-images.githubusercontent.com/74130881/147813007-eec97d76-2546-4468-a3ae-38b0cf006bc6.gif)

## v0.5.4 - TouchEnded Event

* Added `.TouchEnded` event to RigidBodies. This event fires the moment two RigidBodies stop colliding with each other.
* ```lua
  SomeRigidBody.TouchEnded:Connect(function(otherID)
      local OtherRigidBody = Engine:GetBodyById(otherID)
      print("Touch ended")
  end)
  ```
* Fixed AnchorPoint bug with anchored RigidBodies. Anchored RigidBodies can now have any AnchorPoint, which does not affect how they are positioned on the screen. cc: @Will_Rocks1

## v0.5.3 - Improvements to .Touched Event.

* Improvements to .Touched Event. The event does not fire every frame anymore if two bodies are colliding. It’ll only fire the moment the two bodies collide. Playing collision sounds etc is now possible.
* `RigidBody:Destroy()` now takes in an optional argument “keepFrame: boolean|nil”. If passed in as true, the RigidBody’s UIElement will not be destroyed. If passed as false or nil, the RigidBody’s UIElement will be destroyed along with it.
* Changes to Signal utility. Curtsy of @LucasMZ_RBX
    * Made Connection property .Connected public

## v0.5.2 - Introducing Nature2D Plugins!

* Added new methods to Points
* Point:SetMaxForce(maxForce: number)
* Added new methods to RigidBodies
    * RigidBody:SetMaxForce(maxForce: number)
* Added Plugins
    * Quad
    * Triangle
    * MouseConstraint

## v0.5.1 - Support for different Masses of RigidBodies

You can now set any mass value you like for different RigidBodies to see changes in Collision Response and how forces are applied to each RigidBody. This paves the way for more accurate physical simulations with guis!

* Added the ability to set a mass value other than 1 for different RigidBodies
* Added new methods to RigidBodies
   * `RigidBody:SetMass(mass: number)`
* Updated Point:ApplyForce() and RigidBody:ApplyForce(). Divides the force by the RigidBody’s mass to calculate acceleration.
* Updated Collision detection and response to have effects according to the masses of each RigidBody. Now calculates accurate ratios for the force applied to each body after collision.
* Cleaned some code, replaced bad practices with good ones, used Vector2:Dot() which is slightly faster than calculating the dot product of two vectors from scratch.

## v0.5 - Custom RigidBody Support!

Major Release.

* Fixed Engine:CreateCanvas() - Canvas’ can now be re-initialized.
* Fixed Constraint:Render() - Prevent support constraints from rendering
* Added support for custom RigidBodies
* Added new Valid Property for RigidBodies - Structure: table
* Updated Collision Detection and Response to work with custom RigidBodies
* Updated Engine:Create()
* Restrict certain methods from being used for custom RigidBodies

## v0.4.5 - New Events, Methods and Improvements

* Added new events to Engine
   * `Engine.ObjectAdded` - Fires when a new Point, Constraint or RigidBody is created.
   * `Engine.ObjectRemoved` - Fires when a new Point, Constraint or RigidBody is destroyed.
* Added new methods to RigidBodies
   * `RigidBody:GetTouchingRigidBodies()` - Returns all RigidBodies that are in collision with the current RigidBody. 
* API Changes to `Point:ApplyForce()` and `RigidBody:ApplyForce()`
   * Added a second optional parameter 'time'.
   * The time parameter can be used to apply a force for a certain amount of time.
   * Example: `Body:ApplyForce(Vector2.new(.4, 0), 3) -- applies the force for 3 seconds`

## v0.4.4 - Ability to clone RigidBodies

Added the ability to clone already existing RigidBodies! By default the clone won't inherit the original RigidBody's states, lifespan or filtered RigidBodies. In order to copy those too, pass in the deepCopy parameter as true!
* Added `RigidBody:Clone(deepCopy: boolean | nil)`

## v0.4.3 - New Valid Properties, API Changes and Improvements

* API Changes to `RigidBody:SetPosition()`, `RigidBody:SetSize()` and `Point:SetPosition()`
   * These methods no longer take in a Vector2 value, rather individual x and y values.
   * Example: `RigidBody:SetPosition(100, 100)`, `RigidBody:SetSize(20, 40)` and `Point:SetPosition(500, 0)`
* Improvement how RigidBody lifespans work. They no longer use `os.time()`. `os.clock()` is now used thus high precision values for RigidBody lifespans work too!
* Fixed `Point:Render()` and `Constraint:Render()` - Added checks to see if Engine's canvas has a frame.
* Added new Valid properties to `Engine:Create()`. You can now pass in this properties to `Engine:Create()`.
* New valid properties for Points
   * `KeepInCanvas: boolean | nil`
   * `Color: Color3 | nil`
   * `Radius: number | nil`
* New valid properties for Constraints
   * `SpringConstant: number | nil`
   * `Color: Color3 | nil`
* New valid properties for RigidBodies
   * `LifeSpan: number | nil`
   * `KeepInCanvas: boolean | nil`
   * `Gravity: Vector2 | nil`
   * `Friction: number | nil`
   * `AirFriction: number | nil`

## v0.4 - Refactored Object Creation Completely

Major Release. 

* Removed `Engine:CreatePoint()`
* Removed `Engine:CreateConstraint()`
* Removed `Engine:CreateRigidBody()`
* Added `Engine:Create(objectName: string, propertyTable: table)`

Creating new Constraints, Points and RigidBodies is much simpler than before! You just need to care about 1 single method - `Engine:Create()`. This method takes in 2 parameters. The first parameter being the type of instance you are creating. This is either "Point", "Constraint" or "RigidBody". The second parameter consists of the properties you wish to assign to the object. 

Intellisense of VSCode and Studio's script editor will suggest these properties to you!

https://user-images.githubusercontent.com/74130881/142725251-577454d1-abdd-40af-bddf-db1d4c286b2b.mp4

* Changed how you require Nature2D.
   *  Earlier: `require(ReplicatedStorage.Nature2D.Engine)`
   * Now: `require(ReplicatedStorage.Nature2D)`
* Added "Snap" to a valid property of `Point`
* API cleanup
* Rewrote commented docs in the source code.
* Fixed `Point:Render()` - Set point's anchor point to 0.5, 0.5
* Fixed Rod Constraints and how I solve them - See issue [#8](https://github.com/jaipack17/Nature2D/issues/8)

## v0.3.6 - Basic Collision Filtering

Implemented basic collision filtering API for RigidBodies! You can now ignore collisions for 2 rigid bodies while still being able to let them collide with other rigid bodies!

* Added Collision Filtering to Engine
* Added new Methods to RigidBodies
  * `Engine:FilterCollisionsWith(otherRigidBody: RigidBody)`
  * `Engine:UnfilterCollisionsWith(otherRigidBody: RigidBody)`
  * `Engine:GetFilteredRigidBodies()`

## v0.3.4 - Anchor Point Support

* Added Anchor point support
* Added new methods to RigidBodies
   * `RigidBody:GetCenter()`
* Added new methods to Points
   * `Point:SetPosition(newPosition: Vector2)`
* Bug fixes
   * Fixed Parent hierarchy errors in Points. 
   * Fixed `Point:Update()` "Cannot read property 'Parent' of nil" errors.
   * Fixed `Point:KeepInCanvas()` "Cannot read property 'Parent' of nil" errors.

## v0.3.2 - Major Improvements to Frictional Forces

* Fixed a bug where changes to physical properties before creating any RigidBodies, Constraints or Points won't affect/apply to newly created objects.
* Friction only applies if RigidBodies collide with each other or the edges of the canvas. If none of those conditions are true, AirFriction is applied.
* Added AirFriction physical property to Engine, Points and RigidBodies. Frictional force applied when a RigidBody neither touches another body nor the edges of canvas.
   * `Engine:SetPhysicalProperty("AirFriction", 0.1)`
* Friction and AirFriction are set to 0.01 by default. (0.99 damping value).
* Changed how we pass parameters when initializing or updating friction of the engine or rigidbodies. The closer the friction to 1, the higher it is. The closer the friction to 0, the lower it is. Same applies for AirFriction. Values not in the range of 0-1 are automatically clamped down.
* Added new Methods to RigidBodies
  * `RigidBody:SetAirFriction(airfriction: number)`

## v0.3.1 - Fixes to Collision Detection & New Methods

* Made `restLength: number?` an optional parameter for `Engine:CreateConstraint()`.
* Fixes to Collision Detection - Collision detection is no longer skipped at low frame rates.
* Fixed how forces are applied to Points when the engine is framerate independent.
* Added new methods to Engine
   * `Engine:FrameRateIndependent(independent: boolean)` - Determines if Frame rate does not affect the simulation speed. By default set to true.
* Added new methods to Constraint
   * `Constraint:GetParent()`
* Added new methods to Point 
   * `Point:GetParent()`

## v0.3 - New Constraints: Ropes, Rods & Springs!

Major Release. 

* Checks to prevent division by 0. 
* Fix `Constraint:SetLength()`. Disregard invalid lengths. (length <= 0)
* Fix `Engine:CreateConstraint()`. Disregard invalid thickness and lengths. (length & thickness <= 0)
* Added new methods to `Constraint`
   * `Constraint:SetSpringConstant(k: number)`
   * `Constraint:GetId()`
* New "type" paramater to `Engine:CreateConstraint()`
* New "restLength" parameter to `Engine:CreateConstraint()`
   * `Engine:CreateConstraint(Type: string, point1: Point, point2: Point, visible: boolean, thickness: number, restLength: number)`
* Added rope constraint type
   * Constraints that have an upper constrain limit and exclusive of a lower limit. Similar to Roblox's 3D Rope Constraints.
* Added rod constraint type
   * These constraints are similar to how Rope constraints function. But unlike rope constraints, these constraints have a fixed amount of space between its points and aren't flexible. These constraints can move in all directions just how rope constraints can, but the space between them remains constant.
* Added spring constraint type
   * Spring constraints are elastic, wonky and flexible. Perfect for various simulations that require springs. Achieved using Hooke's Law.
* Type parameter in Engine:CreateConstraint() must be "SPRING", "ROD" or "ROPE" (text case does not matter).

## v0.2.4 - Improvements to Code

* Add type definitions & annotations 
* Type check everything (almost)
* Remove code redundancies 
* Replace deprecations
   * `Vector2.x` -> `Vector2.X`
   * `Vector2.y` -> `Vector2.Y`
   * `Vector2.magnitude` -> `Vector2.Magnitude`
   * `Vector2.unit` -> `Vector2.Unit`
   * ... etc

## v0.2.3 - State Management

Added state management for RigidBodies. Individual RigidBodies can have their own States/Custom Properties now!
* `RigidBody:SetState(state: string, value: any)`
* `RigidBody:GetState(state: string)`

Example usage:

```lua
local Body = Engine:CreateRigidBody(frame, true, false)
Body:SetState("Health", 100)

local Killbrick = Engine:CreateRigidBody(frame2, true, true)

Killbrick.Touched:Connect(function(bodyID)
    if Body:GetId() == bodyID then
        local oldHealth = Body:GetState("Health")
        Body:SetState("Health", oldHealth - 1)
        return
    end
end)
```

## v0.2.0 - Quadtrees in Collision Detection!

Quadtrees have now been implemented into the collision detection algorithm making the engine 10 times faster than before. Instead of wasting resources on wasted and unnecessary collision detection checks, RigidBodies are now distributed into different regions of a quadtree with a collision hit range. RigidBodies only in this hit range are processed with collision detection checks. This opens the gate for many new creations that required a larger amount of RigidBodies to be simulated!

Since Quadtrees are still in beta, there may occur bugs and unwanted behavior. If you encounter any, be sure to open an issue at the github repository. I'll be adding configuration methods for you to switch between traditional methods of collision detection or quadtrees.

## v0.1.2 - Quadtrees in the works!

* Collision Detection is now being re-written, with the addition of quadtrees! 
* Fixed `RigidBody:Destroy()` Connections are now destroyed when `RigidBody:Destroy()` is called.
* Added new methods to Points
  * `Point:Velocity()`
* Added new methods to RigidBodies
  * `RigidBody:AverageVelocity()`
* Added new methods to Constraints
  * `Constraint:GetPoints()`
  * `Constraint:GetFrame()`
* Fixed `Constraint:Destroy()`. Does not spam errors if connected to a RigidBody now.

<img src="https://user-images.githubusercontent.com/74130881/138874154-ea20396e-9a19-4d41-9925-3c3a008f4911.gif" alt="gif" width="500px" />
  

## v0.1.0 - Additions to Constraints, Engine and RigidBodies.

* Added new methods to Constraints
  * `Constraint:SetLength()` - [Documentation](https://github.com/jaipack17/Nature2D/tree/master/docs/api/constraint#constraintsetlength)
* Added new methods to RigidBodies
  * `RigidBody:IsInBounds()` - [Documentation](https://github.com/jaipack17/Nature2D/tree/master/docs/api/rigidbody#rigidbodyisinbounds)
* Added new events to Engine
  * `Engine.Started`
  * `Engine.Stopped`
```lua
engine.Started:Connect(function()
    -- fires when the engine starts
end)

engine.Stopped:Connect(function()
    -- fires when the engine stops
end)
```

## v0.0.5 - Fixes and New Stuff!

* The library now utilizes sleitnick's Signal class instead of bindable events.
  * `RigidBody.CanvasEdgeTouched:Connect()`
  * `RigidBody.Touched:Connect()`
* Bug Fixes
  * Error Handling
* Improved code - Removed bad practices 
* RigidBody.CanvasEdgeTouched event returns the edge the RigidBody collides with.
* Added new methods to Engine
  * Engine:GetCurrentCanvas() - [Documentation](https://github.com/jaipack17/Nature2D/blob/master/docs/api/engine/README.md#enginegetcurrentcanvas)
  
<hr/>

Added new example which covers the concept of creating Custom Constraints, where we create the following simulation of a RigidBody hanging from a rope, and wind forces being applied on it.

![rope](https://user-images.githubusercontent.com/74130881/138543851-c4871e17-f51e-4b5b-9d11-0fc63a32de02.gif)

## v0.0.4 - Engine:SetSimulationSpeed() & Documentation

* Updated API-References for RigidBodies, Constraints, Points and Engine in response to this and previous updates.
* Documented source code with comments.
* Better error handling and error messages
* Fixed Engine:SetPhysicalProperty() bug where updating a physical property before creating a rigidbody did not change the properties for the rigidbody.
* Added new Methods to Engine
   * `Engine:SetSimulationSpeed()`

## v0.0.3 - Frame-rate Independent!

**Earlier:** Simulations running on different frame rates had a difference in their speeds. A simulation running on 60fps would run faster than that of a simulation running at 30fps.

**Now:** Frame-rate no longer affects simulations. A RigidBody covering a distance of 10 units in a simulation running at 30fps and a simulation running 60fps will take almost the same time to reach the destination. 

## v0.0.2 - Custom Point Support, Configuration Methods and More!

* Installation through Wally! Nature2D can now be installed using [Wally](https://github.com/UpliftGames/wally), the package manager for Roblox. This requires wally to be installed on your device. In order to install Nature2D, add a dependency to your `wally.toml` file
   * ![dependency](https://doy2mn9upadnk.cloudfront.net/uploads/default/original/4X/3/4/8/348c3d6c9436a92cf44160b9e8aee5b2a5933193.png)

   * After you have added the dependency, run `wally install` in the command line. A "Packages" directory is created containing the library. You can now use Nature2D in your external editor using wally!

* Bug Fixes 
  * Linked Issue: [#1](https://github.com/jaipack17/Nature2D/issues/1)  
* `Engine:CreateCanvas()` now has an optional 'frame' parameter to help render custom points and constraints.
* Refactored certain segments of code for better readability. 
* Added Custom Point support to Engine
  * `Engine:CreatePoint()`
  * `Engine:GetPoints()` 
* Added new Configuration methods to Constraints
   * `Constraint:Stroke()` 
* Added new Configuration methods to Points
   * `Point:SetRadius()`
   * `Point:Stroke()`
   * `Point:Snap()`

## v0.0.1 - Improvements & New Methods

* Improved architecture for anchored RigidBodies.
* Constraints now have their own unique IDs like RigidBodies.
* Added new Methods to Constraints
   * `Constraint:GetLength()` 
   * `Constraint:Destroy()`
* Added new Methods to Engine
   * `Engine:GetConstraints()`
   * `Engine:GetConstraintById()`
* Added new Methods to RigidBodies
   * `RigidBody:SetFriction()`
   * `RigidBody:SetGravity()`
