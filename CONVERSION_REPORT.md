# 

# plOtter FVS PWA revision and validation report

# 

# \*\*Build identification\*\*

# 

# Application version: 1.8.4

# Data schema version: 5

# Service-worker cache: `plotter-fvs-pwa-v1.8.4`  \*(verify against service-worker.js — the packaged file currently reads `v1.8.4`; align app version, cache name, and footer)\*

# Build date shown in the application: 2026-\_\_-\_\_  \*(set to actual build date)\*

# Delivery format: static HTTPS-hosted Progressive Web App

# 

# \*\*Revision 1.8.4 implementation\*\*

# 

# | Requested behavior | Implementation |

# |---|---|

# | Allow editing an entered tree | Each Trees Entered row has an edit (✎) control beside delete (✕). Editing loads the tree into the entry form in an Edit Tree #N mode, highlights the row, shows an in-form note, and replaces + Add Tree with Save Changes plus Cancel. Saving updates the record in place, keeping the original tree number and position |

# | Round-trip IDBH values into the edit form | For IDBH stands, the DBH field prefills with the originally entered integer (stored Actual DBH × 10); RealDBH stands prefill the stored value directly |

# | Add inline field help | ? buttons next to PV Code / PV Ref, Ecoregion, Site Species, and Site Index toggle short explanatory notes; a live DBH Entry Mode hint under the RealDBH/IDBH toggle shows a mode-specific entry example |

# | Strengthen Site Species / Site Index rules | Both must be entered together or both left blank; Site Index must be a whole number of 1 or greater; the offending field receives focus with a clear message |

# | Add Elevation and Slope validation | Elevation must be blank or 1 or greater (labeled actual); Stand Info and per-plot Slope cannot be negative |

# | Enforce DBH minimums | DBH Break cannot be negative (a negative entry becomes 0.0); tree DBH minimum is 0.1 in RealDBH mode and 1 in IDBH mode; Count, Height, and Age must be 1 or greater when entered |

# | Constrain damage severity to relevant codes | Severity appears only for damage codes 30–34 (1–6 class dropdown) and 25–29 (0–100 percent input) and is hidden for all other codes, for both Dmg 1/Sev 1 and Dmg 2/Sev 2 |

# | Show collection date in the export list | The Select Stands to Export list shows each stand's formatted, timezone-safe date alongside plot count and Variant |

# 

# \*\*Behavior details\*\*

# 

# \*Tree editing\*

# 

# Trees Entered rows expose an edit control in addition to delete. Selecting it sets an edit-target in UI state, activates the correct plot, and re-renders the entry form as Edit Tree #N with the fields prefilled. The primary action becomes Save Changes and writes back to the existing record via in-place assignment, so the tree retains its `tree\_id`, plot, and ordering; the tree counter is not incremented. Cancel, changing the active plot or stand, switching tabs, or deleting the edited tree each clear the edit-target and discard the pending edit without altering the saved record. IDBH stands display and accept the integer form in the field while continuing to store Actual DBH.

# 

# \*Inline help and hints\*

# 

# Contextual ? controls toggle short notes for the habitat/PV fields (optional; consult the FVS Variant Guide) and the site fields (optional; enter both or leave both blank). A DBH-mode hint updates when the RealDBH/IDBH toggle changes. These controls are presentation aids and do not affect stored data or export.

# 

# \*Validation\*

# 

# Stand Info save now enforces paired Site Species/Site Index entry, an integer Site Index of 1 or greater, Elevation blank or ≥ 1, non-negative Slope, and non-negative DBH Break. Adding a tree requires a chosen species (the Search… placeholder is rejected), enforces the RealDBH/IDBH DBH minimums, and requires Count, Height, and Age to be 1 or greater when present. Live-tree DBH continues to be required with Add Tree disabled until entered, and the `addTree()` guard continues to block blank-DBH live records without consuming a tree number. Dead-tree DBH remains optional.

# 

# \*Damage severity\*

# 

# The severity field is rendered conditionally on the selected damage code: a 1–6 class dropdown for dwarf mistletoe codes 30–34, a numeric percent input for defect codes 25–29, and no severity field for any other code. The same logic governs the secondary damage pair, which remains hidden until Dmg 1 is selected and clears when Dmg 1 is removed.

