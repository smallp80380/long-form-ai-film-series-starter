# Manifest templates

## Character bible

Create `characters/<character_id>/character.yaml`:

```yaml
id: "char_<name>"
name: "<name>"
revision: "v001"
identity_lock:
  age_range: "<value>"
  face: "<stable facial traits>"
  hair: "<stable hair traits>"
  body: "<stable silhouette/proportions>"
wardrobe:
  default: "<description>"
  forbidden_changes: []
performance:
  posture: "<description>"
  gesture_range: "<description>"
  voice: "<description>"
references:
  approved: []
negative_constraints: []
```

## Location bible

Create `locations/<location_id>/location.yaml`:

```yaml
id: "loc_<name>"
name: "<name>"
revision: "v001"
layout: "<spatial description>"
fixed_landmarks: []
lighting_states: {}
palette: []
props_and_state: []
references:
  approved: []
forbidden_changes: []
```

## Scene manifest

Create `scenes/<episode>/<scene>/scene.yaml`:

```yaml
id: "ep001_sc010"
purpose: "<dramatic purpose>"
location_id: "<location_id>"
character_ids: []
time_of_day: "<value>"
continuity_in: "<state entering scene>"
continuity_out: "<state leaving scene>"
lighting_revision: "<revision>"
shot_ids: []
```

## Shot / take manifest

Create `shots/<episode>/<scene>/<shot>/shot.yaml`:

```yaml
id: "ep001_sc010_sh010"
scene_id: "ep001_sc010"
editorial_purpose: "<why this shot exists>"
duration_target_seconds: null
camera:
  framing: "<wide|medium|close|other>"
  lens: "<value>"
  height: "<value>"
  angle: "<value>"
  movement: "<value>"
action: "<single observable action>"
dialogue_or_audio_cue: "<value>"
start_keyframe: null
end_keyframe: null
takes:
  - id: "ep001_sc010_sh010_tk001"
    parent_take: null
    workflow_revision: "<value>"
    prompt_revision: "<value>"
    seed: null
    status: "planned"
    output: null
    qc:
      decision: null
      notes: []
```

