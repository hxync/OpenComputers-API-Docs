# os_energyturret

## Summary

The `os_energyturret` component controls an OpenSecurity energy turret. It can report the current gun orientation, move to a new yaw and pitch, extend or retract the shaft, arm or disarm the weapon, switch power state, and fire an energy bolt.

## Availability

- Repository: `opensecurity`
- Component name: `os_energyturret`
- Typical host: placed OpenSecurity energy turret

## Usage

```lua
local component = require("component")
local turret = component.os_energyturret

if not turret then
  error("os_energyturret component not installed")
end
```

## Orientation And Motion

- `yaw` is tracked in degrees and wrapped into `0` through `<360`.
- `pitch` is tracked in degrees and clamped to `-90` through `90`.
- Actual reachable pitch is further limited by shaft length and whether the turret is upright.
- `moveToRadians` converts radians to degrees internally.

Base movement speed:

- `4` degrees per tick

Movement upgrades:

- slot `2`: `+2.5` degrees per tick
- slot `3`: `+2.5` degrees per tick

## Shaft Rules

- Requested shaft length is clamped to `0` through `2`.
- If there is no free air block above or below the turret, extension beyond `0.5` is not allowed.
- `extendShaft` returns the actual accepted target length.

## Power, Cooldown, And Upgrades

Passive power drain while powered:

- `10` buffer energy per tick

Damage upgrades:

- slot `0`: damage `* 3`
- slot `1`: damage `* 3`

Energy upgrades:

- slot `6`: firing energy cost `* 0.7`
- slot `7`: firing energy cost `* 0.7`

Cooldown upgrades:

- slot `4`: cooldown decreases 3 extra steps per tick
- slot `5`: cooldown decreases 3 extra steps per tick

Base shot values:

- base damage: `3`
- base firing cost: `75` energy
- base cooldown after firing: `200` ticks

## API

### getYaw

- Syntax: `turret.getYaw()`
- Returns: `number`
- Purpose: Returns the current real yaw in degrees.

### getPitch

- Syntax: `turret.getPitch()`
- Returns: `number`
- Purpose: Returns the current real pitch in degrees.

### isOnTarget

- Syntax: `turret.isOnTarget()`
- Returns: `boolean, delta`
- Purpose: Reports whether the turret has reached its current target orientation.

Behavior:

- `delta` is the combined absolute yaw and pitch error.
- The boolean becomes `true` when `delta < 0.5`.

### isReady

- Syntax: `turret.isReady()`
- Returns: `boolean`
- Purpose: Returns whether the turret is armed, cooled down, and has a fully extended barrel ready to fire.

### isPowered

- Syntax: `turret.isPowered()`
- Returns: `boolean`
- Purpose: Returns whether the turret currently considers itself powered.

### extendShaft

- Syntax: `turret.extendShaft(length)`
- Returns: `number`
- Purpose: Sets the target shaft extension length.

Parameters:

- `length:number`: requested shaft length from `0` to `2`.

Example:

```lua
print("Accepted shaft length:", turret.extendShaft(1.25))
```

### getShaftLength

- Syntax: `turret.getShaftLength()`
- Returns: `number`
- Purpose: Returns the current real shaft extension.

### moveTo

- Syntax: `turret.moveTo(yaw, pitch)`
- Returns: `true`
- Purpose: Changes the turret target orientation in degrees.

Parameters:

- `yaw:number`: target yaw in degrees
- `pitch:number`: target pitch in degrees

Behavior:

- Throws `powered off` when the turret is not powered.

Example:

```lua
assert(turret.moveTo(90, 15))
```

### moveToRadians

- Syntax: `turret.moveToRadians(yaw, pitch)`
- Returns: `true`
- Purpose: Changes the turret target orientation in radians.

Parameters:

- `yaw:number`: target yaw in radians
- `pitch:number`: target pitch in radians

Behavior:

- Throws `powered off` when the turret is not powered.

Example:

```lua
assert(turret.moveToRadians(math.pi / 2, 0))
```

### setArmed

- Syntax: `turret.setArmed(armed)`
- Returns: `true`
- Purpose: Arms or disarms the weapon.

Parameters:

- `armed:boolean`: desired armed state.

Behavior:

- When disarmed, the barrel retracts over time and `isReady()` becomes `false`.

### powerOn

- Syntax: `turret.powerOn()`
- Returns: `true`
- Purpose: Restores turret power state.

### powerOff

- Syntax: `turret.powerOff()`
- Returns: `true`
- Purpose: Turns the turret off and freezes its current targets to the present position.

### fire

- Syntax: `turret.fire()`
- Returns: `true`
- Purpose: Fires one energy bolt.

Behavior:

- Requires the turret to be powered.
- Requires the turret to be armed.
- Requires the barrel extension to be fully ready.
- Sets cooldown to `200` ticks before cooldown upgrades speed it back down.
- Spawns an `EntityEnergyBolt` in the world.

Common failures:

- `powered off`
- `Not armed`
- `gun hasn't cooled`
- `not enough energy`

Example:

```lua
if turret.isReady() then
  assert(turret.fire())
end
```

## Example

```lua
turret.powerOn()
turret.setArmed(true)
turret.extendShaft(1.5)
turret.moveTo(180, 10)

repeat
  os.sleep(0.1)
until turret.isOnTarget()

if turret.isReady() then
  turret.fire()
end
```

## Notes

- When powered, the turret continuously draws energy every tick and automatically powers itself off if the connector cannot pay that drain.
- The actual projectile yaw may be reversed by the `turretReverseRotation` config option.

## Related

- `component`
- `os_alarm`
