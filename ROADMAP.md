Ethos OS Initial Technical Roadmap: Phase 1 (Simplified)

This roadmap defines the key milestones for implementing the eBPF Resource Watchdog, a crucial component detailed in Section 3.1 of the Ethos Protocol Design Document. This component will rate-limit excessive, short-burst resource consumption used for fingerprinting.

Phase 1: Resource Watchdog Implementation

Milestone 1.0: Foundation and Toolchain Setup (2 Weeks)

Task 1.0.1: Set up the C/eBPF development environment (LLVM, Clang, Linux Headers).

Deliverable: Working Docker or VM development environment.

Task 1.0.2: Select and integrate a user-space library (e.g., BCC, libbpf) for loading and managing the eBPF program.

Deliverable: Initial build script (Makefile) configured to compile and link.

Task 1.0.3: Create the basic eBPF Map data structure (BPF_MAP_TYPE_HASH) keyed by Process ID (PID) for tracking resource usage.

Deliverable: watchdog_map.h definition and successful Map creation in the user-space loader.

Milestone 1.1: Core Monitoring Logic (4 Weeks)

Task 1.1.1: Implement the base eBPF program to attach to a generic system call hook (kprobe on a common I/O call like sys_read).

Deliverable: Basic eBPF program that loads and attaches successfully.

Task 1.1.2: Write the logic to extract the current Process ID (PID) from the kernel context.

Deliverable: Function to retrieve and hash the PID for Map lookup.

Task 1.1.3: Implement the core counting mechanism: Lookup the PID in the Map, increment the counter, and update the entry.

Deliverable: Verified ability to track per-process syscall count in the Map.

Milestone 1.2: Enforcement and Rate-Limiting (4 Weeks)

Task 1.2.1: Define the rate-limiting parameters (e.g., $100$ calls per $500\text{ms}$).

Deliverable: MAX_CALLS and TIME_WINDOW constants in the eBPF code.

Task 1.2.2: Implement a time check to reset the counter when the TIME_WINDOW is exceeded. Use bpf_ktime_get_ns().

Deliverable: Counter reset logic that uses kernel time effectively.

Task 1.2.3: Implement the blocking logic: If the counter is over the limit, return an error code (e.g., -EPERM) to the syscall, effectively dropping the operation.

Deliverable: Demonstration that an aggressive test application is blocked by the Watchdog.

Milestone 1.3: Verification and Documentation (2 Weeks)

Task 1.3.1: Write comprehensive comments for all eBPF C code and the user-space loader.

Deliverable: Full C code with inline documentation.

Task 1.3.2: Create a test suite (test.sh or similar) to verify rate-limiting under load and normal conditions.

Deliverable: Automated test that confirms blocking behavior.

Task 1.3.3: Document the complete eBPF program usage and verification process in a new docs/watchdog_guide.md file.

Deliverable: Published guide for community audit.

Next Phase: CPU Time Jitter (Section 4.1)

Upon successful completion of the Resource Watchdog, the next major objective will be implementing the CPU Time Jitter mechanism.
