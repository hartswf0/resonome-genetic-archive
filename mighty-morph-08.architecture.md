
MODEL: Architectural Index of 
mighty-morph-08.html
This document provides a structured index of all entities, morphisms, and subsystems within the 
mighty-morph-08.html
 application. It uses Spencer-Brown-inspired distinctions (marked by ◻ for entities/distinctions and → for indications/morphisms) to recursively model the architecture from the global application down to granular components.

I. Meta-Model: The Application (portalApp)
The portalApp object serves as the global namespace and orchestrator for the entire application. It is the root distinction from which all other subsystems are drawn.

◻ portalApp: The central application namespace.
A. Core State & Context Entities
◻ audioContext: The root of the Web Audio API graph. Null until initialized.
◻ masterGain: The final GainNode before the audio destination. Controls master volume.
◻ globalAnalyser: A global AnalyserNode to measure audio levels for visualization.
◻ globalFrequencyData: A Uint8Array to hold frequency data from the analyser.
◻ globalAudioLevel: A smoothed, normalized value representing the current master audio output.
◻ leftDeck: A 
MorphicDeck
 instance (Drone).
◻ rightDeck: A 
MorphicDeck
 instance (Sequencer).
◻ deferredDeckState: A temporary holding object for state loaded from the URL before decks are initialized.
B. Global Control & UI State Entities
◻ crossfaderValue: Numeric state [0, 1] of the crossfader.
◻ masterOutputVolume: Numeric state [0, 1] for the master volume.
◻ globalFuzzAmount: Numeric state [0, 1] for the global distortion effect.
◻ uiVisible: Boolean state for the visibility of the main control panel.
◻ colorPalette: An array of HSV color values for the sequencer.
◻ activeColorIndex: The currently selected index from the colorPalette.
C. Core Application Morphisms (Methods)
→ initApplication(): The primary entry point after user interaction (click "Start").
→ Creates and resumes the audioContext.
→ Instantiates leftDeck and rightDeck.
→ Initializes all global audio nodes (masterGain, globalAnalyser, globalFuzzNode).
→ Calls 
initAudio()
 on both decks.
→ Applies deferredDeckState loaded from the URL.
→ Sets up all UI event listeners.
→ Starts the 
animate()
 loop.
→ animate(): The main requestAnimationFrame loop.
→ Reads data from globalAnalyser into globalFrequencyData.
→ Calculates globalAudioLevel.
→ Calls 
update()
 on both leftDeck and rightDeck, passing globalAudioLevel.
→ updateStateInURL(): Serializes the entire application state into a base64 string and stores it in the URL hash.
→ loadStateFromURL(): Deserializes state from the URL hash into portalApp state variables and deferredDeckState.
II. Deck Meta-Model: The 
MorphicDeck
 Class
This is the blueprint for both the Drone and Sequencer decks. It encapsulates all shared logic for visuals, audio, and state management.

◻ MorphicDeck: A class representing a single interactive audio-visual deck.
A. Core Entities (Properties)
◻ config: The configuration object (droneConfig or sequencerConfig) passed on instantiation. Defines the type.
◻ renderer: THREE.WebGLRenderer.
◻ scene: THREE.Scene.
◻ camera: THREE.PerspectiveCamera.
◻ material: THREE.ShaderMaterial holding the deck's specific shaders.
◻ outputGain: The deck's master GainNode, which connects to the globalAnalyser.
◻ deckVolume: The deck's individual volume level [0, 1], controlled by its slider.
B. Drone-Specific Entities
◻ mainVoice: Array of OscillatorNodes.
◻ filters: Array of BiquadFilterNodes.
◻ audioFrequency, audioResonance: Parameters controlling the drone sound.
C. Sequencer-Specific Entities
◻ layers: A 4x8 grid [{nodeStates: [bool], colorIndex: int}] holding the sequencer's pattern.
◻ sequencerVoices: Array of 8 audio sources (Oscillator > Gain), one for each step.
◻ sequencerVisualNodes: Array of 64 THREE.Mesh objects for the visual grid.
◻ isPlaying: Boolean flag to start/stop the sequencer.
◻ bpm: The tempo of the sequencer.
◻ schedulerInterval: The setInterval timer that drives the sequence. (Primary target for refactoring).
◻ currentSequencerStep: The currently active step (0-7).
D. Core Morphisms (Methods)
→ constructor(containerId, canvasId, config):
→ Initializes Three.js components (renderer, scene, camera).
→ Creates the main shader material.
→ If config.type === 'sequencer', it starts the schedulerInterval immediately. This is the source of the automatic behavior.
→ initAudio(audioContext, destinationNode):
→ Creates the deck's outputGain.
→ Connects outputGain to the destinationNode (globalAnalyser).
→ Creates the actual audio sources (mainVoice for drone, sequencerVoices for sequencer).
→ scheduler(): (Sequencer only)
→ This function is the "heartbeat" of the sequencer.
→ Checks if this.isPlaying is true.
→ Increments currentSequencerStep.
→ Iterates through layers and triggers audio for active nodes (layerState.nodeStates[step]).
→ Triggers corresponding visual effects (
triggerPulse
, 
shiftBloom
, etc.).
→ handleCanvasClick(event): (Sequencer only)
→ Maps a click's XY coordinates to the 4x8 grid.
→ Toggles the boolean state of the corresponding node in this.layers.
→ Calls 
updateSequencerVisualNodes()
 to reflect the change.
