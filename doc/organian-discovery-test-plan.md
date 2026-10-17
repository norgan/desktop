# Organian discovery integration: Windows test plan

Experimental fork build for discovery progress and batched directory reconciliation. Do not treat this branch as a released client or install into a critical production sync configuration.

## Build gate

- [ ] Windows CI compiles via KDE Craft and its pinned dependency revisions.
- [ ] CTest passes, including DiscoveryProgressTest and RemoteDiscoveryTest.
- [ ] The patched discovery benchmark builds (target DiscoveryBenchmark).
- [ ] Craft packages the actual checked-out branch and uploads an unsigned installer with a SHA256 manifest.
- [ ] Installer SHA256 matches its manifest after download.

## Isolated Windows acceptance test

Use a disposable Windows profile or VM, a separate Nextcloud account, and a small designated test-only sync directory. Preserve the existing desktop client and production sync data, and keep a rollback snapshot.

1. Connect the test client to the isolated account; verify login and expected filesystem integration.
2. Create nested folders and 5,000+ files. Verify the GUI remains responsive during discovery, with real examined-item counts and phase transitions.
3. While reconciling, close/open UI panels, observe tray status, and collect screenshots and timestamps. The UI must not present a misleading percent during indeterminate discovery.
4. Abort and restart a running discovery. Verify no crash, stale progress, incomplete journal update or incorrectly propagated file.
5. Interrupt server connectivity mid-scan, reconnect, and verify recovery and correct waiting/retry states.
6. Test virtual placeholders, hydration and selective offline pin states. Confirm untouched virtual folders do not hydrate unintentionally.
7. Compare final content, counts, checksums and conflict handling against the server in both directions. Run repeat syncs with no changes and a fresh full discovery.
8. Run the Discovery benchmark on the baseline and integrated build, collecting elapsed discovery time, maximum event-loop heartbeat gap and network health-response delay.

## Go/no-go

Never mark the client ready based solely on a green compile. Required: passing automated tests, a reproducible packaged Windows build, intact sync/VFS semantics and successful isolated end-to-end test. The installer is unsigned and Windows SmartScreen may warn; verify SHA256 and source run before testing. Do not bypass enterprise protections or distribute as an official Nextcloud release.
