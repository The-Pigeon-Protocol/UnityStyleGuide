The Ramen Unity Style Guide
===========================

> For file structure, naming conventions and other things

These are guidelines for keeping your project organized and allow your team to quickly find the assets they need. Games are large projects that span several months, thus having standardized conventions that make sense will avoid headaches in the long run.

Note that your team and project might have different needs, use different software, etc. Use this guide only as a base to decide on what works for your team. Every project should have its own, easy to find, style guide, so everyone in the team is up to date in the project's conventions.

# Table of Contents

- [Asset Naming](#asset-naming)
  - [Folders](#folders)
  - [Source code](#source-code)
  - [Non-code assets](#non-code-assets)
- [Directory and File Structure](#directory-and-file-structure)
  - [Assets](#assets)
  - [Scripts](#scripts)
  - [Models](#models)
- [Workflow](#workflow)
  - [Models](#models)
  - [Textures](#textures)
  - [Configuration files](#configuration-files)
  - [Localization](#localization)
  - [Audio](#audio)
- [Be Consistent](#be-consistent)

# Asset Naming

First of all, no\ spaces\ on file or directory names.

## Folders

`PascalCase`

Prefer a deep folder structure over having long asset names.

Directory names should be as concise as possible, prefer one or two words. If a directory name is too long, it probably makes sense to split it into sub directories.

Try to have only one file type per folder. Use `Textures/Trees`, `Models/Trees` and not `Trees/Textures`, `Trees/Models`. That way its easy to set up root directories for the different software involved, for example, Substance Painter would always be set to save to the Textures directory.

If your project contains multiple environments or art sets, use the asset type for the parent directory: `Trees/Jungle`, `Trees/City` not `Jungle/Trees`, `City/Trees`. Since it makes it easier to compare similar assets from different art sets to ensure continuity across art sets.

## Source Code

Use the naming convention of the programming language. For C# and shader files use `PascalCase`, as per C# convention.
Use regions for organising classes if they have a lot of properties.

```C#
class MyClass : MonoBehaviour
{
  #region Editor Properties
  [SerializedField] float _foo = 0f;
  [SerializedField] GameObject _bar = default;
  #endregion

  #region Public Properties
  public float Foo => _foo;
  public bool IsActive {get; set;} = true;
  #endregion

  #region Private Members
  private int _secretNumber = 0;
  private bool _isSecret = true;
  #endregion

  #region Constants
  private const int ULTIMATE_ANSWER = 42;
  private const string PASSWORD = "fortytwo";
  #endregion


  // Unity methods and callbacks

  // Public methods

  // Private utility methods
}
```


## Scenes

`PascalCaseScene`

Make sure to give scenes sensible names that indicate their purpose.

For personal testing scenes consider putting them into a separate folder related to you. E.g. `Assets/Level/Scenes/Bob/PlayerTestScene`.

Having multiple developers work on the same scene at the same time leads to (possibly unresolvable) merge issues. Prefer a workflow where you work on features in a dedicated feature or testing scene that only you access and then exporting to prefabs. Integration to production scenes needs be coordinated and you have to communicate over who is working on those scenes at any given moment.


## Non-Code Assets

`snake_case`

Use `tree_small` not `small_tree`. While the latter sound better in English, it is much more effective to group all tree objects together instead of all small objects.

`camelCase` where necessary. Use `weapon_miniGun` instead of `weapon_gun_mini`. Avoid this if possible, for example, `vehicles_fighterJet` should be `vehicles_jet_fighter` if you plan to have multiple types of jets.

Prefer using descriptive suffixes instead of iterative: `vehicle_truck_damaged` not `vehicle_truck_01`. If using numbers as a suffix, always use 2 digits. And **do not** use it as a versioning system! Use `git` or something similar.

### Scriptable Objects

`SCREAMING_SNAKE_CASE`

Use a prefix that defines the type of the scripo.

```
EVENT_GAME_START        # Game Event
FLOAT_PLAYER_HEALTH     # Reference Variable
```

### Persistent/Important GameObjects

`_snake_case`

Use a leading underscore to make object instances that are not specific to the current scene stand out.

### Debug Objects

`debug_someCase`

Mark objects that are only being used for debugging/testing and are not part of the release with a prefix `debug`.

```
DebugPlayerTestScene    # Debug scene
DEBUG_DECK_NO_ENEMIES   # Debug scripo
debug_enemy_burger      # Debug prefab
```

# Directory and File Structure

```
Root
+---Assets
+---bin             # Builds
\---Tools           # Programs to aid development: compilers, asset managers etc.
```

## Assets

```
Assets
+---Art
|   +---Animations  # Animation Clips and Animator Controller
|   +---Materials
|   +---Models      # FBX and BLEND files
|   +---Textures    # PNG files
|   \---VFX
+---Audio
|   +---Music
|   \---Sound       # Samples and sound effects
+---Code
|   +---Scripts     # C# scripts
|   \---Shaders     # Shader files and shader graphs
+---Level           # Anything related to game design in Unity
|   +---Prefabs
|   +---Scenes
|   \---UI
+---Scripos         # Scriptable Objects with global game data
|   +---Events
|   \---Variables
\---Resources       # Configuration files, localization text and other user files.
```

## Scripts

Use namespaces that match your directory structure.

A Framework directory is great for having code that can be reused across projects.

The Scripts folder varies depending on the project, however, `Environment`, `Framework`, `Tools` and `UI` should be consistent  across projects.

```
Scripts
+---Environment     # Game systems and base classes
+---Framework       # PigeonProject base franework
+---NPC
+---Player
+---Tools
\---UI
```

## Models

Separate files from the modelling program and ready to use, exported models.

```
Models
+---Blend
\---FBX
```

# Workflow

## Models

File extension: `FBX`

Even though Unity supports Blender files by default, it is better to keep what is being worked on and what is a complete, exported model separate. This is also a must when using other software, such as Substance for texturing.

Use `Y up`, `-Z forward` and `uniform scale` when exporting.

## Textures

File extension: `PNG`, `TIFF` or `HDR`

Choose either a `Specularity/Glossiness` or `Roughness/Metallic` workflow. This depends on the software being used and what your artists are more comfortable with. Specularity maps have the advantage of being having the possibility to be RGB maps instead of grayscale (useful for tinted metals), apart from that there is little difference between the result from either workflow.

### Texture Suffixes

Suffix | Texture
:------|:-----------------
`_AL`  | Albedo
`_SP`  | Specular
`_R`   | Roughness
`_MT`  | Metallic
`_GL`  | Glossiness
`_N`   | Normal
`_H`   | Height
`_DP`  | Displacement
`_EM`  | Emission
`_AO`  | Ambient Occlusion
`_M`   | Mask

### RGB Masks

It is good practice to use a single texture to combine black and white masks in a single texture split by each RGB channel. Using this, most textures should have:

```
texture_AL.png  # Albedo
texture_N.png   # Normal Map
texture_M.png   # Mask
```

Channel | Spec/Gloss        | Rough/Metal
:-------|:------------------|:-----------
R       | Specularity       | Roughness
G       | Glossiness        | Metallic
B       | Ambient Occlusion | Ambient Occlusion

#### The blue channel can vary depending on the type of material

- For character materials use the `B` channel for *subsurface opacity/strength*
- For anisotropic materials use the `B` channel for the *anisotropic direction map*

## Configuration Files

File extension: `INI`

Fast and easy to parse, clean and easy to tweak.

`XML`, `JSON`, and `YAML` are also good alternatives, pick one and be consistent.

Use binary file formats for files that should not be changed by the player. For multiplayer games store configuration data on a secure server.

## Localization

File extension: `CSV`

Widely used by localization software, makes it trivial to edit strings using spreadsheets.

## Audio

File extension: `WAV` while mixing, `OGG` in game.

Preload small sound clips to memory, load on the fly for longer music and less frequent ambient noise files.

# Be Consistent

> The point of having style guidelines is to have a common vocabulary of coding so people can concentrate on what you're saying rather than on how you're saying it. We present global style rules here so people know the vocabulary, but local style is also important. If code you add to a file looks drastically different from the existing code around it, it throws readers out of their rhythm when they go to read it. Avoid this.

-- [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)

---
