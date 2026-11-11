# NixAppStore

NixAppStore is a planned desktop app for managing software and local services on a Nix-managed computer. Its first target is a single NixOS workstation running KDE Plasma. The project is at the planning stage.

In the **Apps** view, check the apps you want to install and uncheck the ones you want to remove. Review the changes together and apply them in one system rebuild. Software supplied by another configuration module has a separate source label.

In the **Services** view, find a system or user service, inspect its status and recent logs, and start, stop or restart it. An app with background components can link directly to those services.

The first milestone is a working install-and-remove cycle on NixOS. A later macOS milestone will reuse the selection and review workflow with nix-darwin and launchd. See the [project plan and acceptance criteria](ROADMAP.md).
