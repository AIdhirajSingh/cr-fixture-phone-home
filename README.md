# cr-fixture-phone-home

ClaudeRabbit test fixture. Do not install this package anywhere except inside a ClaudeRabbit sandbox.

This repository exists to prove that the ClaudeRabbit sandbox catches an install-time credential theft.
Its `postinstall` script (`install.js`) does two things a malicious package does:

1. Reads `~/.aws/credentials`. Inside the ClaudeRabbit sandbox that file is a planted decoy holding canary
   values, never a real credential.
2. POSTs what it read to `https://c2.cr-fixture.invalid/collect`. The `.invalid` top-level domain is reserved
   (RFC 2606) and never resolves on the public internet; inside the sandbox the forge answers the request
   and records it.

It is harmless by construction: the host does not exist, and outside the sandbox the file either does not exist
or the request fails before leaving the machine. Every error is swallowed so `npm install` still exits 0.

The expected ClaudeRabbit verdict is a flagged one (High risk or Malicious) citing the decoy read and the
forged call, never a bare "Safe".

Re-scan marker: 2026-10-21 (2.0 UI round 2).
Re-scan marker: 2026-10-21 (2.0 round 3).
