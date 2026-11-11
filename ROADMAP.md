# Project plan

## First user and scope

Build a daily-use tool for one person who already maintains a flake-based NixOS desktop. Success means being able to install or remove an app, then diagnose a local service failure, without opening a terminal. Start on x86_64 Linux with KDE Plasma. The proposed interface uses Qt 6 and QML.

## Apps

Search a small initial catalog from the host's pinned nixpkgs input. Show each app's name, package attribute, availability for this host and installation state. Keep the catalog small until the full apply and recovery paths work.

| User action | Expected result |
| --- | --- |
| Check an available app | Mark it **Pending install**. The running system stays as it is until Apply. |
| Uncheck an app managed here | Mark it **Pending removal**. Toggling it back cancels the change. |
| Inspect an app supplied by another module | Show **Provided by configuration** and the source when known. Only entries owned by the app have editable checkboxes. |
| Review changes | Show additions and removals together. Discard restores the checkboxes to the applied selection. |
| Apply | Build the batch once, show the candidate system changes, then activate the reviewed build on confirmation. Refresh installed state from the running system. |

A host imports one dedicated package-list module. Keep edits within that list and preserve the host's lock file. A removal takes the package off the managed list. If another module or profile still supplies it, show that remaining source when identifiable. App removal preserves personal data.

**Build and recovery.** Stage a candidate from the chosen host configuration. Preview the managed-list diff and the wider system changes, including other pending configuration edits that would be activated. Use the same built system for activation. If the configuration changes after review, refresh the preview. Serialize apply operations. On build failure, retain the pending selection for correction or retry. On interrupted or partial activation, inspect the active generation and service state before offering retry or restoration of the previous configuration. System generation recovery covers configuration and packages. Service databases and personal files have their own backup lifecycle.

## Services

List system and user systemd services with their scope, current state and logs from the current boot. Start with a small explicit mapping between apps and their service units so that a failed background component can be found by the app's name. Other units remain searchable by unit name.

Start, Stop and Restart act immediately on the selected unit. Show the resulting state and any authorization or service error. Request system privileges only for the operation that needs them. A future persistent startup toggle will use the configuration review workflow. Keep it visually distinct from these runtime actions.

## Milestones

The first two milestones form a two-to-four-week personal prototype.

| Milestone | Deliverable | Acceptance checkpoint |
| --- | --- | --- |
| 1 · App changes | Small catalog, editable host list, pending selection, preview and apply | In a disposable NixOS configuration, select two installs and one removal. Cancel one selection, apply once and verify the remaining changes in the running desktop after reboot. Force a build error and verify the previous system still runs and the selection can be retried. |
| 2 · Local services | System and user units, status, current-boot logs and runtime controls | Use a synthetic failing user service. Find it by app name, read the failure, correct its test condition and restart it from the UI. Verify the UI reflects recovery and a denied system operation produces an actionable error. |
| 3 · Daily use | Try the same workflows on the maintainer's workstation for one week | Record completed installs, removals, service diagnoses and occasions that still required a terminal. Pick the next improvement from those observations. |
| 4 · macOS feasibility | Reuse the selection and review UI with a Darwin package catalog, nix-darwin activation and launchd services | On an Apple Silicon Mac, add and remove a supported package through the UI, then inspect and restart a test user agent. Verify the result after login. |

## Next action

Create a minimal public NixOS test flake with one shared package, one app-managed package list and one synthetic failing user service. Use it to implement the checkbox and review workflow first. Keep workstation-specific modules, paths and logs in the maintainer's own configuration.