→ Calls portalApp.updateStateInURL() to save the new state.
→ initSequencerLayers(): (Sequencer only)
→ Initializes the layers array.
→ Hardcodes a default pattern, making the sequencer visually active on load.
→ update(globalAudioLevel): Called by portalApp.animate() on every frame.
→ Updates shader uniforms (time, mouse, audioLevel).
→ Decays visual effect parameters like beatPulse.
→ Renders the Three.js scene.
→ getState() / 
setState(state)
:
→ Serializes/deserializes the deck's specific state for URL persistence. This includes layers, isPlaying, bpm, etc., for the sequencer.
III. Audio Subsystem: How Sound Works
The audio system is a directed graph built with the Web Audio API, flowing from sources to the final destination.

◻ Audio Graph:
A. Drone Path
◻ OscillatorNode (x5) → ◻ BiquadFilterNode (xN) → ◻ leftDeck.outputGain → ◻ portalApp.globalAnalyser
B. Sequencer Path
◻ OscillatorNode (x8) → ◻ GainNode (x8, one per voice) → ◻ rightDeck.outputGain → ◻ portalApp.globalAnalyser
C. Master Path
◻ portalApp.globalAnalyser → ◻ portalApp.masterGain → ◻ Dry/Wet Gains for Fuzz → ◻ Destination
◻ portalApp.masterGain → ◻ Fuzz (WaveShaperNode) → ◻ Dry/Wet Gains for Fuzz → ◻ Destination
Morphisms (Control Flows):
→ Drone Sound Generation:
handleCanvasInteraction (mouse move) directly updates audioFrequency and other parameters.
updateAudioParameters
 is called, which sets the new target values on the OscillatorNode and BiquadFilterNode params.
→ Sequencer Sound Generation:
scheduler()
 checks layers and currentSequencerStep.
If a note is active, it triggers a gain envelope on the corresponding sequencerVoices gain node (linearRampToValueAtTime, exponentialRampToValueAtTime).
→ Volume Control:
Deck Volume: deck-volume-slider → 
setDeckVolume()
 → stores deckVolume property.
Crossfader: crossfader-slider → 
updateCrossfader()
 → calculates gain based on an equal-power curve and applies it to leftDeck.outputGain.gain and rightDeck.outputGain.gain, multiplied by the individual deckVolume.
Master Volume: master-output-volume-slider → 
updateMasterVolume()
 → sets the gain on portalApp.masterGain.gain.
IV. State & Persistence Subsystem
This system ensures the application's state can be saved and restored via the URL.

◻ State Object: A large JSON object representing the entire application state.
→ portalApp.updateStateInURL():
→ Calls 
getState()
 on both decks.
→ Collects global portalApp parameters (crossfaderValue, uiVisible, etc.).
→ JSON.stringify() the state object.
→ btoa() to encode the string.
→ Sets window.location.hash.
◻ URL Hash: The base64-encoded state string.
→ portalApp.loadStateFromURL():
→ atob() to decode the hash.
→ JSON.parse() the string into a state object.
→ Populates portalApp properties directly.
→ Stores the deck-specific parts in portalApp.deferredDeckState.
◻ Deferred State Application:
→ portalApp.applyDeferredDeckState():
→ Called after decks are fully initialized.
→ Calls 
setState()
 on each deck with its respective part of the deferred state. This is where the sequencer's layers and isPlaying status are restored.