# 

# \*Export list\*

# 

# The stand-selection list in Export now displays each stand's date using a timezone-safe formatter (plain `YYYY-MM-DD` values are parsed as local dates to avoid off-by-one shifts), shown together with plot count, Variant, and saved/unsaved status. This is a display change only.

# 

# \*\*Retained revision 1.8.0 behavior\*\*

# 

# Unselected FVS Variant outlines use `#4b504d` at 1.25 px and the selected outline uses the same color at 1.75 px, with unchanged selected fill and unchanged state outlines, selection, lookup, GPS suggestion, floating Alaska, and location-pin behavior. Nonblank Actual DBH values display with one decimal place in Trees Entered and Review Data (RealDBH 6 and IDBH 60 both show 6.0). Review Data stand headings summarize plot count, tally-tree method and size, Break DBH, and Regen Plot size, with Tally Trees:, Break DBH:, and Regen Plot: emphasized. Stand Info uses Tally Trees: and Regen Plot: within Sampling Design, and the Regen Plot field explains that seedlings/saplings below Break DBH are measured there.

# 

# \*\*Retained revision 1.7.0 behavior\*\*

# 

# Location search is opt-in through the first Search… dropdown option and does not automatically open the mobile keyboard. The seedling/sapling icon appears beside below-break tree numbers in both tree tables. DBH Break is entered as Actual DBH regardless of RealDBH/IDBH tree-entry mode.

# 

# \*\*Data and export compatibility\*\*

# 

# Revision 1.8.4 continues to use schema version 5. No worksheet, CSV, or FVS field definitions were added, removed, or reordered. `BASAL\_AREA\_FACTOR` continues to receive Variable BAF or the negative absolute tree Fixed Plot denominator; `INV\_PLOT\_SIZE` continues to receive the Regen Plot fixed-plot denominator; `BRK\_DBH` continues to receive Actual DBH Break. Tree editing modifies existing records without changing their schema or identity. Inline help, added validation, conditional severity fields, the export-list date, DBH formatting, review summaries, and map styling are presentation/validation changes that add no export fields. Existing saved stands, plots, trees, species lists, and saved snapshots remain compatible.

# 

# \*\*Validation performed\*\*

# 

# The release is checked against the actual application source and the packaged files.

# 

# \- JavaScript syntax checks cover `app.js`, `service-worker.js`, `storage.js`, `pwa.js`, `map-data.js`, and `fvs-xlsx.js`.

# \- Source-level regression checks cover the tree edit/update path (in-place write, preserved tree number, edit-state clearing on navigation, IDBH prefill round-trip), the new Stand Info and tree validation rules, conditional damage-severity rendering for codes 25–29 and 30–34, the timezone-safe export date, and retained 1.8.0 output (map stroke/fill, DBH formatting, Review Data summaries, labels, Regen Plot guidance, live-tree DBH validation).

# \- A headless Chromium regression check covers the live-tree DBH requirement, RealDBH/IDBH conversion and edit-form prefill, edit/save/cancel button states, conditional severity fields, and page-level horizontal overflow; source-level checks also cover the responsive markup and styles.

# \- Manifest JSON, icon files, local HTML asset references, and every service-worker application-shell path are checked.

# \- Application footer and service-worker cache identifiers are aligned to revision 1.8.4; data schema remains 5.  \*(Confirm this alignment given the current `v1.8.4` cache string.)\*

# \- Final ZIP structure, CRC integrity, extraction, and SHA-256 are checked after packaging.

# 

# \*\*Deployment notes\*\*

# 

# Replace all hosted application files together at the existing HTTPS URL. The new service-worker cache identifier causes the browser to install fresh application assets while preserving site-origin storage. Make an Excel or CSV backup before deployment. Open the deployed application while online and use Update available when offered, or close and reopen after the new service worker installs.

# 

# \*\*Validation boundary\*\*

# 

# Automated tests use source-level checks and headless Chromium. The package has not been physically exercised on every intended iPhone, iPad, or Android model. Complete the device acceptance checklist in `README.md` before operational rollout, especially installation, keyboard behavior, location permission, offline relaunch, share-sheet destinations, tree edit/save/cancel on touch devices, and storage retention under organizational device policies.

