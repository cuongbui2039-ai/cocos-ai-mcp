# Cocos Creator 3.8 + MCP Rules

## Asset & Component Binding
- All component properties and asset references must be assigned **in the Cocos Editor** (visible in the Properties panel)
- **Do not** use `addComponent()` or assign assets in `onLoad()` / `start()` at runtime
- Scripts should only use `getComponent()` to access components already on the node from the editor
- Use MCP `cocos_component_add` to add components, `cocos_component_set_asset` to assign assets

## MCP `set_property` Is Broken for Certain Types
- `cc.Size` (contentSize): width gets stringified, height becomes 0 → fix directly in `.scene` file via Python
- `boolean` / numeric enum: stored as string `"true"` / `"1"` instead of proper type → fix in `.scene` file
- Plain numbers (e.g. pipeSpeed): echoes success but does not persist → fix in `.scene` file
- **After every `set_property`**: always call `node_inspect` to verify the actual value

## Editing `.scene` Files Directly
- Save the scene in the editor first (`cocos_scene_save`) to prevent the editor from overwriting your changes
- After editing the file, the editor will prompt to reload → choose **Reload**
- Use a Python script for safe replacements — avoid sed/awk with complex JSON

## Node Lookup
- `find('Canvas/UI/StartPanel')` is unreliable at runtime
- Use `this.node.parent.getChildByName()` to traverse the hierarchy instead

## Physics 2D
- Kinematic (Bird) + Dynamic pipe (gravityScale=0) = contact callbacks work ✓
- Kinematic + Kinematic = NO contact events
- `enabledContactListener: true` must be set on RigidBody2D to receive callbacks
- `sensor: true` on BoxCollider2D = overlap detection only, no physical pushback

## Restarting the Game
- Do not use `director.loadScene()` in editor play mode — nodes will be lost
- Reset in-place: reset each component (Bird, PipeSpawner) and reset state/UI

## Preview
- `cocos_preview_screenshot` uses headless Chrome which has no WebGL → screenshot is always black
- Test visuals in a real browser at `http://localhost:7456`

## `@property` Serialized Values
- Changing a default value in TypeScript code does not affect nodes already in the scene
- Must update the value directly in the editor or edit the `.scene` file
